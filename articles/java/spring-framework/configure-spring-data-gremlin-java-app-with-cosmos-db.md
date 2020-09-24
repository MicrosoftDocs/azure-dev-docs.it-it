---
title: Come usare Spring Data Gremlin Starter con l'API SQL di Azure Cosmos DB
description: Informazioni su come configurare un'applicazione creata con Spring Boot Initializr con l'API SQL di Azure Cosmos DB.
services: cosmos-db
documentationcenter: java
ms.date: 08/03/2020
ms.service: cosmos-db
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: data-services
ms.custom: devx-track-java
ms.openlocfilehash: 775e547f61f4f4b2c649505607cf4d7310d0c6c3
ms.sourcegitcommit: 39f3f69e3be39e30df28421a30747f6711c37a7b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/21/2020
ms.locfileid: "90831837"
---
# <a name="how-to-use-the-spring-data-gremlin-starter-with-the-azure-cosmos-db-sql-api"></a>Come usare Spring Data Gremlin Starter con l'API SQL di Azure Cosmos DB

## <a name="overview"></a>Panoramica

Spring Data Gremlin Starter fornisce il supporto Spring Data per il linguaggio di query Gremlin di Apache, che gli sviluppatori possono usare con qualsiasi archivio dati compatibile con Gremlin.

Questo articolo descrive la creazione di un database di Azure Cosmos DB con il portale di Azure per l'uso con l'API Gremlin, l'uso di **[Spring Initializr]** per creare un'applicazione Java personalizzata e quindi aggiungere la funzionalità di Spring Data Gremlin Starter all'applicazione personalizzata per l'archiviazione e il recupero di dati da Azure Cosmos DB con Gremlin.

## <a name="prerequisites"></a>Prerequisites

I prerequisiti seguenti sono necessari per seguire le procedure disponibili in questo articolo:

* Sottoscrizione di Azure; se non si ha una sottoscrizione di Azure, è possibile attivare i [vantaggi per i sottoscrittori di MSDN] oppure iscriversi per ottenere un [account Azure gratuito].
* Java Development Kit (JDK) supportato. Per altre informazioni sulle versioni di JDK utilizzabili per lo sviluppo in Azure, vedere <https://aka.ms/azure-jdks>.
* [Apache Maven](http://maven.apache.org/), versione 3.0 o versione successiva.


## <a name="create-an-azure-cosmos-db-account"></a>Creare un account Azure Cosmos DB

### <a name="create-a-cosmos-db-account-using-the-azure-portal"></a>Creare un account Cosmos DB con il portale di Azure

1. Passare al portale di Azure all'indirizzo <https://portal.azure.com/> e selezionare `+Create a resource`.

   >[!div class="mx-imgBorder"]
   >![create-a-resource][create-a-resource-01]

1. Selezionare `Databases` e quindi `Azure Cosmos DB`.

   >[!div class="mx-imgBorder"]
   >![create-azure-cosmos-db][create-a-resource-02]

1. Nella pagina `Azure Cosmos DB` immettere le informazioni seguenti:

   * Selezionare scegliere la `Subscription` da usare per il database.
   * Specificare se creare un nuovo `Resource Group` per il database o sceglierne uno esistente.
   * Immettere un `Account Name` univoco che verrà usato come parte dell'URI Gremlin del database. Ad esempio, se è stato immesso `account-sample` per `Account Name`, l'URI Gremlin sarà `account-samplewingtiptoysdata.gremlin.cosmosdb.azure.com`.
   * Scegliere `Gremlin (Graph)` come API.
   * Specificare il `Location` per il database.
   
1. Dopo avere specificato queste opzioni, selezionare `Review + create`.

   >[!div class="mx-imgBorder"]
   >![create-azure-cosmos-db-account][create-a-resource-03]

1. Esaminare i valori specificati e selezionare `Create` per creare il database.

1. Dopo avere creato il database, selezionare **Vai alla risorsa**. Il database viene elencato anche nel **Dashboard** di Azure e nelle pagine **Tutte le risorse** e **Azure Cosmos DB**. È possibile selezionare il database in una di queste posizioni per aprire la pagina delle proprietà per la cache.

1. Quando viene visualizzata la pagina delle proprietà per il database, selezionare **Chiavi** e copiare le chiavi di accesso e l'URI per il database. Questi valori verranno usati nell'applicazione Spring Boot.

### <a name="add-a-graph-to-your-azure-cosmos-database"></a>Aggiungere un grafo al database di Azure Cosmos DB

1. Nella pagina Cosmos DB selezionare `Data Explorer`, quindi `New Graph`.

   >[!div class="mx-imgBorder"]
   >![new-graph][create-a-graph-01]

1. Quando viene visualizzata l'opzione `Add Graph`, immettere le informazioni seguenti:

   * Specificare un `Database id` univoco per il database.
   * È possibile specificare una `Storage capacity` personalizzata oppure accettare l'impostazione predefinita.
   * Specificare un `Graph id` univoco per il grafo.
   * Specificare una `Partition key`. Per altre informazioni, vedere [partizione del grafo]. Selezionare `OK`.
   
   Dopo avere specificato queste opzioni, selezionare `OK` per creare il grafo.

   >[!div class="mx-imgBorder"]
   >![add-graph][create-a-graph-02]

1. Dopo aver creato il grafo, è possibile usare `Data Explorer` per visualizzarlo.

   >[!div class="mx-imgBorder"]
   >![graph-detail][create-a-graph-03]
   
   

## <a name="create-a-simple-spring-boot-application-with-the-spring-initializr"></a>Creare un'applicazione Spring Boot semplice con Spring Initializr

1. Passare a <https://start.spring.io/>.

1. Specificare che si vuole generare un progetto **Maven** con **Java**, immettere i nomi per l'applicazione in **Group** (Gruppo) e **Artifact** (Elemento), specificare la versione 2.3.1 come versione di **Spring Boot** e quindi selezionare **GENERATE** (Genera).

> [!NOTE]
>
> Spring Initializr usa i nomi in **Group** (Gruppo) e **Artifact** (Artefatto) per creare il nome del pacchetto, ad esempio `com.example.wintiptoysdata`.


   >[!div class="mx-imgBorder"]
   >![spring-initializr][spring-initializr-01]

1. Quando richiesto, scaricare il progetto in un percorso nel computer locale.

1. Dopo aver estratto i file nel sistema locale, importarli nell'IDE.


## <a name="configure-your-spring-boot-app-to-use-the-spring-data-gremlin-starter"></a>Configurare l'app Spring Boot per l'uso di Spring Data Gremlin Starter

Verranno replicate le configurazioni dell'[esempio Azure Spring Data Gremlin](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/spring/azure-spring-boot-samples/azure-spring-data-sample-gremlin) esistente. Passare all'esempio e seguire i passaggi descritti in questa sezione per configurare l'app Spring Boot.

1. Individuare il file *pom.xml* nella directory dell'app, ad esempio:

   *C:\SpringBoot\wingtiptoysdata\pom.xml*

   -oppure-

   */users/example/home/wingtiptoysdata/pom.xml*

1. Aprire il file *pom.xml* e aggiungere Spring Data Gremlin Starter all'elenco di `<dependencies>`:

   ```xml
   <dependency>
      <groupId>com.azure</groupId>
      <artifactId>azure-spring-data-gremlin</artifactId>
      <version>2.3.1-beta.1</version> <!-- {x-version-update;com.azure:azure-spring-data-gremlin;current} -->
    </dependency>
   ```

1. Salvare e chiudere il file *pom.xml*.

1. Passare alla cartella *src/test/* ed eliminare tutto il contenuto.

1. Passare alla cartella *src/main/java* nell'app di esempio e copiare e sovrascrivere la stessa directory nell'app Spring Boot locale.

1. Nel file *src/main/resources/application.properties* aggiornare le configurazioni in modo da includere:

   | Campo              | Descrizione                                                                                                                                                                                                             |
   |--------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
   | `endpoint`         | Specifica l'URI Gremlin per il database, derivante dall'**ID** univoco specificato quando è stata creata l'istanza di Azure Cosmos DB in un passaggio precedente di questa guida di avvio rapido.                                                 |
   | `port`             | Specifica la porta TCP/IP, che deve essere **443** per HTTPS.                                                                                                                                                           |
   | `username`         | Specifica l'**ID database** e l'**ID grafo** univoci usati quando è stato aggiunto il grafo in un passaggio precedente di questa guida di avvio rapido. Il valore deve essere immesso con la sintassi "/dbs/ **{Database ID}** /colls/ **{Graph ID}** ". |
   | `password`         | Specifica la **chiave di accesso** primaria o secondaria copiata in un passaggio precedente di questa guida di avvio rapido.                                                                                                                      |
   | `sslEnabled`       | Specifica se abilitare o meno SSL.                                                                                                                                                                                           |
   | `telemetryAllowed` | Specificare **true** se si vuole abilitare la telemetria, altrimenti **false**.
   | `maxContentLength` | Specifica la lunghezza massima del contenuto.                                                                                                                                                                                           |

## <a name="build-and-run-the-project"></a>Compilare ed eseguire il progetto

1. Compilare l'applicazione Spring Boot con Maven ed eseguirla, ad esempio:

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

1. Se l'app viene avviata correttamente, è possibile esaminare il grafo nel portale di Azure:

   >[!div class="mx-imgBorder"]
   >![execute-result][execute-result-01]


## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni su Spring in Azure, continuare con la documentazione di Spring in Azure.

> [!div class="nextstepaction"]
> [Spring in Azure](./index.yml)

### <a name="additional-resources"></a>Risorse aggiuntive

Per altre informazioni sul supporto di Azure per Gremlin e l'API Graph, vedere gli articoli seguenti:

* [Introduzione ad Azure Cosmos DB: API Graph](/azure/cosmos-db/graph-introduction)

* [Supporto Gremlin Graph di Azure Cosmos DB](/azure/cosmos-db/gremlin-support)

* [Azure Cosmos DB: Creare un database a grafo con Java e il portale di Azure](/azure/cosmos-db/create-graph-java)

* [Esercitazione: Eseguire query nell'API Graph di Azure Cosmos DB con Gremlin](/azure/cosmos-db/tutorial-query-graph)

* [Spring Data Gremlin Starter]

Per altre informazioni sull'utilizzo di Azure Cosmos DB e Java, vedere gli articoli seguenti:

* [Documentazione di Azure Cosmos DB].

* [Azure Cosmos DB: Create a document database using Java and the Azure portal][Build a SQL API app with Java] (Aure Cosmos DB: creare un database di documenti con Java e il portale di Azure)

* [Spring Data per l'API SQL Azure Cosmos DB]

Per altre informazioni sull'uso delle applicazioni Spring Boot in Azure, vedere gli articoli seguenti:

* [Distribuire un'applicazione Spring Boot nel servizio app di Azure](deploy-spring-boot-java-app-from-container-registry-using-maven-plugin.md)

* [Eseguire un'applicazione Spring Boot in un cluster Kubernetes nel servizio Azure Container](deploy-spring-boot-java-app-on-kubernetes.md)

Per altre informazioni sull'uso di Azure con Java, vedere [Azure per sviluppatori Java] e la documentazione relativa all'[uso di Azure DevOps e Java].

**[Spring Framework]** è una soluzione open source che consente agli sviluppatori Java di creare applicazioni di livello enterprise. Uno dei progetti più comuni che si basa su questa piattaforma è [Spring Boot], che fornisce un approccio semplificato per la creazione di applicazioni Java autonome. Per semplificare le operazioni iniziali con Spring Boot per gli sviluppatori, alcuni pacchetti Spring Boot di esempio sono disponibili all'indirizzo <https://github.com/spring-guides/>. Oltre a consentire di scegliere dall'elenco di progetti Spring Boot di base, **[Spring Initializr]** semplifica le operazioni iniziali degli sviluppatori per la creazione di applicazioni Spring Boot personalizzate.

<!-- URL List -->

[Documentazione di Azure Cosmos DB]: /azure/cosmos-db/
[Azure per sviluppatori Java]: ../index.yml
[Build a SQL API app with Java]: /azure/cosmos-db/create-sql-api-java 
[Spring Data per l'API SQL Azure Cosmos DB]: https://azure.microsoft.com/blog/spring-data-azure-cosmos-db-nosql-data-access-on-azure/
[Spring Data Gremlin Starter]: https://github.com/Microsoft/spring-data-gremlin
[Account Azure gratuito]: https://azure.microsoft.com/pricing/free-trial/
[Uso di Azure DevOps e Java]: /azure/devops/
[vantaggi per i sottoscrittori di MSDN]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/ (Framework di Spring)
[partizione del grafo]: /azure/cosmos-db/graph-partitioning
[azure-spring-data-sample-gremlin]: https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/spring/azure-spring-boot-samples/azure-spring-data-sample-gremlin

<!-- IMG List -->

[create-a-resource-01]: media/configure-spring-data-gremlin-java-app-with-cosmos-db/create-a-resource-01.png
[create-a-resource-02]: media/configure-spring-data-gremlin-java-app-with-cosmos-db/create-a-resource-02.png
[create-a-resource-03]: media/configure-spring-data-gremlin-java-app-with-cosmos-db/create-a-resource-03.png

[create-a-graph-01]: media/configure-spring-data-gremlin-java-app-with-cosmos-db/create-a-graph-01.png
[create-a-graph-02]: media/configure-spring-data-gremlin-java-app-with-cosmos-db/create-a-graph-02.png
[create-a-graph-03]: media/configure-spring-data-gremlin-java-app-with-cosmos-db/create-a-graph-03.png

[spring-initializr-01]: media/configure-spring-data-gremlin-java-app-with-cosmos-db/spring-initializr-01.png

[get-password-01]: media/configure-spring-data-gremlin-java-app-with-cosmos-db/get-password-01.png

[execute-result-01]: media/configure-spring-data-gremlin-java-app-with-cosmos-db/execute-result-01.png