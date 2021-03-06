---
title: Come usare l'API Apache Cassandra per Spring Data con Azure Cosmos DB
description: Informazioni su come usare l'API Apache Cassandra per Spring Data con Azure Cosmos DB.
services: cosmos-db
documentationcenter: java
ms.date: 10/13/2020
ms.service: cosmos-db
ms.tgt_pltfrm: multiple
ms.topic: article
ms.custom: devx-track-java
ms.openlocfilehash: 7879a47bdcbc9b1a4cf41210fc9fb49ad28d8dd8
ms.sourcegitcommit: 8e1d3a384ccb0e083589418d65a70b3a01afebff
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/12/2020
ms.locfileid: "94560342"
---
# <a name="how-to-use-spring-data-apache-cassandra-api-with-azure-cosmos-db"></a>Come usare l'API Apache Cassandra per Spring Data con Azure Cosmos DB

Questo articolo illustra la creazione di un'applicazione di esempio che usa [Spring Data] per archiviare e recuperare le informazioni con l'[API Cassandra di Azure Cosmos DB](/azure/cosmos-db/cassandra-introduction).

## <a name="prerequisites"></a>Prerequisiti

I prerequisiti seguenti sono necessari per completare le procedure disponibili in questo articolo:

* Sottoscrizione di Azure; se non si ha una sottoscrizione di Azure, è possibile attivare i [vantaggi per i sottoscrittori di MSDN] oppure iscriversi per ottenere un [account Azure gratuito].
* Java Development Kit (JDK) supportato. Per altre informazioni sulle versioni di JDK utilizzabili per lo sviluppo in Azure, vedere <https://aka.ms/azure-jdks>.
* [Apache Maven](http://maven.apache.org/), versione 3.0 o versione successiva.
* [Curl](https://curl.haxx.se/) o utilità HTTP simile per testare il funzionamento.
* Un client [Git](https://git-scm.com/downloads).

## <a name="create-an-azure-cosmos-db-account"></a>Creare un account Azure Cosmos DB

La procedura seguente crea e configura un account Cosmos nel portale di Azure.

### <a name="create-a-cosmos-db-account-using-the-azure-portal"></a>Creare un account Cosmos DB con il portale di Azure

> [!NOTE]
> 
> Per informazioni più dettagliate sulla creazione di account Azure Cosmos DB, vedere [Documentazione di Azure Cosmos DB](/azure/cosmos-db/).

1. Passare al portale di Azure all'indirizzo <https://portal.azure.com/> ed eseguire l'accesso.

1. Selezionare **Crea una risorsa**, quindi **Attività iniziali** e quindi **Azure Cosmos DB**.
    
   >[!div class="mx-imgBorder"]
   >![Creare un account di Azure Cosmos DB][COSMOSDB01]

1. Specificare le informazioni seguenti.

   - **Sottoscrizione** specificare la sottoscrizione di Azure da usare.
   - **Gruppo di risorse**: specificare se creare un nuovo gruppo di risorse o sceglierne uno esistente.
   - **Account name** (Nome dell'account): scegliere un nome univoco per l'account Cosmos DB. Tale nome verrà usato per creare un nome di dominio completo, ad esempio *wingtiptoyscassandra.documents.azure.com*.
   - **API**: Specificare *Cassandra* per questa esercitazione.
   - **Località**: specificare l'area geografica più vicina per il database.
   
   >[!div class="mx-imgBorder"]
   >![Specificare le impostazioni dell'account Cosmos DB][COSMOSDB02]
   
1. Dopo aver immesso tutte le informazioni riportate sopra, fare clic su **Rivedi e crea**.

1. Se tutte le informazioni contenute nella pagina visualizzata risultano corrette, fare clic su **Crea**.
   
   >[!div class="mx-imgBorder"]
   >![Rivedere le impostazioni dell'account Cosmos DB][COSMOSDB03]

La distribuzione del database richiederà qualche minuto.

### <a name="add-a-keyspace-to-your-azure-cosmos-db-account"></a>Aggiungere un keyspace all'account Azure Cosmos DB

1. Passare al portale di Azure all'indirizzo <https://portal.azure.com/> ed eseguire l'accesso.

1. Selezionare **Tutte le risorse** e quindi scegliere l'account Azure Cosmos DB appena creato.

1. Selezionare **Esplora dati**, selezionare la freccia giù infine **New Keyspace** (Nuovo keyspace). Immettere un identificatore univoco in **Keyspace id** (ID keyspace) e quindi selezionare **OK**.
    
   >[!div class="mx-imgBorder"]
   >![Selezionare New keyspace][COSMOSDB05]
   
   >[!div class="mx-imgBorder"]
   >![Creare un keyspace Cosmos DB][COSMOSDB05-1]

### <a name="retrieve-the-connection-settings-for-your-azure-cosmos-db-account"></a>Recuperare le impostazioni di connessione per l'account Azure Cosmos DB

1. Passare al portale di Azure all'indirizzo <https://portal.azure.com/> ed eseguire l'accesso.

1. Selezionare **Tutte le risorse** e quindi scegliere l'account Azure Cosmos DB appena creato.

1. Selezionare **Stringhe di connessione** e copiare i valori dei campi **Punto di contatto**, **Porta**, **Nome utente** e **Password primaria**. Questi valori verranno usati più avanti per configurare l'applicazione.
   
   >[!div class="mx-imgBorder"]
   >![Recuperare le impostazioni di connessione di Cosmos DB][COSMOSDB06]

## <a name="configure-the-sample-application"></a>Configurare l'applicazione di esempio

La procedura seguente configura l'applicazione di test.

1. Aprire una shell dei comandi e clonare il progetto di esempio con un comando git come quello riportato nell'esempio seguente:

   ```shell
   git clone https://github.com/Azure-Samples/spring-data-cassandra-on-azure.git
   ```

1. Individuare il file *application.properties* nella directory *resources* del progetto di esempio oppure creare il file se non esiste già.

1. Aprire il file *application.properties* in un editor di testo e aggiungere o configurare le righe seguenti nel file, sostituendo i valori di esempio con i valori appropriati dei passaggi precedenti:

   ```yaml
   spring.data.cassandra.contact-points=wingtiptoyscassandra.cassandra.cosmos.azure.com
   spring.data.cassandra.port=10350
   spring.data.cassandra.username=wingtiptoyscassandra
   spring.data.cassandra.password=********
   ```
   Dove:

   | Parametro | Descrizione |
   |---|---|
   | `spring.data.cassandra.contact-points` | Specifica il valore di **Punto di contatto** copiato in precedenza in questo articolo. |
   | `spring.data.cassandra.port` | Specifica il valore di **Porta** copiato in precedenza in questo articolo. |
   | `spring.data.cassandra.username` | Specifica il valore di **Nome utente** copiato in precedenza in questo articolo. |
   | `spring.data.cassandra.password` | Specifica il valore di **Password primaria** copiato in precedenza in questo articolo. |

1. Salvare e chiudere il file *application.properties*.

## <a name="package-and-test-the-sample-application"></a>Creare il pacchetto dell'applicazione di esempio e testarla 

Passare alla directory che contiene il file con estensione POM per compilare e testare l'applicazione.

1. Compilare l'applicazione di esempio con Maven. Ad esempio:

   ```shell
   mvn clean package
   ```

1. Avviare l'applicazione di esempio. Ad esempio:

   ```shell
   java -jar target/spring-data-cassandra-on-azure-0.1.0-SNAPSHOT.jar
   ```

1. Creare nuovi record usando `curl` al prompt dei comandi come negli esempi seguenti:

   ```shell
   curl -s -d "{\"name\":\"dog\",\"species\":\"canine\"}" -H "Content-Type: application/json" -X POST http://localhost:8080/pets

   curl -s -d "{\"name\":\"cat\",\"species\":\"feline\"}" -H "Content-Type: application/json" -X POST http://localhost:8080/pets
   ```

   L'applicazione dovrebbe restituire valori come i seguenti:

   ```shell
   Added Pet{id=60fa8cb0-0423-11e9-9a70-39311962166b, name='dog', species='canine'}.

   Added Pet{id=72c1c9e0-0423-11e9-9a70-39311962166b, name='cat', species='feline'}.
   ```

1. Recuperare tutti i record esistenti usando `curl` al prompt dei comandi come negli esempi seguenti:

   ```shell
   curl -s http://localhost:8080/pets
   ```

   L'applicazione dovrebbe restituire valori come i seguenti:

   ```json
   [{"id":"60fa8cb0-0423-11e9-9a70-39311962166b","name":"dog","species":"canine"},{"id":"72c1c9e0-0423-11e9-9a70-39311962166b","name":"cat","species":"feline"}]
   ```

## <a name="summary"></a>Riepilogo

In questa esercitazione è stata creata un'applicazione Java di esempio che usa Spring Data per archiviare e recuperare le informazioni con l'API Cassandra di Azure Cosmos DB.

## <a name="clean-up-resources"></a>Pulire le risorse

Quando le risorse create in questo articolo non sono più necessarie, usare il [portale di Azure](https://portal.azure.com/) per eliminarle in modo da evitare addebiti imprevisti.

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni su Spring e Azure, passare al centro di documentazione di Spring in Azure.

> [!div class="nextstepaction"]
> [Spring in Azure](./index.yml)

### <a name="additional-resources"></a>Risorse aggiuntive

Per altre informazioni sull'uso di Azure con Java, vedere [Azure per sviluppatori Java] e la documentazione relativa all'[uso di Azure DevOps e Java].

<!-- URL List -->

[Azure per sviluppatori Java]: ../index.yml
[Account Azure gratuito]: https://azure.microsoft.com/pricing/free-trial/
[Uso di Azure DevOps e Java]: /azure/devops/
[vantaggi per i sottoscrittori di MSDN]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Data]: https://spring.io/projects/spring-data
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[COSMOSDB01]: media/configure-spring-data-apache-cassandra-with-cosmos-db/create-cosmos-db-01.png
[COSMOSDB02]: media/configure-spring-data-apache-cassandra-with-cosmos-db/create-cosmos-db-02.png
[COSMOSDB03]: media/configure-spring-data-apache-cassandra-with-cosmos-db/create-cosmos-db-03.png
[COSMOSDB05]: media/configure-spring-data-apache-cassandra-with-cosmos-db/create-cosmos-db-05.png
[COSMOSDB05-1]: media/configure-spring-data-apache-cassandra-with-cosmos-db/create-cosmos-db-05-1.png
[COSMOSDB06]: media/configure-spring-data-apache-cassandra-with-cosmos-db/create-cosmos-db-06.png