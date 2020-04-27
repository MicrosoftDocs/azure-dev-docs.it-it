---
title: Usare Spring Data R2DBC con Database di Azure per MySQL
description: Informazioni su come usare Spring Data R2DBC con un database di Azure per MySQL.
documentationcenter: java
ms.date: 03/18/2020
ms.service: mysql
ms.tgt_pltfrm: multiple
ms.author: judubois
ms.topic: article
ms.openlocfilehash: 62c151ba4c09b348c241658df985e1489908299f
ms.sourcegitcommit: 0af39ee9ff27c37ceeeb28ea9d51e32995989591
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/21/2020
ms.locfileid: "81668547"
---
# <a name="use-spring-data-r2dbc-with-azure-database-for-mysql"></a>Usare Spring Data R2DBC con Database di Azure per MySQL

Questo argomento illustra come creare un'applicazione di esempio che usa [Spring Data R2DBC](https://spring.io/projects/spring-data-r2dbc) per archiviare e recuperare informazioni in [Database di Azure per MySQL](https://docs.microsoft.com/azure/mysql/) con l'implementazione R2DBC per MySQL del [repository GitHub r2dbc-mysql](https://github.com/mirromutth/r2dbc-mysql).

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
AZ_MYSQL_USERNAME=r2dbc
AZ_MYSQL_PASSWORD=<YOUR_MYSQL_PASSWORD>
AZ_LOCAL_IP_ADDRESS=<YOUR_LOCAL_IP_ADDRESS>
```

Sostituire i segnaposto con i valori seguenti, che vengono usati nell'intero articolo:

- `<YOUR_DATABASE_NAME>`: il nome del server MySQL. Deve essere univoco in Azure.
- `<YOUR_AZURE_REGION>`: l'area di Azure da usare. È possibile usare `eastus` per impostazione predefinita, ma è consigliabile configurare un'area più vicina a dove si risiede. Per l'elenco completo di aree disponibili, immettere `az account list-locations`.
- `<YOUR_MYSQL_PASSWORD>`: la password del server di database MySQL. La password deve essere composta da un minimo di otto caratteri di tre categorie seguenti: lettere maiuscole, lettere minuscole, numeri (0-9) e caratteri non alfanumerici (!, $, #, % e così via).
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

## <a name="create-an-azure-database-for-mysql-instance"></a>Creare un'istanza di Database di Azure per MySQL

Il primo componente che verrà creato è un server MySQL gestito.

> [!NOTE]
> Per informazioni più dettagliate sulla creazione di server MySQL, vedere [Creare un server di Database di Azure per MySQL nel portale di Azure](/azure/mysql/quickstart-create-mysql-server-database-using-azure-portal).

In [Azure Cloud Shell](https://shell.azure.com/) eseguire lo script seguente:

```azurecli
az mysql server create \
    --resource-group $AZ_RESOURCE_GROUP \
    --name $AZ_DATABASE_NAME \
    --location $AZ_LOCATION \
    --sku-name B_Gen5_1 \
    --storage-size 5120 \
    --admin-user $AZ_MYSQL_USERNAME \
    --admin-password $AZ_MYSQL_PASSWORD \
    | jq
```

Questo comando crea un piccolo server MySQL.

### <a name="configure-a-firewall-rule-for-your-mysql-server"></a>Configurare una regola del firewall per il server MySQL

Le istanze di Database di Azure per MySQL sono protette per impostazione predefinita. Includono un firewall che non consente alcuna connessione in ingresso. Per poter usare il database, è necessario aggiungere una regola del firewall che consenta all'indirizzo IP locale di accedere al server di database.

Poiché l'indirizzo IP locale è stato configurato all'inizio di questo articolo, è possibile aprire il firewall del server eseguendo:

```azurecli
az mysql server firewall-rule create \
    --resource-group $AZ_RESOURCE_GROUP \
    --name $AZ_DATABASE_NAME-database-allow-local-ip \
    --server $AZ_DATABASE_NAME \
    --start-ip-address $AZ_LOCAL_IP_ADDRESS \
    --end-ip-address $AZ_LOCAL_IP_ADDRESS \
    | jq
```

### <a name="configure-a-mysql-database"></a>Configurare un database MySQL

Il server MySQL creato in precedenza è vuoto. Non include nessun database che è possibile usare con l'applicazione Spring Boot. Creare un nuovo database denominato `r2dbc`:

```azurecli
az mysql db create \
    --resource-group $AZ_RESOURCE_GROUP \
    --name r2dbc \
    --server-name $AZ_DATABASE_NAME \
    | jq
```

## <a name="create-a-reactive-spring-boot-application"></a>Creare un'applicazione Spring Boot reattiva

Per creare un'applicazione Spring Boot reattiva, si userà [Spring Initializr](https://start.spring.io/). L'applicazione che verrà creata usa:

- Spring Boot 2.3.0 M3.
- Java 8 (ma funziona anche con versioni più recenti come Java 11).
- Le dipendenze seguenti: Spring Reactive Web (anche nota come Spring WebFlux) e Spring data R2DBC.

### <a name="generate-the-application-by-using-spring-initializr"></a>Generare l'applicazione con Spring Initializr

Per generare l'applicazione, immettere quanto segue sulla riga di comando:

```bash
curl https://start.spring.io/starter.tgz -d dependencies=webflux,data-r2dbc -d baseDir=azure-r2dbc-workshop -d bootVersion=2.3.0.M3 -d javaVersion=8 | tar -xzvf -
```

### <a name="add-the-reactive-mysql-driver-implementation"></a>Aggiungere l'implementazione del driver MySQL reattivo

Aprire il file *pom.xml* del progetto generato per aggiungere il driver MySQL reattivo dal [repository GitHub r2dbc-mysql](https://github.com/mirromutth/r2dbc-mysql).

Dopo la dipendenza `spring-boot-starter-webflux` aggiungere il frammento di codice seguente:

```xml
<dependency>
   <groupId>dev.miku</groupId>
   <artifactId>r2dbc-mysql</artifactId>
   <version>0.8.1.RELEASE</version>
   <scope>runtime</scope>
</dependency>
```

### <a name="configure-spring-boot-to-use-azure-database-for-mysql"></a>Configurare Spring Boot per l'uso di Database di Azure per MySQL

Aprire il file *src/main/resources/application.properties* e aggiungere:

```properties
logging.level.org.springframework.data.r2dbc=DEBUG

spring.r2dbc.url=r2dbc:mysql://$AZ_DATABASE_NAME.mysql.database.azure.com:3306/r2dbc
spring.r2dbc.username=r2dbc@$AZ_DATABASE_NAME
spring.r2dbc.password=$AZ_MYSQL_USERNAME
```

- Sostituire le due variabili `$AZ_DATABASE_NAME` con il valore configurato all'inizio di questo articolo.
- Sostituire la variabile `$AZ_MYSQL_USERNAME` con il valore configurato all'inizio di questo articolo.

A questo punto dovrebbe essere possibile avviare l'applicazione usando il wrapper Maven fornito:

```bash
./mvnw spring-boot:run
```

Ecco uno screenshot dell'applicazione in esecuzione per la prima volta:

![L'applicazione in esecuzione][R2DBC-MYSQL01]

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
CREATE TABLE todo (id SERIAL PRIMARY KEY, description VARCHAR(255), details VARCHAR(4096), done BOOLEAN);
```

Usare il comando seguente per arrestare e rieseguire l'applicazione. L'applicazione userà ora il database `r2dbc` creato in precedenza e creerà una `todo` tabella al suo interno.

```bash
./mvnw spring-boot:run
```

Ecco uno screenshot della tabella di database creata:

   ![Creazione della tabella di database][R2DBC-MYSQL02]

## <a name="code-the-application"></a>Codice dell'applicazione

Aggiungere quindi il codice Java che userà R2DBC per archiviare e recuperare i dati dal server MySQL.

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

   ![Eseguire il test con cURL][R2DBC-MYSQL03]

Congratulazioni! È stata creata un'applicazione Spring Boot completamente reattiva che usa R2DBC per archiviare e recuperare i dati da Database di Azure per MySQL.

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

<!-- IMG List -->

[R2DBC-MYSQL01]: media/configure-spring-data-r2dbc-with-azure-mysql/create-mysql-01.png
[R2DBC-MYSQL02]: media/configure-spring-data-r2dbc-with-azure-mysql/create-mysql-02.png
[R2DBC-MYSQL03]: media/configure-spring-data-r2dbc-with-azure-mysql/create-mysql-03.png
