---
ms.custom: devx-track-js
ms.topic: include
ms.date: 02/17/2021
ms.openlocfilehash: e37f278149f8173c3addddda085f37112929bf33
ms.sourcegitcommit: 576c878c338d286060010646b96f3ad0fdbcb814
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/04/2021
ms.locfileid: "102118629"
---
## <a name="create-a-cosmos-db-resource-for-cassandra-db"></a>Creare una risorsa Cosmos DB per il database Cassandra

Usare il comando [AZ cosmosdb create](/cli/azure/cosmosdb#az_cosmosdb_create) dell'interfaccia della riga di comando di Azure seguente nel [Azure cloud Shell](https://shell.azure.com) per creare una nuova risorsa per il database Cassandra. 

```azurecli
az cosmosdb create \
    --subscription YOUR-SUBSCRIPTION-ID-OR-NAME \
    --resource-group YOUR-RESOURCE-GROUP \
    --name YOUR-RESOURCE_NAME \
    --capabilities EnableCassandra
```

Il completamento di questo comando può richiedere alcuni minuti e creare una risorsa disponibile pubblicamente. Non è necessario configurare le regole del firewall per consentire l'indirizzo IP del client tramite. 

La risposta include i dettagli di configurazione del server, tra cui: 

* `location`: usato per il `localDataCenter` valore nel codice JavaScript con l'unità Cassandra 

```json
{
  "apiProperties": null,
  "capabilities": [
    {
      "name": "EnableCassandra"
    }
  ],
  "connectorOffer": null,
  "consistencyPolicy": {
    "defaultConsistencyLevel": "Session",
    "maxIntervalInSeconds": 5,
    "maxStalenessPrefix": 100
  },
  "cors": [],
  "databaseAccountOfferType": "Standard",
  "disableKeyBasedMetadataWriteAccess": false,
  "documentEndpoint": "https://YOUR-RESOURCE_NAME.documents.azure.com:443/",
  "enableAnalyticalStorage": false,
  "enableAutomaticFailover": false,
  "enableCassandraConnector": null,
  "enableFreeTier": false,
  "enableMultipleWriteLocations": false,
  "failoverPolicies": [
    {
      "failoverPriority": 0,
      "id": "YOUR-RESOURCE_NAME-centralus",
      "locationName": "Central US"
    }
  ],
  "id": "/subscriptions/YOUR-SUBSCRIPTION-ID-OR-NAME/resourceGroups/YOUR-RESOURCE-GROUP/providers/Microsoft.DocumentDB/databaseAccounts/YOUR-RESOURCE_NAME",
  "ipRules": [],
  "isVirtualNetworkFilterEnabled": false,
  "keyVaultKeyUri": null,
  "kind": "GlobalDocumentDB",
  "location": "Central US",
  "locations": [
    {
      "documentEndpoint": "https://YOUR-RESOURCE_NAME-centralus.documents.azure.com:443/",
      "failoverPriority": 0,
      "id": "YOUR-RESOURCE_NAME-centralus",
      "isZoneRedundant": false,
      "locationName": "Central US",
      "provisioningState": "Succeeded"
    }
  ],
  "name": "YOUR-RESOURCE_NAME",
  "privateEndpointConnections": null,
  "provisioningState": "Succeeded",
  "publicNetworkAccess": "Enabled",
  "readLocations": [
    {
      "documentEndpoint": "https://YOUR-RESOURCE_NAME-centralus.documents.azure.com:443/",
      "failoverPriority": 0,
      "id": "YOUR-RESOURCE_NAME-centralus",
      "isZoneRedundant": false,
      "locationName": "Central US",
      "provisioningState": "Succeeded"
    }
  ],
  "resourceGroup": "YOUR-RESOURCE-GROUP",
  "systemData": {
    "createdAt": "2021-02-16T23:00:23.0375775Z"
  },
  "tags": {},
  "type": "Microsoft.DocumentDB/databaseAccounts",
  "virtualNetworkRules": [],
  "writeLocations": [
    {
      "documentEndpoint": "https://YOUR-RESOURCE_NAME-centralus.documents.azure.com:443/",
      "failoverPriority": 0,
      "id": "YOUR-RESOURCE_NAME-centralus",
      "isZoneRedundant": false,
      "locationName": "Central US",
      "provisioningState": "Succeeded"
    }
  ]
}
```


## <a name="create-a-keyspace-on-the-server-with-azure-cli"></a>Creare uno spazio di spazio sul server con l'interfaccia della riga di comando di Azure

Usare l'interfaccia della riga di comando di Azure [AZ cosmosdb Cassandra DataSpace create](/cli/azure/cosmosdb/cassandra/keyspace#az_cosmosdb_cassandra_keyspace_create) nel [Azure cloud Shell](https://shell.azure.com) per creare una nuova area di lavoro di Cassandra nel server. 

```azurecli
az cosmosdb cassandra keyspace create \
    --subscription YOUR-SUBSCRIPTION-ID-OR-NAME \
    --resource-group YOUR-RESOURCE-GROUP \
    --account-name YOUR-RESOURCE_NAME \
    --name YOUR-KEYSPACE-NAME
```

## <a name="create-a-table-on-the-keyspace-with-azure-cli"></a>Creare una tabella con l'interfaccia della riga di comando di Azure

Usare l'interfaccia della riga di comando di Azure [AZ cosmosdb Cassandra Table Create](/cli/azure/cosmosdb/cassandra/table#az_cosmosdb_cassandra_table_create) nel [Azure cloud Shell](https://shell.azure.com) per creare una nuova area di lavoro di Cassandra nel server. 

```azurecli
az cosmosdb cassandra table create \
    --subscription YOUR-SUBSCRIPTION-ID-OR-NAME \
    --resource-group YOUR-RESOURCE-GROUP \
    --account-name YOUR-RESOURCE_NAME \
    --keyspace-name YOUR-KEYSPACE-NAME \
    --name YOUR-TABLE-NAME \
    --schema @schema.json
```

Il codice JSON del file di schema definisce le colonne della tabella, i tipi di dati e la chiave di partizione:

```json
{
    "columns": [
        {
            "name": "Name",
            "type": "Ascii"
        },
        {
            "name": "Alias",
            "type": "Ascii"
        },
        {
            "name": "Region",
            "type": "Ascii"
        }        
    ],
    "partitionKeys": [
        {
            "name": "Region"
        }
    ]
}
```

## <a name="get-the-cassandra-connection-string-with-azure-cli"></a>Ottenere la stringa di connessione Cassandra con l'interfaccia della riga di comando Azure
Recuperare la stringa di connessione MongoDB per questa istanza con il comando [AZ cosmosdb keys List](/cli/azure/cosmosdb/keys#az_cosmosdb_keys_list) :

```azurecli
az cosmosdb keys list \
    --subscription YOUR-SUBSCRIPTION-ID-OR-NAME \
    --resource-group YOUR-RESOURCE-GROUP \
    --name YOUR-RESOURCE-NAME \
    --type connection-strings 
```

Vengono restituite le stringhe di connessione. Il codice JSON seguente è un esempio di risultato con le informazioni di sicurezza sostituite da:

* `YOUR-RESOURCE-NAME`: usato per nome utente e parte del nome host o dell'endpoint dell'account
* `YOUR-USERNAME`: stesso valore di `YOUR-RESOURCE-NAME`
* `PASSWORD-1`
* `PASSWORD-2`
* `ACCOUNT-KEY-1`
* `ACCOUNT-KEY-2`

```json
{
    "connectionStrings": [
      {
        "connectionString": "AccountEndpoint=https://YOUR-RESOURCE-NAME.documents.azure.com:443/;AccountKey=PASSWORD-1;",
        "description": "Primary SQL Connection String"
      },
      {
        "connectionString": "AccountEndpoint=https://YOUR-RESOURCE-NAME.documents.azure.com:443/;AccountKey=PASSWORD-2;",
        "description": "Secondary SQL Connection String"
      },
      {
        "connectionString": "AccountEndpoint=https://YOUR-RESOURCE-NAME.documents.azure.com:443/;AccountKey=ACCOUNT-KEY-1;",
        "description": "Primary Read-Only SQL Connection String"
      },
      {
        "connectionString": "AccountEndpoint=https://YOUR-RESOURCE-NAME.documents.azure.com:443/;AccountKey=ACCOUNT-KEY-2;",
        "description": "Secondary Read-Only SQL Connection String"
      },
      {
        "connectionString": "HostName=YOUR-RESOURCE-NAME.cassandra.cosmos.azure.com;Username=YOUR-RESOURCE-NAME;Password=PASSWORD-1;Port=10350",
        "description": "Primary Cassandra Connection String"
      },
      {
        "connectionString": "HostName=YOUR-RESOURCE-NAME.cassandra.cosmos.azure.com;Username=YOUR-RESOURCE-NAME;Password=PASSWORD-2;Port=10350",
        "description": "Secondary Cassandra Connection String"
      },
      {
        "connectionString": "HostName=YOUR-RESOURCE-NAME.cassandra.cosmos.azure.com;Username=YOUR-RESOURCE-NAME;Password=ACCOUNT-KEY-1;Port=10350",
        "description": "Primary Read-Only Cassandra Connection String"
      },
      {
        "connectionString": "HostName=YOUR-RESOURCE-NAME.cassandra.cosmos.azure.com;Username=YOUR-RESOURCE-NAME;Password=ACCOUNT-KEY-2;Port=10350",
        "description": "Secondary Read-Only Cassandra Connection String"
      }
    ]
  }
```

Connettersi al database Cassandra con una stringa di connessione. Il nome utente Cassandra è il nome della risorsa. 