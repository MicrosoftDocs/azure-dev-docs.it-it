---
title: Client e pipeline HTTP in Azure SDK per Java
description: Panoramica della funzionalità client e pipeline HTTP in Azure SDK per Java
author: srnagar
ms.date: 02/02/2021
ms.topic: conceptual
ms.custom: devx-track-java
ms.author: srnagar
ms.openlocfilehash: 93595096bc6d9fde1226f6875b866ea28b0a85bb
ms.sourcegitcommit: 71847ee0a1fee3f3320503629d9a8c82319a1f6a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/03/2021
ms.locfileid: "99522125"
---
# <a name="http-clients-and-pipelines-in-the-azure-sdk-for-java"></a>Client e pipeline HTTP in Azure SDK per Java

Questo articolo fornisce una panoramica dell'uso della funzionalità client e pipeline HTTP in Azure SDK per Java. Questa funzionalità offre un'esperienza coerente, potente e flessibile per gli sviluppatori che usano tutte le librerie di Azure SDK per Java.

## <a name="http-clients"></a>Client HTTP

Azure SDK per Java viene implementato usando un' `HttpClient` astrazione. Questa astrazione consente un'architettura modulare che accetta più librerie client HTTP o implementazioni personalizzate. Tuttavia, per semplificare la gestione delle dipendenze per la maggior parte degli utenti, tutte le librerie client di Azure dipendono da `azure-core-http-netty` . Di conseguenza, il client http [Netty](https://netty.io) è il client predefinito usato in tutte le librerie di Azure SDK per Java.

Sebbene Netty sia il client HTTP predefinito, l'SDK fornisce tre implementazioni client, a seconda delle dipendenze già presenti nel progetto. Queste implementazioni sono per:

* [Netty](https://netty.io)
* [OkHttp](https://square.github.io/okhttp/)
* Il nuovo [HttpClient](https://openjdk.java.net/groups/net/httpclient/intro.html) introdotto in JDK 11

### <a name="replace-the-default-http-client"></a>Sostituire il client HTTP predefinito

Se si preferisce un'altra implementazione, è possibile rimuovere la dipendenza da Netty escludendo tale dipendenza nei file di configurazione della compilazione. In un file di *pom.xml* Maven è possibile escludere la dipendenza Netty e includere un'altra dipendenza.

Nell'esempio seguente viene illustrato come escludere la dipendenza Netty da una dipendenza reale dalla `azure-security-keyvault-secrets` libreria. Assicurarsi di escludere Netty da tutte le `com.azure` librerie appropriate, come illustrato di seguito:

```xml
<dependency>
    <groupId>com.azure</groupId>
    <artifactId>azure-security-keyvault-secrets</artifactId>
    <version>4.2.2.</version>
    <exclusions>
      <exclusion>
        <groupId>com.azure</groupId>
        <artifactId>azure-core-http-netty</artifactId>
      </exclusion>
    </exclusions>
</dependency>

<dependency>
  <groupId>com.azure</groupId>
  <artifactId>azure-core-http-okhttp</artifactId>
  <version>1.3.3</version>
</dependency>
```

> [!NOTE]
> Se si rimuove la dipendenza Netty ma non si specifica alcuna implementazione, l'applicazione non verrà avviata. Nell'oggetto `HttpClient` classpath deve esistere un'implementazione.

### <a name="configure-http-clients"></a>Configurare i client HTTP

Quando si compila un client del servizio, verrà utilizzato il valore predefinito `HttpClient.createDefault()` . Questo metodo restituisce un'istanza di base `HttpClient` basata sull'implementazione client HTTP specificata. Se è necessario un client HTTP più complesso, ad esempio un proxy, ogni implementazione offre un generatore che consente di creare un'istanza configurata `HttpClient` . I generatori sono `NettyAsyncHttpClientBuilder` , `OkHttpAsyncHttpClientBuilder` e `JdkAsyncHttpClientBuilder` .

Gli esempi seguenti illustrano come compilare `HttpClient` istanze usando Netty, OkHttp e il client http JDK 11. Queste istanze eseguono il proxy tramite `http://localhost:3128` ed eseguono l'autenticazione con l' *esempio* utente con password *weakPassword*.

```java
// Netty
HttpClient httpClient = new NettyAsyncHttpClientBuilder()
    .proxy(new ProxyOptions(ProxyOptions.Type.HTTP, new InetSocketAddress("localhost", 3128))
        .setCredentials("example", "weakPassword"))
    .build();

// OkHttp
HttpClient httpClient = new OkHttpAsyncHttpClientBuilder()
    .proxy(new ProxyOptions(ProxyOptions.Type.HTTP, new InetSocketAddress("localhost", 3128))
        .setCredentials("example", "weakPassword"))
    .build();

// JDK 11 HttpClient
HttpClient client = new JdkAsyncHttpClientBuilder()
    .proxy(new ProxyOptions(ProxyOptions.Type.HTTP, new InetSocketAddress("localhost", 3128))
        .setCredentials("example", "weakPassword"))
    .build();
```

È ora possibile passare l' `HttpClient` istanza costruita in un generatore client del servizio per l'uso come client per la comunicazione con il servizio. Nell'esempio seguente viene utilizzata la nuova `HttpClient` istanza per compilare un client BLOB del servizio di archiviazione di Azure.

```java
BlobClient blobClient = new BlobClientBuilder()
    .connectionString(<connection string>)
    .containerName("container")
    .blobName("blob")
    .httpClient(httpClient)
    .build();
```

Per le librerie di gestione, è possibile impostare durante la configurazione di gestione `HttpClient` .

```java
AzureResourceManager azureResourceManager = AzureResourceManager.configure()
    .withHttpClient(httpClient)
    .authenticate(credential, profile)
    .withDefaultSubscription();
```

## <a name="http-pipeline"></a>Pipeline HTTP

La pipeline HTTP è uno dei componenti principali per ottenere la coerenza e la diagnostica nelle librerie client Java per Azure. Una pipeline HTTP è composta da:

* Un trasporto HTTP
* Criteri della pipeline HTTP

Quando si crea un client, è possibile specificare una pipeline HTTP personalizzata. Se non si specifica una pipeline, la libreria client ne creerà una configurata per l'uso con la libreria client specifica.

### <a name="http-transport"></a>Trasporto HTTP

Il trasporto HTTP è responsabile dell'instaurazione della connessione al server e dell'invio e della ricezione di messaggi HTTP. Il trasporto HTTP costituisce il gateway per le librerie client di Azure SDK per interagire con i servizi di Azure. Come indicato in precedenza in questo articolo, Azure SDK per Java usa [Netty](https://netty.io/) per impostazione predefinita per il trasporto HTTP. Tuttavia, l'SDK fornisce anche un trasporto HTTP collegabile, in modo da poter usare altre implementazioni laddove appropriato. L'SDK fornisce anche altre due implementazioni del trasporto HTTP per OkHttp e il client HTTP fornito con JDK 11 e versioni successive.

### <a name="http-pipeline-policies"></a>Criteri della pipeline HTTP

Una pipeline è costituita da una sequenza di passaggi eseguiti per ogni round trip HTTP richiesta-risposta. Ogni criterio ha uno scopo dedicato e agirà su una richiesta o una risposta o a volte. Poiché tutte le librerie client hanno un livello "Azure core" standard, questo livello garantisce che ogni criterio venga eseguito in ordine nella pipeline. Quando si invia una richiesta, i criteri vengono eseguiti nell'ordine in cui sono stati aggiunti alla pipeline. Quando si riceve una risposta dal servizio, i criteri vengono eseguiti in ordine inverso. Tutti i criteri aggiunti alla pipeline vengono eseguiti prima di inviare la richiesta e dopo la ricezione di una risposta. Il criterio deve decidere se agire sulla richiesta, la risposta o entrambi. Un criterio di registrazione, ad esempio, registrerà la richiesta e la risposta, ma i criteri di autenticazione sono interessati solo alla modifica della richiesta.

Il Framework di base di Azure fornirà i criteri con i dati di richiesta e risposta necessari insieme a qualsiasi contesto necessario per eseguire il criterio. Il criterio può quindi eseguire l'operazione con i dati specificati e passare il controllo insieme ai criteri successivi nella pipeline.

![Diagramma pipeline HTTP](./media/http-pipeline.svg)

### <a name="http-pipeline-policy-position"></a>Posizione dei criteri della pipeline HTTP

Quando si effettuano richieste HTTP a servizi cloud, è importante gestire gli errori temporanei e ripetere i tentativi non riusciti. Poiché questa funzionalità è un requisito comune, Azure Core fornisce un criterio di ripetizione dei tentativi che può controllare gli errori temporanei e ripetere automaticamente la richiesta.

Questo criterio di ripetizione dei tentativi suddivide l'intera pipeline in due parti, ovvero i criteri che vengono eseguiti prima del criterio di ripetizione dei tentativi e dei criteri eseguiti dopo i criteri di ripetizione dei tentativi. I criteri aggiunti prima che i criteri di ripetizione dei tentativi vengono eseguiti solo una volta per ogni operazione API e i criteri aggiunti dopo i criteri di ripetizione vengono eseguiti il numero di volte dei tentativi.

Quando si compila la pipeline HTTP, è quindi necessario comprendere se eseguire un criterio per ogni tentativo di richiesta o una volta per ogni operazione dell'API.

### <a name="common-http-pipeline-policies"></a>Criteri comuni della pipeline HTTP

Le pipeline HTTP per i servizi basati su REST hanno configurazioni con criteri per l'autenticazione, nuovi tentativi, registrazione, telemetria e specificano l'ID della richiesta nell'intestazione. Azure core è già caricato con questi criteri HTTP comunemente necessari che è possibile aggiungere alla pipeline.

| Criteri                | Collegamento di GitHub        |
|-----------------------|--------------------|
| Criteri di ripetizione          | [RetryPolicy. Java](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/core/azure-core/src/main/java/com/azure/core/http/policy/RetryPolicy.java) |
| Authentication Policy | [BearerTokenAuthenticationPolicy. Java](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/core/azure-core/src/main/java/com/azure/core/http/policy/BearerTokenAuthenticationPolicy.java) |
| Criteri di registrazione        | [HttpLoggingPolicy. Java](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/core/azure-core/src/main/java/com/azure/core/http/policy/HttpLoggingPolicy.java) |
| Criterio ID richiesta     | [RequestIdPolicy. Java](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/core/azure-core/src/main/java/com/azure/core/http/policy/RequestIdPolicy.java) |
| Criteri di telemetria      | [UserAgentPolicy. Java](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/core/azure-core/src/main/java/com/azure/core/http/policy/UserAgentPolicy.java) |

### <a name="custom-http-pipeline-policy"></a>Criteri della pipeline HTTP personalizzati

Il criterio della pipeline HTTP fornisce un meccanismo pratico per modificare o decorare la richiesta e la risposta. È possibile aggiungere criteri personalizzati alla pipeline creati dall'utente o dallo sviluppatore della libreria client. Quando si aggiungono i criteri alla pipeline, è possibile specificare se il criterio deve essere eseguito per chiamata o per tentativo.

Per creare un criterio personalizzato della pipeline HTTP, è sufficiente estendere un tipo di criteri di base e implementare un metodo astratto. È quindi possibile collegare i criteri alla pipeline.

## <a name="next-steps"></a>Passaggi successivi

Ora che si ha familiarità con la funzionalità client HTTP in Azure SDK per Java, vedere [configurare proxy in Azure SDK per Java](proxying.md) per informazioni su come personalizzare ulteriormente il client HTTP in uso.
