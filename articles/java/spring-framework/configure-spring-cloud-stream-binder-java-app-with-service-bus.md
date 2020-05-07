---
title: Come usare Spring Cloud Stream Binder per il bus di servizio di Azure
description: Questo articolo illustra come usare Spring Cloud Stream Binder per inviare e ricevere messaggi dal bus di servizio di Azure.
author: seanli1988
manager: kyliel
ms.author: seal
ms.date: 08/21/2019
ms.topic: article
ms.openlocfilehash: 033025b82a493cf701abad7a6a97802611b7e48d
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/05/2020
ms.locfileid: "81669067"
---
# <a name="how-to-use-spring-cloud-azure-stream-binder-for-azure-service-bus"></a>Come usare Spring Cloud Stream Binder per il bus di servizio di Azure

[!INCLUDE [spring-boot-20-note.md](includes/spring-boot-20-note.md)]

Azure fornisce una piattaforma di messaggistica asincrona denominata [bus di servizio di Azure](/azure/service-bus-messaging/service-bus-messaging-overview) ("bus di servizio") basata sullo standard [Advanced Message Queueing Protocol 1.0](http://www.amqp.org/) ("AMQP 1.0"). Il bus di servizio può essere usato nella gamma di piattaforme di Azure supportate.

Questo articolo illustra come usare Spring Cloud Stream Binder per inviare e ricevere messaggi da `queues` e `topics` del bus di servizio.

## <a name="prerequisites"></a>Prerequisites

Per questo articolo sono necessari i prerequisiti seguenti:

1. Se non si ha una sottoscrizione di Azure, è possibile attivare i [vantaggi per i sottoscrittori di MSDN](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/) oppure iscriversi per ottenere un [account gratuito](https://azure.microsoft.com/free/).

1. Una versione supportata di Java Development Kit (JDK), versione 8 o successive. Per altre informazioni sulle versioni di JDK utilizzabili per lo sviluppo in Azure, vedere <https://aka.ms/azure-jdks>.

1. [Maven](http://maven.apache.org/) di Apache, versione 3.2 o successive.

1. Se è già presente una coda o un argomento configurato del bus di servizio, assicurarsi che lo spazio dei nomi del bus di servizio soddisfi i requisiti seguenti:

    1. Consente l'accesso da tutte le reti
    1. È Standard o versione successiva
    1. Dispone di un criterio di accesso in lettura/scrittura per la coda e l'argomento

1. Se non è presente una coda o un argomento configurato del bus di servizio, usare il portale di Azure per [creare una coda del bus di servizio](/azure/service-bus-messaging/service-bus-quickstart-portal) o per [creare un argomento del bus di servizio](/azure/service-bus-messaging/service-bus-quickstart-topics-subscriptions-portal). Verificare che lo spazio dei nomi soddisfi i requisiti specificati nel passaggio precedente. Prendere nota inoltre della stringa di connessione nello spazio dei nomi. Sarà necessaria per l'app di test di questa esercitazione.

1. Se non si dispone di un'applicazione Spring Boot, [creare un progetto **Maven** con Spring Initializr](https://start.spring.io/). Ricordarsi di selezionare **Maven Project** (Progetto Maven) e in **Dependencies** (Dipendenze) aggiungere la dipendenza **Web**.

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
        <groupId>com.microsoft.azure</groupId>
        <artifactId>spring-cloud-azure-servicebus-queue-stream-binder</artifactId>
        <version>1.1.0.RC5</version>
    </dependency>
    ```

    ![Modificare il file pom.xml per la coda del bus di servizio.](media/configure-spring-cloud-stream-binder-java-app-with-service-bus/add-stream-binder-starter-pom-file-dependency-for-service-bus-queue.png)

    **Argomento del bus di servizio**

    ```xml
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>spring-cloud-azure-servicebus-topic-stream-binder</artifactId>
        <version>1.1.0.RC5</version>
    </dependency>
    ```

    ![Modificare il file pom.xml per l'argomento del bus di servizio.](media/configure-spring-cloud-stream-binder-java-app-with-service-bus/add-stream-binder-starter-pom-file-dependency-for-service-bus-topic.png)

1. Salvare e chiudere il file *pom.xml*.

## <a name="configure-the-app-for-your-service-bus"></a>Configurare l'app per il bus di servizio

È possibile configurare l'app in base alla stringa di connessione o a un file di credenziali. In questa esercitazione viene usata una stringa di connessione. Per altre informazioni sull'uso di file di credenziali, vedere l'[esempio di codice Spring Cloud Stream Binder per una coda del bus di servizio](https://github.com/microsoft/spring-cloud-azure/tree/release/1.1.0.RC4/spring-cloud-azure-samples/servicebus-queue-binder-sample#credential-file-based-usage
) e l'[esempio di codice Spring Cloud Stream Binder per un argomento del bus di servizio](https://github.com/microsoft/spring-cloud-azure/tree/release/1.1.0.RC4/spring-cloud-azure-samples/servicebus-topic-binder-sample#credential-file-based-usage).

1. Individuare il file *application.properties* nella directory *resources* dell'app, ad esempio:

   `C:\SpringBoot\servicebus\src\main\resources\application.properties`

   -oppure-

   `/users/example/home/servicebus/src/main/resources/application.properties`

1. Aprire il file *application.properties* in un editor di testo.

1. Aggiungere il codice appropriato alla fine del file *application.properties* a seconda che si stia usando una coda o un argomento del bus di servizio. Usare la [tabella relativa alle descrizioni dei campi](#fd) per sostituire i valori di esempio con le proprietà appropriate per il bus di servizio.

    **coda del bus di servizio**

    ```yaml
    spring.cloud.azure.servicebus.connection-string=<ServiceBusNamespaceConnectionString>
    spring.cloud.stream.bindings.input.destination=examplequeue
    spring.cloud.stream.bindings.output.destination=examplequeue
    spring.cloud.stream.servicebus.queue.bindings.input.consumer.checkpoint-mode=MANUAL
    ```

    **Argomento del bus di servizio**

    ```yaml
    spring.cloud.azure.servicebus.connection-string=<ServiceBusNamespaceConnectionString>
    spring.cloud.stream.bindings.input.destination=exampletopic
    spring.cloud.stream.bindings.input.group=examplesubscription
    spring.cloud.stream.bindings.output.destination=exampletopic
    spring.cloud.stream.servicebus.topic.bindings.input.consumer.checkpoint-mode=MANUAL
    ```

    **<a name="fd">Descrizioni dei campi</a>**

    |                                        Campo                                   |                                                                                   Descrizione                                                                                    |
    |--------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
    |               `spring.cloud.azure.servicebus.connection-string`                |                                        Specificare la stringa di connessione ottenuta nello spazio dei nomi del bus di servizio dal portale di Azure.                                   |
    |               `spring.cloud.stream.bindings.input.destination`                 |                            Specificare la coda o l'argomento del bus di servizio usato in questa esercitazione.                         |
    |                  `spring.cloud.stream.bindings.input.group`                    |                                            Se è stato usato un argomento del bus di servizio, specificare la sottoscrizione dell'argomento.                                |
    |               `spring.cloud.stream.bindings.output.destination`                |                               Specificare lo stesso valore usato per la destinazione input.                        |
    | `spring.cloud.stream.servicebus.queue.bindings.input.consumer.checkpoint-mode` |                                                       Specificare `MANUAL`.                                                   |
    | `spring.cloud.stream.servicebus.topic.bindings.input.consumer.checkpoint-mode` |                                                       Specificare `MANUAL`.                                                   |

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

    import org.springframework.boot.SpringApplication;
    import org.springframework.boot.autoconfigure.SpringBootApplication;

    @SpringBootApplication
    public class ServiceBusBinderApplication {

        public static void main(String[] args) {
            SpringApplication.run(ServiceBusBinderApplication.class, args);
        }
    }
    ```

1. Salvare e chiudere il file.

### <a name="create-a-new-class-for-the-source-connector"></a>Creare una nuova classe per il connettore di origine

1. Usando un editor di testo, creare un file Java denominato *StreamBinderSource.java* nella directory del pacchetto dell'app.

1. Aggiungere al nuovo file il codice seguente:

    ```java
    package com.example;

    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.cloud.stream.annotation.EnableBinding;
    import org.springframework.cloud.stream.messaging.Source;
    import org.springframework.messaging.support.GenericMessage;
    import org.springframework.web.bind.annotation.PostMapping;
    import org.springframework.web.bind.annotation.RequestParam;
    import org.springframework.web.bind.annotation.RestController;

    @EnableBinding(Source.class)
    @RestController
    public class StreamBinderSource {

        @Autowired
        private Source source;

        @PostMapping("/messages")
        public String postMessage(@RequestParam String message) {
            this.source.output().send(new GenericMessage<>(message));
            return message;
        }
    }
    ```

1. Salvare e chiudere il file *StreamBinderSources.java*.

### <a name="create-a-new-class-for-the-sink-connector"></a>Creare una nuova classe per il connettore sink

1. Usando un editor di testo, creare un file Java denominato *StreamBinderSink.java* nella directory del pacchetto dell'app.

1. Aggiungere al nuovo file le righe di codice seguenti:

    ```java
    package com.example;

    import com.microsoft.azure.spring.integration.core.AzureHeaders;
    import com.microsoft.azure.spring.integration.core.api.Checkpointer;
    import org.springframework.cloud.stream.annotation.EnableBinding;
    import org.springframework.cloud.stream.annotation.StreamListener;
    import org.springframework.cloud.stream.messaging.Sink;
    import org.springframework.messaging.handler.annotation.Header;

    @EnableBinding(Sink.class)
    public class StreamBinderSink {

        @StreamListener(Sink.INPUT)
        public void handleMessage(String message, @Header(AzureHeaders.CHECKPOINTER) Checkpointer checkpointer) {
            System.out.println(String.format("New message received: '%s'", message));
            checkpointer.success().handle((r, ex) -> {
                if (ex == null) {
                    System.out.println(String.format("Message '%s' successfully checkpointed", message));
                }
                return null;
            });
        }
    }
    ```

1. Salvare e chiudere il file *StreamBinderSink.java*.

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

## <a name="clean-up-resources"></a>Pulire le risorse

Quando le risorse create in questo articolo non sono più necessarie, usare il [portale di Azure](https://portal.azure.com/) per eliminarle in modo da evitare addebiti imprevisti.

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"]
> [Spring in Azure](/java/azure/spring-framework)