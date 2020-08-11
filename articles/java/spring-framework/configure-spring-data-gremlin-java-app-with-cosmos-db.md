---
title: Come usare Spring Data Gremlin Starter con l'API SQL di Azure Cosmos DB
description: Informazioni su come configurare un'applicazione creata con Spring Boot Initializer con l'API SQL di Azure Cosmos DB.
services: cosmos-db
documentationcenter: java
ms.date: 01/10/2020
ms.service: cosmos-db
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: data-services
ms.custom: devx-track-java
ms.openlocfilehash: 81a80d14e4cf371801cf75af1618048dda8775d7
ms.sourcegitcommit: b224b276a950b1d173812f16c0577f90ca2fbff4
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/05/2020
ms.locfileid: "87810624"
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


## <a name="create-resource"></a>Creare la risorsa

### <a name="create-azure-cosmos-db"></a>Creare l'istanza di Azure Cosmos DB

1. Passare al portale di Azure all'indirizzo <https://portal.azure.com/> e fare clic su `+Create a resource`.

   >[!div class="mx-imgBorder"]
   >![create-a-resource][create-a-resource-01]

1. Fare clic su `Databases`, quindi su `Azure Cosmos DB`.

   >[!div class="mx-imgBorder"]
   >![create-azure-cosmos-db][create-a-resource-02]

1. Nella pagina `Azure Cosmos DB` immettere le informazioni seguenti:

   * Selezionare scegliere la `Subscription` da usare per il database.
   * Specificare se creare un nuovo `Resource Group` per il database o sceglierne uno esistente.
   * Immettere un `Account Name` univoco che verrà usato come parte dell'URI Gremlin del database. Ad esempio, se è stato immesso `account-sample` per `Account Name`, l'URI Gremlin sarà `account-samplewingtiptoysdata.gremlin.cosmosdb.azure.com`.
   * Scegliere `Gremlin (Graph)` come API.
   * Specificare il `Location` per il database.
   
1. Dopo avere specificato queste opzioni, fare clic su `Review + create`.

   >[!div class="mx-imgBorder"]
   >![create-azure-cosmos-db-account][create-a-resource-03]

1. Esaminare i valori specificati e fare clic su `Create` per creare il database.

### <a name="add-a-graph-to-your-azure-cosmos-database"></a>Aggiungere un grafo al database di Azure Cosmos DB

1. Nella pagina Cosmos DB fare clic su `Data Explorer`, quindi su `New Graph`.

   >[!div class="mx-imgBorder"]
   >![new-graph][create-a-graph-01]

1. Quando viene visualizzata l'opzione `Add Graph`, immettere le informazioni seguenti:

   * Specificare un `Database id` univoco per il database.
   * È possibile specificare una `Storage capacity` personalizzata oppure accettare l'impostazione predefinita.
   * Specificare un `Graph id` univoco per il grafo.
   * Specificare una `Partition key`. Per altre informazioni, vedere [partizione del grafo].
Fare clic su `OK`.
   
   Dopo avere specificato queste opzioni, fare clic su `OK` per creare il grafo.

   >[!div class="mx-imgBorder"]
   >![add-graph][create-a-graph-02]

1. Dopo aver creato il grafo, è possibile usare `Data Explorer` per visualizzarlo.

   >[!div class="mx-imgBorder"]
   >![graph-detail][create-a-graph-03]
   
   

## <a name="create-simple-spring-boot-application-with-the-spring-initializr"></a>Creare un'applicazione Spring Boot semplice con Spring Initializr

1. Passare a <https://start.spring.io/>.

1. Specificare i metadati del progetto, quindi fare clic su `GENERATE`:

   >[!div class="mx-imgBorder"]
   >![spring-initializr][spring-initializr-01]

1. Decomprimere il file e quindi importarlo nell'IDE.


## <a name="update-code-according-to-the-sample-project"></a>Aggiornare il codice in base al progetto di esempio

Modificare il progetto come il progetto di esempio: [azure-spring-data-sample-gremlin].

1. Aggiungere la dipendenza di `azure-spring-data-gremlin`

1. Elimina tutto il contenuto in `src/test/`

1. Aggiungere tutti i file Java in `src/main/java`, esattamente come in questo esempio.

1. Aggiornare la configurazione in `src/main/resorces/application.properties`, dove:

   | Campo              | Descrizione                                                                                                                                                                                                             |
   |--------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
   | `endpoint`         | Specifica l'URI Gremlin per il database, derivante dall'**ID** univoco specificato quando è stata creata l'istanza di Azure Cosmos DB in un passaggio precedente dell'esercitazione.                                                 |
   | `port`             | Specifica la porta TCP/IP, che deve essere **443** per HTTPS.                                                                                                                                                           |
   | `username`         | Specifica l'**ID database** e l'**ID grafo** univoci usati quando è stato aggiunto il grafo in un passaggio precedente dell'esercitazione. Il valore deve essere immesso con la sintassi "/dbs/**{ID database}**/colls/**{ID grafo}**". |
   | `password`         | Specifica la **chiave di accesso** primaria o secondaria copiata in un passaggio precedente dell'esercitazione.                                                                                                                      |
   | `sslEnabled`       | Specifica se abilitare o meno SSL.                                                                                                                                                                                           |
   | `telemetryAllowed` | Specificare **true** se si vuole abilitare la telemetria, altrimenti **false**.
   | `maxContentLength` | Specifica la lunghezza massima del contenuto.                                                                                                                                                                                           |

1. Informazioni su come ottenere la password:

   >[!div class="mx-imgBorder"]
   >![get-password][get-password-01]

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

Per altre informazioni su Spring e Azure, passare al centro di documentazione di Spring in Azure.

> [!div class="nextstepaction"]
> [Spring in Azure](/azure/developer/java/spring-framework)

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
[Azure per sviluppatori Java]: /azure/developer/java/
[Build a SQL API app with Java]: /azure/cosmos-db/create-sql-api-java 
[Spring Data per l'API SQL Azure Cosmos DB]: https://azure.microsoft.com/blog/spring-data-azure-cosmos-db-nosql-data-access-on-azure/
[Spring Data Gremlin Starter]: https://github.com/Microsoft/spring-data-gremlin
[Account Azure gratuito]: https://azure.microsoft.com/pricing/free-trial/
[Uso di Azure DevOps e Java]: /azure/devops/
[vantaggi per i sottoscrittori di MSDN]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/ (Framework di Spring)
[partizione del grafo]: https://docs.microsoft.com/azure/cosmos-db/graph-partitioning
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
