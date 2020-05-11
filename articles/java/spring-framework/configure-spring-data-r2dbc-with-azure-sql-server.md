---
title: Usare Spring Data R2DBC con il database SQL di Azure
description: Informazioni su come usare Spring Data R2DBC con un database SQL di Azure.
documentationcenter: java
ms.date: 04/28/2020
ms.service: sql-database
ms.tgt_pltfrm: multiple
ms.author: judubois
ms.topic: article
ms.openlocfilehash: 80ccbbc84e4d23ff9083777f38615eb5d676e484
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/05/2020
ms.locfileid: "82766160"
---
# <a name="use-spring-data-r2dbc-with-azure-sql-database"></a>Usare Spring Data R2DBC con il database SQL di Azure

Questo argomento illustra come creare un'applicazione di esempio che usa [Spring Data R2DBC](https://spring.io/projects/spring-data-r2dbc) per archiviare e recuperare informazioni nel [database SQL di Azure](https://docs.microsoft.com/azure/sql-database/) con l'implementazione R2DBC per Microsoft SQL Server del [repository GitHub r2dbc-mssql](https://github.com/r2dbc/r2dbc-mssql).

[R2DBC](https://r2dbc.io/) introduce le API reattive nei database relazionali tradizionali. È possibile usarlo con Spring WebFlux per creare applicazioni Spring Boot completamente reattive che usano API non bloccanti. Offre una migliore scalabilità rispetto all'approccio classico di "un thread per connessione".

## <a name="prerequisites"></a>Prerequisiti

- Un account Azure. Se non è disponibile, [ottenere una versione di valutazione gratuita](https://azure.microsoft.com/free/).
- [Azure Cloud Shell](/azure/cloud-shell/quickstart) o [interfaccia della riga di comando di Azure](/cli/azure/install-azure-cli). È consigliabile usare Azure Cloud Shell in modo che l'accesso venga eseguito automaticamente e che sia possibile accedere a tutti gli strumenti necessari.
- [Java 8](https://www.azul.com/downloads/zulu/) (incluso in Azure Cloud Shell).
- [cURL](https://curl.haxx.se) o utilità HTTP simile per testare la funzionalità.

## <a name="prepare-the-working-environment"></a>Preparare l'ambiente di lavoro

Prima di tutto, configurare alcune variabili di ambiente usando i comandi seguenti:

```bash
AZ_RESOURCE_GROUP=r2dbc-workshop
AZ_DATABASE_NAME=<YOUR_DATABASE_NAME>
AZ_LOCATION=<YOUR_AZURE_REGION>
AZ_SQL_SERVER_USERNAME=r2dbc
AZ_SQL_SERVER_PASSWORD=<YOUR_AZURE_SQL_PASSWORD>
AZ_LOCAL_IP_ADDRESS=<YOUR_LOCAL_IP_ADDRESS>
```

Sostituire i segnaposto con i valori seguenti, che vengono usati nell'intero articolo:

- `<YOUR_DATABASE_NAME>`: il nome del server di database SQL di Azure. Deve essere univoco in Azure.
- `<YOUR_AZURE_REGION>`: l'area di Azure da usare. È possibile usare `eastus` per impostazione predefinita, ma è consigliabile configurare un'area più vicina a dove si risiede. Per l'elenco completo di aree disponibili, immettere `az account list-locations`.
- `<AZ_SQL_SERVER_PASSWORD>`: la password del server di database SQL di Azure. La password deve essere composta da un minimo di otto caratteri di tre categorie seguenti: lettere maiuscole, lettere minuscole, numeri (0-9) e caratteri non alfanumerici (!, $, #, % e così via).
- `<YOUR_LOCAL_IP_ADDRESS>`: l'indirizzo IP del computer locale, da cui verrà eseguita l'applicazione Spring Boot. Un modo pratico per trovarlo è puntare il browser all'indirizzo [whatismyip.akamai.com](http://whatismyip.akamai.com/).

Successivamente, creare un gruppo di risorse:

```azurecli
az group create \
    --name $AZ_RESOURCE_GROUP \
    --location $AZ_LOCATION \
    | jq
```

> [!NOTE]
> Per visualizzare i dati JSON e renderli più leggibili, viene usata l'utilità `jq`, installata per impostazione predefinita in [Azure Cloud Shell](https://shell.azure.com/).
> Se non si preferisce usare questa utilità, è possibile rimuovere tranquillamente la parte `| jq` di tutti i comandi che verranno usati.

## <a name="create-an-azure-sql-database-instance"></a>Creare un'istanza del database SQL di Azure

Il primo componente che verrà creato è un server di database SQL di Azure gestito.

> [!NOTE]
> Per informazioni più dettagliate sulla creazione di server di database SQL di Azure, vedere [Avvio rapido: Creare un database SQL di Azure singolo](/azure/sql-database/sql-database-single-database-get-started).

In [Azure Cloud Shell](https://shell.azure.com/) eseguire lo script seguente:

```azurecli
az sql server create \
    --resource-group $AZ_RESOURCE_GROUP \
    --name $AZ_DATABASE_NAME \
    --location $AZ_LOCATION \
    --admin-user $AZ_SQL_SERVER_USERNAME \
    --admin-password $AZ_SQL_SERVER_PASSWORD \
    | jq
```

Questo comando crea un database SQL di Azure.

### <a name="configure-a-firewall-rule-for-your-azure-sql-database-server"></a>Configurare una regola del firewall per il server di database SQL di Azure

Le istanze del database SQL di Azure sono protette per impostazione predefinita. Includono un firewall che non consente alcuna connessione in ingresso. Per poter usare il database, è necessario aggiungere una regola del firewall che consenta all'indirizzo IP locale di accedere al server di database.

Poiché l'indirizzo IP locale è stato configurato all'inizio di questo articolo, è possibile aprire il firewall del server eseguendo:

```azurecli
az sql server firewall-rule create \
    --resource-group $AZ_RESOURCE_GROUP \
    --name $AZ_DATABASE_NAME-database-allow-local-ip \
    --server $AZ_DATABASE_NAME \
    --start-ip-address $AZ_LOCAL_IP_ADDRESS \
    --end-ip-address $AZ_LOCAL_IP_ADDRESS \
    | jq
```

### <a name="configure-a-azure-sql-database"></a>Configurare un database SQL di Azure

Il database SQL di Azure creato in precedenza è vuoto. Non include nessun database che è possibile usare con l'applicazione Spring Boot. Creare un nuovo database denominato `r2dbc`:

```azurecli
az sql db create \
    --resource-group $AZ_RESOURCE_GROUP \
    --name r2dbc \
    --server $AZ_DATABASE_NAME \
    | jq
```

## <a name="create-a-reactive-spring-boot-application"></a>Creare un'applicazione Spring Boot reattiva

Per creare un'applicazione Spring Boot reattiva, si userà [Spring Initializr](https://start.spring.io/). L'applicazione che verrà creata usa:

- Spring Boot 2.3.0 M4.
- Java 8 (ma funziona anche con versioni più recenti come Java 11).
- Le dipendenze seguenti: Spring Reactive Web (anche nota come Spring WebFlux) e Spring data R2DBC.

### <a name="generate-the-application-by-using-spring-initializr"></a>Generare l'applicazione con Spring Initializr

Per generare l'applicazione, immettere quanto segue sulla riga di comando:

```bash
curl https://start.spring.io/starter.tgz -d dependencies=webflux,data-r2dbc -d baseDir=azure-r2dbc-workshop -d bootVersion=2.3.0.M4 -d javaVersion=8 | tar -xzvf -
```

### <a name="add-the-reactive-azure-sql-database-driver-implementation"></a>Aggiungere l'implementazione del driver del database SQL di Azure reattivo

Aprire il file *pom.xml* del progetto generato per aggiungere il driver del database SQL di Azure reattivo dal [repository GitHub r2dbc-mssql](https://github.com/r2dbc/r2dbc-mssql).

Dopo la dipendenza `spring-boot-starter-webflux` aggiungere il frammento di codice seguente:

```xml
<dependency>
    <groupId>io.r2dbc</groupId>
    <artifactId>r2dbc-mssql</artifactId>
    <scope>runtime</scope>
</dependency>
```

### <a name="configure-spring-boot-to-use-azure-sql-database"></a>Configurare Spring Boot per l'uso del database SQL di Azure

Aprire il file *src/main/resources/application.properties* e aggiungere:

```properties
logging.level.org.springframework.data.r2dbc=DEBUG

spring.r2dbc.url=r2dbc:pool:mssql://$AZ_DATABASE_NAME.database.windows.net:1433/r2dbc
spring.r2dbc.username=r2dbc@$AZ_DATABASE_NAME
spring.r2dbc.password=$AZ_SQL_SERVER_PASSWORD
```

- Sostituire le due variabili `$AZ_DATABASE_NAME` con il valore configurato all'inizio di questo articolo.
- Sostituire la variabile `$AZ_SQL_SERVER_PASSWORD` con il valore configurato all'inizio di questo articolo.

> [!NOTE]
> Per prestazioni più elevate, la proprietà `spring.r2dbc.url` viene configurata per l'uso di un pool di connessioni tramite [r2dbc-pool](https://github.com/r2dbc/r2dbc-pool).

A questo punto dovrebbe essere possibile avviare l'applicazione usando il wrapper Maven fornito:

```bash
./mvnw spring-boot:run
```

Ecco uno screenshot dell'applicazione in esecuzione per la prima volta:

[![Applicazione in esecuzione](media/configure-spring-data-r2dbc-with-azure-azure-sql/create-azure-sql-01.png)](media/configure-spring-data-r2dbc-with-azure-azure-sql/create-azure-sql-01.png#lightbox)

### <a name="create-the-database-schema"></a>Creare lo schema del database

All'interno della classe `DemoApplication` principale configurare un nuovo bean Spring che creerà uno schema del database:

```java
    @Bean
    public ConnectionFactoryInitializer initializer(ConnectionFactory connectionFactory) {
        ConnectionFactoryInitializer initializer = new ConnectionFactoryInitializer();
        initializer.setConnectionFactory(connectionFactory);
        ResourceDatabasePopulator populator = new ResourceDatabasePopulator(new ClassPathResource("schema.sql"));
        initializer.setDatabasePopulator(populator);
        return initializer;
    }
```

Questo bean Spring usa un file denominato *schema.sql*, quindi creare il file nella cartella *src/main/resources*:

```sql
DROP TABLE IF EXISTS todo;
CREATE TABLE todo (id INT IDENTITY PRIMARY KEY, description VARCHAR(255), details VARCHAR(4096), done BIT);
```

Usare il comando seguente per arrestare e rieseguire l'applicazione. L'applicazione userà ora il database `r2dbc` creato in precedenza e creerà una `todo` tabella al suo interno.

```bash
./mvnw spring-boot:run
```

Ecco uno screenshot della tabella di database creata:

[![Creazione della tabella di database](media/configure-spring-data-r2dbc-with-azure-azure-sql/create-azure-sql-02.png)](media/configure-spring-data-r2dbc-with-azure-azure-sql/create-azure-sql-02.png#lightbox)

## <a name="code-the-application"></a>Codice dell'applicazione

Aggiungere quindi il codice Java che userà R2DBC per archiviare e recuperare i dati dal server di database SQL di Azure.

Creare una nuova classe Java `Todo` accanto alla classe `DemoApplication`:

```java
package com.example.demo;

import org.springframework.data.annotation.Id;

public class Todo {

    public Todo() {
    }

    public Todo(String description, String details, boolean done) {
        this.description = description;
        this.details = details;
        this.done = done;
    }

    @Id
    private Long id;

    private String description;

    private String details;

    private boolean done;

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getDescription() {
        return description;
    }

    public void setDescription(String description) {
        this.description = description;
    }

    public String getDetails() {
        return details;
    }

    public void setDetails(String details) {
        this.details = details;
    }

    public boolean isDone() {
        return done;
    }

    public void setDone(boolean done) {
        this.done = done;
    }
}
```

Questa classe è un modello di dominio mappato alla tabella `todo` creata in precedenza.

Per gestire tale classe, è necessario un repository. Definire una nuova interfaccia `TodoRepository` nello stesso pacchetto:

```java
package com.example.demo;

import org.springframework.data.repository.reactive.ReactiveCrudRepository;

public interface TodoRepository extends ReactiveCrudRepository<Todo, Long> {
}
```

Questo repository è un repository reattivo gestito da Spring Data R2DBC.

Completare l'applicazione creando un controller in grado di archiviare e recuperare dati. Implementare una classe `TodoController` nello stesso pacchetto e aggiungere il codice seguente:

```java
package com.example.demo;

import org.springframework.http.HttpStatus;
import org.springframework.web.bind.annotation.*;
import reactor.core.publisher.Flux;
import reactor.core.publisher.Mono;

@RestController
@RequestMapping("/")
public class TodoController {

    private final TodoRepository todoRepository;

    public TodoController(TodoRepository todoRepository) {
        this.todoRepository = todoRepository;
    }

    @PostMapping("/")
    @ResponseStatus(HttpStatus.CREATED)
    public Mono<Todo> createTodo(@RequestBody Todo todo) {
        return todoRepository.save(todo);
    }

    @GetMapping("/")
    public Flux<Todo> getTodos() {
        return todoRepository.findAll();
    }
}
```

Infine, arrestare l'applicazione e riavviarla:

```bash
./mvnw spring-boot:run
```

## <a name="test-the-application"></a>Test dell'applicazione

Per testare l'applicazione, è possibile usare cURL.

Creare prima di tutto un elemento "todo" nel database:

```bash
curl  --header "Content-Type: application/json" \
          --request POST \
          --data '{"description":"configuration","details":"congratulations, you have set up R2DBC correctly!","done": "true"}' \
          http://127.0.0.1:8080
```

Questo comando restituirà l'elemento creato:

```json
{"id":1,"description":"configuration","details":"congratulations, you have set up R2DBC correctly!","done":true}
```

Recuperare quindi i dati usando una nuova richiesta cURL:

```bash
curl http://127.0.0.1:8080
```

Questo comando restituirà l'elenco di elementi "todo", incluso quello creato:

```json
[{"id":1,"description":"configuration","details":"congratulations, you have set up R2DBC correctly!","done":true}]
```

Ecco uno screenshot di queste richieste cURL:

[![Eseguire il test con cURL](media/configure-spring-data-r2dbc-with-azure-azure-sql/create-azure-sql-03.png)](media/configure-spring-data-r2dbc-with-azure-azure-sql/create-azure-sql-03.png#lightbox)

Congratulazioni! È stata creata un'applicazione Spring Boot completamente reattiva che usa R2DBC per archiviare e recuperare i dati dal database SQL di Azure.

## <a name="clean-up-resources"></a>Pulire le risorse

Per pulire tutte le risorse usate in questo argomento di avvio rapido, eliminare il gruppo di risorse:

```azurecli
az group delete \
    --name $AZ_RESOURCE_GROUP \
    --yes
```

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni su Spring e Azure, passare al centro di documentazione di Spring in Azure.

> [!div class="nextstepaction"]
> [Spring in Azure](/azure/developer/java/spring-framework)

### <a name="additional-resources"></a>Risorse aggiuntive

Per altre informazioni su Spring Data R2DBC, vedere la [documentazione di riferimento](https://docs.spring.io/spring-data/r2dbc/docs/1.0.x/reference/html/#reference) di Spring.

Per altre informazioni sull'uso di Azure con Java, vedere [Azure per sviluppatori Java](/azure/developer/java/) e la documentazione relativa all'[uso di Azure DevOps e Java](/azure/devops/).