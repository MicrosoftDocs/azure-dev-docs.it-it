---
title: Guida per sviluppatori di Spring Data Azure Cosmos DB
description: Questa guida descrive gli aspetti da considerare quando si usa Spring Data Azure Cosmos DB SDK.
author: kushagraThapar
ms.author: kuthapar
ms.topic: conceptual
ms.date: 1/9/2019
ms.openlocfilehash: 6721a85ca00d277776545c5d717ccdb2948da207
ms.sourcegitcommit: d9f585bea70b01ba6657a75ea245d8519d4a5aad
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/03/2020
ms.locfileid: "76972695"
---
# <a name="spring-data-azure-cosmos-db-developers-guide"></a>Guida per sviluppatori di Spring Data Azure Cosmos DB

Questo argomento descrive le funzionalità di [Spring Data Cosmos DB](https://github.com/microsoft/spring-data-cosmosdb) con l'API SQL. Include anche indicazioni sui problemi comuni, le soluzioni alternative e i passaggi di diagnostica.

[Azure Cosmos DB](/azure/cosmos-db/introduction) è un servizio di database distribuito a livello globale che consente agli sviluppatori di lavorare con i dati usando un'ampia varietà di API standard. Spring Data Cosmos DB SDK è basato sul framework [Spring Data](https://spring.io/projects/spring-data) e prevede l'integrazione con Azure Cosmos DB tramite l'API SQL. Per informazioni sulle altre API supportate, vedere gli argomenti seguenti:

- [Come usare l'API MongoDB per Spring Data con Azure Cosmos DB](/azure/java/spring-framework/configure-spring-data-mongodb-with-cosmos-db)
- [Come usare l'API Apache Cassandra per Spring Data con Azure Cosmos DB](/azure/java/spring-framework/configure-spring-data-apache-cassandra-with-cosmos-db)
- [Come usare Spring Data Gremlin Starter con l'API SQL di Azure Cosmos DB](/azure/java/spring-framework/configure-spring-data-gremlin-java-app-with-cosmos-db)

Spring Data Cosmos DB SDK è disponibile come open source in GitHub nel repository [spring-data-cosmosdb](https://github.com/microsoft/spring-data-cosmosdb). Questo repository include un elenco di problemi attivi ([Issues](https://github.com/microsoft/spring-data-cosmosdb/issues)) in cui è possibile segnalare bug o verificare se sono disponibili soluzioni alternative per problemi già presentati. Per verificare se un problema è stato corretto in una versione più recente, è anche possibile controllare l'elenco [Releases](https://github.com/microsoft/spring-data-cosmosdb/releases) (Versioni). Il train di rilascio della versione 2.2.x di Spring Data Cosmos DB SDK supporta spring-data-commons versione 2.2.0.RELEASE, mentre il train di rilascio della versione 2.1.x dell'SDK supporta spring-data-common versione 2.1.0.RELEASE.

## <a name="available-features"></a>Funzionalità disponibili

Le sezioni seguenti descrivono le funzionalità attualmente disponibili.

### <a name="crudrepository-and-reactivecrudrepository-support"></a>Supporto di CrudRepository e ReactiveCrudRepository

Spring Data Cosmos DB SDK include le interfacce `CosmosRepository` e `ReactiveCosmosRepository`, che estendono le interfacce `ReactiveCrudRepository` e `CrudRepository` di Spring Data.

L'esempio seguente illustra come estendere queste interfacce.

```java
@Repository
public interface SampleRepository extends CosmosRepository<SampleEntity, String> {
    List<SampleEntity> findByName(String name);
}

@Repository
public interface ReactiveSampleRepository extends ReactiveCosmosRepository<SampleEntity, String> {
    Flux<SampleEntity> findByName(String name);
}
```

A seconda dell'utilizzo, è necessario abilitare entrambi i repository separatamente nella classe `Configuration`. Ad esempio:

```java
@Configuration
@PropertySource(value = {"classpath:application.properties"})
@EnableCosmosRepositories
@EnableReactiveCosmosRepositories
public class TestRepositoryConfig extends AbstractCosmosConfiguration {
    ...
}
```

### <a name="define-a-simple-entity"></a>Definire un'entità semplice

È possibile definire le entità aggiungendo l'annotazione `@Document` e specificando le proprietà correlate alla raccolta, ad esempio il nome della raccolta, le unità richiesta (UR), la durata e il flag di creazione automatica della raccolta.

Per impostazione predefinita, il nome della raccolta corrisponde al nome della classe di dominio utente. Per personalizzarlo, aggiungere l'annotazione `@Document(collection="myCustomCollectionName")` alla classe di dominio. Il campo della raccolta supporta anche espressioni [Spring Expression Language](https://docs.spring.io/spring/docs/3.0.x/reference/expressions.html) (SpEL), quindi è possibile specificare i nomi delle raccolte a livello di codice tramite proprietà di configurazione. Ad esempio, è possibile usare espressioni come `collection = "${dynamic.collection.name}"` e `collection = "#{@someBean.getCollectionName()}"`.

Esistono due modi per eseguire il mapping di un campo di una classe di dominio al campo `id` di un documento di Azure Cosmos DB:

- Annotare il campo con `@Id`.
- Impostare il nome del campo su `id`.

L'esempio seguente mostra l'uso delle annotazioni `@Document` e `@Id`.

```java
@Document(collection = "myCollection")
class MyDocument {

    @Id
    private String myId;

    @PartitionKey
    private String data;

    @Version
    private String _etag;
}
```

Per impostazione predefinita, `IndexingPolicy` è impostato dal servizio di Azure. Per personalizzarlo, aggiungere l'annotazione `@DocumentIndexingPolicy` alla classe di dominio. Questa annotazione ha quattro attributi:

```java
boolean automatic;     // Indicates whether the indexing policy is automatic.
IndexingMode mode;     // The indexing policy mode; the options are Consistent, Lazy, or None.
String[] includePaths; // Included paths for indexing.
String[] excludePaths; // Excluded paths for indexing.
```

L'SDK supporta anche il partizionamento. Per altre informazioni, vedere [Partizionamento e scalabilità orizzontale in Azure Cosmos DB](/azure/cosmos-db/partition-data). Per specificare un campo di una classe di dominio da usare come campo della chiave di partizione, annotarlo con `@PartitionKey`. Quindi, quando si eseguono operazioni CRUD, specificare il valore della partizione.

L'esempio seguente illustra come usare l'annotazione `@PartitionKey` durante l'esecuzione di operazioni CRUD.

```java
@Document(ru = "400")
public class Address {
    @Id
    String postalCode;

    @PartitionKey
    String city;

    String street;
    String country;
    String phoneNumber;

    ...
}

class AddressService {

    @Autowired
    AddressRepository repository;

    final Address newAddress = new Address("12345", "city");

    // There's no need to specify a partition key in the save operation.
    repository.save(updatedAddress);

    // Provide a partition key when performing a find-by-id operation.
    final Optional<Address> addressById = repository.findById("12345", new PartitionKey("city"));

    final Address foundAddress = addressById.get();

    // Provide a partition key when performing a delete-by-id operation.
    repository.deleteById(foundAddress.getPostalCode(), new PartitionKey(foundAddress.getCity())); 
}
```

L'SDK supporta anche le operazioni di ricerca con query personalizzate di Spring Data, ad esempio `findByAFieldAndBField`. Per altre informazioni, vedere [Definizione dei metodi di query](https://docs.spring.io/spring-data/commons/docs/current/reference/html/#repositories.query-methods.details) nella documentazione di Spring.

## <a name="best-practices"></a>Procedure consigliate

Le sezioni seguenti descrivono le procedure consigliate per l'uso dell'SDK.

### <a name="configuring-the-application"></a>Configurazione dell'applicazione

Per configurare l'applicazione, eseguire questa procedura:

1. Estendere la classe `AbstractCosmosConfiguration` per impostare la configurazione dell'applicazione (chiave, URL, nome del database Cosmos DB e così via).
1. Aggiungere l'annotazione `@Configuration`.
1. A seconda dell'utilizzo del repository, aggiungere una o entrambe le annotazioni `@EnableCosmosRepositories` e `@EnableReactiveCosmosRepositories`.

La funzionalità `CosmosKeyCredential` consente di alternare le chiavi in tempo reale. È possibile cambiare le chiavi usando il metodo `switchToSecondaryKey`.

L'esempio di codice seguente mostra una configurazione dell'applicazione che illustra l'uso di `switchToSecondaryKey`.

```java
@Configuration
@EnableCosmosRepositories
public class AppConfiguration extends AbstractCosmosConfiguration {

    @Value("${azure.cosmosdb.uri}")
    private String uri;

    @Value("${azure.cosmosdb.key}")
    private String key;

    @Value("${azure.cosmosdb.secondaryKey}")
    private String secondaryKey;

    @Value("${azure.cosmosdb.database}")
    private String dbName;

    @Value("${azure.cosmosdb.populateQueryMetrics}")
    private boolean populateQueryMetrics;

    private CosmosKeyCredential cosmosKeyCredential;

    @Bean
    public CosmosDBConfig getConfig() {
        this.cosmosKeyCredential = new CosmosKeyCredential(key);
        CosmosDbConfig cosmosdbConfig = CosmosDBConfig.builder(uri,
            this.cosmosKeyCredential, dbName).build();
        cosmosdbConfig.setPopulateQueryMetrics(populateQueryMetrics);
        cosmosdbConfig.setResponseDiagnosticsProcessor(new ResponseDiagnosticsProcessorImplementation());
        return cosmosdbConfig;
    }

    public void switchToSecondaryKey() {
        this.cosmosKeyCredential.key(secondaryKey);
    }
}
```

È anche possibile personalizzare la configurazione per cambiare la modalità di connessione, le dimensioni massime del pool di connessione, il timeout delle richieste e così via, come mostrato nell'esempio seguente.

```java
public CosmosDBConfig getConfig() {

    this.cosmosKeyCredential = new CosmosKeyCredential(key);
    ConnectionPolicy customizedConnectionPolicy = new ConnectionPolicy();

    // Set the connection mode to Direct (TCP).
    customizedConnectionPolicy.setConnectionMode(ConnectionMode.DIRECT);

    // Set the maximum number of HTTP/TCP connections to 1000 per application.
    customizedConnectionPolicy.setMaxPoolSize(1000);

    // Set the request timeout to 10 seconds.
    customizedConnectionPolicy.requestTimeoutInMillis(10000);

    // Set the idle connection timeout to two minutes.
    customizedConnectionPolicy.idleConnectionTimeoutInMillis(120000);
    CosmosDBConfig cosmosDbConfig = CosmosDBConfig.builder(uri,   this.cosmosKeyCredential, dbName)
                                                  .connectionPolicy  (customizedConnectionPolic  y)
                                                  .build();
    return cosmosDbConfig;
}
```

### <a name="response-diagnostics-and-query-metrics"></a>Diagnostica delle risposte e metriche di query

La versione 2.2.x di Spring Data Cosmos DB SDK supporta la stringa di diagnostica delle risposte e le metriche di query.

Per abilitare le metriche di query, impostare il flag `populateQueryMetrics` su **true** nel file `application.properties`. Estendere quindi l'interfaccia `ResponseDiagnosticsProcessor` e implementare il metodo `processResponseDiagnostics` per registrare le informazioni di diagnostica. Infine, passare un'istanza dell'implementazione al metodo `CosmosDbConfig.setResponseDiagnosticsProcessor`. Il codice seguente mostra un esempio dell'implementazione.

```java
@Configuration
@EnableCosmosRepositories
public class AppConfiguration extends AbstractCosmosConfiguration {

    ...

    @Value("${azure.cosmosdb.populateQueryMetrics}")
    private boolean populateQueryMetrics;

    private static class ResponseDiagnosticsProcessorImplementation implements ResponseDiagnosticsProcessor {

        @Override
        public void processResponseDiagnostics(@Nullable ResponseDiagnostics responseDiagnostics) {
            log.info("Response Diagnostics {}", responseDiagnostics);
        }
    }

    @Bean
    public CosmosDBConfig getConfig() {
        this.cosmosKeyCredential = new CosmosKeyCredential(key);
        CosmosDbConfig cosmosdbConfig = CosmosDBConfig.builder(uri, this.cosmosKeyCredential, dbName).build();
        cosmosdbConfig.setPopulateQueryMetrics(populateQueryMetrics);
        cosmosdbConfig.setResponseDiagnosticsProcessor(new ResponseDiagnosticsProcessorImplementation());
        return cosmosdbConfig;
    }
}
```

### <a name="pagination-and-sorting"></a>Paginazione e ordinamento

Spring Data Cosmos DB SDK supporta la paginazione e l'ordinamento di Spring Data. Per altre informazioni, vedere [Gestione dei parametri speciali](https://docs.spring.io/spring-data/commons/docs/current/reference/html/#repositories.special-parameters) nella documentazione di Spring.

In base alle unità richiesta (UR) disponibili nell'account del database, Cosmos DB può restituire documenti di dimensioni minori o uguali a quelle richieste. Per altre informazioni, vedere [Unità richiesta in Azure Cosmos DB](/azure/cosmos-db/request-units).

Non è consigliabile basarsi sul valore di `totalPageSize`, perché il numero di documenti restituiti in ogni iterazione è variabile. È invece preferibile eseguire l'iterazione su un oggetto `Pageable` come illustrato nell'esempio seguente.

```java
final Sort sort = Sort.by(Sort.Direction.DESC, "name");
final CosmosPageRequest pageRequest = new CosmosPageRequest(0, pageSize,   null, sort);
Page<T> page = tRepository.findAll(pageRequest);
List<T> pageContent = page.getContent();
while(page.hasNext()) {
    Pageable nextPageable = page.nextPageable();
    page = repository.findAll(nextPageable);
    pageContent = page.getContent();
}
```

## <a name="common-issues-and-workarounds"></a>Problemi comuni e soluzioni alternative

Le sezioni seguenti descrivono i problemi che è necessario tenere presenti quando si usa Spring Data Cosmos DB SDK.

### <a name="getting-the-correct-cosmos-db-configuration"></a>Recupero della configurazione corretta di Cosmos DB

L'estensione dell'interfaccia `AbstractCosmosConfiguration` può essere complessa a causa delle varie annotazioni e configurazioni presenti nella classe. Il problema più comune riguarda l'annotazione `Enable Repositories`.

Se i repository estendono `CosmosRepository`, assicurarsi di aggiungere l'annotazione `@EnableCosmosRepositories`. Se i repository estendono `ReactiveCosmosRepository`, assicurarsi di aggiungere l'annotazione `@EnableReactiveCosmosRepositories`. L'esempio seguente illustra l'uso di queste annotazioni.

```java
@Configuration
@PropertySource(value = {"classpath:application.properties"})
@EnableCosmosRepositories
@EnableReactiveCosmosRepositories
public class TestRepositoryConfig extends AbstractCosmosConfiguration {
    ...
}
```

Quando si crea o si personalizza un bean `CosmosDBConfig`, assicurarsi di usare l'oggetto `CosmosKeyCredential` invece di usare direttamente la chiave.

La funzionalità `CosmosKeyCredential` consente di alternare le chiavi in tempo reale. È possibile cambiare le chiavi usando il metodo `switchToSecondaryKey`.

`CosmosKeyCredential` deve essere un oggetto singleton perché Cosmos DB SDK usa lo stesso oggetto internamente per rilevare le modifiche nel valore della chiave all'interno di questo oggetto.

### <a name="custom-query-execution"></a>Esecuzione di query personalizzate

La funzionalità di annotazione delle query non è ancora supportata da Spring Data Cosmos DB SDK. Per il momento, è possibile eseguire query personalizzate e complesse direttamente nel bean `cosmosClient` esposto dal contesto dell'applicazione Spring.

Il codice seguente illustra un semplice esempio di come eseguire query di offset e di limite usando il bean `cosmosClient`.

```java
final FeedOptions feedOptions = new FeedOptions();

// Enable cross-partition queries.
feedOptions.enableCrossPartitionQuery(true);

// Set the page size.
feedOptions.maxItemCount(20);

// Set the number of parallel operations on the client-side SDK when executing parallel queries.
feedOptions.maxDegreeOfParallelism(2);

// Populate query metrics from Cosmos DB.
feedOptions.populateQueryMetrics(true);

final String query = "SELECT * from c OFFSET " + skipCount + " LIMIT " + takeCount;

final CosmosClient cosmosClient = applicationContext.getBean(CosmosClient.class);

Flux<FeedResponse<CosmosItemProperties>> feedResponseFlux =
    cosmosClient.getDatabase(databaseId)
                .getContainer(collectionId)
                .queryItems(query, feedOptions);
    feedResponseFlux.subscribeOn(Schedulers.parallel())
                    .flatMap(feedResponse -> {
                        List<CosmosItemProperties> results =
                        feedResponseFlux.results();
                        log.info("Results are {}", results);
                        return feedResponse;
                    })
                    .subscribe();
```

### <a name="enable-diagnostics-and-query-metrics"></a>Abilitare la diagnostica e le metriche di query

Quando si esegue il debug, è utile avere la stringa di diagnostica delle risposte e le metriche di query di Cosmos DB SDK. Cosmos DB SDK registra la stringa di diagnostica delle risposte sul lato client. Il back-end registra le metriche di query e le fornisce a Cosmos DB SDK.

Il metodo `ResponseDiagnosticsProcessor.processResponseDiagnostics` viene chiamato dopo ogni chiamata API in Spring Data Cosmos DB SDK. È quindi importante che l'implementazione garantisca prestazioni elevate senza bug ed evitando la complessità. Ad esempio, non è consigliabile registrare il set completo di informazioni di diagnostica in questo metodo, perché la quantità di informazioni coinvolte creerebbe un costo significativo per le prestazioni. È inoltre consigliabile usare il livello di registrazione `Debug` per evitare di influire sulle prestazioni dell'applicazione.

Il codice seguente mostra un esempio di implementazione dell'interfaccia `ResponseDiagnosticsProcessor`.

```java
private static class ResponseDiagnosticsProcessorImplementation implements ResponseDiagnosticsProcessor {

    @Override
    public void processResponseDiagnostics(@Nullable ResponseDiagnostics responseDiagnostics) {

        // To log everything:
        if (log.isDebugEnabled()) {
            log.debug("Response diagnostics {}", responseDiagnostics);
        }

        // To log Cosmos DB response diagnostics:
        if (responseDiagnostics != null && log.isDebugEnabled()) {
            CosmosResponseDiagnostics cosmosResponseDiagnostics = responseDiagnostics.getCosmosResponseDiagnostics();
            log.debug("Cosmos DB response diagnostics {}", cosmosResponseDiagnostics);
        }

        // To log just the request latency:
        if (responseDiagnostics != null && log.isDebugEnabled()) {
            CosmosResponseDiagnostics cosmosResponseDiagnostics = responseDiagnostics.getCosmosResponseDiagnostics();
            log.debug("Request latency {}", cosmosResponseDiagnostics.requestLatency());
        }

        // To log query metrics:
        if (responseDiagnostics != null && log.isDebugEnabled()) {
            FeedResponseDiagnostics feedResponseDiagnostics =
                responseDiagnostics.getFeedResponseDiagnostics();
            log.debug("Query metrics {}", feedResponseDiagnostics);
        }
    }
}
```

## <a name="how-to-troubleshoot"></a>Come risolvere i problemi

Le sezioni seguenti descrivono come risolvere i problemi comuni.

### <a name="connection-issues"></a>Problemi di connessione

Se si verificano problemi di connessione, assicurarsi che tutte le annotazioni necessarie nella classe di configurazione siano presenti e corrette, come descritto nella sezione [Recupero della configurazione corretta di Cosmos DB](#getting-the-correct-cosmos-db-configuration).

### <a name="api-exceptions"></a>Eccezioni delle API

La versione 2.2.1 di Spring Data Cosmos DB SDK prevede i miglioramenti seguenti alla gestione delle eccezioni:

- Tutte le API generano `CosmosDBAccessException`, che espone un campo di `cosmosClientException` tramite un getter.
- Cosmos DB SDK genera `CosmosClientException`, che è possibile usare per implementare la logica di ripetizione dei tentativi sul lato client.
- Le eccezioni comuni da riprovare sono quelle con i messaggi `Resource already exists`, `Request rate too large`, `Request timeout exception` e così via.

### <a name="api-or-query-slowness"></a>Lentezza di API o query

Se si riscontrano latenze elevate con l'esecuzione di API o query, provare a registrare le stringhe di diagnostica e le metriche di query come descritto nella sezione [Abilitare la diagnostica e le metriche di query](#enable-diagnostics-and-query-metrics). Verificare l'utilizzo della CPU, la larghezza di banda di rete e lo spazio su disco di I/O, che possono essere la causa radice della lentezza sul lato client.
