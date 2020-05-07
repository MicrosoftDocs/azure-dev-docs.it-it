---
title: Come usare l'utilità di avvio Spring Boot per il JMS del bus di servizio di Azure
description: Questo articolo illustra come usare l'utilità di avvio Spring per il JMS per inviare e ricevere messaggi dal bus di servizio di Azure.
author: seanli1988
manager: kyliel
ms.author: seal
ms.date: 08/21/2019
ms.topic: article
ms.openlocfilehash: d997b679d1a608351748b67f99977d48d95febe2
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/05/2020
ms.locfileid: "81674297"
---
# <a name="how-to-use-the-spring-boot-starter-for-azure-service-bus-jms"></a>Come usare l'utilità di avvio Spring Boot per il JMS del bus di servizio di Azure

[!INCLUDE [spring-boot-20-note.md](includes/spring-boot-20-note.md)]

Azure fornisce una piattaforma di messaggistica asincrona denominata [bus di servizio di Azure](/azure/service-bus-messaging/service-bus-messaging-overview) ("bus di servizio") basata sullo standard [Advanced Message Queueing Protocol 1.0](http://www.amqp.org/) ("AMQP 1.0"). Il bus di servizio può essere usato nella gamma di piattaforme di Azure supportate.

L'utilità di avvio Spring Boot per il JMS del bus di servizio di Azure fornisce l'integrazione Spring con il bus di servizio.

Questo articolo illustra come usare l'utilità di avvio Spring Boot per il JMS del bus di servizio di Azure per inviare e ricevere messaggi da `queues` e `topics` del bus di servizio.

## <a name="prerequisites"></a>Prerequisites

Per questo articolo sono necessari i prerequisiti seguenti:

1. Se non si ha una sottoscrizione di Azure, è possibile attivare i [vantaggi per i sottoscrittori di MSDN](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/) oppure iscriversi per ottenere un [account gratuito](https://azure.microsoft.com/free/).

1. Una versione supportata di Java Development Kit (JDK), versione 8 o successive. Per altre informazioni sulle versioni di JDK utilizzabili per lo sviluppo in Azure, vedere <https://aka.ms/azure-jdks>.

1. [Maven](http://maven.apache.org/) di Apache, versione 3.2 o successive.

1. Se è già presente una coda o un argomento configurato del bus di servizio, assicurarsi che lo spazio dei nomi del bus di servizio soddisfi i requisiti seguenti:

    1. Consente l'accesso da tutte le reti
    1. È una versione Premium (o superiore)
    1. Dispone di un criterio di accesso in lettura/scrittura per la coda e l'argomento

1. Se non è presente una coda o un argomento configurato del bus di servizio, usare il portale di Azure per [creare una coda del bus di servizio](/azure/service-bus-messaging/service-bus-quickstart-portal) o per [creare un argomento del bus di servizio](/azure/service-bus-messaging/service-bus-quickstart-topics-subscriptions-portal). Verificare che lo spazio dei nomi soddisfi i requisiti specificati nel passaggio precedente. Prendere nota inoltre della stringa di connessione nello spazio dei nomi. Sarà necessaria per l'app di test di questa esercitazione.

1. Se non si dispone di un'applicazione Spring Boot, [creare un progetto **Maven** con Spring Initializr](https://start.spring.io/). Ricordarsi di selezionare **Maven Project** (Progetto Maven) e in **Dependencies** (Dipendenze) aggiungere la dipendenza **Web**.

## <a name="use-the-azure-service-bus-jms-starter"></a>Usare l'utilità di avvio per il JMS del bus di servizio di Azure

1. Individuare il file *pom.xml* nella directory padre dell'app, ad esempio:

    `C:\SpringBoot\servicebus\pom.xml`

    -oppure-

    `/users/example/home/servicebus/pom.xml`

1. Aprire il file *pom.xml* in un editor di testo.

1. Aggiungere l'utilità di avvio Spring Boot per il JMS del bus di servizio di Azure all'elenco di `<dependencies>`:

    ```xml
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-servicebus-jms-spring-boot-starter</artifactId>
        <version>2.1.7</version>
    </dependency>
    ```

    ![Aggiungere la sezione dependency al file pom.xml.](media/configure-spring-boot-starter-java-app-with-azure-service-bus/add-dependency-section-new.png)

1. Salvare e chiudere il file *pom.xml*.

## <a name="configure-the-app-for-your-service-bus"></a>Configurare l'app per il bus di servizio 

In questa sezione viene illustrato come configurare l'app per l'uso di una coda o di un argomento del bus di servizio.

### <a name="use-a-service-bus-queue"></a>Usare una coda del bus di servizio

1. Individuare *application.properties* nella directory *resources* dell'app, ad esempio:

    `C:\SpringBoot\servicebus\application.properties`

    -oppure-

    `/users/example/home/servicebus/application.properties`

1. Aprire il file *application.properties* in un editor di testo.

1. Aggiungere il codice seguente alla fine del file *application.properties*. Sostituire i valori di esempio con i valori appropriati per il bus di servizio:

    ```yml
    spring.jms.servicebus.connection-string=<ServiceBusNamespaceConnectionString>
    spring.jms.servicebus.idle-timeout=<IdleTimeout>
    ```

    **Descrizioni dei campi**

    | Campo                                     | Descrizione                                                                                     |
    |-------------------------------------------|-------------------------------------------------------------------------------------------------|
    | `spring.jms.servicebus.connection-string` | Specificare la stringa di connessione ottenuta nello spazio dei nomi del bus di servizio dal portale di Azure. |
    | `spring.jms.servicebus.idle-timeout`      | Specificare il timeout di inattività in millisecondi. Il valore consigliato per questa esercitazione è 1800000.   |

1. Salvare e chiudere il file *application.properties*.

### <a name="use-service-bus-topic"></a>Usare l'argomento del bus di servizio

1. Individuare *application.properties* nella directory *resources* dell'app, ad esempio:

    `C:\SpringBoot\servicebus\application.properties`

    -oppure-

    `/users/example/home/servicebus/application.properties`

1. Aprire il file *application.properties* in un editor di testo.

1. Aggiungere il codice seguente alla fine del file *application.properties*. Sostituire i valori di esempio con i valori appropriati per il bus di servizio:

    ```yml
    spring.jms.servicebus.connection-string=<ServiceBusNamespaceConnectionString>
    spring.jms.servicebus.topic-client-id=<ServiceBusTopicClientId>
    spring.jms.servicebus.idle-timeout=<IdleTimeout>
    ```

    **Descrizioni dei campi**

    | Campo                                     | Descrizione                                                                                       |
    |-------------------------------------------|---------------------------------------------------------------------------------------------------|
    | `spring.jms.servicebus.connection-string` | Specificare la stringa di connessione ottenuta nello spazio dei nomi del bus di servizio dal portale di Azure.   |
    | `spring.jms.servicebus.topic-client-id`   | Specificare l'ID client JMS se si usa un argomento del bus di servizio di Azure con una sottoscrizione durevole. |
    | `spring.jms.servicebus.idle-timeout`      | Specificare il timeout di inattività in millisecondi. Il valore consigliato per questa esercitazione è 1800000.     |

1. Salvare e chiudere il file *application.properties*.

## <a name="implement-basic-service-bus-functionality"></a>Implementare la funzionalità di base del bus di servizio

In questa sezione vengono create le classi Java necessarie per l'invio di messaggi alla coda o all'argomento del bus di servizio e per la ricezione di messaggi dalla sottoscrizione della coda o dell'argomento corrispondente.

### <a name="modify-the-main-application-class"></a>Modificare la classe dell'applicazione main

1. Individuare il file Java dell'applicazione main nella directory del pacchetto dell'app, ad esempio:

    `C:\SpringBoot\servicebus\src\main\java\com\wingtiptoys\servicebus\ServiceBusJmsStarterApplication.java`

    -oppure-

    `/users/example/home/servicebus/src/main/java/com/wingtiptoys/servicebus/ServiceBusJmsStarterApplication.java`

1. Aprire il file Java dell'applicazione principale in un editor di testo.

1. Aggiungere il codice seguente al file:

   ```java
    package com.wingtiptoys.servicebus;

    import org.springframework.boot.SpringApplication;
    import org.springframework.boot.autoconfigure.SpringBootApplication;

    @SpringBootApplication
    public class ServiceBusJmsStarterApplication {

        public static void main(String[] args) {
            SpringApplication.run(ServiceBusJmsStarterApplication.class, args);
        }
    }
    ```

1. Salvare e chiudere il file.

### <a name="define-a-test-java-class"></a>Definire una classe Java di test

1. Usare un editor di testo per creare un file Java denominato *User.java* nella directory del pacchetto dell'app.

1. Definire una classe utente generica che archivia e recupera il nome dell'utente:

    ```java
    package com.wingtiptoys.servicebus;

    import java.io.Serializable;

    // Define a generic User class.
    public class User implements Serializable {

        private static final long serialVersionUID = -295422703255886286L;

        private String name;

        public User() {
        }

        public User(String name) {
            setName(name);
        }

        public String getName() {
            return name;
        }

        public void setName(String name) {
            this.name = name;
        }

    }
    ```

    `Serializable` viene implementato per usare il metodo `send` in `JmsTemplate` nel framework Spring. In caso contrario, sarebbe necessario definire un bean `MessageConverter` personalizzato per serializzare il contenuto json in formato testo. Per altre informazioni su `MessageConverter`, vedere il [progetto dell'utilità di avvio Spring per il JMS](https://spring.io/guides/gs/messaging-jms/) ufficiale.

1. Salvare e chiudere il file *User.java*.

### <a name="create-a-new-class-for-the-message-send-controller"></a>Creare una nuova classe per il controller di invio dei messaggi

1. Usare un editor di testo per creare un file Java denominato *SendController.java* nella directory del pacchetto dell'app

1. Aggiungere al nuovo file il codice seguente:

    ```java
    package com.wingtiptoys.servicebus;

    import org.slf4j.Logger;
    import org.slf4j.LoggerFactory;
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.jms.core.JmsTemplate;
    import org.springframework.web.bind.annotation.PostMapping;
    import org.springframework.web.bind.annotation.RequestParam;
    import org.springframework.web.bind.annotation.RestController;

    @RestController
    public class SendController {

        private static final String DESTINATION_NAME = "<DestinationName>";

        private static final Logger logger = LoggerFactory.getLogger(SendController.class);

        @Autowired
        private JmsTemplate jmsTemplate;

        @PostMapping("/messages")
        public String postMessage(@RequestParam String message) {
            logger.info("Sending message");
            jmsTemplate.convertAndSend(DESTINATION_NAME, new User(message));
            return message;
        }
    }
    ```

    > [!NOTE]
    > Sostituire `<DestinationName>` con il nome della coda o dell'argomento configurato nello spazio dei nomi del bus di servizio.

1. Salvare e chiudere il file *SendController.Java*.

### <a name="create-a-class-for-the-message-receive-controller"></a>Creare una classe per il controller di ricezione dei messaggi

#### <a name="receive-messages-from-a-service-bus-queue"></a>Ricevere messaggi da una coda del bus di servizio

1. Usare un editor di testo per creare un file Java denominato *QueueReceiveController.java* nella directory del pacchetto dell'app

1. Aggiungere al nuovo file il codice seguente:

    ```java
    package com.wingtiptoys.servicebus;

    import org.slf4j.Logger;
    import org.slf4j.LoggerFactory;
    import org.springframework.jms.annotation.JmsListener;
    import org.springframework.stereotype.Component;

    @Component
    public class QueueReceiveController {

        private static final String QUEUE_NAME = "<ServiceBusQueueName>";

        private final Logger logger = LoggerFactory.getLogger(QueueReceiveController.class);

        @JmsListener(destination = QUEUE_NAME, containerFactory = "jmsListenerContainerFactory")
        public void receiveMessage(User user) {
            logger.info("Received message: {}", user.getName());
        }
    }
    ```

    > [!NOTE]
    > Sostituire `<ServiceBusQueueName>` con il nome della coda configurato nello spazio dei nomi del bus di servizio.

1. Salvare e chiudere il file *QueueReceiveController.java*.

#### <a name="receive-messages-from-a-service-bus-subscription"></a>Ricevere messaggi da una sottoscrizione del bus di servizio

1. Usare un editor di testo per creare un file Java denominato *TopicReceiveController.java* nella directory del pacchetto dell'app. 

1. Aggiungere il codice seguente nel nuovo file. Sostituire il segnaposto `<ServiceBusTopicName>` con il nome dell'argomento configurato nello spazio dei nomi del bus di servizio. Sostituire il segnaposto `<ServiceBusSubscriptionName>` con il nome della sottoscrizione per l'argomento del bus di servizio.

    ```java
    package com.wingtiptoys.servicebus;

    import org.slf4j.Logger;
    import org.slf4j.LoggerFactory;
    import org.springframework.jms.annotation.JmsListener;
    import org.springframework.stereotype.Component;

    @Component
    public class TopicReceiveController {

        private static final String TOPIC_NAME = "<ServiceBusTopicName>";

        private static final String SUBSCRIPTION_NAME = "<ServiceBusSubscriptionName>";

        private final Logger logger = LoggerFactory.getLogger(TopicReceiveController.class);

        @JmsListener(destination = TOPIC_NAME, containerFactory = "topicJmsListenerContainerFactory",
                subscription = SUBSCRIPTION_NAME)
        public void receiveMessage(User user) {
            logger.info("Received message: {}", user.getName());
        }
    }
    ```

1. Salvare e chiudere il file *TopicReceiveController.java*.

## <a name="build-and-test-your-application"></a>Compilare e testare l'applicazione

1. Aprire il prompt dei comandi e cambiare directory passando alla posizione in cui si trova il file *pom.xml*, ad esempio:

    `cd C:\SpringBoot\servicebus`

    -oppure-

    `cd cd /users/example/home/servicebus`

1. Compilare l'applicazione Spring Boot con Maven ed eseguirla:

    ```shell
    mvn clean spring-boot:run
    ```

1. Quando l'applicazione è in esecuzione, è possibile testarla usando *curl*:

    ```shell
    curl -X POST localhost:8080/messages?message=hello
    ```

    Nel registro applicazioni dovrebbero essere visibili "Sending message" e "hello":

    ```shell
    [nio-8080-exec-1] com.wingtiptoys.servicebus.SendController : Sending message
    [enerContainer-1] com.wingtiptoys.servicebus.ReceiveController : Received message: hello
    ```

## <a name="clean-up-resources"></a>Pulire le risorse

Quando le risorse create in questo articolo non sono più necessarie, usare il [portale di Azure](https://portal.azure.com/) per eliminarle in modo da evitare addebiti imprevisti.

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"]
> [Come usare l'API JMS con il bus di servizio e AMQP 1.0](/azure/service-bus-messaging/service-bus-java-how-to-use-jms-api-amqp)