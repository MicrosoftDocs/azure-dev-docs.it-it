---
title: Come usare Spring Boot Starter per Apache Kafka con Hub eventi di Azure
description: Informazioni su come configurare un'applicazione creata con Spring Boot Initializr per l'uso di Apache Kafka con Hub eventi di Azure.
services: event-hubs
documentationcenter: java
ms.date: 10/13/2018
ms.service: event-hubs
ms.topic: article
ms.custom: devx-track-java, devx-track-azurecli
ms.openlocfilehash: 53a50a7a32ff9e555f821d69688cc566fb7a3c62
ms.sourcegitcommit: 8e1d3a384ccb0e083589418d65a70b3a01afebff
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/12/2020
ms.locfileid: "94560422"
---
# <a name="how-to-use-the-spring-boot-starter-for-apache-kafka-with-azure-event-hubs"></a>Come usare Spring Boot Starter per Apache Kafka con Hub eventi di Azure

Questo articolo illustra come configurare un'applicazione Spring Cloud Stream Binder basata su Java creata con Spring Boot Initializer per l'uso di [Apache Kafka] con Hub eventi di Azure.

## <a name="prerequisites"></a>Prerequisites

I prerequisiti seguenti sono necessari per seguire le procedure disponibili in questo articolo:

* Sottoscrizione di Azure; se non si ha una sottoscrizione di Azure, è possibile attivare i [vantaggi per i sottoscrittori di MSDN] oppure iscriversi per ottenere un [account Azure gratuito].
* Java Development Kit (JDK) supportato. Per altre informazioni sulle versioni di JDK utilizzabili per lo sviluppo in Azure, vedere <https://aka.ms/azure-jdks>.
* [Apache Maven](http://maven.apache.org/), versione 3.0 o versione successiva.

> [!NOTE]
> * Per completare i passaggi descritti in questo articolo è necessario Spring Boot versione 2.0 o successiva.
> * Spring Initializr usa Java 11 come versione predefinita. Per usare le utilità di avvio di Spring Boot descritte in questo argomento, è necessario selezionare invece Java 8.

## <a name="create-an-azure-event-hub-using-the-azure-portal"></a>Creare un hub eventi di Azure con il portale di Azure

### <a name="create-an-azure-event-hub-namespace"></a>Creare uno spazio dei nomi dell'hub eventi di Azure

1. Passare al portale di Azure all'indirizzo <https://portal.azure.com/> ed eseguire l'accesso.

1. Selezionare **Create a resource** (Crea una risorsa), quindi **Search the Marketplace** (Cerca nel Marketplace), infine cercare *Event Hubs* (Hub eventi).

1. Selezionare **Create** (Crea).

   ![Creare uno spazio dei nomi dell'hub eventi di Azure][IMG01]

1. Nella pagina **Crea spazio dei nomi** immettere le informazioni seguenti:

   * Scegliere la **sottoscrizione** da usare per lo spazio dei nomi.
   * Specificare se creare un nuovo **gruppo di risorse** per lo spazio dei nomi o sceglierne uno esistente.
   * Immettere un **nome di spazio dei nomi** univoco, che diventerà parte dell'URI dello spazio dei nomi dell'hub eventi. Se, ad esempio, si immette *wingtiptoys-space* in **Nome**, l'URI sarà `wingtiptoys-space.servicebus.windows.net`.
   * Specificare la **località** per lo spazio dei nomi dell'hub eventi.
   * Specificare il **piano tariffario**, che consente di limitare gli scenari di utilizzo.
   * È anche possibile specificare le **unità elaborate** per lo spazio dei nomi.

   ![Specificare le opzioni per lo spazio dei nomi dell'hub eventi di Azure][IMG02]

1. Dopo aver specificato le opzioni elencate sopra, selezionare **Rivedi + Crea**.

1. Esaminare i valori specificati e selezionare **Crea** per creare lo spazio dei nomi.

### <a name="create-an-azure-event-hub-in-your-namespace"></a>Creare un hub eventi di Azure nello spazio dei nomi

Dopo la distribuzione dello spazio dei nomi, è possibile creare un hub eventi al suo interno.

1. Passare allo spazio dei nomi creato nel passaggio precedente.

1. Selezionare **Hub eventi** nella barra dei menu in alto.

1. Assegnare un nome all'hub eventi.

1. Selezionare **Crea**.

   ![Creare un hub eventi][IMG05]

## <a name="create-a-simple-spring-boot-application-with-the-spring-initializr"></a>Creare un'applicazione Spring Boot semplice con Spring Initializr

1. Passare a <https://start.spring.io/>.

1. Specificare le opzioni seguenti:

   * Generare un progetto **Maven** con **Java**.
   * Specificare **Spring Boot** versione 2.0 o successiva.
   * Specificare i nomi di **Group** (Gruppo) e **Artifact** (Artefatto) per l'applicazione.
   * Aggiungere la dipendenza **Web**.

      ![Opzioni di base di Spring Initializr][SI01]

   > [!NOTE]
   > 1. Spring Initializr usa i nomi in **Group** (Gruppo) e **Artifact** (Artefatto) per creare il nome del pacchetto, ad esempio *com.wingtiptoys.kafka*.
   > 2. Spring Initializr usa Java 11 come versione predefinita. Per usare le utilità di avvio di Spring Boot descritte in questo argomento, è necessario selezionare invece Java 8.

1. Dopo aver specificato le opzioni elencate sopra, fare clic su **Generate Project** (Genera progetto).

1. Quando richiesto, scaricare il progetto in un percorso nel computer locale.

1. Dopo l'estrazione dei file nel sistema locale, la semplice applicazione Spring Boot sarà pronta per la modifica.

## <a name="configure-your-spring-boot-app-to-use-the-spring-cloud-kafka-stream-and-azure-event-hub-starters"></a>Configurare l'app Spring Boot per l'uso delle utilità di avvio Spring Cloud per Hub eventi di Azure e flussi Kafka

1. Individuare il file *pom.xml* nella directory radice dell'app, ad esempio:

   *C:\SpringBoot\kafka\pom.xml*

   -oppure-

   */users/example/home/kafka/pom.xml*

1. Aprire il file *pom.xml* in un editor di testo e aggiungere le utilità di avvio Kafka di Hub eventi all'elenco di `<dependencies>`:

   ```xml
   <dependency>
     <groupId>com.microsoft.azure</groupId>
     <artifactId>spring-cloud-starter-azure-eventhubs-kafka</artifactId>
     <version>1.2.8</version>
   </dependency>
   ```

1. Salvare e chiudere il file *pom.xml*.

## <a name="create-an-azure-credential-file"></a>Creare un file di credenziali di Azure

1. Aprire un prompt dei comandi.

1. Passare alla directory *resources* dell'app Spring Boot, ad esempio:

   ```cmd
   cd C:\SpringBoot\kafka\src\main\resources
   ```

   -oppure-

   ```bash
   cd /users/example/home/kafka/src/main/resources
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
         "name": "gena.soto@wingtiptoys.com",
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

   *C:\SpringBoot\kafka\src\main\resources\application.properties*

   -oppure-

   */users/example/home/kafka/src/main/resources/application.properties*

2. Aprire il file *application.properties* in un editor di testo, aggiungere le righe seguenti e quindi sostituire i valori di esempio con le proprietà appropriate per l'hub eventi:

   ```yaml
   spring.cloud.azure.credential-file-path=my.azureauth
   spring.cloud.azure.resource-group=wingtiptoysresources
   spring.cloud.azure.region=West US
   spring.cloud.azure.eventhub.namespace=wingtiptoys

   spring.cloud.stream.bindings.input.destination=wingtiptoyshub
   spring.cloud.stream.bindings.input.group=$Default
   spring.cloud.stream.bindings.output.destination=wingtiptoyshub
   ```
   Dove:

   |                       Campo                       |                                                                                   Descrizione                                                                                    |
   |---------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
   |     `spring.cloud.azure.credential-file-path`     |                                                    Specifica il file di credenziali di Azure creato in precedenza in questa esercitazione.                                                    |
   |        `spring.cloud.azure.resource-group`        |                                                      Specifica il gruppo di risorse di Azure contenente l'hub eventi di Azure.                                                      |
   |            `spring.cloud.azure.region`            |                                           Specifica l'area geografica indicata al momento della creazione dell'hub eventi di Azure.                                            |
   |      `spring.cloud.azure.eventhub.namespace`      |                                          Specifica il nome univoco fornito al momento della creazione dello spazio dei nomi dell'hub eventi di Azure.                                           |
   | `spring.cloud.stream.bindings.input.destination`  |                            Specifica l'hub eventi di Azure destinazione di input, che in questo caso è l'hub creato in precedenza in questa esercitazione.                            |
   |    `spring.cloud.stream.bindings.input.group `    | Specifica un gruppo di consumer dell'hub eventi di Azure, che è possibile impostare su "$Default" per usare il gruppo di consumer di base creato al momento della creazione dell'hub eventi di Azure. |
   | `spring.cloud.stream.bindings.output.destination` |                               Specifica l'hub eventi di Azure destinazione di output, che per questa esercitazione è uguale alla destinazione di input.                               |


3. Salvare e chiudere il file *application.properties*.

## <a name="add-sample-code-to-implement-basic-event-hub-functionality"></a>Aggiungere codice di esempio per implementare le funzionalità di base dell'hub eventi

In questa sezione si creano le classi Java necessarie per inviare eventi all'hub eventi.

### <a name="modify-the-main-application-class"></a>Modificare la classe dell'applicazione main

1. Individuare il file Java dell'applicazione main nella directory del pacchetto dell'app, ad esempio:

   *C:\SpringBoot\kafka\src\main\java\com\wingtiptoys\kafka\EventhubApplication.java*
   
   -oppure-

   */users/example/home/kafka/src/main/java/com/wingtiptoys/kafka/EventhubApplication.java*

1. Aprire il file Java dell'applicazione main in un editor di testo e aggiungere le righe seguenti al file:

   ```java
   package com.wingtiptoys.kafka;

   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;

   @SpringBootApplication
   public class EventhubApplication {
      public static void main(String[] args) {
         SpringApplication.run(EventhubApplication.class, args);
      }
   }
   ```

1. Salvare e chiudere il file Java dell'applicazione main.


### <a name="create-a-new-class-for-the-source-connector"></a>Creare una nuova classe per il connettore di origine

1. Creare un nuovo file Java denominato *KafkaSource.java* nella directory del pacchetto dell'app, quindi aprire il file in un editor di testo e aggiungere le righe seguenti:

   ```java
   package com.wingtiptoys.kafka;

   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.cloud.stream.annotation.EnableBinding;
   import org.springframework.cloud.stream.messaging.Source;
   import org.springframework.messaging.support.GenericMessage;
   import org.springframework.web.bind.annotation.PostMapping;
   import org.springframework.web.bind.annotation.RequestBody;
   import org.springframework.web.bind.annotation.RequestParam;
   import org.springframework.web.bind.annotation.RestController;

   @EnableBinding(Source.class)
   @RestController
   public class KafkaSource {
      @Autowired
      private Source source;

      @PostMapping("/messages")
      public String sendMessage(@RequestBody String message) {
         this.source.output().send(new GenericMessage<>(message));
         return message;
      }
   }
   ```

1. Salvare e chiudere il file *KafkaSource.java*.

### <a name="create-a-new-class-for-the-sink-connector"></a>Creare una nuova classe per il connettore sink

1. Creare un nuovo file Java denominato *KafkaSink.java* nella directory del pacchetto dell'app, quindi aprire il file in un editor di testo e aggiungere le righe seguenti:

   ```java
   package com.wingtiptoys.kafka;

   import org.slf4j.Logger;
   import org.slf4j.LoggerFactory;
   import org.springframework.cloud.stream.annotation.EnableBinding;
   import org.springframework.cloud.stream.annotation.StreamListener;
   import org.springframework.cloud.stream.messaging.Sink;

   @EnableBinding(Sink.class)
   public class KafkaSink {
      private static final Logger LOGGER = LoggerFactory.getLogger(KafkaSink.class);

      @StreamListener(Sink.INPUT)
      public void handleMessage(String message) {
         LOGGER.info("New message received: " + message);
      }
   }
   ```

1. Salvare e chiudere il file *KafkaSink.java*.

## <a name="build-and-test-your-application"></a>Compilare e testare l'applicazione

1. Aprire un prompt dei comandi e cambiare la directory passando alla cartella in cui si trova il file *pom.xml*, ad esempio:

   ```cmd
   cd C:\SpringBoot\kafka
   ```
   
   -oppure-

   ```bash
   cd /users/example/home/kafka
   ```
   
1. Compilare l'applicazione Spring Boot con Maven ed eseguirla, ad esempio:

   ```shell
   mvn clean package -Dmaven.test.skip=true
   mvn spring-boot:run
   ```

1. Quando l'applicazione è in esecuzione, è possibile testarla usando *curl*, ad esempio:

   ```shell
   curl -X POST -H "Content-Type: text/plain" -d "hello" http://localhost:8080/messages
   ```
   Verrà visualizzato l'inserimento di "hello" nei log dell'applicazione. Ad esempio:

   ```output
   2020-10-12 16:56:19.827  INFO 13272 --- [nio-8080-exec-1] o.a.kafka.common.utils.AppInfoParser     : Kafka version: 2.5.1
   2020-10-12 16:56:19.828  INFO 13272 --- [nio-8080-exec-1] o.a.kafka.common.utils.AppInfoParser     : Kafka commitId: 0efa8fb0f4c73d92
   2020-10-12 16:56:19.830  INFO 13272 --- [nio-8080-exec-1] o.a.kafka.common.utils.AppInfoParser     : Kafka startTimeMs: 1602492979827
   2020-10-12 16:56:22.277  INFO 13272 --- [container-0-C-1] com.wingtiptoys.kafka.KafkaSink          : New message received: hello
   ```


> [!NOTE]
> 
> A scopo di test, è possibile modificare il file *KafkaSource.java* in modo che contenga un semplice modulo HTML come nell'esempio seguente:
> 
> ```java
> package com.wingtiptoys.kafka;
>    
> import org.springframework.beans.factory.annotation.Autowired;
> import org.springframework.cloud.stream.annotation.EnableBinding;
> import org.springframework.cloud.stream.messaging.Source;
> import org.springframework.messaging.support.GenericMessage;
> import org.springframework.web.bind.annotation.GetMapping;
> import org.springframework.web.bind.annotation.PostMapping;
> import org.springframework.web.bind.annotation.RequestBody;
> import org.springframework.web.bind.annotation.RequestParam;
> import org.springframework.web.bind.annotation.RestController;
> 
> @EnableBinding(Source.class)
> @RestController
> public class KafkaSource {
>   @Autowired
>   private Source source;
> 
>   @GetMapping("/")
>   public String sendForm() {
>     return "<html><body>" +
>       "<form action=\"/messages\" method=\"post\">" +
>       "<input type=\"text\" name=\"text\">" +
>       "<input type=\"submit\">" +
>       "</form></body><html>";
>     }
> 
>   @PostMapping("/messages")
>   public String sendMessage(@RequestBody String message) {
>     this.source.output().send(new GenericMessage<>(message));
>     return message;
>   }
> }
> ```
> 
> Questo consentirà di usare il Web browser per testare l'applicazione:
> 
> ![Test dell'applicazione con un Web browser][TB01]
> 
> Quando si invierà il modulo, l'applicazione visualizzerà i risultati:
> 
> ![Risposta dell'applicazione in un Web browser][TB02]
> 

## <a name="clean-up-resources"></a>Pulire le risorse

Quando le risorse create in questo articolo non sono più necessarie, usare il [portale di Azure](https://portal.azure.com/) per eliminarle in modo da evitare addebiti imprevisti.

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni su Spring e Azure, passare al centro di documentazione di Spring in Azure.

> [!div class="nextstepaction"]
> [Spring in Azure](./index.yml)

### <a name="additional-resources"></a>Risorse aggiuntive

Per altre informazioni sul supporto di Azure per Apache Kafka e Stream Binder per Hub eventi, vedere gli articoli seguenti:

* [Che cos'è Hub eventi di Azure?](/azure/event-hubs/event-hubs-about)

* [Hub eventi di Azure per Apache Kafka](/azure/event-hubs/event-hubs-for-kafka-ecosystem-overview)

* [Creare uno spazio dei nomi di Hub eventi e un hub eventi usando il Portale di Azure](/azure/event-hubs/event-hubs-create)

* [Creare hub eventi con supporto per Apache Kafka](/azure/event-hubs/event-hubs-create-kafka-enabled)

Per altre informazioni sull'uso di Azure con Java, vedere [Azure per sviluppatori Java] e la documentazione relativa all'[uso di Azure DevOps e Java].

**[Spring Framework]** è una soluzione open source che consente agli sviluppatori Java di creare applicazioni di livello enterprise. Uno dei progetti più comuni che si basa su questa piattaforma è [Spring Boot], che fornisce un approccio semplificato per la creazione di applicazioni Java autonome. Per semplificare le operazioni iniziali con Spring Boot per gli sviluppatori, alcuni pacchetti Spring Boot di esempio sono disponibili all'indirizzo <https://github.com/spring-guides/>. Oltre a consentire di scegliere dall'elenco di progetti Spring Boot di base, **[Spring Initializr]** semplifica le operazioni iniziali degli sviluppatori per la creazione di applicazioni Spring Boot personalizzate.

<!-- URL List -->

[Apache Kafka]: http://kafka.apache.org
[Account Azure gratuito]: https://azure.microsoft.com/pricing/free-trial/
[Uso di Azure DevOps e Java]: /azure/devops/
[vantaggi per i sottoscrittori di MSDN]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/ (Framework di Spring)

<!-- IMG List -->

[IMG01]: media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/create-kafka-event-hub-01.png
[IMG02]: media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/create-kafka-event-hub-02.png
[IMG05]: media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/create-kafka-event-hub-05.png

[SI01]: media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/create-project-01.png

[TB01]: media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/test-browser-01.png
[TB02]: media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/test-browser-02.png
