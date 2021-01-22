---
title: Come usare Spring Boot Starter con l'API SQL di Azure Cosmos DB
description: Informazioni su come configurare un'applicazione creata con Spring Boot Initializer con l'API SQL di Azure Cosmos DB.
services: cosmos-db
documentationcenter: java
author: KarlErickson
ms.author: karler
ms.date: 10/13/2020
ms.service: cosmos-db
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: data-services
ms.custom: devx-track-java
ms.openlocfilehash: 10f85c7d1208ff77f2c85ec14e77ced450d5fcc8
ms.sourcegitcommit: 593d177cfb5f56f236ea59389e43a984da30f104
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/18/2021
ms.locfileid: "98561306"
---
# <a name="how-to-use-the-spring-boot-starter-with-the-azure-cosmos-db-sql-api"></a>Come usare Spring Boot Starter con l'API SQL di Azure Cosmos DB

Azure Cosmos DB è un servizio di database distribuito a livello globale che consente agli sviluppatori di lavorare con i dati usando varie API standard, ad esempio le API SQL, MongoDB, Graph e Table. Microsoft Spring Boot Starter consente agli sviluppatori di usare applicazioni Spring Boot che si integrano facilmente con Azure Cosmos DB usando l'API SQL.

Questo articolo illustra la creazione di un Azure Cosmos DB usando il portale di Azure, quindi l'uso di **[Spring Initializr]** per creare un'applicazione Spring boot personalizzata e quindi aggiungere l' [Starter di Azure spring boot per Azure Cosmos DB] all'applicazione personalizzata per archiviare i dati in e recuperare i dati dal Azure Cosmos DB usando Spring data e l'API SQL di Cosmos DB.

## <a name="prerequisites"></a>Prerequisiti

* Sottoscrizione di Azure; se non si ha una sottoscrizione di Azure, è possibile attivare i [vantaggi per i sottoscrittori di MSDN] oppure iscriversi per ottenere un [account Azure gratuito].
* Java Development Kit (JDK) supportato. Per altre informazioni sulle versioni di JDK utilizzabili per lo sviluppo in Azure, vedere <https://aka.ms/azure-jdks>.

## <a name="create-an-azure-cosmos-db-by-using-the-azure-portal"></a>Creare un Azure Cosmos DB usando il portale di Azure

1. Passare alla portale di Azure in <https://portal.azure.com/> e selezionare **Crea una risorsa**.

1. Selezionare **Database** e quindi selezionare **Azure Cosmos DB**.

    ![Selezione di Azure Cosmos DB nel portale di Azure.][AZ02]

1. Nella pagina **Azure Cosmos DB** immettere le informazioni seguenti:

    * Selezionare la **sottoscrizione** da usare per il database.
    * Specificare se creare un nuovo **gruppo di risorse** per il database o sceglierne uno esistente.
    * Immettere un **nome account** univoco, che verrà usato come URI per il database, Ad esempio: *contosoaccount*.
    * Scegliere **Core (SQL)** come API.
    * Specificare il **percorso** per il database.

    Dopo aver specificato queste opzioni, selezionare **Verifica + crea**, verificare le specifiche e selezionare **Crea**.

    ![Selezionare Rivedi e crea per continuare.][AZ03]

1. Quando il database è stato creato, viene elencato nel **Dashboard** di Azure e nelle pagine tutte le **risorse** e **Azure Cosmos DB** . È possibile selezionare il database per una di queste posizioni per aprire la pagina delle proprietà per la cache.

1. Quando viene visualizzata la pagina delle proprietà per il database, selezionare **Chiavi** e copiare le chiavi di accesso e l'URI per il database. Questi valori verranno usati nell'applicazione Spring Boot.

    ![Copiare l'URI e le chiavi di accesso nella sezione Chiavi.][AZ05]

## <a name="create-a-spring-boot-application-with-the-spring-initializr"></a>Creare un'applicazione Spring boot con Spring Initializr

Usare la procedura seguente per creare un nuovo progetto di applicazione Spring Boot con il supporto di Azure. In alternativa, è possibile usare l'esempio [azure-spring-boot-sample-cosmosdb](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/spring/azure-spring-boot-samples/azure-spring-boot-sample-cosmos) nel repository [azure-sdk-for-java](https://github.com/Azure/azure-sdk-for-java). Quindi, è possibile passare direttamente alla sezione [Compilare e testare l'app](#build-and-test-your-app).

1. Passare a <https://start.spring.io/>.

1. Specificare le opzioni seguenti:

   * Generare un progetto **Maven** con **Java**.
   * Specificare la versione di **Spring Boot** in uso.
   * Specificare i nomi di **Group** (Gruppo) e **Artifact** (Artefatto) per l'applicazione.
   * Selezionare **11** per la versione di Java.
   * Aggiungere **Supporto di Azure** nelle dipendenze.

   >[!div class="mx-imgBorder"]
   >![Opzioni di base di Spring Initializr][SI01]

   > [!NOTE]
   > 1. Spring Initializr usa i nomi in **Group** (Gruppo) e **Artifact** (Artefatto) per creare il nome del pacchetto, ad esempio *com.example.wingtiptoysdata*.
   > 1. La versione di Spring boot può essere superiore alla versione supportata dal supporto tecnico di Azure. Dopo che il progetto è stato generato automaticamente, è possibile modificare manualmente la versione di Spring boot con la versione più recente supportata da Azure, che è possibile trovare [qui][azure-spring-boot-version-matrix].

1. Dopo aver specificato le opzioni elencate sopra, selezionare **GENERA**.

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
        <groupId>com.azure.spring</groupId>
        <artifactId>azure-spring-boot-starter-cosmos</artifactId>
        <version>3.0.0</version>
    </dependency>
    ```

1. Verificare che l'elemento *properties* indichi le versioni necessarie di Java e Azure:

    ```xml
    <properties>
        <java.version>11</java.version>
        <azure.version>3.0.0</azure.version>
    </properties>
    ```

1. Salvare e chiudere il file *pom.xml*.

## <a name="configure-your-spring-boot-application-to-use-your-azure-cosmos-db"></a>Configurare l'applicazione Spring Boot per l'uso di Azure Cosmos DB

1. Individuare il file *application.properties* nella directory *resources* dell'app, ad esempio:

    `C:\SpringBoot\wingtiptoysdata\src\main\resources\application.properties`

    -oppure-

    `/users/example/home/wingtiptoysdata/src/main/resources/application.properties`

1. Aprire il file *application.properties* in un editor di testo, quindi aggiungere le righe seguenti al file e sostituire i valori di esempio con le proprietà appropriate per il database:

    ```properties
    # Specify the DNS URI of your Azure Cosmos DB.
    azure.cosmos.uri=https://contosoaccount.documents.azure.com:443/
    
    # Specify the access key for your database.
    azure.cosmos.key=replace-your-access-key-here
    
    # Specify the name of your database. 
    azure.cosmos.database=contosoaccount
    azure.cosmos.populateQueryMetrics=true
    ```

1. Salvare e chiudere il file *application.properties*.

## <a name="add-sample-code-to-implement-basic-database-functionality"></a>Aggiungere il codice di esempio per implementare le funzionalità di base del database

In questa sezione si creano due classi Java per l'archiviazione dei dati utente, quindi si modifica la classe dell'applicazione main per creare un'istanza della classe *User* e salvarla nel database.

### <a name="define-a-base-class-for-storing-user-data"></a>Definire una classe di base per l'archiviazione dei dati utente

1. Creare un nuovo file denominato *User.java* nella stessa directory del file dell'applicazione Java main.

1. Aprire il file *User.java* in un editor di testo e aggiungere le righe seguenti al file per definire una classe User generica che consenta di archiviare e recuperare i valori nel database:

    ```java
    package com.example.wingtiptoysdata;
    
    import com.azure.spring.data.cosmos.core.mapping.Container;
    import com.azure.spring.data.cosmos.core.mapping.PartitionKey;
    import org.springframework.data.annotation.Id;
    
    @Container(containerName = "mycollection")
    public class User {
        @Id
        private String id;
        private String firstName;
        @PartitionKey
        private String lastName;
        private String address;
    
        public User() {
    
        }
    
        public User(String id, String firstName, String lastName, String address) {
            this.id = id;
            this.firstName = firstName;
            this.lastName = lastName;
            this.address = address;
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
    
    import com.azure.spring.data.cosmos.repository.ReactiveCosmosRepository;
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
    
    import org.springframework.boot.SpringApplication;
    import org.springframework.boot.autoconfigure.SpringBootApplication;
    
    import org.slf4j.Logger;
    import org.slf4j.LoggerFactory;
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.boot.CommandLineRunner;
    import org.springframework.util.Assert;
    import reactor.core.publisher.Flux;
    import reactor.core.publisher.Mono;
    
    import java.util.Optional;
    
    @SpringBootApplication
    public class WingtiptoysdataApplication implements CommandLineRunner {
    
        private static final Logger LOGGER = LoggerFactory.getLogger(WingtiptoysdataApplication.class);
    
        @Autowired
        private UserRepository repository;
    
        public static void main(String[] args) {
            SpringApplication.run(WingtiptoysdataApplication.class, args);
        }
    
        public void run(String... var1) {
            this.repository.deleteAll().block();
            LOGGER.info("Deleted all data in container.");
    
            final User testUser = new User("testId", "testFirstName", "testLastName", "test address line one");
    
            // Save the User class to Azure Cosmos DB database.
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
    
            firstNameUserFlux.collectList().block();
    
            final Optional<User> optionalUserResult = repository.findById(testUser.getId()).blockOptional();
            Assert.isTrue(optionalUserResult.isPresent(), "Cannot find user.");
    
            final User result = optionalUserResult.get();
            Assert.state(result.getFirstName().equals(testUser.getFirstName()), "query result firstName doesn't match!");
            Assert.state(result.getLastName().equals(testUser.getLastName()), "query result lastName doesn't match!");
    
            LOGGER.info("findOne in User collection get result: {}", result.toString());
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
    mvnw clean
    ```

    Questo comando esegue l'applicazione automaticamente come parte della fase di test. È anche possibile usare:

    ```console
    mvnw spring-boot:run
    ```

    Dopo l'output di compilazione e test, la finestra della console visualizzerà un messaggio simile al seguente:

    ```console
    INFO 1365 --- [           main] c.e.w.WingtiptoysdataApplication         : Deleted all data in container.
    
    ... (omitting connection and diagnostics output) ...
    
    INFO 1365 --- [           main] c.e.w.WingtiptoysdataApplication         : findOne in User collection get result: testFirstName testLastName, test address line one
    ```

    I messaggi di output precedenti indicano che i dati sono stati salvati correttamente in Cosmos DB e quindi recuperati nuovamente.

## <a name="clean-up-resources"></a>Pulire le risorse

Se non si prevede di continuare a usare questa applicazione, assicurarsi di eliminare il gruppo di risorse contenente l'istanza di Cosmos DB creata in precedenza. Questa operazione può essere eseguita dal portale di Azure.

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni su Spring e Azure, passare al centro di documentazione di Spring in Azure.

> [!div class="nextstepaction"]
> [Spring in Azure](./index.yml)

### <a name="more-resources"></a>Altre risorse

Per altre informazioni sull'utilizzo di Azure Cosmos DB e Java, vedere gli articoli seguenti:

* [Documentazione di Azure Cosmos DB].

* [Azure Cosmos DB: Create a document database using Java and the Azure portal][Build a SQL API app with Java] (Aure Cosmos DB: creare un database di documenti con Java e il portale di Azure)

* [Spring Data per l'API SQL Azure Cosmos DB]

Per altre informazioni sull'uso delle applicazioni Spring Boot in Azure, vedere gli articoli seguenti:

* [Starter di Azure Spring boot per Azure Cosmos DB]

* [Distribuire un'applicazione Spring Boot in Linux nel Servizio app di Azure](deploy-spring-boot-java-app-on-linux.md)

* [Eseguire un'applicazione Spring Boot in un cluster Kubernetes nel servizio Azure Container](deploy-spring-boot-java-app-on-kubernetes.md)

Per altre informazioni sull'uso di Azure con Java, vedere [Azure per sviluppatori Java] e la documentazione relativa all'[uso di Azure DevOps e Java].

**[Spring Framework]** è una soluzione open source che consente agli sviluppatori Java di creare applicazioni di livello enterprise. Uno dei progetti più comuni che si basa su questa piattaforma è [Spring Boot], che fornisce un approccio semplificato per la creazione di applicazioni Java autonome. Per semplificare le operazioni iniziali con Spring Boot per gli sviluppatori, alcuni pacchetti Spring Boot di esempio sono disponibili all'indirizzo <https://github.com/spring-guides/>. Oltre a consentire di scegliere dall'elenco di progetti Spring Boot di base, **[Spring Initializr]** semplifica le operazioni iniziali degli sviluppatori per la creazione di applicazioni Spring Boot personalizzate.

<!-- URL List -->

[Documentazione di Azure Cosmos DB]: /azure/cosmos-db/
[Azure per sviluppatori Java]: ../index.yml
[Build a SQL API app with Java]: /azure/cosmos-db/create-sql-api-java
[Spring Data per l'API SQL Azure Cosmos DB]: https://azure.microsoft.com/blog/spring-data-azure-cosmos-db-nosql-data-access-on-azure/
[Starter di Azure Spring boot per Azure Cosmos DB]: https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/spring/azure-spring-boot-starter-cosmos
[Account Azure gratuito]: https://azure.microsoft.com/pricing/free-trial/
[Uso di Azure DevOps e Java]: https://azure.microsoft.com/services/devops/java/
[vantaggi per i sottoscrittori di MSDN]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/ (Framework di Spring)
[azure-spring-boot-version-matrix]: https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/spring#azure-spring-boot

<!-- IMG List -->
[AZ02]: media/configure-spring-boot-starter-java-app-with-cosmos-db/AZ02.png
[AZ03]: media/configure-spring-boot-starter-java-app-with-cosmos-db/AZ03.png
[AZ05]: media/configure-spring-boot-starter-java-app-with-cosmos-db/AZ05.png

[SI01]: media/configure-spring-boot-starter-java-app-with-cosmos-db/SI01.png
