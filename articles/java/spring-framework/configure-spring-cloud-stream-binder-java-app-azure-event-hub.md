---
title: Come creare un'applicazione Spring Cloud Stream Binder con Hub eventi di Azure
description: Informazioni su come configurare un'applicazione Spring Cloud Stream Binder basata su Java creata con Spring Boot Initializr con Hub eventi di Azure.
services: event-hubs
documentationcenter: java
ms.date: 10/13/2020
ms.service: event-hubs
ms.tgt_pltfrm: na
ms.topic: article
ms.custom: devx-track-java, devx-track-azurecli
ms.openlocfilehash: 0cc3289243c1a146cf59ecb15c5150327f49c236
ms.sourcegitcommit: 8e1d3a384ccb0e083589418d65a70b3a01afebff
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/12/2020
ms.locfileid: "94560304"
---
# <a name="how-to-create-a-spring-cloud-stream-binder-application-with-azure-event-hubs"></a>Come creare un'applicazione Spring Cloud Stream Binder con Hub eventi di Azure

Questo articolo illustra come configurare un'applicazione Spring Cloud Stream Binder basata su Java creata con Spring Boot Initializer con Hub eventi di Azure.

## <a name="prerequisites"></a>Prerequisites

* Sottoscrizione di Azure; se non si ha una sottoscrizione di Azure, è possibile attivare i [vantaggi per i sottoscrittori di MSDN] oppure iscriversi per ottenere un [account Azure gratuito].
* Java Development Kit (JDK) supportato. Per altre informazioni sulle versioni di JDK utilizzabili per lo sviluppo in Azure, vedere <https://aka.ms/azure-jdks>.
* [Apache Maven](http://maven.apache.org/), versione 3.0 o versione successiva.

> [!IMPORTANT]
> Per completare i passaggi contenuti in questo articolo, è necessario Spring Boot versione 2.2 o successiva.

## <a name="create-an-azure-event-hub-using-the-azure-portal"></a>Creare un hub eventi di Azure con il portale di Azure

La procedura seguente crea un hub eventi di Azure.

### <a name="create-an-azure-event-hub-namespace"></a>Creare uno spazio dei nomi dell'hub eventi di Azure

1. Passare al portale di Azure all'indirizzo <https://portal.azure.com/> ed eseguire l'accesso.

1. Selezionare **+ Crea una risorsa** e quindi cercare *Hub eventi*.

1. Selezionare **Crea**.

   >[!div class="mx-imgBorder"]
   >![Creare uno spazio dei nomi dell'hub eventi di Azure][IMG01]

1. Nella pagina **Crea spazio dei nomi** immettere le informazioni seguenti:

   * Scegliere la **sottoscrizione** da usare per lo spazio dei nomi.
   * Specificare se creare un nuovo **gruppo di risorse** per lo spazio dei nomi o sceglierne uno esistente.
   * Immettere un **nome di spazio dei nomi** univoco, che diventerà parte dell'URI dello spazio dei nomi dell'hub eventi. Se, ad esempio, si immette *wingtiptoys-space* in **Nome dello spazio dei nomi**, l'URI sarà `wingtiptoys-space.servicebus.windows.net`.
   * Specificare la **località** per lo spazio dei nomi dell'hub eventi.
   * Piano tariffario.
   * È anche possibile specificare le **unità elaborate** per lo spazio dei nomi.
   
   >[!div class="mx-imgBorder"]
   >![Specificare le opzioni per lo spazio dei nomi dell'hub eventi di Azure][IMG02]

1. Dopo aver specificato le opzioni elencate in precedenza, selezionare **Rivedi e crea**, esaminare le specifiche e selezionare **Crea** per creare lo spazio dei nomi.

## <a name="create-an-azure-event-hub-in-your-namespace"></a>Creare un hub eventi di Azure nello spazio dei nomi

Dopo la distribuzione dello spazio dei nomi, selezionare **Vai alla risorsa** per aprire la pagina **Spazio dei nomi di Hub eventi**, in cui è possibile creare un hub eventi.

1. Passare allo spazio dei nomi creato nella sezione precedente.

1. Selezionare **+ Hub eventi** nella barra dei menu in alto.

1. Assegnare un nome all'hub eventi.

1. Selezionare **Crea**.

   >[!div class="mx-imgBorder"]
   >![Creare un hub eventi][IMG05]

### <a name="create-an-azure-storage-account-for-your-event-hub-checkpoints"></a>Creare un account di archiviazione di Azure per i checkpoint dell'hub eventi

La procedura seguente crea un account di archiviazione per i checkpoint dell'hub eventi.

1. Passare al portale di Azure all'indirizzo <https://portal.azure.com/>.

1. Selezionare **+ Crea una risorsa**, **Archiviazione** e quindi **Account di archiviazione**.

1. Nella pagina **Crea account di archiviazione** immettere le informazioni seguenti:

   * Scegliere la **sottoscrizione** da usare per l'account di archiviazione.
   * Specificare se creare un nuovo **gruppo di risorse** per l'account di archiviazione o sceglierne uno esistente.
   * Immettere un nome univoco per l'account di archiviazione in **Nome**.
   * Specificare la **località** per l'account di archiviazione.

   >[!div class="mx-imgBorder"]
   >![Specificare le opzioni per l'account di archiviazione di Azure][IMG08]

1. Dopo aver specificato le opzioni elencate sopra, selezionare **Rivedi e crea** per creare l'account di archiviazione.

1. Esaminare le specifiche e selezionare **Crea**.  La distribuzione richiede diversi minuti.

## <a name="create-a-simple-spring-boot-application-with-the-spring-initializr"></a>Creare un'applicazione Spring Boot semplice con Spring Initializr

La procedura seguente crea un'applicazione Spring Boot.

1. Passare a <https://start.spring.io/>.

1. Specificare le opzioni seguenti:

   * Generare un progetto **Maven** con **Java**.
   * Specificare **Spring Boot** versione 2.2 o successiva.
   * Specificare i nomi di **Group** (Gruppo) e **Artifact** (Artefatto) per l'applicazione.
   * Selezionare **8** per la versione di Java.
   * Aggiungere la dipendenza *Web*.

   >[!div class="mx-imgBorder"]
   >![Opzioni di base di Spring Initializr][SI01]

   > [!NOTE]
   > Spring Initializr usa i nomi in **Group** (Gruppo) e **Artifact** (Artefatto) per creare il nome del pacchetto, ad esempio *com.contoso.eventhubs.sample*.

1. Dopo aver specificato le opzioni elencate sopra, selezionare **GENERA**.

1. Quando richiesto, scaricare il progetto in un percorso nel computer locale.

1. Dopo l'estrazione dei file nel sistema locale, la semplice applicazione Spring Boot sarà pronta per la modifica.

## <a name="configure-your-spring-boot-app-to-use-the-azure-event-hub-starter"></a>Configurare l'app Spring Boot per l'uso dell'utilità di avvio per Hub eventi di Azure

1. Individuare il file *pom.xml* nella directory radice dell'app, ad esempio:

   *C:\SpringBoot\eventhubs-sample\pom.xml*

   -oppure-

   */users/example/home/eventhubs-sample/pom.xml*

1. Aprire il file *pom.xml* in un editor di testo e aggiungere l'utilità di avvio Spring Cloud Stream Binder per Hub eventi di Azure all'elenco in `<dependencies>`:

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>spring-cloud-azure-eventhubs-stream-binder</artifactId>
      <version>1.2.7</version>
   </dependency>
   ```

1. Se si usa JDK versione 9 o successiva, aggiungere le dipendenze seguenti:

   ```xml
   <dependency>
       <groupId>javax.xml.bind</groupId>
       <artifactId>jaxb-api</artifactId>
       <version>2.3.1</version>
   </dependency>
   <dependency>
       <groupId>org.glassfish.jaxb</groupId>
       <artifactId>jaxb-runtime</artifactId>
       <version>2.3.1</version>
       <scope>runtime</scope>
   </dependency>
   ```

1. Salvare e chiudere il file *pom.xml*.

## <a name="create-an-azure-credential-file"></a>Creare un file di credenziali di Azure

1. Aprire un prompt dei comandi.

1. Passare alla directory *resources* dell'app Spring Boot, ad esempio:

   ```cmd
   cd C:\SpringBoot\eventhubs-sample\src\main\resources
   ```

   -oppure-

   ```bash
   cd /users/example/home/eventhubs-sample/src/main/resources
   ```

1. Accedere all'account Azure:

   ```azurecli
   az login
   ```

1. Elencare le sottoscrizioni:

   ```azurecli
   az account list
   ```
   Azure restituirà un elenco delle sottoscrizioni e sarà necessario copiare il GUID per la sottoscrizione che si vuole usare, ad esempio:

   ```json
   [
     {
       "cloudName": "AzureCloud",
       "id": "11111111-1111-1111-1111-111111111111",
       "isDefault": true,
       "name": "Converted Windows Azure MSDN - Visual Studio Ultimate",
       "state": "Enabled",
       "tenantId": "22222222-2222-2222-2222-222222222222",
       "user": {
         "name": "user@contoso.com",
         "type": "user"
       }
     }
   ]
   ```
   
1. Specificare il GUID per la sottoscrizione che si vuole usare con Azure, ad esempio:

   ```azurecli
   az account set -s 11111111-1111-1111-1111-111111111111
   ```

1. Creare il file di credenziali di Azure:

   ```azurecli
   az ad sp create-for-rbac --sdk-auth > my.azureauth
   ```

   Questo comando creerà un file *my.azureauth* nella directory *resources* con contenuto simile all'esempio seguente:

   ```json
   {
     "clientId": "33333333-3333-3333-3333-333333333333",
     "clientSecret": "44444444-4444-4444-4444-444444444444",
     "subscriptionId": "11111111-1111-1111-1111-111111111111",
     "tenantId": "22222222-2222-2222-2222-222222222222",
     "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
     "resourceManagerEndpointUrl": "https://management.azure.com/",
     "activeDirectoryGraphResourceId": "https://graph.windows.net/",
     "sqlManagementEndpointUrl": "https://management.core.windows.net:8443/",
     "galleryEndpointUrl": "https://gallery.azure.com/",
     "managementEndpointUrl": "https://management.core.windows.net/"
   }
   ```

## <a name="configure-your-spring-boot-app-to-use-your-azure-event-hub"></a>Configurare l'app Spring Boot per l'uso dell'hub eventi di Azure

1. Individuare *application.properties* nella directory *resources* dell'app, ad esempio:

   *C:\SpringBoot\eventhubs-sample\src\main\resources\application.properties*

   -oppure-

   */users/example/home/eventhubs-sample/src/main/resources/application.properties*

2. Aprire il file *application.properties* in un editor di testo, aggiungere le righe seguenti e quindi sostituire i valori di esempio con le proprietà appropriate per l'hub eventi:

   ```yaml
   spring.cloud.azure.credential-file-path=my.azureauth
   spring.cloud.azure.resource-group=wingtiptoysresources
   spring.cloud.azure.region=West US
   spring.cloud.azure.eventhub.namespace=wingtiptoysnamespace
   spring.cloud.azure.eventhub.checkpoint-storage-account=wingtiptoysstorage
   spring.cloud.stream.bindings.input.destination=wingtiptoyshub
   spring.cloud.stream.bindings.input.group=$Default
   spring.cloud.stream.eventhub.bindings.input.consumer.checkpoint-mode=MANUAL
   spring.cloud.stream.bindings.output.destination=wingtiptoyshub
   ```
   Dove:

   |                          Campo                           |                                                                                   Descrizione                                                                                    |
   |----------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
   |        `spring.cloud.azure.credential-file-path`         |                                                    Specifica il file di credenziali di Azure creato in precedenza in questa esercitazione.                                                    |
   |           `spring.cloud.azure.resource-group`            |                                                      Specifica il gruppo di risorse di Azure contenente l'hub eventi di Azure.                                                      |
   |               `spring.cloud.azure.region`                |                                           Specifica l'area geografica indicata al momento della creazione dell'hub eventi di Azure.                                            |
   |         `spring.cloud.azure.eventhub.namespace`          |                                          Specifica il nome univoco fornito al momento della creazione dello spazio dei nomi dell'hub eventi di Azure.                                           |
   | `spring.cloud.azure.eventhub.checkpoint-storage-account` |                                                    Specifica l'account di archiviazione di Azure creato in precedenza in questa esercitazione.                                                    |
   |     `spring.cloud.stream.bindings.input.destination`     |                            Specifica l'hub eventi di Azure destinazione di input, che in questo caso è l'hub creato in precedenza in questa esercitazione.                            |
   |       `spring.cloud.stream.bindings.input.group `        | Specifica un gruppo di consumer dell'hub eventi di Azure, che è possibile impostare su "$Default" per usare il gruppo di consumer di base creato al momento della creazione dell'hub eventi di Azure. |
   |    `spring.cloud.stream.bindings.output.destination`     |                               Specifica l'hub eventi di Azure destinazione di output, che per questa esercitazione è uguale alla destinazione di input.                               |

3. Salvare e chiudere il file *application.properties*.

## <a name="add-sample-code-to-implement-basic-event-hub-functionality"></a>Aggiungere codice di esempio per implementare le funzionalità di base dell'hub eventi

In questa sezione si creano le classi Java necessarie per inviare eventi all'hub eventi.

### <a name="modify-the-main-application-class"></a>Modificare la classe dell'applicazione main

1. Individuare il file Java dell'applicazione main nella directory del pacchetto dell'app, ad esempio:

   *C:\SpringBoot\eventhubs-sample\src\main\java\com\wingtiptoys\eventhub\EventhubApplication.java*

   -oppure-

   */users/example/home/eventhubs-sample/src/main/java/com/wingtiptoys/eventhub/EventhubApplication.java*

1. Aprire il file Java dell'applicazione main in un editor di testo e aggiungere le righe seguenti al file:

   ```java
    package com.contoso.eventhubs.sample;
    
    import org.springframework.boot.SpringApplication;
    import org.springframework.boot.autoconfigure.SpringBootApplication;
    
    @SpringBootApplication
    public class EventhubsSampleApplication {
    
        public static void main(String[] args) {
            SpringApplication.run(EventhubsSampleApplication.class, args);
        }
    
    }
   ```

1. Salvare e chiudere il file Java dell'applicazione main.

### <a name="create-a-new-class-for-the-source-connector"></a>Creare una nuova classe per il connettore di origine

1. Creare un nuovo file Java denominato *EventhubSource.java* nella directory del pacchetto dell'app, quindi aprire il file in un editor di testo e aggiungere le righe seguenti:

    ```java
    package com.contoso.eventhubs.sample;
    
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.cloud.stream.annotation.EnableBinding;
    import org.springframework.cloud.stream.messaging.Source;
    import org.springframework.messaging.support.GenericMessage;
    import org.springframework.web.bind.annotation.PostMapping;
    import org.springframework.web.bind.annotation.RequestBody;
    import org.springframework.web.bind.annotation.RestController;
    
    @EnableBinding(Source.class)
    @RestController
    public class EventhubSource {
    
        @Autowired
        private Source source;
    
        @PostMapping("/messages")
        public String postMessage(@RequestBody String message) {
            this.source.output().send(new GenericMessage<>(message));
            return message;
        }
    }
    ```
1. Salvare e chiudere il file *EventhubSource.java*.

### <a name="create-a-new-class-for-the-sink-connector"></a>Creare una nuova classe per il connettore sink

1. Creare un nuovo file Java denominato *EventhubSink.java* nella directory del pacchetto dell'app, quindi aprire il file in un editor di testo e aggiungere le righe seguenti:

   ```java
   package com.contoso.eventhubs.sample;

   import com.microsoft.azure.spring.integration.core.AzureHeaders;
   import com.microsoft.azure.spring.integration.core.api.reactor.Checkpointer;
   import org.slf4j.Logger;
   import org.slf4j.LoggerFactory;
   import org.springframework.cloud.stream.annotation.EnableBinding;
   import org.springframework.cloud.stream.annotation.StreamListener;
   import org.springframework.cloud.stream.messaging.Sink;
   import org.springframework.messaging.handler.annotation.Header;

   @EnableBinding(Sink.class)
   public class EventhubSink {

      private static final Logger LOGGER = LoggerFactory.getLogger(EventhubSink.class);

      @StreamListener(Sink.INPUT)
      public void handleMessage(String message, @Header(AzureHeaders.CHECKPOINTER) Checkpointer checkpointer) {
        LOGGER.info("New message received: '{}'", message);
        checkpointer.success()
                .doOnSuccess(s -> LOGGER.info("Message '{}' successfully checkpointed", message))
                .doOnError((msg) -> {
                    LOGGER.error(String.valueOf(msg));
                })
                .subscribe();
      }
   }
   ```

1. Salvare e chiudere il file *EventhubSink.java*.

## <a name="build-and-test-your-application"></a>Compilare e testare l'applicazione

Usare le procedure seguenti per compilare ed eseguire l'applicazione.

1. Aprire un prompt dei comandi e cambiare la directory passando alla cartella in cui si trova il file *pom.xml*, ad esempio:

   ```cmd
    cd C:\SpringBoot\eventhubs-sample
   ```
   -oppure-

   ```bash
   cd /users/example/home/eventhubs-sample
   ```

1. Compilare l'applicazione Spring Boot con Maven ed eseguirla, ad esempio:

   ```bash
   mvn clean package -Dmaven.test.skip=true
   mvn spring-boot:run
   ```

1. Quando l'applicazione è in esecuzione, è possibile testarla usando `curl`, ad esempio:

   ```bash
   curl -X POST -H "Content-Type: text/plain" -d "hello" http://localhost:8080/messages
   ```
   Verrà visualizzato l'inserimento di "hello" nei log dell'applicazione. Ad esempio:

   ```output
   2020-09-11 15:11:12.138  INFO 7616 --- [      elastic-4] c.contoso.eventhubs.sample.EventhubSink  : New message received: 'hello'
   2020-09-11 15:11:12.406  INFO 7616 --- [ctor-http-nio-1] c.contoso.eventhubs.sample.EventhubSink  : Message 'hello' successfully checkpointed
   ```

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni su Spring e Azure, passare al centro di documentazione di Spring in Azure.

> [!div class="nextstepaction"]
> [Spring in Azure](./index.yml)

### <a name="additional-resources"></a>Risorse aggiuntive

Per altre informazioni sul supporto di Azure per Stream Binder per Hub eventi, vedere gli articoli seguenti:

* [Che cos'è Hub eventi di Azure?](/azure/event-hubs/event-hubs-about)

* [Creare uno spazio dei nomi di Hub eventi e un hub eventi usando il Portale di Azure](/azure/event-hubs/event-hubs-create)

* [Come usare Spring Boot Starter per Apache Kafka con Hub eventi di Azure](configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub.md)

Per altre informazioni sull'uso di Azure con Java, vedere [Azure per sviluppatori Java] e la documentazione relativa all'[uso di Azure DevOps e Java].

**[Spring Framework]** è una soluzione open source che consente agli sviluppatori Java di creare applicazioni di livello enterprise. Uno dei progetti più comuni che si basa su questa piattaforma è [Spring Boot], che fornisce un approccio semplificato per la creazione di applicazioni Java autonome. Per semplificare le operazioni iniziali con Spring Boot per gli sviluppatori, alcuni pacchetti Spring Boot di esempio sono disponibili all'indirizzo <https://github.com/spring-guides/>. Oltre a consentire di scegliere dall'elenco di progetti Spring Boot di base, **[Spring Initializr]** semplifica le operazioni iniziali degli sviluppatori per la creazione di applicazioni Spring Boot personalizzate.

<!-- URL List -->

[Account Azure gratuito]: https://azure.microsoft.com/pricing/free-trial/
[Azure per sviluppatori Java]: ../index.yml
[Uso di Azure DevOps e Java]: /azure/devops/
[vantaggi per i sottoscrittori di MSDN]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/ (Framework di Spring)

<!-- IMG List -->

[IMG01]: media/configure-spring-cloud-stream-binder-java-app-azure-event-hub/create-event-hub-01.png
[IMG02]: media/configure-spring-cloud-stream-binder-java-app-azure-event-hub/create-event-hub-02.png
[IMG05]: media/configure-spring-cloud-stream-binder-java-app-azure-event-hub/create-event-hub-05.png
[IMG08]: media/configure-spring-cloud-stream-binder-java-app-azure-event-hub/create-event-hub-08.png
[SI01]: media/configure-spring-cloud-stream-binder-java-app-azure-event-hub/create-project-01.png
