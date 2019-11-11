---
title: Introduzione a Logz.io per progetti Java in esecuzione in Azure
description: Questa esercitazione illustra come integrare e configurare Logz.io per i progetti Java in esecuzione in Azure.
author: jdubois
manager: bborges
ms.devlang: java
ms.topic: tutorial
ms.service: azure
ms.date: 11/05/2019
ms.author: judubois
ms.openlocfilehash: 49fd2ada98bcfdb02db3f4b79afb2f80f2d700f2
ms.sourcegitcommit: 380300c283f3df8a87c7c02635eae3596732fb72
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/06/2019
ms.locfileid: "73661282"
---
# <a name="tutorial-getting-started-with-logzio-for-java-projects-running-on-azure"></a>Esercitazione: introduzione a Logz.io per progetti Java in esecuzione in Azure

Questa esercitazione illustra come configurare un'applicazione Java classica per inviare i log al servizio [Logz.io](https://logz.io/) per l'inserimento e l'analisi. Logz.io offre una soluzione di monitoraggio completa basata su Elasticsearch, Logstash, Kibana e Grafana.

L'esercitazione presuppone l'uso di Log4J o Logback. Queste librerie sono le due più diffuse per la registrazione in Java, quindi l'esercitazione dovrebbe funzionare per la maggior parte delle applicazioni in esecuzione in Azure. Se si usa già lo stack Elastic per monitorare l'applicazione Java, questa esercitazione illustra come eseguire la riconfigurazione per l'endpoint Logz.io.

In questa esercitazione si apprenderà come:

> [!div class="checklist"]
> * Inviare i log da un'applicazione Java esistente a Logz.io.
> * Inviare i log di diagnostica e le metriche dai servizi di Azure a Logz.io.

## <a name="prerequisites"></a>Prerequisiti

* [Java Developer Kit](https://aka.ms/azure-jdks), versione 8 o successiva
* Un account [Logz.io](https://logz.io/). In alternativa, è possibile acquistare Logz.io in [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/logz.logzio-elk-as-a-service-pro).
* Un'applicazione Java esistente che usa Log4J o Logback

## <a name="send-java-application-logs-to-logzio"></a>Inviare i log applicazioni Java a Logz.io

In primo luogo si apprenderà come configurare l'applicazione Java con un token che consente di accedere all'account Logz.io.

### <a name="get-your-logzio-access-token"></a>Ottenere il token di accesso a Logz.io

Per ottenere il token, accedere all'account Logz.io, selezionare l'icona a forma di ingranaggio nell'angolo in alto a destra, quindi selezionare **Settings > General** (Impostazioni > Generale). Copiare il token di accesso visualizzato nelle impostazioni dell'account per poterlo usare in seguito.

### <a name="install-and-configure-the-logzio-library-for-log4j-or-logback"></a>Installare e configurare la libreria Logz.io per Log4J o Logback

La libreria Java Logz.io è disponibile in Maven Central, quindi è possibile aggiungerla come dipendenza alla configurazione del progetto. Verificare il numero di versione in Maven Central e usare la versione più recente nelle impostazioni di configurazione seguenti.

Se si usa Maven, aggiungere la dipendenza seguente al file `pom.xml`:

**Log4J:**

```xml
<dependency>
    <groupId>io.logz.log4j2</groupId>
    <artifactId>logzio-log4j2-appender</artifactId>
    <version>1.0.11</version>
</dependency>
```

**Logback:**

```xml
<dependency>
    <groupId>io.logz.logback</groupId>
    <artifactId>logzio-logback-appender</artifactId>
    <version>1.0.22</version>
</dependency>
```

Se si usa Gradle, aggiungere la dipendenza seguente allo script di compilazione:

**Log4J:**

```
implementation 'io.logz.log4j:logzio-log4j-appender:1.0.11'
```

**Logback:**

```
implementation 'io.logz.logback:logzio-logback-appender:1.0.22'
```

Aggiornare quindi il file di configurazione Log4J o Logback:

**Log4J:**

```xml
<Appenders>
    <LogzioAppender name="Logzio">
        <logzioToken>{{your-logz-io-token}}</logzioToken>
        <logzioType>java-application</logzioType>
        <logzioUrl>https://listener-wa.logz.io:8071</logzioUrl>
    </LogzioAppender>
</Appenders>

<Loggers>
    <Root level="info">
        <AppenderRef ref="Logzio"/>
    </Root>
</Loggers>
```

**Logback:**

```xml
<configuration>
    <!-- Use shutdownHook so that we can close gracefully and finish the log drain -->
    <shutdownHook class="ch.qos.logback.core.hook.DelayingShutdownHook"/>
    <appender name="LogzioLogbackAppender" class="io.logz.logback.LogzioLogbackAppender">
        <token>{{your-logz-io-token}}</token>
        <logzioUrl>https://listener-wa.logz.io:8071</logzioUrl>
        <logzioType>java-application</logzioType>
        <filter class="ch.qos.logback.classic.filter.ThresholdFilter">
            <level>INFO</level>
        </filter>
    </appender>

    <root level="debug">
        <appender-ref ref="LogzioLogbackAppender"/>
    </root>
</configuration>
```

L'elemento `logzioType` fa riferimento a un campo logico in Elasticsearch che viene usato per separare documenti diversi. È essenziale configurare correttamente questo parametro per ottenere il massimo da Logz.io.

Un "tipo" di Logz.io è il formato del log, ad esempio Apache, NGinx, MySQL, e non l'origine in uso, ad esempio server1, server2, server3. Per questa esercitazione viene chiamato il tipo `java-application` perché vengono configurate applicazioni Java e si prevede che il formato di tutte le applicazioni sia lo stesso.

Per un uso avanzato, è possibile raggruppare le applicazioni Java in tipi diversi, ognuno con il formato di log specifico (configurabile con Log4J e Logback). È possibile ad esempio che siano presenti un tipo "spring-boot-monolith" e un tipo "spring-boot-microservice".

### <a name="test-your-configuration-and-log-analysis-on-logzio"></a>Testare la configurazione e l'analisi dei log in Logz.io

Dopo la configurazione della libreria Logz.io, l'applicazione deve ora inviare i log direttamente al dispositivo. Per verificare che il funzionamento sia corretto, passare alla console di Logz.io, selezionare la scheda **Live Tail**, quindi selezionare **run** (esegui). Viene visualizzato un messaggio simile al seguente per indicare che la connessione è funzionante:

```output
Requesting Live Tail access...
Access granted. Opening connection...
Connected. Tailing...
````

Successivamente, avviare l'applicazione o usarla per generare alcuni log. I log devono essere visualizzati direttamente sullo schermo. Ecco ad esempio i primi messaggi di avvio di un'applicazione Spring Boot:

```output
2019-09-19 12:54:40.685Z Starting JavaApp on javaapp-default-9-5cfcb8797f-dfp46 with PID 1 (/workspace/BOOT-INF/classes started by cnb in /workspace)
2019-09-19 12:54:40.686Z The following profiles are active: prod
2019-09-19 12:54:42.052Z Bootstrapping Spring Data repositories in DEFAULT mode.
2019-09-19 12:54:42.169Z Finished Spring Data repository scanning in 103ms. Found 6 repository interfaces.
2019-09-19 12:54:43.426Z Bean 'spring.task.execution-org.springframework.boot.autoconfigure.task.TaskExecutionProperties' of type [org.springframework.boot.autoconfigure.task.TaskExecutionProperties] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
```

Ora che i log sono stati elaborati da Logz.io, è possibile sfruttare tutti i servizi della piattaforma.

## <a name="send-azure-services-data-to-logzio"></a>Inviare i dati dei servizi di Azure a Logz.io

Si apprenderà come inviare i log e le metriche dalle risorse di Azure a Logz.io.

### <a name="deploy-the-template"></a>Distribuire il modello

Il primo passaggio consiste nel distribuire il modello di integrazione tra Logz.io e Azure. L'integrazione si basa su un modello di distribuzione di Azure pronto per la configurazione di tutti i blocchi predefiniti della pipeline necessari. Il modello crea uno spazio dei nomi dell'hub eventi, un hub eventi, due BLOB di archiviazione e tutte le autorizzazioni e le connessioni corrette necessarie. Le risorse impostate dalla distribuzione automatica possono raccogliere i dati per una singola area di Azure e inviarli a Logz.io.

Trovare il pulsante **Distribuisci in Azure** visualizzato nel [primo passaggio del file Leggimi del repository](https://github.com/logzio/logzio-azure-serverless).

Quando si seleziona **Distribuisci in Azure**, la pagina **Distribuzione personalizzata** nel portale di Azure verrà visualizzata con un elenco di campi precompilati.

È possibile lasciare invariata la maggior parte dei campi, ma è necessario immettere le impostazioni seguenti:

* **Gruppo di risorse**: Selezionare un gruppo esistente o crearne uno nuovo.
* **Host log/metriche Logzio**: Immettere l'URL del listener di Logz.io. Se non si è certi dell'URL, controllare l'URL di accesso. Se l'URL è app.logz.io, usare listener.logz.io (impostazione predefinita). Se l'URL è app-eu.logz.io, usare listener-eu.logz.io.
* **Token metriche/log Logzio**: Immettere il token dell'account Logz.io a cui si vogliono inviare i log o le metriche di Azure. Questo token è reperibile nella pagina dell'account nell'interfaccia utente di Logz.io.

Accettare le condizioni nella parte inferiore della pagina e quindi selezionare **Acquista**. In Azure viene avviata la distribuzione del modello, operazione che può richiedere un minuto o due. Nella parte superiore del portale viene visualizzato il messaggio "Distribuzione riuscita".

Per esaminare le risorse distribuite, è possibile visitare il gruppo di risorse definito.

Per informazioni su come configurare logzio-azure-serverless per eseguire il backup dei dati in Archiviazione BLOB di Azure, vedere [Inviare i log attività di Azure](https://docs.logz.io/shipping/log-sources/azure-activity-logs.html).

### <a name="stream-azure-logs-and-metrics-to-logzio"></a>Trasmettere log e metriche di Azure a Logz.io

Ora che è stato distribuito il modello di integrazione, è necessario configurare Azure per trasmettere i dati di diagnostica all'hub eventi distribuito. Quando i dati vengono inseriti nell'hub eventi, l'app per le funzioni li inoltrerà a Logz.io.

1. Nella barra di ricerca digitare "Diagnostica" e quindi selezionare **Impostazioni di diagnostica**.

2. Scegliere una risorsa nell'elenco di risorse, quindi selezionare **Aggiungi impostazione di diagnostica** per aprire il pannello **Impostazioni di diagnostica** per la risorsa specifica.

    ![Pannello Impostazioni di diagnostica](media/java-get-started-with-logzio/diagnostics-settings.png)

3. Inserire un valore nel campo **Nome** per le impostazioni di diagnostica.

4. Selezionare **Streaming a un hub eventi**, quindi selezionare **Configura** per aprire il pannello **Selezionare l'hub eventi**.

5. Scegliere l'hub eventi:

    * **Selezionare lo spazio dei nomi dell'hub eventi**. Scegliere lo spazio dei nomi che inizia con **Logzio** (ad esempio `LogzioNS6nvkqdcci10p`).
    * **Selezionare il nome dell'hub eventi**. Per i log, scegliere **insights-operational-logs**, mentre per le metriche scegliere **insights-operational-metrics**.
    * **Selezionare il nome dei criteri per l'hub eventi**. Scegliere **LogzioSharedAccessKey**.

6. Selezionare **OK** per tornare al pannello **Impostazioni di diagnostica**.

7. Nella sezione Log selezionare i dati da trasmettere e quindi selezionare **Salva**.

I dati selezionati vengono trasmessi all'hub eventi.

### <a name="visualize-your-data"></a>Visualizzare i dati

Successivamente, attendere il tempo necessario affinché i dati vengano trasferiti dal sistema in uso a Logz.io e quindi aprire Kibana. Vengono visualizzati i dati (con il tipo _eventhub_) nei dashboard. Per altre informazioni su come creare i dashboard, vedere [Creazione del dashboard Kibana perfetto](https://logz.io/blog/perfect-kibana-dashboard/).

Qui è possibile eseguire una query per i dati specifici nella scheda **Individua** oppure creare oggetti Kibana per visualizzare i dati nella scheda **Visualizza**.

## <a name="clean-up-resources"></a>Pulire le risorse

Dopo aver terminato le operazioni relative alle risorse di Azure create in questa esercitazione, è possibile eliminarle usando il comando seguente:

```azurecli-interactive
az group delete --name <resource group>
```

## <a name="next-steps"></a>Passaggi successivi

In questa esercitazione si è appreso come configurare l'applicazione Java e i servizi di Azure per l'invio di log e metriche a Logz.io.

Per altre informazioni sull'uso dell'hub eventi per monitorare l'applicazione, vedere:

> [!div class="nextstepaction"]
> [Trasmettere i dati di monitoraggio di Azure a un hub eventi per il consumo da parte di uno strumento esterno](/azure/azure-monitor/platform/stream-monitoring-data-event-hubs)
