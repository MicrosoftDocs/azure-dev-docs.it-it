---
title: Guida per sviluppatori di Spring Data Azure Cosmos DB
description: Questa guida descrive gli aspetti da considerare quando si usa Spring Data Azure Cosmos DB SDK.
author: anfeldma-ms
ms.author: anfeldma
ms.topic: conceptual
ms.date: 11/23/2020
ms.custom: devx-track-java
ms.openlocfilehash: 45872eba2fd6929cf406df4a551559fd5ad78c40
ms.sourcegitcommit: 63732132cb88206b99876f0bcd035b52c301f315
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/02/2020
ms.locfileid: "96523136"
---
# <a name="spring-data-azure-cosmos-db-developers-guide"></a>Guida per sviluppatori di Spring Data Azure Cosmos DB

Questo argomento descrive le funzionalità di [Spring Data Azure Cosmos DB](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/cosmos/azure-spring-data-cosmos) con l'API SQL. Include anche indicazioni sui problemi comuni, le soluzioni alternative e i passaggi di diagnostica.

[Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/introduction) è un servizio di database distribuito a livello globale che consente agli sviluppatori di lavorare con i dati usando un'ampia varietà di API standard. Spring Data Azure Cosmos DB SDK è basato sul framework [Spring Data](https://spring.io/projects/spring-data) e prevede l'integrazione con Azure Cosmos DB tramite l'API SQL. Per informazioni sulle altre API supportate, vedere gli argomenti seguenti:

- [Come usare l'API MongoDB per Spring Data con Azure Cosmos DB](./configure-spring-data-mongodb-with-cosmos-db.md)
- [Come usare l'API Apache Cassandra per Spring Data con Azure Cosmos DB](./configure-spring-data-apache-cassandra-with-cosmos-db.md)
- [Come usare Spring Data Gremlin Starter con l'API SQL di Azure Cosmos DB](./configure-spring-data-gremlin-java-app-with-cosmos-db.md)

Spring Data Azure Cosmos DB SDK è disponibile come open source in GitHub nel repository [azure-sdk-for-java](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/cosmos/azure-spring-data-cosmos). Questo repository include un elenco di problemi attivi ([Issues](https://github.com/Azure/azure-sdk-for-java/issues)) in cui è possibile segnalare bug o verificare se sono disponibili soluzioni alternative per problemi già presentati. Per verificare se un problema è stato corretto in una versione più recente, è anche possibile controllare l'elenco [Releases](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/cosmos/azure-spring-data-cosmos/CHANGELOG.md) (Versioni). 

## <a name="available-features"></a>Funzionalità disponibili

Le sezioni seguenti descrivono le funzionalità attualmente disponibili.

### <a name="crudrepository-and-reactivecrudrepository-support"></a>Supporto di CrudRepository e ReactiveCrudRepository

Spring Data Azure Cosmos DB SDK include le interfacce `CosmosRepository` e `ReactiveCosmosRepository`, che estendono le interfacce `ReactiveCrudRepository` e `CrudRepository` di Spring Data.

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
@EnableConfigurationProperties(CosmosProperties.class)
@EnableCosmosRepositories
@EnableReactiveCosmosRepositories
@PropertySource("classpath:application.properties")
public class TestRepositoryConfig extends AbstractCosmosConfiguration {
    ...
}
```

### <a name="define-a-simple-entity"></a>Definire un'entità semplice

È possibile definire le entità aggiungendo l'annotazione `@Container` e specificando le proprietà correlate alla raccolta, ad esempio il nome della raccolta, le unità richiesta (UR), la durata e il flag di creazione automatica della raccolta.

Per impostazione predefinita, il nome della raccolta corrisponde al nome della classe di dominio utente. Per personalizzarlo, aggiungere l'annotazione `@Container(containerName="myCustomCollectionName")` alla classe di dominio. Il campo `containerName` supporta anche espressioni in linguaggio [SpEL (Spring Expression Language)](https://docs.spring.io/spring/docs/3.0.x/reference/expressions.html), quindi è possibile specificare i nomi delle raccolte a livello di codice tramite proprietà di configurazione. Ad esempio, è possibile usare espressioni come `containerName = "${dynamic.container.name}"` e `containerName = "#{@someBean.getContainerName()}"`.

Esistono due modi per eseguire il mapping di un campo di una classe di dominio al campo `id` di un documento di Azure Cosmos DB:

- Annotare il campo con `@Id`.
- Impostare il nome del campo su `id`.

L'esempio seguente mostra l'uso delle annotazioni `@Container` e `@Id`.

```java
@Container(containerName = "myContainer")
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

L'SDK supporta anche il partizionamento. Per altre informazioni, vedere [Partizionamento e scalabilità orizzontale in Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/partitioning-overview#:~:text=%20Partitioning%20and%20horizontal%20scaling%20in%20Azure%20Cosmos,partitions.%20Azure%20Cosmos%20DB%20transparently%20and...%20More%20). Per specificare un campo di una classe di dominio da usare come campo della chiave di partizione, annotarlo con `@PartitionKey`. Quindi, quando si eseguono operazioni CRUD, specificare il valore della partizione.

L'esempio seguente illustra come usare l'annotazione `@PartitionKey` durante l'esecuzione di operazioni CRUD.

```java
@Container(ru = "400")
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

    final Address newAddress = new Address("12345", "Seattle");

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

### <a name="pulling-configuration-properties-into-the-application"></a>Eseguire il pull delle proprietà di configurazione nell'applicazione

È possibile creare una classe di proprietà che espone le impostazioni di **application.properties** come metodi di accesso Java. La struttura di **application.properties** può essere:

```xml
cosmos.uri=${ACCOUNT_HOST}
cosmos.key=${ACCOUNT_KEY}
cosmos.secondaryKey=${SECONDARY_ACCOUNT_KEY}

# Populate query metrics
cosmos.queryMetricsEnabled=true
```

Rispecchiando questa struttura, creare una classe `CosmosProperties` Java strutturata come segue.

```java
@ConfigurationProperties(prefix = "cosmos")
public class CosmosProperties {

    private String uri;

    private String key;

    private String secondaryKey;

    private boolean queryMetricsEnabled;

    public String getUri() {
        return uri;
    }

    public void setUri(String uri) {
        this.uri = uri;
    }

    public String getKey() {
        return key;
    }

    public void setKey(String key) {
        this.key = key;
    }

    public String getSecondaryKey() {
        return secondaryKey;
    }

    public void setSecondaryKey(String secondaryKey) {
        this.secondaryKey = secondaryKey;
    }

    public boolean isQueryMetricsEnabled() {
        return queryMetricsEnabled;
    }

    public void setQueryMetricsEnabled(boolean enableQueryMetrics) {
        this.queryMetricsEnabled = enableQueryMetrics;
    }
}
```

Si noti che questa classe ha un membro che corrisponde a ogni proprietà di configurazione di **application.properties** e che per ogni membro `CosmosProperties` espone i metodi *get* e *set*. L'annotazione `@ConfigurationProperties` indica che la classe rappresenta proprietà di configurazione e l'argomento `prefix = "cosmos"` indica che un determinato *membro* di `CosmosProperties` è associato alla proprietà `cosmos.member` in **application.properties**.

Nella sezione seguente verrà illustrato come incorporare la classe `CosmosProperties` nel flusso di configurazione automatizzato. In fase di configurazione verrà creata un'istanza di `CosmosProperties` e i relativi metodi verranno popolati con le impostazioni di configurazione di **application.properties**. Questa istanza di proprietà consente all'applicazione di leggere e modificare le proprietà di configurazione in fase di esecuzione.

### <a name="configuring-the-application-based-on-properties"></a>Configurazione dell'applicazione in base a proprietà

Il passaggio successivo consiste nel creare una classe di configurazione che automatizza la configurazione dell'applicazione. Ecco un esempio, poi verrà descritto come viene creato

```java
@Configuration
@EnableConfigurationProperties(CosmosProperties.class)
@EnableCosmosRepositories
@EnableReactiveCosmosRepositories
@PropertySource("classpath:application.properties")
public class AppConfiguration extends AbstractCosmosConfiguration {

    private static final Logger logger = LoggerFactory.getLogger(QuickstartSampleConfiguration.class);

    @Autowired
    private CosmosProperties properties;

    private AzureKeyCredential azureKeyCredential;

    @Bean
    public CosmosClientBuilder cosmosClientBuilder() {
        this.azureKeyCredential = new AzureKeyCredential(properties.getKey());
        return new CosmosClientBuilder()
            .endpoint(properties.getUri())
            .key(this.azureKeyCredential)
    }

    @Bean
    public CosmosConfig cosmosConfig() {
        DirectConnectionConfig directConnectionConfig = DirectConnectionConfig.getDefaultConfig();        
        return CosmosConfig.builder()
                           .responseDiagnosticsProcessor(new ResponseDiagnosticsProcessorImplementation())
                           .enableQueryMetrics(properties.isQueryMetricsEnabled())
                           .directMode(directConnectionConfig);                           
                           .build();
    }

    @Override
    protected String getDatabaseName() {
        return "testdb";
    }

    private static class ResponseDiagnosticsProcessorImplementation implements ResponseDiagnosticsProcessor {

        @Override
        public void processResponseDiagnostics(@Nullable ResponseDiagnostics responseDiagnostics) {
            logger.info("Response Diagnostics {}", responseDiagnostics);
        }
    }

    public void switchToSecondaryKey() {
        this.cosmosKeyCredential.key(secondaryKey);
    }
}
```

Per creare la struttura della classe di configurazione:

1. Estendere la classe `AbstractCosmosConfiguration` per impostare la configurazione dell'applicazione (chiave, URL, nome del database Cosmos DB e così via).
1. Aggiungere l'annotazione `@Configuration`.
1. A seconda dell'utilizzo del repository, aggiungere una o entrambe le annotazioni `@EnableCosmosRepositories` e `@EnableReactiveCosmosRepositories`.
1. Aggiungere l'annotazione `@PropertySource("classpath:application.properties")`, che indica di estrarre coppie chiave-valore delle proprietà da **application.properties**
1. Aggiungere l'annotazione `@EnableConfigurationProperties`, che punta Spring Data a una classe in grado di archiviare le coppie chiave-valore di **application.properties**. Questa annotazione accetta la definizione di classe come argomento. Passare `CosmosProperties.class`.

La classe di configurazione utilizzerà i membri seguenti:

1. Dichiarare e definire un membro `logger` log4j2 che verrà utilizzato da Spring Data per tutti gli output dei log
1. Dichiarare un membro `@Autowired` `CosmosProperties`, **che è quello in cui verranno depositate le impostazioni di application.properties**

La funzionalità `AzureKeyCredential` consente di alternare le chiavi in tempo reale. Per abilitarla, definire un membro `AzureKeyCredential`. È possibile cambiare chiave aggiungendo un metodo `switchToSecondaryKey`, come illustrato nell'esempio precedente.

Successivamente, è necessario definire come verrà implementata la configurazione automatizzata.
1. Definire un metodo `@Bean` `cosmosClientBuilder` per gestire l'inizializzazione del client usando `CosmosClientBuilder`. Lo scopo di questo metodo è eseguire la configurazione di base del client, ossia la specifica dell'URI dell'endpoint e della chiave di accesso. In genere, l'URI dell'endpoint e la chiave di accesso dell'account si definiscono in **application.properties** e verranno poi popolati in `properties`. È possibile inizializzare il membro `azureKeyCredential` con `properties.getKey()` e quindi inserire `properties.getUri()` e `this.azureKeyCredential` rispettivamente nei metodi del generatore `endpoint` e `key`. 

Si noti che nell'esempio precedente `cosmosClientBuilder` non chiama `build()` nel generatore client, ma restituisce la struttura del generatore non finalizzata. Spring Data consente di eseguire la configurazione in due fasi: prima di tutto `cosmosClientBuilder` può applicare la configurazione di base e restituire la relativa struttura, poi Spring Data chiamerà un metodo `cosmosConfig` che consente di definire una configurazione più avanzata, ad esempio con metriche e diagnostica. Questa configurazione avanzata nel metodo `cosmosConfig` verrà descritta di seguito:
1. Creare un metodo `@Bean` `cosmosConfig` come illustrato sopra.
1. Azure Cosmos DB può restituire la diagnostica lato server associata a ogni richiesta. Spring Data consente di trasformare l'output della diagnostica non elaborata prima che venga registrato, definendo un processore di diagnostica personalizzato. Come illustrato sopra, definire una classe che implementa `ResponseDiagnosticsProcessor` ed esegue l'override del metodo `processResponseDiagnostics`. È possibile definire `processResponseDiagnostics` per controllare come viene gestito l'output di diagnostica. L'esempio precedente registra la diagnostica non elaborata.
1. Per abilitare la diagnostica e inizializzare il relativo processore, chiamare il metodo del generatore `responseDiagnosticsProcessor`, passando una nuova istanza della classe del processore personalizzato:

    ```java
    return CosmosConfig.builder()
                       .responseDiagnosticsProcessor(new ResponseDiagnosticsProcessorImplementation())
    ```
1. Azure Cosmos DB include anche una funzionalità più specifica per le metriche delle prestazioni delle query. Come illustrato nella sezione precedente, la procedura consigliata consiste nell'avere un'impostazione di **application.properties** che abilita/disabilita le metriche delle query. Applicare questa impostazione di configurazione associando il metodo del generatore `.enableQueryMetrics(properties.isQueryMetricsEnabled())` in `cosmosConfig`.
1. La connettività in modalità diretta è la scelta consigliata per ridurre al minimo la latenza e massimizzare la velocità effettiva, in modo che sia possibile applicare questa configurazione anche nel generatore client.

Una volta completata la configurazione avanzata in `cosmosConfig`, è necessario attivare la creazione del client chiamando `build()` nella struttura di configurazione. Viene generata un'istanza del client Azure Cosmos DB basata sulle impostazioni di configurazione.

L'ultimo passaggio per definire il processo di configurazione consiste nell'aggiungere un metodo `@Override` `getDatabaseName()` che restituisce il nome del database Azure Cosmos DB come stringa.

### <a name="customizing-the-configuration"></a>Personalizzazione della configurazione

È anche possibile personalizzare la configurazione per cambiare la modalità di connessione, le dimensioni massime del pool di connessione, il timeout delle richieste e così via, come mostrato nell'esempio seguente.

```java
    @Bean
    public CosmosConfig cosmosConfig() {

        // Set the connection mode to Direct (TCP) which applies to data plane operations
        DirectConnectionConfig directConnectionConfig = DirectConnectionConfig.getDefaultConfig(); 

        // Even in Direct mode, some control plane operations always pass through the gateway as HTTP requests (i.e. container/database CRUD.)
        // Optionally, you can customize connection properties for these specific operations which are
        // always Gateway mode
        GatewayConnectionConfig gatewayConnectionConfig = GatewayConnectionConfig.getDefaultConfig(); 

        // Set the maximum number of HTTP connections to 1000 per application.
        gatewayConnectionConfig.setMaxConnectionPoolSize(1000);

        // Set the request timeout to 10 seconds.
        gatewayConnectionConfig.setIdleConnectionTimeout(Duration.ofMillis(10000));

        return CosmosConfig.builder()
                           .responseDiagnosticsProcessor(new ResponseDiagnosticsProcessorImplementation())
                           .enableQueryMetrics(properties.isQueryMetricsEnabled())
                           .directMode(directConnectionConfig, gatewayConnectionConfig); // directMode() has an override which accepts Gateway config                        
                           .build();
    }
```

### <a name="response-diagnostics-and-query-metrics"></a>Diagnostica delle risposte e metriche di query

Spring Data Azure Cosmos DB SDK supporta la stringa di diagnostica di risposta e le metriche delle query a partire dalla versione 2.

Per abilitare le metriche di query, impostare il flag `queryMetricsEnabled` su **true** nel file `application.properties`. Seguire quindi la procedura descritta nella sezione precedente per estendere l'interfaccia `ResponseDiagnosticsProcessor` e implementare il metodo `processResponseDiagnostics` per registrare le informazioni di diagnostica. Infine, passare un'istanza dell'implementazione al metodo `CosmosDbConfig.setResponseDiagnosticsProcessor`.

### <a name="pagination-and-sorting"></a>Paginazione e ordinamento

Spring Data Azure Cosmos DB SDK supporta la paginazione e l'ordinamento di Spring Data. Per altre informazioni, vedere [Gestione dei parametri speciali](https://docs.spring.io/spring-data/commons/docs/current/reference/html/#repositories.special-parameters) nella documentazione di Spring.

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

Le sezioni seguenti descrivono i problemi che è necessario tenere presenti quando si usa Spring Data Azure Cosmos DB SDK.

### <a name="getting-the-correct-cosmos-db-configuration"></a>Recupero della configurazione corretta di Cosmos DB

L'estensione dell'interfaccia `AbstractCosmosConfiguration` può essere complessa a causa delle varie annotazioni e configurazioni presenti nella classe. Il problema più comune riguarda l'annotazione `Enable Repositories`.

Se i repository estendono `CosmosRepository`, assicurarsi di aggiungere l'annotazione `@EnableCosmosRepositories`. Se i repository estendono `ReactiveCosmosRepository`, assicurarsi di aggiungere l'annotazione `@EnableReactiveCosmosRepositories`. L'esempio seguente illustra l'uso di queste annotazioni.

```java
@Configuration
@EnableConfigurationProperties(CosmosProperties.class)
@EnableCosmosRepositories
@EnableReactiveCosmosRepositories
@PropertySource("classpath:application.properties")
public class TestRepositoryConfig extends AbstractCosmosConfiguration {
    ...
}
```

Quando si crea o si personalizza un bean `CosmosDBConfig`, assicurarsi di usare l'oggetto `AzureKeyCredential` invece di usare direttamente la chiave.

La funzionalità `AzureKeyCredential` consente di alternare le chiavi in tempo reale. È possibile cambiare le chiavi usando il metodo `switchToSecondaryKey`.

`AzureKeyCredential` deve essere un oggetto singleton perché Cosmos DB SDK usa lo stesso oggetto internamente per rilevare le modifiche nel valore della chiave all'interno di questo oggetto.

### <a name="custom-query-execution"></a>Esecuzione di query personalizzate

Spring Data Azure Cosmos DB SDK 3.x.x supporta l'annotazione `@query` per definire le query personalizzate.

Il codice seguente illustra un semplice esempio di come eseguire query di offset e di limite usando l'annotazione `@query`:

```java
@Repository
public interface SampleRepository extends CosmosRepository<SampleEntity, String> {

    ...

    @Query(value = "SELECT * from c OFFSET @skipCount LIMIT @takeCount")
    List<SampleEntity> findByName(@Param("skipCount") int skipCount, @Param("takeCount") int takeCount);
}
```

### <a name="enable-diagnostics-and-query-metrics"></a>Abilitare la diagnostica e le metriche di query

Quando si esegue il debug, è utile avere la stringa di diagnostica delle risposte e le metriche di query di Cosmos DB SDK. Cosmos DB SDK registra la stringa di diagnostica delle risposte sul lato client. Il back-end registra le metriche di query e le fornisce a Cosmos DB SDK.

Il metodo `ResponseDiagnosticsProcessor.processResponseDiagnostics` viene chiamato dopo ogni chiamata API in Spring Data Azure Cosmos DB SDK. È quindi importante che l'implementazione garantisca prestazioni elevate senza bug ed evitando la complessità. Ad esempio, non è consigliabile registrare il set completo di informazioni di diagnostica in questo metodo, perché la quantità di informazioni coinvolte creerebbe un costo significativo per le prestazioni. È inoltre consigliabile usare il livello di registrazione `Debug` per evitare di influire sulle prestazioni dell'applicazione.

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

### <a name="naming-changes"></a>Modifiche relative all'assegnazione dei nomi

Le versioni 3.1.0 e successive di Spring Data Azure Cosmos DB SDK includono le modifiche di rilievo seguenti ai nome e alle interfacce di classi, metodi, annotazioni e artefatti Maven:
* Aggiornato l'ID gruppo a `com.azure`.
* Aggiornato l'ID artefatto a `azure-spring-data-cosmos`.
* Aggiornati i tipi restituiti delle API di sincronizzazione ai tipi `Iterable` invece di `List`.
* Da `CosmosDbFactory` a `CosmosFactory`.
* Da `CosmosDBConfig` a `CosmosConfig`.
* Da `CosmosDBAccessException` a `CosmosAccessException`.
* Dall'annotazione `Document` a `Container`.
* Dall'annotazione `DocumentIndexingPolicy` a `CosmosIndexingPolicy`.
* Da `DocumentQuery` a `CosmosQuery`.
* Dal flag `populateQueryMetrics` di **application.properties** a `queryMetricsEnabled`.

### <a name="key-bug-fixes"></a>Principali correzioni di bug

Nelle versioni 3.1.0 e successive di Spring Data Azure Cosmos DB SDK sono state applicate le principali correzioni di bug seguenti
* Corretto un problema per cui le query annotate non selezionano il nome del contenitore annotato.
* Pianificata l'attività di registrazione diagnostica su thread paralleli per evitare di bloccare i thread di I/O Netty.
* Corretto il blocco ottimistico sull'operazione di eliminazione.
* Corretto un problema di escape delle query per la clausola IN.
* Corretto un problema consentendo il tipo di dati long per @Id.
* Corretto un problema consentendo i tipi di dati boolean, long, int, double per l'annotazione @PartitionKey.
* Corrette le parole chiave IgnoreCase e AllIgnoreCase per le query che ignorano le maiuscole/minuscole.
* Rimosso il valore predefinito 4000 delle unità richiesta durante la creazione automatica di contenitori.
* Corretto un bug della chiave di partizione annidata durante l'uso dell'annotazione @GeneratedValue.

### <a name="api-or-query-slowness"></a>Lentezza di API o query

Se si riscontrano latenze elevate con l'esecuzione di API o query, provare a registrare le stringhe di diagnostica e le metriche di query come descritto nella sezione [Abilitare la diagnostica e le metriche di query](#enable-diagnostics-and-query-metrics). Verificare l'utilizzo della CPU, la larghezza di banda di rete e lo spazio su disco di I/O, che possono essere la causa radice della lentezza sul lato client.