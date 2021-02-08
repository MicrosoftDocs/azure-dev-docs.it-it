---
title: Come creare un'applicazione Spring Cloud Stream Binder con Hub eventi di Azure
description: Informazioni su come configurare un'applicazione Spring Cloud Stream Binder basata su Java creata con Spring Boot Initializr con Hub eventi di Azure.
services: event-hubs
documentationcenter: java
ms.date: 02/08/2021
ms.service: event-hubs
ms.tgt_pltfrm: na
ms.topic: article
ms.custom: devx-track-java, devx-track-azurecli
ms.openlocfilehash: d0c87ce32caddc0100b91abd800a18179ba4101e
ms.sourcegitcommit: bccbab4883e6b6b4926fc194c35ad948b11ccc3f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/08/2021
ms.locfileid: "99822719"
---
# <a name="how-to-create-a-spring-cloud-stream-binder-application-with-azure-event-hubs"></a>Come creare un'applicazione Spring Cloud Stream Binder con Hub eventi di Azure

Questo articolo illustra come configurare un'applicazione Spring Cloud Stream Binder basata su Java creata con Spring Boot Initializer con Hub eventi di Azure.

## <a name="prerequisites"></a>Prerequisites

* Sottoscrizione di Azure; se non si ha una sottoscrizione di Azure, è possibile attivare i [vantaggi per i sottoscrittori di MSDN] oppure iscriversi per ottenere un [account Azure gratuito].
* Java Development Kit (JDK) supportato. Per altre informazioni sulle versioni di JDK utilizzabili per lo sviluppo in Azure, vedere <https://aka.ms/azure-jdks>.
* [Apache Maven](http://maven.apache.org/), versione 3.0 o versione successiva.

> [!IMPORTANT]
> Per completare i passaggi descritti in questo articolo, è necessaria la versione Spring Boot 2,2 o 2,3.

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
   * Specificare una versione di **Spring boot** uguale a **2,3**.
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
     <groupId>com.azure.spring</groupId>
     <artifactId>azure-spring-cloud-stream-binder-eventhubs</artifactId>
     <version>2.1.0</version>
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

## <a name="configure-your-spring-boot-app-to-use-your-azure-event-hub"></a>Configurare l'app Spring Boot per l'uso dell'hub eventi di Azure

1. Individuare l' *applicazione. YAML* nella directory *Resources* dell'app; Per esempio:

   *C:\SpringBoot\eventhubs-sample\src\main\resources\application.yaml*

   -oppure-

   */users/example/home/eventhubs-sample/src/main/resources/application.yaml*

2. Aprire il file *Application. YAML* in un editor di testo, aggiungere le righe seguenti e quindi sostituire i valori di esempio con le proprietà appropriate per l'hub eventi:

   ```yaml
    spring:
      cloud:
        azure:
          eventhub:
            connection-string: [eventhub-namespace-connection-string]
            checkpoint-storage-account: wingtiptoysstorage
            checkpoint-access-key: [checkpoint-access-key]
            checkpoint-container: wingtiptoyscontainer
            
        stream:
          bindings:
            consume-in-0:
              destination: wingtiptoyshub
              group: $Default
            supply-out-0:
              destination: wingtiptoyshub
   
          eventhub:
            bindings:
              consume-in-0:
                consumer:
                  checkpoint-mode: MANUAL
          function:
            definition: consume;supply;
          poller:
            initial-delay: 0
            fixed-delay: 1000
   ```

   Dove:

   |                          Campo                           |                                                                                   Descrizione                                                                                    |
   |----------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
   |               `spring.cloud.azure.eventhub.connection-string`                |                                        Specificare la stringa di connessione ottenuta nello spazio dei nomi dell'hub eventi dalla portale di Azure.                                   |
   |               `spring.cloud.azure.function.definition`                |                                        Specificare il fagiolo funzionale da associare alle destinazioni esterne esposte dalle associazioni.                                   |
   |               `spring.cloud.azure.poller.fixed-delay`                |                                        Specificare un ritardo fisso per il polling predefinito in millisecondi, valore predefinito di 1000.                                   |
   |               `spring.cloud.azure.poller.initial-delay`                |                                       Specificare il ritardo iniziale per i trigger periodici, valore predefinito 0.                                   |
   |               `spring.cloud.stream.bindings.consume-in-0.destination`                 |                            Specificare l'hub eventi usato in questa esercitazione.                         |
   |               `spring.cloud.stream.bindings.consume-in-0.group`                    |                               Specificare i gruppi di consumer nell'istanza di hub eventi.                                |
   |               `spring.cloud.stream.bindings.supply-out-0.destination`                |                             Specificare lo stesso hub eventi usato in questa esercitazione.                        |
   | `spring.cloud.stream.eventhub.bindings.consume-in-0.consumer.checkpoint-mode` |                                                       Specificare `MANUAL`.                                                   |
   |               `spring.cloud.stream.eventhub.checkpoint-access-key` |                                                      Specificare la chiave di accesso dell'account di archiviazione.                                                   |
   |               `spring.cloud.stream.eventhub.checkpoint-container` |                                                       Specificare il contenitore dell'account di archiviazione.                                                   |
   |               `spring.cloud.stream.eventhub.checkpoint-storage-account` |                                                 Specificare l'account di archiviazione creato in questa esercitazione.                                               |

3. Salvare e chiudere il file *Application. YAML* .

## <a name="add-sample-code-to-implement-basic-event-hub-functionality"></a>Aggiungere codice di esempio per implementare le funzionalità di base dell'hub eventi

In questa sezione si creano le classi Java necessarie per inviare eventi all'hub eventi.

### <a name="modify-the-main-application-class"></a>Modificare la classe dell'applicazione main

1. Individuare il file Java dell'applicazione main nella directory del pacchetto dell'app, ad esempio:

   *C:\SpringBoot\eventhubs-sample\src\main\java\com\contoso\eventhubs\sample\EventhubSampleApplication.java*

   -oppure-

   */users/example/home/eventhubs-sample/src/main/java/com/contoso/eventhubs/sample/EventhubSampleApplication.java*

1. Aprire il file Java dell'applicazione main in un editor di testo e aggiungere le righe seguenti al file:

   ```java
    package com.contoso.eventhubs.sample;
    
    import com.azure.spring.integration.core.api.reactor.Checkpointer;
    import org.slf4j.Logger;
    import org.slf4j.LoggerFactory;
    import org.springframework.boot.SpringApplication;
    import org.springframework.boot.autoconfigure.SpringBootApplication;
    import org.springframework.context.annotation.Bean;
    import org.springframework.messaging.Message;
    
    import java.util.function.Consumer;
    
    import static com.azure.spring.integration.core.AzureHeaders.CHECKPOINTER;
    
    @SpringBootApplication
    public class EventhubSampleApplication {
    
        public static final Logger LOGGER = LoggerFactory.getLogger(EventhubSampleApplication.class);
    
        public static void main(String[] args) {
            SpringApplication.run(EventhubSampleApplication.class, args);
        }
    
        @Bean
        public Consumer<Message<String>> consume() {
            return message -> {
                Checkpointer checkpointer = (Checkpointer) message.getHeaders().get(CHECKPOINTER);
                LOGGER.info("New message received: '{}'", message);
                checkpointer.success()
                            .doOnSuccess(success -> LOGGER.info("Message '{}' successfully checkpointed", message))
                            .doOnError(error -> LOGGER.error("Exception: {}", error.getMessage()))
                            .subscribe();
            };
        }
    
    }
   ```

1. Salvare e chiudere il file Java dell'applicazione main.

### <a name="create-a-new-configuration-class"></a>Creare una nuova classe di configurazione

1. Creare un nuovo file Java denominato *EventProducerConfiguration. Java* nella directory del pacchetto dell'app, quindi aprire il file in un editor di testo e aggiungere le righe seguenti:

    ```java
    package com.contoso.eventhubs.sample;
    
    import org.slf4j.Logger;
    import org.slf4j.LoggerFactory;
    import org.springframework.context.annotation.Bean;
    import org.springframework.context.annotation.Configuration;
    import org.springframework.messaging.Message;
    import reactor.core.publisher.EmitterProcessor;
    import reactor.core.publisher.Flux;
    
    import java.util.function.Supplier;
    
    @Configuration
    public class EventProducerConfiguration {
    
        private static final Logger LOGGER = LoggerFactory.getLogger(EventProducerConfiguration.class);
    
        @Bean
        public EmitterProcessor<Message<String>> emitter() {
            return EmitterProcessor.create();
        }
    
        @Bean
        public Supplier<Flux<Message<String>>> supply(EmitterProcessor<Message<String>> emitter) {
            return () -> Flux.from(emitter)
                             .doOnNext(m -> LOGGER.info("Manually sending message {}", m))
                             .doOnError(t -> LOGGER.error("Error encountered", t));
        }
    }
    ```
1. Salvare e chiudere il file *EventProducerConfiguration. Java* .

### <a name="create-a-new-controller-class"></a>Creare una nuova classe controller

1. Creare un nuovo file Java denominato *EventProducerController. Java* nella directory del pacchetto dell'app, quindi aprire il file in un editor di testo e aggiungere le righe seguenti:

   ```java
   package com.contoso.eventhubs.sample;
   
   import org.slf4j.Logger;
   import org.slf4j.LoggerFactory;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.http.ResponseEntity;
   import org.springframework.messaging.Message;
   import org.springframework.messaging.support.MessageBuilder;
   import org.springframework.web.bind.annotation.PostMapping;
   import org.springframework.web.bind.annotation.RequestBody;
   import org.springframework.web.bind.annotation.RestController;
   import reactor.core.publisher.EmitterProcessor;
   
   @RestController
   public class EventProducerController {
   
       public static final Logger LOGGER = LoggerFactory.getLogger(EventProducerController.class);
   
       @Autowired
       private EmitterProcessor<Message<String>> emitterProcessor;
   
       @PostMapping("/messages")
       public ResponseEntity<String> sendMessage(@RequestBody String message) {
           LOGGER.info("Going to add message {} to emitter", message);
           emitterProcessor.onNext(MessageBuilder.withPayload(message).build());
           return ResponseEntity.ok("Sent!");
       }
   }
   ```

1. Salvare e chiudere il file *EventProducerController. Java* .

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
   2020-09-11 15:11:12.138  INFO 7616 --- [      elastic-4] c.contoso.eventhubs.sample.EventhubSampleApplication  : New message received: 'hello'
   2020-09-11 15:11:12.406  INFO 7616 --- [ctor-http-nio-1] c.contoso.eventhubs.sample.EventhubSampleApplication  : Message 'hello' successfully checkpointed
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
