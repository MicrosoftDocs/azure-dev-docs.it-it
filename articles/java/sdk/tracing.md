---
title: Configurare la traccia in Azure SDK per Java
description: Panoramica dei concetti relativi alla traccia di Azure SDK per Java
author: samvaity
ms.date: 02/02/2021
ms.topic: conceptual
ms.custom: devx-track-java
ms.author: savaity
ms.openlocfilehash: 2dc2085ac71167cefd8fed5475dc9744cf520fea
ms.sourcegitcommit: 71847ee0a1fee3f3320503629d9a8c82319a1f6a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/03/2021
ms.locfileid: "99522071"
---
# <a name="configure-tracing-in-the-azure-sdk-for-java"></a>Configurare la traccia in Azure SDK per Java

Questo articolo fornisce una panoramica su come configurare Azure SDK per Java per integrare la funzionalità di traccia.

Azure SDK per Java consente la traccia in tutte le librerie client includendo una dipendenza dal plug-in [Azure-Core-Tracing-opentelemetry](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/core/azure-core-tracing-opentelemetry#azure-tracing-opentelemetry-client-library-for-java)basato su [OpenTelemetery](https://opentelemetry.io/). OpenTelemetry è un noto framework di osservabilità open source per la generazione, l'acquisizione e la raccolta di dati di telemetria per il software nativo del cloud.

Sono disponibili due concetti chiave relativi alla traccia: **span** e **Trace**. Un intervallo rappresenta una singola operazione in una traccia. Un intervallo può rappresentare una richiesta HTTP, una chiamata di procedura remota (RPC), una query di database o anche il percorso preso dal codice. Una traccia è un albero di intervalli che mostrano il percorso di lavoro tramite un sistema. È possibile distinguere una traccia in base a una sequenza univoca a 16 byte denominata TraceID. Per ulteriori informazioni su questi concetti e su come sono correlati a OpenTelemetry, vedere la [documentazione di OpenTelemetry](https://opentelemetry.io/docs/).

Esistono due modi per abilitare la traccia nelle librerie client di Azure per Java:

1. Abilitando la funzionalità incorporata in Azure SDK per Java.
2. Consentendo a un agente in-process di raccogliere i dati di traccia e di inviarli senza apportare modifiche al codice.

## <a name="enable-tracing-in-the-azure-sdk-for-java"></a>Abilitare la traccia in Azure SDK per Java

Per abilitare la traccia per tutte le librerie client Java di Azure, aggiungere le `azure-core-tracing-opentelemetry` `opentelemetry-sdk` dipendenze e all'applicazione. Ad esempio, in Maven aggiungere le dipendenze seguenti:

```xml
<dependency>
  <groupId>com.azure</groupId>
  <artifactId>azure-core-tracing-opentelemetry</artifactId>
  <version>1.0.0-beta.6</version>
</dependency>

<dependency>
  <groupId>io.opentelemetry</groupId>
  <artifactId>opentelemetry-sdk</artifactId>
  <version>0.8.0</version>
</dependency>
```

Con l'aggiunta di questa dipendenza, la traccia è abilitata con le tracce incluse con tutte le richieste HTTP. Ci sono ora due problemi:

1. Non esiste alcuna integrazione con alcun intervallo padre in ingresso.
2. Le tracce generate non vengono esportate in nessun punto per un'analisi successiva.

Le sezioni seguenti risolvono questi problemi.

### <a name="integrate-parent-spans"></a>Integra intervalli padre

Come indicato in precedenza, incluse le dipendenze consentiranno la traccia nelle librerie client di Azure. Tuttavia, non si integrerà con i dati di traccia in ingresso, ad esempio in un ambiente Web in cui una richiesta in ingresso comporta una chiamata a una libreria client di Azure. Per abilitare la traccia, è possibile creare un intervallo radice nell'applicazione e passarlo alle chiamate della libreria client di Azure, in modo che questo intervallo possa essere incapsulato in richieste in uscita appropriate per i servizi di Azure. Per eseguire questa attività, è possibile usare il `Context` parametro in tutti i metodi client, come illustrato nell'esempio seguente:

```java
// The 'span' given in this context is the parent span key received from the incoming request.
Context traceContext = new Context(PARENT_SPAN_KEY, span);

// This example code passes a new configuration setting to a service, but also includes
// the traceContext from above, so that it may be picked up by the http transport and included as appropriate.
appConfigClient.setConfigurationSettingWithResponse(new ConfigurationSetting().setKey("hello").setValue("world"), true, traceContext);
```

Nei casi in cui non viene fornito alcun intervallo padre, viene creato un nuovo intervallo padre per incapsulare tutte le richieste in uscita delle librerie client. Per ogni chiamata a un metodo della libreria client di Azure, vengono creati due intervalli: uno che traccia l'avanzamento attraverso le librerie client e l'altro traccia l'intervallo di richieste HTTP in uscita.

#### <a name="tracer-span-attributes"></a>Attributi span di traccia

Oltre agli attributi standard richiesti documentati nelle [convenzioni semantiche](https://github.com/open-telemetry/opentelemetry-specification/blob/e9340d74f1ba0b651b3581d6bd5df6a92b772e18/semantic-conventions.md)di OpenTelemetry, le librerie client di Azure annotano gli intervalli con gli attributi seguenti:

* `az.namespace`: [Spazi dei nomi](/azure/azure-resource-manager/management/azure-services-resource-providers) del provider di risorse Microsoft mappati ai servizi di Azure.
* `x-ms-request-id`: Identificatore univoco per la richiesta.
* `span.kind`: Descrive la relazione tra l'intervallo, i relativi elementi padre e i relativi elementi figlio in una traccia.
* `span.status.message`: Rappresenta lo stato di un intervallo terminato.
* `span.status.code`: Rappresenta il codice di stato di un intervallo terminato.

Un numero maggiore di metadati relativi all'operazione eseguita viene acquisito nei nomi degli intervalli. I nomi di intervallo HTTP sono impostati sul valore del percorso URI e l'intervallo di chiamata del metodo di libreria è nel formato `<namespace qualified type>.<method name>` .

Ad esempio, una richiesta del client di configurazione dell'app per impostare l'impostazione di configurazione, ovvero, comporterà `appConfigClient.setConfigurationSettingWithResponse(new ConfigurationSetting().setKey("hello").setValue("world")` i due intervalli seguenti:

* Estensione del metodo della libreria denominata `AppConfig.setKey` .
* Un intervallo di richieste HTTP in uscita denominato `/kv/hello` .

### <a name="configure-tracing-exports"></a>Configurare le esportazioni della traccia

Le applicazioni che desiderano utilizzare le informazioni di traccia devono esportare le tracce in un archivio di analisi distribuito, ad esempio [Zipkin](https://zipkin.io/), [Jaeger](https://www.jaegertracing.io/)e [monitoraggio di Azure](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/monitor/microsoft-opentelemetry-exporter-azuremonitor#azure-monitor-opentelemetry-exporter-client-library-for-java). Nell'esempio seguente viene configurata l'esportazione delle informazioni di traccia in un archivio di traccia distribuito Jaeger in esecuzione sulla porta localhost 14250, usando le API specifiche di Jaeger:

```java
ManagedChannel channel = ManagedChannelBuilder.forAddress("localhost", 14250).usePlaintext().build();
JaegerGrpcSpanExporter exporter = JaegerGrpcSpanExporter.newBuilder()
    .setChannel(channel)
    .setServiceName("Sample")
    .setDeadline(0)
    .build();
TracerSdkFactory tracerSdkFactory = (TracerSdkFactory) OpenTelemetry.getTracerFactory();
tracerSdkFactory.addSpanProcessor(SimpleSpansProcessor.newBuilder(exporter).build());
```

## <a name="enable-tracing-with-the-in-process-agent"></a>Abilitare la traccia con l'agente in-process

È possibile usare Application Insights, una funzionalità di [monitoraggio di Azure](/azure/azure-monitor/overview), per la raccolta e la trasmissione automatica dei dati per un'analisi successiva delle applicazioni in sistemi distribuiti su larga scala. Questa strumentazione monitora l'applicazione e indirizza i dati di telemetria a una [risorsa applicazione Azure Insights](/azure/azure-monitor/app/app-insights-overview) usando un GUID univoco denominato "chiave di strumentazione".

Utilizzando un [agente Java in-process](/azure/azure-monitor/app/java-in-process-agent), è possibile abilitare il monitoraggio delle applicazioni senza apportare modifiche al codice. È anche necessario aggiungere la dipendenza [Azure-Core-Tracing-opentelemetry](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/core/azure-core-tracing-opentelemetry#azure-tracing-opentelemetry-client-library-for-java) al progetto. Dopo aver completato questa attività, è possibile usare il Dashboard Application Insights per instrumentare le richieste, raccogliere i contatori delle prestazioni, diagnosticare problemi di prestazioni ed eccezioni e scrivere codice per tenere traccia delle operazioni eseguite dagli utenti all'interno di un'applicazione.

## <a name="next-steps"></a>Passaggi successivi

Ora che si ha familiarità con la funzionalità di base di cross-cutting in Azure SDK per Java, vedere [autenticazione di Azure con Java e identità di Azure](identity.md) per informazioni su come creare applicazioni sicure.
