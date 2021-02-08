---
title: Come usare Spring Cloud Stream Binder per il bus di servizio di Azure
description: Questo articolo illustra come usare Spring Cloud Stream Binder per inviare e ricevere messaggi dal bus di servizio di Azure.
author: seanli1988
manager: kyliel
ms.author: seal
ms.date: 02/04/2021
ms.topic: article
ms.custom: devx-track-java
ms.openlocfilehash: 4223927cd99e652e8fb06dd8250c39b191f36a94
ms.sourcegitcommit: bccbab4883e6b6b4926fc194c35ad948b11ccc3f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/08/2021
ms.locfileid: "99822704"
---
# <a name="how-to-use-spring-cloud-azure-stream-binder-for-azure-service-bus"></a>Come usare Spring Cloud Stream Binder per il bus di servizio di Azure

[!INCLUDE [spring-boot-20-note.md](includes/spring-boot-20-note.md)]

Questo articolo illustra come usare Spring Cloud Stream Binder per inviare e ricevere messaggi da `queues` e `topics` del bus di servizio.

Azure fornisce una piattaforma di messaggistica asincrona denominata [bus di servizio di Azure](/azure/service-bus-messaging/service-bus-messaging-overview) ("bus di servizio") basata sullo standard [Advanced Message Queueing Protocol 1.0](http://www.amqp.org/) ("AMQP 1.0"). Il bus di servizio può essere usato nella gamma di piattaforme di Azure supportate.

## <a name="prerequisites"></a>Prerequisites

Per questo articolo sono necessari i prerequisiti seguenti:

1. Sottoscrizione di Azure. Se non si ha una sottoscrizione di Azure, è possibile attivare i [vantaggi per i sottoscrittori di MSDN](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/) oppure iscriversi per ottenere un [account gratuito](https://azure.microsoft.com/free/).

1. Una versione supportata di Java Development Kit (JDK), versione 8 o successive. Per altre informazioni sulle versioni di JDK utilizzabili per lo sviluppo in Azure, vedere <https://aka.ms/azure-jdks>.

1. [Maven](http://maven.apache.org/) di Apache, versione 3.2 o successive.

1. Se è già presente una coda o un argomento configurato del bus di servizio, assicurarsi che lo spazio dei nomi del bus di servizio soddisfi i requisiti seguenti:

    1. Consente l'accesso da tutte le reti
    1. È Standard o versione successiva
    1. Dispone di un criterio di accesso in lettura/scrittura per la coda e l'argomento

1. Se non è presente una coda o un argomento configurato del bus di servizio, usare il portale di Azure per [creare una coda del bus di servizio](/azure/service-bus-messaging/service-bus-quickstart-portal) o per [creare un argomento del bus di servizio](/azure/service-bus-messaging/service-bus-quickstart-topics-subscriptions-portal). Verificare che lo spazio dei nomi soddisfi i requisiti specificati nel passaggio precedente. Prendere nota inoltre della stringa di connessione nello spazio dei nomi. Sarà necessaria per l'app di test di questa esercitazione.

1. Se non è disponibile un'applicazione Spring Boot, creare un progetto **Maven** con [Spring Initializr](https://start.spring.io/). Ricordarsi di selezionare il **progetto Maven** e in **dipendenze** aggiungere la dipendenza **Web** , in **Spring boot**, selezionare 2.3.8, quindi selezionare **8** versione Java.


## <a name="use-the-spring-cloud-stream-binder-starter"></a>Usare l'utilità di avvio Spring Cloud Stream Binder

1. Individuare il file *pom.xml* nella directory padre dell'app, ad esempio:

    `C:\SpringBoot\servicebus\pom.xml`

    -oppure-

    `/users/example/home/servicebus/pom.xml`

1. Aprire il file *pom.xml* in un editor di testo.

1. Aggiungere il blocco di codice seguente sotto l'elemento **&lt;dependencies>** , a seconda che si stia usando una coda o un argomento del bus di servizio:

    **coda del bus di servizio**

    ```xml
    <dependency>
        <groupId>com.azure.spring</groupId>
        <artifactId>azure-spring-cloud-stream-binder-servicebus-queue</artifactId>
        <version>2.1.0</version>
    </dependency>
    ```

    **Argomento del bus di servizio**

    ```xml
    <dependency>
        <groupId>com.azure.spring</groupId>
        <artifactId>azure-spring-cloud-stream-binder-servicebus-topic</artifactId>
        <version>2.1.0</version>
    </dependency>
    ```


1. Salvare e chiudere il file *pom.xml*.

## <a name="configure-the-app-for-your-service-bus"></a>Configurare l'app per il bus di servizio

È possibile configurare l'app in base alla stringa di connessione o a un file di credenziali. In questa esercitazione viene usata una stringa di connessione. Per altre informazioni sull'uso di file di credenziali, vedere l'[esempio di codice Spring Cloud Stream Binder per una coda del bus di servizio](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/spring/azure-spring-boot-samples/azure-spring-cloud-sample-servicebus-queue-binder
) e l'[esempio di codice Spring Cloud Stream Binder per un argomento del bus di servizio](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/spring/azure-spring-cloud-stream-binder-servicebus-topic).

1. Individuare il file *application.properties* nella directory *resources* dell'app, ad esempio:

   `C:\SpringBoot\servicebus\src\main\resources\application.properties`

   -oppure-

   `/users/example/home/servicebus/src/main/resources/application.properties`

1. Aprire il file *application.properties* in un editor di testo.

1. Aggiungere il codice appropriato alla fine del file *application.properties* a seconda che si stia usando una coda o un argomento del bus di servizio. Usare la [tabella relativa alle descrizioni dei campi](#fd) per sostituire i valori di esempio con le proprietà appropriate per il bus di servizio.

    **Coda del bus di servizio**

    ```yaml
    spring:
      cloud:
        azure:
          servicebus:
            connection-string: <ServiceBusNamespaceConnectionString>
        stream:
          bindings:
            consume-in-0:
              destination: examplequeue
            supply-out-0:
              destination: examplequeue
          servicebus:
            queue:
              bindings:
                consume-in-0:
                  consumer:
                    checkpoint-mode: MANUAL
          function:
            definition: consume;supply;
          poller:
            fixed-delay: 1000
            initial-delay: 0
    ```

    **Argomento del bus di servizio**

    ```yaml
    spring:
      cloud:
        azure:
          servicebus:
            connection-string: <ServiceBusNamespaceConnectionString>
        stream:
          bindings:
            consume-in-0:
              destination: exampletopic
              group: examplesubscription
            supply-out-0:
              destination: exampletopic
          servicebus:
            topic:
              bindings:
                consume-in-0:
                  consumer:
                    checkpoint-mode: MANUAL
          function:
            definition: consume;supply;
          poller:
            fixed-delay: 1000
            initial-delay: 0
    ```

    **<a name="fd">Descrizioni dei campi</a>**

    |                                        Campo                                   |                                                                                   Descrizione                                                                                    |
    |--------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
    |               `spring.cloud.azure.function.definition`                |                                        Specificare il fagiolo funzionale da associare alle destinazioni esterne esposte dalle associazioni.                                   |
    |               `spring.cloud.azure.poller.fixed-delay`                |                                        Specificare un ritardo fisso per il polling predefinito in millisecondi, valore predefinito di 1000.                                   |
    |               `spring.cloud.azure.poller.initial-delay`                |                                       Specificare il ritardo iniziale per i trigger periodici, valore predefinito 0.                                   |
    |               `spring.cloud.azure.servicebus.connection-string`                |                                        Specificare la stringa di connessione ottenuta nello spazio dei nomi del bus di servizio dal portale di Azure.                                   |
    |               `spring.cloud.stream.bindings.consume-in-0.destination`                 |                            Specificare la coda o l'argomento del bus di servizio usato in questa esercitazione.                         |
    |                  `spring.cloud.stream.bindings.consume-in-0.group`                    |                                            Se è stato usato un argomento del bus di servizio, specificare la sottoscrizione dell'argomento.                                |
    |               `spring.cloud.stream.bindings.supply-out-0.destination`                |                               Specificare lo stesso valore usato per la destinazione input.                        |
    | `spring.cloud.stream.servicebus.queue.bindings.consume-in-0.consumer.checkpoint-mode` |                                                       Specificare `MANUAL`.                                                   |
    | `spring.cloud.stream.servicebus.topic.bindings.consume-in-0.consumer.checkpoint-mode` |                                                       Specificare `MANUAL`.                                                   |

1. Salvare e chiudere il file *application.properties*.

## <a name="implement-basic-service-bus-functionality"></a>Implementare la funzionalità di base del bus di servizio

In questa sezione è possibile creare le classi Java necessarie per inviare messaggi al bus di servizio.

### <a name="modify-the-main-application-class"></a>Modificare la classe dell'applicazione main

1. Individuare il file Java dell'applicazione main nella directory del pacchetto dell'app, ad esempio:

    `C:\SpringBoot\servicebus\src\main\java\com\example\ServiceBusBinderApplication.java`

   -oppure-

   `/users/example/home/servicebus/src/main/java/com/example/ServiceBusBinderApplication.java`

1. Aprire il file Java dell'applicazione principale in un editor di testo.

1. Aggiungere il codice seguente al file:

    ```java
   package com.example;
   
   import com.azure.spring.integration.core.api.Checkpointer;
   import org.slf4j.Logger;
   import org.slf4j.LoggerFactory;
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   import org.springframework.context.annotation.Bean;
   import org.springframework.messaging.Message;
   
   import java.util.function.Consumer;
   
   import static com.azure.spring.integration.core.AzureHeaders.CHECKPOINTER;
   
   @SpringBootApplication
   public class ServiceBusBinderApplication {
   
       private static final Logger LOGGER = LoggerFactory.getLogger(ServiceBusBinderApplication.class);
   
       public static void main(String[] args) {
           SpringApplication.run(ServiceBusBinderApplication.class, args);
       }
   
       @Bean
       public Consumer<Message<String>> consume() {
           return message -> {
               Checkpointer checkpointer = (Checkpointer) message.getHeaders().get(CHECKPOINTER);
               LOGGER.info("New message received: '{}'", message);
               checkpointer.success().handle((r, ex) -> {
                   if (ex == null) {
                       LOGGER.info("Message '{}' successfully checkpointed", message);
                   }
                   return null;
               });
           };
       }
   }
    ```

1. Salvare e chiudere il file.

### <a name="create-a-new-producer-configuration-class"></a>Creare una nuova classe di configurazione Producer

1. Usando un editor di testo, creare un file Java denominato *ServiceProducerConfiguration. Java* nella directory del pacchetto dell'app.

1. Aggiungere al nuovo file il codice seguente:

    ```java
   package com.example;
   
   import org.slf4j.Logger;
   import org.slf4j.LoggerFactory;
   import org.springframework.context.annotation.Bean;
   import org.springframework.context.annotation.Configuration;
   import org.springframework.messaging.Message;
   import reactor.core.publisher.EmitterProcessor;
   import reactor.core.publisher.Flux;
   
   import java.util.function.Supplier;
   
   @Configuration
   public class ServiceProducerConfiguration {
   
       private static final Logger LOGGER = LoggerFactory.getLogger(ServiceProducerConfiguration.class);
   
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

1. Salvare e chiudere il file *ServiceProducerConfiguration. Java* .

### <a name="create-a-new-controller-class"></a>Creare una nuova classe controller

1. Usando un editor di testo, creare un file Java denominato *ServiceProducerController. Java* nella directory del pacchetto dell'app.

1. Aggiungere al nuovo file le righe di codice seguenti:

    ```java
   package com.example;
   
   import org.slf4j.Logger;
   import org.slf4j.LoggerFactory;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.http.ResponseEntity;
   import org.springframework.messaging.Message;
   import org.springframework.messaging.support.MessageBuilder;
   import org.springframework.web.bind.annotation.GetMapping;
   import org.springframework.web.bind.annotation.PostMapping;
   import org.springframework.web.bind.annotation.RequestParam;
   import org.springframework.web.bind.annotation.RestController;
   import reactor.core.publisher.EmitterProcessor;
   
   @RestController
   public class ServiceProducerController {
   
       private static final Logger LOGGER = LoggerFactory.getLogger(ServiceProducerController.class);
   
       @Autowired
       private EmitterProcessor<Message<String>> emitterProcessor;
   
       @PostMapping("/messages")
       public ResponseEntity<String> sendMessage(@RequestParam String message) {
           LOGGER.info("Going to add message {} to emitter", message);
           emitterProcessor.onNext(MessageBuilder.withPayload(message).build());
           return ResponseEntity.ok("Sent!");
       }
   
       @GetMapping("/")
       public String welcome() {
           return "welcome";
       }
   }
    ```

1. Salvare e chiudere il file *ServiceProducerController. Java* .

## <a name="build-and-test-your-application"></a>Compilare e testare l'applicazione

1. Aprire un prompt dei comandi.

1. Cambiare directory passando al percorso in cui si trova il file *pom.xml*, ad esempio:

    `cd C:\SpringBoot\servicebus`

    -oppure-

    `cd /users/example/home/servicebus`

2. Compilare l'applicazione Spring Boot con Maven ed eseguirla:

    ```shell
    mvn clean spring-boot:run
    ```

3. Quando l'applicazione è in esecuzione, è possibile testarla usando *curl*:

    ```shell
    curl -X POST localhost:8080/messages?message=hello
    ```

    Si vedrà che è stato inserito "hello" nel log dell'applicazione:

    ```shell
    New message received: 'hello'
    Message 'hello' successfully checkpointed
    ```

## <a name="clean-up-resources"></a>Pulizia delle risorse

Quando le risorse create in questo articolo non sono più necessarie, usare il [portale di Azure](https://portal.azure.com/) per eliminarle in modo da evitare addebiti imprevisti.

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"]
> [Spring in Azure](/java/azure/spring-framework)