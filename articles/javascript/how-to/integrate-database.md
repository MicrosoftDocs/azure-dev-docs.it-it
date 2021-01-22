---
title: Database con app Node.js in Azure
description: Azure offre diversi database che è possibile usare con le app Web e altre app Node.js.
ms.topic: how-to
ms.date: 12/08/2020
ms.custom: devx-track-js
ms.openlocfilehash: b39a7d3e39600081148893a68d3dbc064c1db380
ms.sourcegitcommit: 593d177cfb5f56f236ea59389e43a984da30f104
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/18/2021
ms.locfileid: "98561687"
---
# <a name="integrate-databases-in-nodejs-apps"></a>Integrare database nelle app Node.js

I database di Azure forniscono una soluzione eccellente per dati cloud gestiti, insieme a un'API nativa o un Azure SDK per la connessione al database. 

## <a name="database-and-data-storage-solutions-on-azure"></a>Soluzioni per database e archiviazione dei dati in Azure

La tabella seguente contiene collegamenti a diversi articoli per la connessione e l'uso di database di Azure con Node.js. Per un elenco con diverse opzioni di database a confronto, vedere [Database - Servizi di database intelligenti completamente gestiti](https://azure.microsoft.com/product-categories/databases/).

| Servizio | Avvio rapido | Pacchetto npm |
| --- | --- | --- |
| **SQL Server** in Cosmos DB| [Creare un'app Web Node.js con Azure Cosmos DB usando SQL Server](/azure/cosmos-db/create-sql-api-nodejs) | [@azure/cosmos](https://www.npmjs.com/package/@azure/cosmos) |
| **MongoDB** in Cosmos DB| [Creare un'app Web Node.js e MongoDB in Azure](/azure/app-service-web/app-service-web-tutorial-nodejs-mongodb-app) | Qualsiasi client MongoDB |
| **Cassandra** in Cosmos DB|[Creare un'app Cassandra con Node.js SDK e Azure Cosmos DB](/azure/cosmos-db/create-cassandra-nodejs)|[npm cassandra-driver](https://www.npmjs.com/package/cassandra-driver)|
| **Gremlin** in Cosmos DB|[Creare un'applicazione Node.js usando l'account dell'API Gremlin di Azure Cosmos DB](/azure/cosmos-db/create-graph-nodejs)|[npm gremlin](https://www.npmjs.com/package/gremlin)|
| **Cache Redis** in Cosmos DB| [Come usare Cache Redis di Azure con Node.js](/azure/redis-cache/cache-nodejs-get-started) | [npm redis](https://www.npmjs.com/package/redis)|
| **Database SQL di Azure** | [Usare Node.js per eseguire query su un database SQL di Azure](/azure/sql-database/sql-database-connect-query-nodejs) |[npm tedious](https://www.npmjs.com/package/tedious) |
| **MySQL** | [Usare Node.js per connettersi ai dati ed eseguire query sui dati](/azure/mysql/connect-nodejs) | [npm mysql](https://www.npmjs.com/package/mysql)|
| **PostgreSQL** | [Usare Node.js per connettersi ai dati ed eseguire query sui dati](/azure/postgresql/connect-nodejs) |[npm pg](https://www.npmjs.com/package/pg) |

## <a name="cosmos-db-connection-strings-with-azure-cli"></a>Stringhe di connessione di Cosmos DB con l'interfaccia della riga di comando di Azure

Usare il comando seguente, [az cosmosdb keys list](/cli/azure/cosmosdb#az-cosmosdb-list-connection-strings):

```azurecli-interactive
az cosmosdb keys list \
    -n $accountName \
    -g $resourceGroupName \
    --type connection-strings
```

## <a name="sql-connection-strings-with-azure-cli"></a>Stringhe di connessione di SQL con l'interfaccia della riga di comando di Azure

Usare il comando seguente, [az sql db show-connection-string](/cli/azure/sql/db#az_sql_db_show_connection_string):

```azurecli-interactive
az sql db show-connection-string \
    --client {ado.net, jdbc, odbc, php, php_pdo, sqlcmd} \
     [--auth-type {ADIntegrated, ADPassword, SqlPassword}] \
     [--ids] \
     [--name] \
     [--server] \
     [--subscription]
```

## <a name="mysql-username-and-password-with-azure-cli"></a>Nome utente e password di MySQL con l'interfaccia della riga di comando di Azure

Questi valori vengono impostati al [momento della creazione della risorsa](/cli/azure/mysql/server#az_mysql_server_create). 

## <a name="postgresql-username-and-password-with-azure-cli"></a>Nome utente e password di PostgreSQL con l'interfaccia della riga di comando di Azure

Questi valori vengono impostati al [momento della creazione della risorsa](/cli/azure/postgres/server#az_postgres_server_create). 

## <a name="azure-storage-solutions-for-files-and-data"></a>Soluzioni di Archiviazione di Azure per file e dati

È anche possibile usare Archiviazione di Azure per l'archiviazione di file (BLOB), tabelle e code (messaggi):

| Servizio | Avvio rapido |SDK consigliato |
| --- | --- |--- |
| **BLOB** | [Caricare, scaricare, elencare ed eliminare BLOB usando Archiviazione di Azure v10 SDK per JavaScript](/azure/storage/blobs/storage-quickstart-blobs-nodejs-v10) |[@azure/storage-blob](https://www.npmjs.com/package/@azure/storage-blob)|
| **Code** | [Come usare l'archiviazione di accodamento da Node.js](/azure/storage/queues/storage-nodejs-how-to-use-queues) |[npm @azure/storage-queue](https://www.npmjs.com/package/@azure/storage-queue)|
| **Tabelle** | [Come usare l'archiviazione tabelle da Node.js](/azure/cosmos-db/table-storage-how-to-use-nodejs) |[npm azure-storage](https://www.npmjs.com/package/azure-storage)|

## <a name="next-steps"></a>Passaggi successivi

* [Scrivere codice serverless](develop-serverless-apps.md) con Funzioni di Azure