---
title: Introduzione ai database di Azure
description: Informazioni sulle attività comuni per l'uso di qualsiasi database ospitato in Azure.
ms.topic: how-to
ms.date: 02/09/2021
ms.custom: devx-track-js
ms.openlocfilehash: 5d104dcbfe6aa89db6ee434fafcb2f4796de1b18
ms.sourcegitcommit: b380f6e637b47e6e3822b364136853e1d342d5cd
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/14/2021
ms.locfileid: "100487144"
---
# <a name="getting-started-with-databases-on-azure"></a>Introduzione ai database in Azure

La piattaforma cloud di Azure consente di usare i database di Azure (come servizi) o di portare il proprio database. Una volta configurati il server e il database, il codice esistente dovrà modificare solo le impostazioni di connessione. 

Quando si usa un database in Azure, è necessario eseguire diverse attività comuni per usare il database dall'app JavaScript. Scopri di più su come ottenere e usare il tuo database in Azure. 

## <a name="select-a-database-to-use-on-azure"></a>Selezionare un database da usare in Azure

Microsoft fornisce servizi gestiti per i database seguenti:

|Database|Servizio di Azure|Altre informazioni|
|--|--|--|
|Cassandra|[Azure Cosmos DB](/azure/cosmos-db/)|[Avvio rapido: Creare un'app Cassandra con Node.js SDK e Azure Cosmos DB](/azure/cosmos-db/create-cassandra-nodejs)|
|Gremlin|[Azure Cosmos DB](/azure/cosmos-db/)|[Avvio rapido: Creare un'applicazione Node.js usando l'account dell'API Gremlin di Azure Cosmos DB](/azure/cosmos-db/create-graph-nodejs)|
|MongoDB|[Azure Cosmos DB](/azure/cosmos-db/)|[Come sviluppare un'applicazione JavaScript con MongoDB in Azure](use-mongodb-as-cosmosdb.md)<br>[Avvio rapido: Eseguire la migrazione di un'app Web Node.js MongoDB esistente ad Azure Cosmos DB](/azure/cosmos-db/create-mongodb-nodejs)|
|MariaDB|[Database di Azure per MariaDB](/azure/mariadb/)|[Come sviluppare un'applicazione JavaScript con MariaDB in Azure](use-mariadb.md)|
|MySQL|[Database di Azure per MySQL](/azure/mysql/)|[Guida introduttiva: usare Node.js per connettersi ai dati ed eseguire query nel Database di Azure per MySQL](/azure/mysql/connect-nodejs)<br>[Come sviluppare un'applicazione JavaScript con MySQL in Azure](use-mysql-db.md)|
|PostgreSQL|[Database di Azure per PostgreSQL](/azure/postgresql/)|[Avvio rapido: Usare Node.js per connettersi ed eseguire query sui dati in Database di Azure per PostgreSQL - Server singolo](/azure/postgresql/connect-nodejs)<br>[Come sviluppare un'applicazione JavaScript con PostgreSQL in Azure](use-postgresql-db.md)|
|Redis|[Cache Redis di Azure](/azure/azure-cache-for-redis/)|[Avvio rapido: Usare la cache di Azure per Redis in Node.js](/azure/azure-cache-for-redis/cache-nodejs-get-started)|
|SQL|[Azure Cosmos DB](/azure/cosmos-db/)|[Avvio rapido: Usare Node.js per connettersi ai dati ed eseguire query da un account API SQL di Azure Cosmos DB](/azure/cosmos-db/create-sql-api-nodejs)|
|Tabelle|[Azure Cosmos DB](/azure/cosmos-db/)|[Come usare l'archiviazione tabelle di Azure o l'API Tabelle di Azure Cosmos DB da Node.js](/azure/cosmos-db/table-storage-how-to-use-nodejs)|

**Serve aiuto per la scelta?** 
* Selezionare il database in base a [ciò che si desidera eseguire](https://azure.microsoft.com/product-categories/databases/)
* Usare il [servizio migrazione del database di Azure](/azure/dms/) per passare ad Azure. 

**Il database non è stato trovato?**
Portare il database come contenitore o macchina virtuale. È possibile importare qualsiasi tipo di database con questi servizi e avere disponibilità elevata e sicurezza per le altre risorse di Azure. Il compromesso è che è necessario gestire l'infrastruttura (contenitore o macchina virtuale) autonomamente. Il resto di questo documento può essere utile per il contenitore o la macchina virtuale, ma è più utile quando si sceglie un servizio di database di Azure. 

## <a name="create-the-server"></a>Creare il server

La creazione di un server viene completata creando una risorsa per il servizio di Azure specifico nella sottoscrizione in cui è ospitato il database. 

La creazione di una risorsa viene eseguita con:

|Strumento|Scopo|
|--|--|
|Portale di Azure|L'utilizzo di per il database prima o raramente creato è il portale di Azure.|
|Interfaccia della riga di comando di Azure|Utilizzabile per scenari ripetibili o con script.|
|Estensione Visual Studio Code (per il servizio)|Usare per restare nell'IDE di sviluppo.|
|libreria ARM NPM (per il servizio)|Usare per rimanere all'interno del linguaggio JavaScript.| 

Una volta creato il server, a seconda del servizio, potrebbe essere ancora necessario:

* Configurare le impostazioni di sicurezza, ad esempio il firewall e l'imposizione SSL
* Ottenere le informazioni di connessione
* Creare il database

## <a name="configure-security-settings-for-your-database"></a>Configurare le impostazioni di sicurezza per il database

Le impostazioni di sicurezza comuni da configurare per il servizio includono:

* Apertura del firewall per l'indirizzo IP del client
* Configurazione dell'imposizione SSL
* Accettazione di richieste pubbliche o richiesta di tutte le richieste provenienti da un altro servizio di Azure

## <a name="create-a-database-on-the-azure-server"></a>Creare un database nel server di Azure

È possibile ottenere le informazioni di connessione utilizzando lo stesso strumento creato per il server. Utilizzare le informazioni di connessione per accedere al server. È comunque necessario creare un database specifico per l'applicazione. 

Accedere al server: 

* Usare uno strumento specifico per il tipo di database, ad esempio pgAdmin, SQL Server Management Studio e MySQL Workbench. 
* Continua a usare gli strumenti Microsoft
    * [Azure cloud Shell](https://shell.azure.com) include molti interfacce della riga di database, ad esempio psql e MySQL.
    * Estensione di Visual Studio Code
    * pacchetti NPM per JavaScript
    * Portale di Azure

## <a name="programmatically-access-the-server-and-database-with-javascript"></a>Accedere a livello di codice al server e al database con JavaScript

Una volta fornite le informazioni di connessione, è possibile accedere al server con i pacchetti NPM standard del settore e JavaScript. 

Dopo la creazione o la migrazione di un database, è necessario modificare solo le informazioni di connessione al nuovo server e al nuovo database. 

## <a name="configure-an-azure-web-apps-connection-to-database"></a>Configurare la connessione di un'app Web di Azure al database

Se l'app Web di Azure si connette al database, è necessario modificare l'impostazione dell'app per le informazioni di connessione. 

## <a name="next-steps"></a>Passaggi successivi

* [Sviluppare un'applicazione JavaScript con MongoDB in Azure](use-mongodb-as-cosmosdb.md)