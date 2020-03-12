---
title: Eseguire la migrazione di applicazioni Java SE a Java SE nel servizio app di Azure
description: Questa guida descrive gli aspetti da considerare per la migrazione di un'applicazione Spring Boot o di un'altra applicazione Web autonoma esistente al servizio app di Azure usando Java SE.
author: yevster
ms.author: yebronsh
ms.topic: conceptual
ms.date: 01/22/2019
ms.openlocfilehash: 2846dc10ff782568d596daee4baa8ecbd1195729
ms.sourcegitcommit: 56e5f51daf6f671f7b6e84d4c6512473b35d31d2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/07/2020
ms.locfileid: "78894194"
---
# <a name="migrate-executable-jar-web-applications-to-java-se-on-azure-app-service"></a>Eseguire la migrazione di applicazioni Web JAR eseguibili a Java SE nel servizio app di Azure

Questa guida descrive gli aspetti da considerare per la migrazione di un'applicazione Spring Boot o di un'altra applicazione Web server incorporata esistente al servizio app di Azure usando Java SE.

## <a name="before-you-start"></a>Prima di iniziare

Se non è possibile soddisfare i requisiti di pre-migrazione, vedere le guide alla migrazione complementari seguenti:

* Eseguire la migrazione di applicazioni JAR eseguibili ai contenitori nel servizio Azure Kubernetes (in pianificazione)
* Eseguire la migrazione di applicazioni JAR eseguibili alle macchine virtuali di Azure (in pianificazione)

## <a name="pre-migration"></a>Pre-migrazione

### <a name="switch-to-a-supported-platform"></a>Passare a una piattaforma supportata

Il servizio app offre versioni specifiche di Java SE. Per garantire la compatibilità, eseguire la migrazione dell'applicazione a una delle versioni supportate dell'ambiente corrente prima di procedere con i passaggi rimanenti. Assicurarsi di testare completamente la configurazione risultante. Usare la versione stabile più recente della distribuzione Linux in questi test.

[!INCLUDE [note-obtain-your-current-java-version](includes/migration/note-obtain-your-current-java-version.md)]

### <a name="inventory-external-resources"></a>Inventario delle risorse esterne

Identificare le risorse esterne, ad esempio le origini dati, i broker dei messaggi JMS e gli URL di altri servizi. Nelle applicazioni Spring Boot, in genere è possibile trovare la configurazione per tali risorse nella directory *src/main/directory*, in un file generalmente denominato *application.properties* o *application.yml*. Verificare inoltre le variabili di ambiente della distribuzione di produzione per rilevare eventuali impostazioni di configurazione pertinenti.

#### <a name="databases"></a>Database

Per qualsiasi database SQL, identificare la stringa di connessione.

Per un'applicazione Spring Boot, le stringhe di connessione sono in genere incluse nei file di configurazione. 

Ecco un esempio di un file *application.properties*:

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/mysql_db
spring.datasource.username=dbuser
spring.datasource.driver-class-name=com.mysql.jdbc.Driver
```

Ecco un esempio di un file *application.yaml*:

```yaml
spring:
  data:
    mongodb:
      uri: mongodb://mongouser:deepsecret@mongoserver.contoso.com:27017
```

#### <a name="jms-message-brokers"></a>Broker di messaggi JMS

Identificare quali broker vengono usati. A tale scopo, esaminare il manifesto di compilazione (in genere *pom.xml* o *build.gradle*) per individuare le dipendenze pertinenti.

Ad esempio, un'applicazione Spring Boot che usa ActiveMQ in genere contiene questa dipendenza in *pom.xml*:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-activemq</artifactId>
</dependency>
```

Le applicazioni Spring Boot che usano broker proprietari contengono in genere le dipendenze direttamente nelle librerie di driver JMS dei broker. Ecco un esempio di un file *build.gradle*:

```json
    dependencies {
      ...
      compile("com.ibm.mq:com.ibm.mq.allclient:9.0.4.0")
      ...
    }
```

Una volta identificati i broker usati, trovare le impostazioni corrispondenti, che in genere si trovano nei file *application.properties* e *application.yml* per Spring Boot.

Ecco un esempio di un file *application.properties*:

```properties
spring.activemq.brokerurl=broker:(tcp://localhost:61616,network:static:tcp://remotehost:61616)?persistent=false&useJmx=true
spring.activemq.user=admin
spring.activemq.password=tryandguess
```

Ecco un esempio di un file *application.yaml*:

```yaml
ibm:
  mq:
    queueManager: qm1
    channel: dev.ORDERS
    connName: localhost(14)
    user: admin
    password: big$ecr3t
```

#### <a name="all-other-external-resources"></a>Tutte le altre risorse esterne

Non è possibile documentare tutte le possibili dipendenze esterne in questa guida. È responsabilità del team verificare che sia possibile soddisfare tutte le dipendenze esterne dell'applicazione dopo la migrazione al servizio app.

### <a name="inventory-secrets"></a>Inventario dei segreti

#### <a name="passwords-and-secure-strings"></a>Password e stringhe sicure

Controllare tutte le proprietà e i file di configurazione e tutte le variabili di ambiente nei server di produzione per verificare la presenza di stringhe segrete e password. In un'applicazione Spring Boot, tali stringhe in genere si trovano in *application.properties* o *application.yml*.

[!INCLUDE [inventory-certificates](includes/migration/inventory-certificates.md)]

[!INCLUDE [inventory-persistence-usage](includes/migration/inventory-persistence-usage.md)]

### <a name="special-cases"></a>Casi speciali

Alcuni scenari di produzione possono richiedere ulteriori modifiche o imporre limitazioni aggiuntive. Sebbene tali scenari siano poco frequenti, è importante assicurarsi che siano inapplicabili all'applicazione o risolti correttamente.

#### <a name="determine-whether-application-relies-on-scheduled-jobs"></a>Determinare se l'applicazione si basa su processi pianificati

I processi pianificati, ad esempio le attività dell'utilità di Quartz Scheduler o i processi Cron, non possono essere usati con il servizio app. Il servizio app non impedisce la distribuzione interna di un'applicazione contenente attività pianificate. Tuttavia, se l'applicazione viene ampliata, lo stesso processo pianificato può essere eseguito più di una volta per ogni periodo pianificato. Questa situazione può provocare conseguenze indesiderate.

Creare un inventario di tutti i processi pianificati, all'interno o all'esterno del processo dell'applicazione.

#### <a name="determine-whether-your-application-contains-os-specific-code"></a>Determinare se l'applicazione contiene codice specifico del sistema operativo

Se l'applicazione contiene codice che ospita il sistema operativo in cui è in esecuzione, è necessario eseguire il refactoring dell'applicazione in modo che NON si basi sul sistema operativo sottostante. Ad esempio, potrebbe essere necessario sostituire qualsiasi utilizzo di `/` o `\` nei percorsi del file system con [`File.Separator`](https://docs.oracle.com/javase/8/docs/api/java/io/File.html#separator) o [`Path.get`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Paths.html#get-java.lang.String-java.lang.String...-).

#### <a name="identify-all-outside-processesdaemons-running-on-the-production-servers"></a>Identificare tutti i processi/daemon esterni in esecuzione nei server di produzione

I processi in esecuzione all'esterno del server applicazioni, ad esempio i daemon di monitoraggio, dovranno essere trasferiti altrove o eliminati.

#### <a name="identify-handling-of-non-http-requests-or-multiple-ports"></a>Identificare la gestione di richieste non HTTP o di più porte

Il servizio app supporta solo un singolo endpoint HTTP in una singola porta. Se l'applicazione è in ascolto su più porte o accetta richieste tramite protocolli diversi da HTTP, non usare il servizio app di Azure.

## <a name="migration"></a>Migrazione

### <a name="parameterize-the-configuration"></a>Parametrizzare la configurazione

Assicurarsi che le coordinate di tutte le risorse esterne, ad esempio le stringhe di connessione di database, e altre impostazioni personalizzabili possano essere lette dalle variabili di ambiente. Se si esegue la migrazione di un'applicazione Spring Boot, è necessario che tutte le impostazioni di configurazione siano già [esternalizzabili](https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-external-config).

Ecco un esempio che fa riferimento a una variabile di ambiente `SERVICEBUS_CONNECTION_STRING` da un file *application.properties*:

```properties
spring.jms.servicebus.connection-string=${SERVICEBUS_CONNECTION_STRING}
spring.jms.servicebus.topic-client-id=contoso1
spring.jms.servicebus.idle-timeout=10000
```

### <a name="provision-an-app-service-plan"></a>Effettuare il provisioning di un piano di servizio app

Nell'[elenco di piani di servizio disponibili](https://azure.microsoft.com/pricing/details/app-service/linux/) selezionare quello le cui specifiche soddisfano o superano quelle dell'hardware di produzione corrente.

> [!NOTE]
> Se si prevede di eseguire distribuzioni staging/canary o di usare gli [slot di distribuzione](/azure/app-service/deploy-staging-slots), il piano di servizio app deve includere tale capacità aggiuntiva. È consigliabile usare piani Premium o superiori per le applicazioni Java.

[Creare il piano di servizio app](/azure/app-service/app-service-plan-manage#create-an-app-service-plan).

### <a name="create-and-deploy-web-apps"></a>Creare e distribuire app Web

Sarà necessario creare un'app Web nel piano di servizio app, scegliendo "Java SE" come stack di runtime, per ogni file JAR eseguibile che si intende eseguire.

#### <a name="maven-applications"></a>Applicazioni Maven

Se l'applicazione viene compilata da un file POM Maven, [usare il plug-in Webapp per Maven](/azure/java/spring-framework/deploy-spring-boot-java-app-with-maven-plugin#configure-maven-plugin-for-azure-app-service) per creare l'app Web e distribuire l'applicazione.

#### <a name="non-maven-applications"></a>Applicazioni non Maven

Se non è possibile usare il plug-in Maven, sarà necessario effettuare il provisioning dell'app Web tramite altri meccanismi, ad esempio:

* [Azure portal](https://portal.azure.com/#create/Microsoft.WebSite)
* [Interfaccia della riga di comando di Azure](/cli/azure/webapp?view=azure-cli-latest#az-webapp-create)
* [Azure PowerShell](/powershell/module/az.websites/new-azwebapp)

Una volta creata l'app Web, usare uno dei [meccanismi di distribuzione disponibili](/azure/app-service/deploy-ftp) per distribuire l'applicazione. Se possibile, l'applicazione deve essere caricata in */home/site/wwwroot/app.jar*. Se non si vuole rinominare il file JAR in app.jar, è possibile caricare uno script della shell con il comando per eseguire il file JAR. Incollare quindi il percorso completo di questo script nella casella di testo [File di avvio](/azure/app-service/containers/app-service-linux-faq#built-in-images) nella sezione Configurazione del portale. Lo script di avvio non viene eseguito dalla directory in cui si trova. Pertanto, usare sempre percorsi assoluti per fare riferimento ai file dello script di avvio, ad esempio `java -jar /home/myapp/myapp.jar`.

### <a name="migrate-jvm-runtime-options"></a>Eseguire la migrazione delle opzioni di runtime JVM

Se l'applicazione richiede opzioni di runtime specifiche, [usare il meccanismo più appropriato per specificarle](/azure/app-service/containers/configure-language-java#set-java-runtime-options).

[!INCLUDE [configure-custom-domain-and-ssl](includes/migration/configure-custom-domain-and-ssl.md)]

[!INCLUDE [import-backend-certificates](includes/migration/import-backend-certificates.md)]

### <a name="migrate-external-resource-coordinates-and-other-settings"></a>Eseguire la migrazione delle coordinate delle risorse esterne e di altre impostazioni

Seguire [questa procedura per eseguire la migrazione delle stringhe di connessione e di altre impostazioni](/azure/app-service/containers/configure-language-java#spring-boot-1).

> [!NOTE]
> Per tutte le impostazioni dell'applicazione Spring Boot parametrizzate con variabili nella sezione [Parametrizzare la configurazione](#parameterize-the-configuration), è necessario definire tali variabili di ambiente nella configurazione dell'applicazione. Le impostazioni dell'applicazione Spring Boot non parametrizzate in modo esplicito con le variabili di ambiente possono comunque essere sostituite da queste ultime tramite la configurazione dell'applicazione. Ad esempio:

  ```properties
  spring.jms.servicebus.connection-string=${CUSTOMCONNSTR_SERVICE_BUS}
  spring.jms.servicebus.topic-client-id=contoso1
  spring.jms.servicebus.idle-timeout=1800000
  ```

![Configurazione dell'applicazione del servizio app](media/migrate-java-se-to-java-se-app-service/app-service-parameterized-spring-boot-app-settings.png)

[!INCLUDE [migrate-scheduled-jobs](includes/migration/migrate-scheduled-jobs.md)]

### <a name="restart-and-smoke-test"></a>Riavvio e smoke test

Infine, sarà necessario riavviare l'app Web per applicare tutte le modifiche alla configurazione. Al termine del riavvio, verificare che l'applicazione venga eseguita correttamente.

## <a name="post-migration"></a>Post-migrazione

Ora che è stata eseguita la migrazione dell'applicazione al servizio app di Azure, è necessario verificare che funzioni come previsto. Al termine, sono disponibili alcune raccomandazioni per rendere l'applicazione maggiormente nativa del cloud.

### <a name="recommendations"></a>Consigli

* Se si è scelto di usare la directory */home* per l'archiviazione dei file, provare a [sostituirla con Archiviazione di Azure](/azure/app-service/containers/how-to-serve-content-from-azure-storage).

* Se nella directory */home* è presente una configurazione contenente stringhe di connessione, chiavi SSL e altre informazioni segrete, valutare se usare una combinazione di [Azure Key Vault](/azure/app-service/app-service-key-vault-references) e/o [inserimento di parametri con le impostazioni dell'applicazione](/azure/app-service/configure-common#configure-app-settings) laddove possibile.

* Provare a [usare gli slot di distribuzione](/azure/app-service/deploy-staging-slots) per distribuzioni affidabili senza tempi di inattività.

* Progettare e implementare una strategia DevOps. Per mantenere l'affidabilità aumentando al tempo stesso la velocità di sviluppo, è consigliabile [automatizzare le distribuzioni e i test con Azure Pipelines](/azure/devops/pipelines/ecosystems/java-webapp). Se si usano gli slot di distribuzione, è possibile [automatizzare la distribuzione in uno slot](/azure/devops/pipelines/targets/webapp?view=azure-devops&tabs=yaml#deploy-to-a-slot) e lo scambio di slot successivo.

* Progettare e implementare una strategia di continuità aziendale e ripristino di emergenza. Per le applicazioni cruciali, considerare un'[architettura di distribuzione in più aree](/azure/architecture/reference-architectures/app-service-web-app/multi-region).
