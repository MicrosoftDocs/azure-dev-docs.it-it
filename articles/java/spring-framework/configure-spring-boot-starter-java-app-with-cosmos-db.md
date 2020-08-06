---
title: Come usare Spring Boot Starter con l'API SQL di Azure Cosmos DB
description: Informazioni su come configurare un'applicazione creata con Spring Boot Initializer con l'API SQL di Azure Cosmos DB.
services: cosmos-db
documentationcenter: java
author: KarlErickson
ms.author: karler
ms.date: 10/02/2019
ms.service: cosmos-db
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: data-services
ms.custom: devx-track-java
ms.openlocfilehash: 9f17491cfa7c8c4ec82945fe1fed69b1d681ba8b
ms.sourcegitcommit: da9fab1b718c71e40c7cbe0a08526c316dcdd6df
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/03/2020
ms.locfileid: "87525816"
---
# <a name="how-to-use-the-spring-boot-starter-with-the-azure-cosmos-db-sql-api"></a>Come usare Spring Boot Starter con l'API SQL di Azure Cosmos DB

Azure Cosmos DB è un servizio di database distribuito a livello globale che consente agli sviluppatori di usare i dati usando un'ampia gamma di API standard, come SQL, MongoDB, Graph e Table. Microsoft Spring Boot Starter consente agli sviluppatori di usare applicazioni Spring Boot che si integrano facilmente con Azure Cosmos DB usando l'API SQL.

Questo articolo descrive la creazione di un database di Azure Cosmos DB con il portale di Azure, l'uso di **[Spring Initializr]** per creare un'applicazione Spring Boot personalizzata e quindi l'aggiunta delle funzionalità di [Spring Boot Cosmos DB Starter per Azure] all'applicazione personalizzata per l'archiviazione e il recupero di dati da Azure Cosmos DB usando Spring Data e l'API SQL di Cosmos DB.

## <a name="prerequisites"></a>Prerequisiti

I prerequisiti seguenti sono necessari per seguire le procedure disponibili in questo articolo:

* Sottoscrizione di Azure; se non si ha una sottoscrizione di Azure, è possibile attivare i [vantaggi per i sottoscrittori di MSDN] oppure iscriversi per ottenere un [account Azure gratuito].
* Java Development Kit (JDK) supportato. Per altre informazioni sulle versioni di JDK utilizzabili per lo sviluppo in Azure, vedere <https://aka.ms/azure-jdks>.

## <a name="create-an-azure-cosmos-db-by-using-the-azure-portal"></a>Creare un Azure Cosmos DB usando il portale di Azure

1. Passare al portale di Azure all'indirizzo <https://portal.azure.com/> e fare clic su **Crea una risorsa**.

1. Fare clic su **Database** e quindi su **Azure Cosmos DB**.

    ![Portale di Azure][AZ02]

1. Nella pagina **Azure Cosmos DB** immettere le informazioni seguenti:

    * Selezionare la **sottoscrizione** da usare per il database.
    * Specificare se creare un nuovo **gruppo di risorse** per il database o sceglierne uno esistente.
    * Immettere un **nome account** univoco, che verrà usato come URI per il database, ad esempio *wingtiptoysdata*.
    * Scegliere **Core (SQL)** come API.
    * Specificare il **percorso** per il database.

    Dopo aver specificato queste opzioni, fare clic su **Rivedi e crea**, verificare le specifiche, quindi fare clic su **Crea**.

    ![Portale di Azure][AZ03]

1. Al termine della creazione, il database viene elencato nel **Dashboard** di Azure e nelle pagine **Tutte le risorse** e **Azure Cosmos DB**. È possibile fare clic sul database in una di queste posizioni per aprire la pagina delle proprietà per la cache.

1. Quando viene visualizzata la pagina delle proprietà per il database, fare clic su **Chiavi** e copiare le chiavi di accesso e l'URI per il database. Questi valori verranno usati nell'applicazione Spring Boot.

    ![Portale di Azure][AZ05]

## <a name="create-a-simple-spring-boot-application-with-the-spring-initializr"></a>Creare un'applicazione Spring Boot semplice con Spring Initializr

Usare la procedura seguente per creare un nuovo progetto di applicazione Spring Boot con il supporto di Azure. In alternativa, è possibile usare l'esempio [azure-spring-boot-sample-cosmosdb](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/spring/azure-spring-boot-samples/azure-spring-boot-sample-cosmosdb) nel repository [azure-sdk-for-java](https://github.com/Azure/azure-sdk-for-java). Quindi, è possibile passare direttamente alla sezione [Compilare e testare l'app](#build-and-test-your-app).

1. Passare a <https://start.spring.io/>.

1. Specificare che si vuole generare un progetto **Maven** con **Java**, specificare la versione di **Spring Boot**, immettere i nomi di **Gruppo** (Gruppo) e **Artifact** (Artefatto) per l'applicazione, aggiungere **Azure Support** (Supporto di Azure) nelle dipendenze e quindi fare clic sul pulsante **Generate Project** (Genera progetto).

    ![Opzioni di base di Spring Initializr][SI01]

    > [!NOTE]
    > Spring Initializr usa i nomi in **Group** (Gruppo) e **Artifact** (Artefatto) per creare il nome del pacchetto, ad esempio *com.example.wingtiptoysdata*.

1. Quando richiesto, scaricare il progetto in un percorso nel computer locale ed estrarre i file.

La semplice applicazione Spring Boot è ora pronta per la modifica.

## <a name="configure-your-spring-boot-application-to-use-the-azure-spring-boot-starter"></a>Configurare l'applicazione Spring Boot per l'uso di Azure Spring Boot Starter

1. Individuare il file *pom.xml* nella directory dell'app, ad esempio:

    `C:\SpringBoot\wingtiptoysdata\pom.xml`

    -oppure-

    `/users/example/home/wingtiptoysdata/pom.xml`

1. Aprire il file *pom.xml* in un editor di testo e aggiungere quanto esegue all'elemento `<dependencies>`:

    ```xml
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-cosmosdb-spring-boot-starter</artifactId>
        <version>3.0.0-beta.1</version>
    </dependency>
    ```

1. Verificare che l'elemento *properties* indichi le versioni necessarie di Java e Azure:

    ```xml
    <properties>
       <java.version>1.8</java.version>
       <azure.version>2.2.0</azure.version>
    </properties>
    ```

1. Salvare e chiudere il file *pom.xml*.

## <a name="configure-your-spring-boot-application-to-use-your-azure-cosmos-db"></a>Configurare l'applicazione Spring Boot per l'uso di Azure Cosmos DB

1. Individuare il file *application.properties* nella directory *resources* dell'app, ad esempio:

    `C:\SpringBoot\wingtiptoysdata\src\main\resources\application.properties`

    -oppure-

    `/users/example/home/wingtiptoysdata/src/main/resources/application.properties`

1. Aprire il file *application.properties* in un editor di testo, quindi aggiungere le righe seguenti al file e sostituire i valori di esempio con le proprietà appropriate per il database:

    ```text
    # Specify the DNS URI of your Azure Cosmos DB.
    azure.cosmosdb.uri=https://wingtiptoys.documents.azure.com:443/

    # Specify the access key for your database.
    azure.cosmosdb.key=57686f6120447564652c20426f6220526f636b73==

    # Specify the name of your database.
    azure.cosmosdb.database=wingtiptoysdata
    ```

1. Salvare e chiudere il file *application.properties*.

## <a name="add-sample-code-to-implement-basic-database-functionality"></a>Aggiungere il codice di esempio per implementare le funzionalità di base del database

In questa sezione si creano due classi Java per l'archiviazione dei dati utente, quindi si modifica la classe dell'applicazione main per creare un'istanza della classe *User* e salvarla nel database.

### <a name="define-a-base-class-for-storing-user-data"></a>Definire una classe di base per l'archiviazione dei dati utente

1. Creare un nuovo file denominato *User.java* nella stessa directory del file dell'applicazione Java main.

1. Aprire il file *User.java* in un editor di testo e aggiungere le righe seguenti al file per definire una classe User generica che consenta di archiviare e recuperare i valori nel database:

    ```java
    package com.example.wingtiptoysdata;

    import com.microsoft.azure.spring.data.cosmosdb.core.mapping.Document;
    import com.microsoft.azure.spring.data.cosmosdb.core.mapping.PartitionKey;
    import org.springframework.data.annotation.Id;

    @Document(collection = "mycollection")
    public class User {

        @Id
        private String id;
        private String firstName;

        @PartitionKey
        private String lastName;
        private String address;

        public User(String id, String firstName, String lastName, String address) {
            this.id = id;
            this.firstName = firstName;
            this.lastName = lastName;
            this.address = address;
        }

        public User() {
        }

        public String getId() {
            return id;
        }

        public void setId(String id) {
            this.id = id;
        }

        public String getFirstName() {
            return firstName;
        }

        public void setFirstName(String firstName) {
            this.firstName = firstName;
        }

        public String getLastName() {
            return lastName;
        }

        public void setLastName(String lastName) {
            this.lastName = lastName;
        }

        public String getAddress() {
            return address;
        }

        public void setAddress(String address) {
            this.address = address;
        }

        @Override
        public String toString() {
            return String.format("%s %s, %s", firstName, lastName, address);
        }
    }
    ```

1. Salvare e chiudere il file *User.java*.

### <a name="define-a-data-repository-interface"></a>Definire un'interfaccia del repository di dati

1. Creare un nuovo file denominato *UserRepository.java* nella stessa directory del file dell'applicazione Java main.

1. Aprire il file *UserRepository.java* in un editor di testo e aggiungere le righe seguenti al file per definire l'interfaccia utente del repository che estende l'interfaccia `ReactiveCosmosRepository` predefinita:

    ```java
    package com.example.wingtiptoysdata;

    import com.microsoft.azure.spring.data.cosmosdb.repository.ReactiveCosmosRepository;
    import org.springframework.stereotype.Repository;
    import reactor.core.publisher.Flux;

    @Repository
    public interface UserRepository extends ReactiveCosmosRepository<User, String> {
        Flux<User> findByFirstName(String firstName);
    }
    ```

    L'interfaccia `ReactiveCosmosRepository` sostituisce l'interfaccia `DocumentDbRepository` della versione precedente di Starter. La nuova interfaccia include API sincrone e reattive per operazioni di base di salvataggio, eliminazione e ricerca.

1. Salvare e chiudere il file *UserRepository.java*.

### <a name="modify-the-main-application-class"></a>Modificare la classe dell'applicazione main

1. Individuare il file Java dell'applicazione main nella directory del pacchetto dell'app, ad esempio:

    `C:\SpringBoot\wingtiptoysdata\src\main\java\com\example\wingtiptoysdata\WingtiptoysdataApplication.java`

    -oppure-

    `/users/example/home/wingtiptoysdata/src/main/java/com/example/wingtiptoysdata/WingtiptoysdataApplication.java`

1. Aprire il file Java dell'applicazione main in un editor di testo e aggiungere le righe seguenti al file:

    ```java
    package com.example.wingtiptoysdata;

    import org.slf4j.Logger;
    import org.slf4j.LoggerFactory;
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.boot.CommandLineRunner;
    import org.springframework.boot.SpringApplication;
    import org.springframework.boot.autoconfigure.SpringBootApplication;
    import org.springframework.util.Assert;
    import reactor.core.publisher.Flux;
    import reactor.core.publisher.Mono;

    import javax.annotation.PostConstruct;
    import javax.annotation.PreDestroy;
    import java.util.Optional;

    @SpringBootApplication
    public class WingtiptoysdataApplication implements CommandLineRunner {

        private static final Logger LOGGER = LoggerFactory.getLogger(WingtiptoysdataApplication.class);

        @Autowired
        private UserRepository repository;

        public static void main(String[] args) {
            SpringApplication.run(WingtiptoysdataApplication.class, args);
        }

        public void run(String... var1) throws Exception {
            final User testUser = new User("1", "Tasha", "Calderon", "4567 Main St Buffalo, NY 98052");

            LOGGER.info("Saving user: {}", testUser);

            // Save the User class to Azure CosmosDB database.
            final Mono<User> saveUserMono = repository.save(testUser);

            final Flux<User> firstNameUserFlux = repository.findByFirstName("testFirstName");

            //  Nothing happens until we subscribe to these Monos.
            //  findById will not return the user as user is not present.
            final Mono<User> findByIdMono = repository.findById(testUser.getId());
            final User findByIdUser = findByIdMono.block();
            Assert.isNull(findByIdUser, "User must be null");

            final User savedUser = saveUserMono.block();
            Assert.state(savedUser != null, "Saved user must not be null");
            Assert.state(savedUser.getFirstName().equals(testUser.getFirstName()), "Saved user first name doesn't match");

            LOGGER.info("Saved user");

            firstNameUserFlux.collectList().block();

            final Optional<User> optionalUserResult = repository.findById(testUser.getId()).blockOptional();
            Assert.isTrue(optionalUserResult.isPresent(), "Cannot find user.");

            final User result = optionalUserResult.get();
            Assert.state(result.getFirstName().equals(testUser.getFirstName()), "query result firstName doesn't match!");
            Assert.state(result.getLastName().equals(testUser.getLastName()), "query result lastName doesn't match!");

            LOGGER.info("Found user by findById : {}", result);
        }

        @PostConstruct
        public void setup() {
            LOGGER.info("Clear the database");
            this.repository.deleteAll().block();
        }

        @PreDestroy
        public void cleanup() {
            LOGGER.info("Cleaning up users");
            this.repository.deleteAll().block();
        }
    }
    ```

1. Salvare e chiudere il file Java dell'applicazione main.

## <a name="build-and-test-your-app"></a>Compilare e testare l'app

1. Aprire un prompt dei comandi e passare alla cartella in cui si trova il file *pom.xml*, ad esempio:

    `cd C:\SpringBoot\wingtiptoysdata`

    -oppure-

    `cd /users/example/home/wingtiptoysdata`

1. Usare il comando seguente per compilare ed eseguire l'applicazione:

    ```console
    mvnw clean test
    ```

    Questo comando esegue l'applicazione automaticamente come parte della fase di test. È anche possibile usare:

    ```console
    mvnw clean spring-boot:run
    ```

    Dopo l'output di compilazione e test, la finestra della console visualizzerà un messaggio simile al seguente:

    ```console
      .   ____          _            __ _ _
     /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
    ( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
     \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
      '  |____| .__|_| |_|_| |_\__, | / / / /
     =========|_|==============|___/=/_/_/_/
     :: Spring Boot ::            (v2.2.0.RC1)
    >
    > 2019-10-04 15:19:06.817  INFO 30013 --- [           main] c.e.w.WingtiptoysdataApplicationTests    : Starting WingtiptoysdataApplicationTests on devmachine03 with PID 30013 (started by <user> in /d/source/repos/wingtiptoysdata)
    > 2019-10-04 15:19:06.818  INFO 30013 --- [           main] c.e.w.WingtiptoysdataApplicationTests    : No active profile set, falling back to default profiles: default
    > 2019-10-04 15:19:08.329  INFO 30013 --- [           main] .s.d.r.c.RepositoryConfigurationDelegate : Bootstrapping Spring Data repositories in DEFAULT mode.
    > 2019-10-04 15:19:09.720  INFO 30013 --- [           main] .s.d.r.c.RepositoryConfigurationDelegate : Finished Spring Data repository scanning in 1369ms. Found 1 repository interfaces.
    > 2019-10-04 15:19:09.734  INFO 30013 --- [           main] .s.d.r.c.RepositoryConfigurationDelegate : Bootstrapping Spring Data repositories in DEFAULT mode.
    > 2019-10-04 15:19:09.748  INFO 30013 --- [           main] .s.d.r.c.RepositoryConfigurationDelegate : Finished Spring Data repository scanning in 13ms. Found 0 repository interfaces.

    ... (omitting Cosmos DB connection output) ...

    > 2019-10-04 15:19:46.584  INFO 30013 --- [           main] c.e.w.WingtiptoysdataApplicationTests    : Started WingtiptoysdataApplicationTests in 40.702 seconds (JVM running for 44.647)
    > 2019-10-04 15:19:46.587  INFO 30013 --- [           main] c.e.w.WingtiptoysdataApplication         : Saving user: Tasha Calderon, 4567 Main St Buffalo, NY 98052
    > 2019-10-04 15:19:47.122  INFO 30013 --- [           main] c.e.w.WingtiptoysdataApplication         : Saved user
    > 2019-10-04 15:19:47.289  INFO 30013 --- [           main] c.e.w.WingtiptoysdataApplication         : Found user by findById : Tasha Calderon, 4567 Main St Buffalo, NY 98052
    > [INFO] Tests run: 1, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 44.003 s - in com.example.wingtiptoysdata.WingtiptoysdataApplicationTests
    > 2019-10-04 15:19:48.124  INFO 30013 --- [extShutdownHook] c.a.d.c.internal.RxDocumentClientImpl    : Shutting down ...
    > 2019-10-04 15:19:48.194  INFO 30013 --- [extShutdownHook] c.a.d.c.internal.RxDocumentClientImpl    : Shutting down ...
    > 2019-10-04 15:19:48.200  INFO 30013 --- [extShutdownHook] c.e.w.WingtiptoysdataApplication         : Cleaning up users
    > [INFO]
    > [INFO] Results:
    > [INFO]
    > [INFO] Tests run: 1, Failures: 0, Errors: 0, Skipped: 0
    > [INFO]
    > [INFO]
    > [INFO] --- maven-jar-plugin:3.1.2:jar (default-jar) @ wingtiptoysdata ---
    > [INFO] Building jar: /d/source/repos/wingtiptoysdata/target/wingtiptoysdata-0.0.1-SNAPSHOT.jar
    > [INFO]
    > [INFO] --- spring-boot-maven-plugin:2.2.0.RC1:repackage (repackage) @ wingtiptoysdata ---
    > [INFO] Replacing main artifact with repackaged archive
    > [INFO] ------------------------------------------------------------------------
    > [INFO] BUILD SUCCESS
    > [INFO] ------------------------------------------------------------------------
    > [INFO] Total time:  02:18 min
    > [INFO] Finished at: 2019-10-04T15:20:05-07:00
    > [INFO] ------------------------------------------------------------------------
    ```

    ![Output positivo dall'applicazione][JV02]

    I messaggi `Saved user` e `Found user` indicano che i dati sono stati salvati in Cosmos DB e quindi recuperati correttamente.

## <a name="clean-up-resources"></a>Pulire le risorse

Se non si prevede di continuare a usare questa applicazione, assicurarsi di eliminare il gruppo di risorse contenente l'istanza di Cosmos DB creata in precedenza. Questa operazione può essere eseguita dal portale di Azure.

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni su Spring e Azure, passare al centro di documentazione di Spring in Azure.

> [!div class="nextstepaction"]
> [Spring in Azure](/azure/developer/java/spring-framework)

### <a name="additional-resources"></a>Risorse aggiuntive

Per altre informazioni sull'utilizzo di Azure Cosmos DB e Java, vedere gli articoli seguenti:

* [Documentazione di Azure Cosmos DB].

* [Azure Cosmos DB: Create a document database using Java and the Azure portal][Build a SQL API app with Java] (Aure Cosmos DB: creare un database di documenti con Java e il portale di Azure)

* [Spring Data per l'API SQL Azure Cosmos DB]

Per altre informazioni sull'uso delle applicazioni Spring Boot in Azure, vedere gli articoli seguenti:

* [Spring Boot Cosmos DB Starter per Azure]

* [Distribuire un'applicazione Spring Boot nel servizio app di Azure](deploy-spring-boot-java-app-from-container-registry-using-maven-plugin.md)

* [Eseguire un'applicazione Spring Boot in un cluster Kubernetes nel servizio Azure Container](deploy-spring-boot-java-app-on-kubernetes.md)

Per altre informazioni sull'uso di Azure con Java, vedere [Azure per sviluppatori Java] e la documentazione relativa all'[uso di Azure DevOps e Java].

**[Spring Framework]** è una soluzione open source che consente agli sviluppatori Java di creare applicazioni di livello enterprise. Uno dei progetti più comuni che si basa su questa piattaforma è [Spring Boot], che fornisce un approccio semplificato per la creazione di applicazioni Java autonome. Per semplificare le operazioni iniziali con Spring Boot per gli sviluppatori, alcuni pacchetti Spring Boot di esempio sono disponibili all'indirizzo <https://github.com/spring-guides/>. Oltre a consentire di scegliere dall'elenco di progetti Spring Boot di base, **[Spring Initializr]** semplifica le operazioni iniziali degli sviluppatori per la creazione di applicazioni Spring Boot personalizzate.

<!-- URL List -->

[Documentazione di Azure Cosmos DB]: /azure/cosmos-db/
[Azure per sviluppatori Java]: /azure/developer/java/
[Build a SQL API app with Java]: /azure/cosmos-db/create-sql-api-java
[Spring Data per l'API SQL Azure Cosmos DB]: https://azure.microsoft.com/blog/spring-data-azure-cosmos-db-nosql-data-access-on-azure/
[Spring Boot Cosmos DB Starter per Azure]: https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/spring/azure-spring-boot-starter-cosmosdb
[Account Azure gratuito]: https://azure.microsoft.com/pricing/free-trial/
[Uso di Azure DevOps e Java]: https://azure.microsoft.com/services/devops/java/
[vantaggi per i sottoscrittori di MSDN]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/ (Framework di Spring)

<!-- IMG List -->

[AZ01]: media/configure-spring-boot-starter-java-app-with-cosmos-db/AZ01.png
[AZ02]: media/configure-spring-boot-starter-java-app-with-cosmos-db/AZ02.png
[AZ03]: media/configure-spring-boot-starter-java-app-with-cosmos-db/AZ03.png
[AZ04]: media/configure-spring-boot-starter-java-app-with-cosmos-db/AZ04.png
[AZ05]: media/configure-spring-boot-starter-java-app-with-cosmos-db/AZ05.png

[SI01]: media/configure-spring-boot-starter-java-app-with-cosmos-db/SI01.png

[JV02]: media/configure-spring-boot-starter-java-app-with-cosmos-db/JV02.png
