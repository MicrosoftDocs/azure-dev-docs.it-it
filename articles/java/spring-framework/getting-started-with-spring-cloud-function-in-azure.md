---
title: Introduzione a Spring Cloud Function in Azure
description: Informazioni sull'uso di Spring Cloud Function in Azure.
documentationcenter: java
author: jdubois
manager: brborges
ms.author: judubois
ms.date: 07/17/2019
ms.service: azure-functions
ms.tgt_pltfrm: multiple
ms.topic: article
ms.custom: devx-track-java
ms.openlocfilehash: f4fa9df0edf0fbb6b748f9bf3010be9115ad1ffd
ms.sourcegitcommit: d5dabc6dde727ed167a9dc8a4eaaf21025b3efa8
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/12/2020
ms.locfileid: "91947526"
---
# <a name="getting-started-with-spring-cloud-function-in-azure"></a>Introduzione a Spring Cloud Function in Azure

Questo articolo illustra come usare [Spring Cloud Function](https://spring.io/projects/spring-cloud-function) per sviluppare una funzione Java e pubblicarla in Funzioni di Azure. Al termine, il codice della funzione viene eseguito nel [piano a consumo](/azure/azure-functions/functions-scale#consumption-plan) in Azure e può essere attivato usando una richiesta HTTP.

[!INCLUDE [quickstarts-free-trial-note](includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Prerequisites

Per sviluppare funzioni con Java, è necessario che siano installati gli elementi seguenti:

- [Java Developer Kit](../fundamentals/java-jdk-long-term-support.md), versione 8
- [Apache Maven](https://maven.apache.org), versione 3.0 o successive
- [Interfaccia della riga di comando di Azure](/cli/azure)
- [Azure Functions Core Tools](/azure/azure-functions/functions-run-local#v2) versione 2.7.1158 o successive

> [!IMPORTANT]
> Per completare questa guida introduttiva, è necessario impostare la variabile di ambiente JAVA_HOME sul percorso di installazione di JDK.

## <a name="what-we-are-going-to-build"></a>Risultato finale

Verrà creata una funzione "Hello, World" classica, che viene eseguita in Funzioni di Azure ed è configurata con Spring Cloud Function.

La funzione riceverà un oggetto JSON `User` semplice, contenente un nome utente, e restituirà un oggetto `Greeting`, con il messaggio di benvenuto per l'utente.

Il progetto che verrà creato qui è disponibile in [https://github.com/Azure-Samples/hello-spring-function-azure](https://github.com/Azure-Samples/hello-spring-function-azure). È quindi possibile usare direttamente tale repository di esempio per visualizzare il risultato finale delle operazioni descritte in dettaglio in questo argomento di avvio rapido.

## <a name="create-a-new-maven-project"></a>Creare un nuovo progetto Maven

Verrà creato un progetto Maven vuoto che sarà configurato con Spring Cloud Function e Funzioni di Azure.

In una cartella vuota creare un nuovo file *pom.xml* e quindi copiare e incollare il contenuto dal progetto di esempio disponibile in [https://github.com/Azure-Samples/hello-spring-function-azure/blob/master/pom.xml](https://github.com/Azure-Samples/hello-spring-function-azure/blob/master/pom.xml).

> [!NOTE]
> Questo file usa le dipendenze Maven di Spring Boot e Spring Cloud Function e configura i plug-in Maven di Spring Boot e Funzioni di Azure.

Per l'applicazione è necessario personalizzare alcune proprietà:

- `<functionAppName>` è il nome della funzione di Azure
- `<functionAppRegion>` è il nome dell'area di Azure in cui è distribuita la funzione
- `<functionResourceGroup>` è il nome del gruppo di risorse di Azure che si sta usando

È necessario modificare queste proprietà direttamente nella parte superiore del file *pom.xml*:

```xml
<properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
    <azure.functions.maven.plugin.version>1.9.0</azure.functions.maven.plugin.version>
    <functionAppName>my-spring-function</functionAppName>
    <functionAppRegion>westus</functionAppRegion>
    <stagingDirectory>${project.build.directory}/azure-functions/${functionAppName}</stagingDirectory>
    <functionResourceGroup>my-resource-group</functionResourceGroup>
    <start-class>com.example.HelloFunction</start-class>
    <spring.boot.wrapper.version>1.0.25.RELEASE</spring.boot.wrapper.version>
</properties>
```

## <a name="create-azure-configuration-files"></a>Creare i file di configurazione di Azure

Creare una cartella *src/main/azure* e aggiungervi i file di configurazione di Funzioni di Azure seguenti.

*host.json*:

```json
{
  "version": "2.0",
  "functionTimeout": "00:10:00"
}
```

*local.settings.json*:

```json
{
  "IsEncrypted": false,
  "Values": {
    "AzureWebJobsStorage": "",
    "FUNCTIONS_WORKER_RUNTIME": "java",
    "AzureWebJobsDashboard": ""
  }
}
```

## <a name="create-domain-objects"></a>Creare gli oggetti di dominio

Funzioni di Azure può ricevere e inviare oggetti in formato JSON.
Verranno ora creati gli oggetti `User` e `Greeting` che rappresentano il modello di dominio.
È possibile creare oggetti più complessi, con un numero maggiore di proprietà, se si vuole personalizzare questo avvio rapido e renderlo più interessante.

Creare una cartella *src/main/java/com/example/model* e aggiungere i due file seguenti:

*User.java*:

```java
package com.example.model;

public class User {

    public User() {
    }

    public User(String name) {
        this.name = name;
    }

    private String name;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```

*Greeting.java*:

```java
package com.example.model;

public class Greeting {

    public Greeting() {
    }

    public Greeting(String message) {
        this.message = message;
    }

    private String message;

    public String getMessage() {
        return message;
    }

    public void setMessage(String message) {
        this.message = message;
    }
}
```

## <a name="create-the-spring-boot-application"></a>Creare l'applicazione Spring Boot

Questa applicazione gestirà tutta la logica di business e avrà accesso all'ecosistema completo di Spring Boot.
L'applicazione offre quindi due vantaggi principali rispetto a una funzione standard di Azure:

- Non si basa sulle API di Funzioni di Azure e pertanto può essere trasferita facilmente in altri sistemi. Può ad esempio essere riutilizzata in una normale applicazione Spring Boot.
- Può usare tutte le annotazioni `@Enable` di Spring Boot per aggiungere facilmente nuove funzionalità avanzate.

Nella cartella *src/main/java/com/example* creare il file seguente, che è una normale applicazione Spring Boot:

*HelloFunction.java*:

```java
package com.example;

import com.example.model.Greeting;
import com.example.model.User;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.annotation.Bean;

import java.util.function.Function;

@SpringBootApplication
public class HelloFunction {

    public static void main(String[] args) throws Exception {
        SpringApplication.run(HelloFunction.class, args);
    }

    @Bean
    public Function<User, Greeting> hello() {
        return user -> new Greeting("Welcome, " + user.getName());
    }
}
```

> [!NOTE] 
> La funzione `hello()` è piuttosto specifica:
> 
> - Restituisce una funzione `java.util.function.Function`, ovvero la funzione che verrà usata in questo avvio rapido. Contiene la logica di business e usa un'API Java standard per trasformare un oggetto in un altro.
> - Poiché ha l'annotazione `@Bean`, è uno Spring Bean e, per impostazione predefinita, il suo nome corrisponde a quello del metodo, ovvero `hello`. Questo è importante se si vogliono creare altre funzioni nell'applicazione, poiché questo nome deve corrispondere al nome della funzione di Azure che verrà creata nella sezione successiva.

## <a name="create-the-azure-function"></a>Creare la funzione di Azure

Per sfruttare l'API di Funzioni di Azure completa, verrà ora codificata una classe specifica, ovvero una funzione di Azure di cui verrà delegata l'esecuzione alla funzione Spring Cloud creata nel passaggio precedente.

Nella cartella *src/main/java/com/example* creare la funzione di Azure seguente:

*HelloHandler.java*:

```java
package com.example;

import com.example.model.Greeting;
import com.example.model.User;
import com.microsoft.azure.functions.*;
import com.microsoft.azure.functions.annotation.AuthorizationLevel;
import com.microsoft.azure.functions.annotation.FunctionName;
import com.microsoft.azure.functions.annotation.HttpTrigger;
import org.springframework.cloud.function.adapter.azure.AzureSpringBootRequestHandler;

import java.util.Optional;

public class HelloHandler extends AzureSpringBootRequestHandler<User, Greeting> {

    @FunctionName("hello")
    public HttpResponseMessage execute(
            @HttpTrigger(name = "request", methods = {HttpMethod.GET, HttpMethod.POST}, authLevel = AuthorizationLevel.ANONYMOUS) HttpRequestMessage<Optional<User>> request,
            ExecutionContext context) {

        context.getLogger().info("Greeting user name: " + request.getBody().get().getName());
        return request
                .createResponseBuilder(HttpStatus.OK)
                .body(handleRequest(request.getBody().get(), context))
                .header("Content-Type", "application/json")
                .build();
    }
}
```

Questa classe Java è una funzione di Azure con interessanti caratteristiche, come descritto di seguito:

- Estende `AzureSpringBootRequestHandler`, che stabilisce il collegamento tra Funzioni di Azure e Spring Cloud Function. Questo viene fornito dal metodo `handleRequest()` usato nel metodo `execute()` corrispondente.
- Il nome della funzione, come definito dall'annotazione `@FunctionName("hello")`, corrisponde a quello dello Spring Bean configurato nel passaggio precedente, `hello`.
- Si tratta di una funzione di Azure reale ed è pertanto possibile usare qui l'API di Funzioni di Azure completa.

## <a name="add-unit-tests"></a>Aggiungere unit test

Naturalmente questo passaggio è facoltativo, ma per un risultato ottimale è opportuno aggiungere unit test per verificare che l'applicazione funzioni correttamente.

Creare una cartella *src/test/java/com/example* e aggiungere i due test JUnit seguenti:

*HelloFunctionTest.java*:

```java
package com.example;

import com.example.model.Greeting;
import com.example.model.User;
import org.junit.Test;
import org.springframework.cloud.function.adapter.azure.AzureSpringBootRequestHandler;

import static org.assertj.core.api.Assertions.assertThat;

public class HelloFunctionTest {

    @Test
    public void test() {
        Greeting result = new HelloFunction().hello().apply(new User("foo"));
        assertThat(result.getMessage()).isEqualTo("Welcome, foo");
    }

    @Test
    public void start() throws Exception {
        AzureSpringBootRequestHandler<User, Greeting> handler = new AzureSpringBootRequestHandler<>(
                HelloFunction.class);
        Greeting result = handler.handleRequest(new User("foo"), null);
        handler.close();
        assertThat(result.getMessage()).isEqualTo("Welcome, foo");
    }
}
```

È ora possibile testare la funzione di Azure con Maven:

```bash
mvn clean test
```

## <a name="run-the-function-locally"></a>Eseguire la funzione in locale

Prima di distribuire l'applicazione in Funzioni di Azure, è opportuno eseguirne il test in locale.

Per prima cosa è necessario creare il pacchetto dell'applicazione in un file con estensione jar:

```bash
mvn package
```

Ora che l'applicazione è assemblata in un pacchetto, è possibile eseguirla usando il plug-in `azure-functions` di Maven:

```bash
mvn azure-functions:run
```

La funzione di Azure dovrebbe ora essere disponibile nel localhost, sulla porta 7071. È possibile testare la funzione inviando una richiesta POST, con un oggetto `User` in formato JSON. È ad esempio possibile usare cURL:

```bash
curl http://localhost:7071/api/hello -d "{\"name\":\"Azure\"}"
```

La funzione dovrebbe rispondere con un oggetto `Greeting`, sempre in formato JSON:

```Output
{
  "message": "Welcome, Azure"
}
```

Ecco uno screenshot della richiesta cURL nella parte superiore della schermata e della funzione di Azure locale nella parte inferiore:

 ![Esecuzione in locale della funzione di Azure][RFL01]

## <a name="deploy-the-function-to-azure-functions"></a>Distribuire la funzione in Funzioni di Azure

La funzione di Azure verrà ora pubblicata nell'ambiente di produzione. Tenere presente che per configurare la funzione verranno usate le proprietà `<functionAppName>`, `<functionAppRegion>` e `<functionResourceGroup>` definite nel file *pom.xml*.

> [!NOTE]
> Se è installata l'interfaccia della riga di comando di Azure, il plug-in Maven deve eseguire l'autenticazione con Azure, quindi usare `az login` prima di continuare.
> Fare clic [qui](https://github.com/microsoft/azure-maven-plugins/wiki/Authentication) per altre opzioni di autenticazione.

Eseguire Maven per distribuire automaticamente la funzione:

```bash
mvn azure-functions:deploy
```

Passare ora al [portale di Azure](https://portal.azure.com) per trovare la risorsa `Function App` che è stata creata.

Fare clic sulla funzione:

- Nella panoramica della funzione prendere nota del relativo URL.
- Selezionare la scheda **Funzionalità della piattaforma**  per trovare il servizio **Flusso di registrazione** e quindi selezionare il servizio per controllare la funzione in esecuzione.

A questo punto, come nella sezione precedente, usare cURL per accedere alla funzione in esecuzione. Sostituire `your-function-name` con il nome effettivo della funzione:

```bash
curl https://your-function-name.azurewebsites.net/api/hello -d "{\"name\":\"Azure\"}"
```

Come nella sezione precedente, la funzione dovrebbe rispondere con un oggetto `Greeting`, sempre in formato JSON:

```Output
{
  "message": "Welcome, Azure"
}
```

A questo punto è stata creata una funzione Spring Cloud che viene eseguita in Funzioni di Azure.

<!-- IMG List -->

[RFL01]: media/getting-started-with-spring-cloud-function-in-azure/RFL01.png
