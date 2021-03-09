---
ms.custom: devx-track-js
ms.topic: include
ms.date: 02/08/2021
ms.openlocfilehash: e820cb17038a5251e658c9b7286cec65ddc40fca
ms.sourcegitcommit: b0a119a624e9cb6b76d968951543a414bd08eaa0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/09/2021
ms.locfileid: "102118269"
---
## <a name="create-a-cosmos-db-resource-for-mongodb"></a>Creare una risorsa Cosmos DB per MongoDB

Usare il comando [AZ cosmosdb create](/cli/azure/cosmosdb#az_cosmosdb_create) dell'interfaccia della riga di comando di Azure seguente nel [Azure cloud Shell](https://shell.azure.com) per creare una nuova risorsa Cosmos DB per un database MongoDB. 

```azurecli
az cosmosdb create \
    --subscription YOUR-SUBSCRIPTION-ID-OR-NAME \
    --resource-group YOUR-RESOURCE-GROUP \
    --name YOUR-RESOURCE_NAME \
    --locations regionName=eastus \
    --kind MongoDB \
    --enable-public-network true \
    --ip-range-filter 123.123.123.123 
```

Sostituire `123.123.123.123` con l'indirizzo IP del client o rimuovere completamente il parametro. 

Il completamento di questo comando può richiedere alcuni minuti e creare una risorsa disponibile pubblicamente nell' `eastus` area. 

```text
{
  "apiProperties": {
    "serverVersion": "3.6"
  },
  "capabilities": [
    {
      "name": "EnableMongo"
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
  "documentEndpoint": "https://mongo-2.documents.azure.com:443/",
  "enableAnalyticalStorage": false,
  "enableAutomaticFailover": false,
  "enableCassandraConnector": null,
  "enableFreeTier": false,
  "enableMultipleWriteLocations": false,
  "failoverPolicies": [
    {
      "failoverPriority": 0,
      "id": "mongodb-2",
      "locationName": "East US"
    }
  ],
  "id": "/subscriptions/.../resourceGroups/my-resource-group/providers/Microsoft.DocumentDB/databaseAccounts/mongo-2",
  "ipRules": [
    {
      "ipAddressOrRange": "123.123.123.123"
    }
  ],
  "isVirtualNetworkFilterEnabled": false,
  "keyVaultKeyUri": null,
  "kind": "MongoDB",
  "location": "Central US",
  "locations": [
    {
      "documentEndpoint": "https://mongodb-2.documents.azure.com:443/",
      "failoverPriority": 0,
      "id": "mongodb-2",
      "isZoneRedundant": false,
      "locationName": "East US",
      "provisioningState": "Succeeded"
    }
  ],
  "name": "mongo-2",
  "privateEndpointConnections": null,
  "provisioningState": "Succeeded",
  "publicNetworkAccess": "Enabled",
  "readLocations": [
    {
      "documentEndpoint": "https://mongodb-2.documents.azure.com:443/",
      "failoverPriority": 0,
      "id": "mongodb-2",
      "isZoneRedundant": false,
      "locationName": "East US",
      "provisioningState": "Succeeded"
    }
  ],
  "resourceGroup": "my-resource-group",
  "systemData": {
    "createdAt": "2021-02-08T20:21:05.9519342Z"
  },
  "tags": {},
  "type": "Microsoft.DocumentDB/databaseAccounts",
  "virtualNetworkRules": [],
  "writeLocations": [
    {
      "documentEndpoint": "https://mongodb-2.documents.azure.com:443/",
      "failoverPriority": 0,
      "id": "mongodb-2",
      "isZoneRedundant": false,
      "locationName": "East US",
      "provisioningState": "Succeeded"
    }
  ]
}
```

## <a name="add-firewall-rule-for-your-client-ip-address"></a>Aggiungere una regola del firewall per l'indirizzo IP del client

Per impostazione predefinita, le regole del firewall non sono configurate. È necessario aggiungere l'indirizzo IP del client in modo che la connessione client al server con JavaScript abbia esito positivo.

Usare il comando [AZ cosmosdb Update](/cli/azure/cosmosdb#az_cosmosdb_update) per aggiornare le regole del firewall.

```azurecli
az cosmosdb update \
    --subscription YOUR-SUBSCRIPTION-ID-OR-NAME \
    --resource-group YOUR-RESOURCE-GROUP \
    --name YOUR-RESOURCE_NAME \
    --ip-range-filter 123.123.123.123
```

Per configurare più indirizzi IP, usare un elenco delimitato da virgole.

```azurecli
az cosmosdb update \
    --subscription YOUR-SUBSCRIPTION-ID-OR-NAME \
    --resource-group YOUR-RESOURCE-GROUP \
    --name YOUR-RESOURCE_NAME \
    --ip-range-filter 123.123.123.123,456.456.456.456
```

## <a name="get-the-mongodb-connection-string-for-your-resource"></a>Ottenere la stringa di connessione MongoDB per la risorsa

Recuperare la stringa di connessione MongoDB per questa istanza con il comando [AZ cosmosdb keys List](/cli/azure/cosmosdb/keys#az_cosmosdb_keys_list) :

```azurecli
az cosmosdb keys list \
    --subscription YOUR-SUBSCRIPTION-ID-OR-NAME \
    --resource-group YOUR-RESOURCE-GROUP \
    --name YOUR-RESOURCE-NAME \
    --type connection-strings 
```

Restituisce 4 stringhe di connessione, 2 di lettura/scrittura e 2 di sola lettura. Ci sono due in modo che sia possibile assegnare a due diversi sistemi o sviluppatori una stringa di connessione da usare singolarmente. 

Connettersi al database mongoDB con una stringa di connessione. Verificare che il servizio sia disponibile con uno dei seguenti elementi:

* disponibile pubblicamente
* impostazioni del firewall per l'indirizzo IP del client

## <a name="configure-your-azure-web-app-with-the-connection-string"></a>Configurare l'app Web di Azure con la stringa di connessione

Aggiungere una variabile di ambiente **MONGODB_URL** app Web di Azure con il comando [AZ webapp config appSettings impostato](/cli/azure/webapp/config/appsettings#az_webapp_config_appsettings_set) in modo che l'app Web si connetta alla risorsa Cosmos DB:

```azurecli
az webapp config appsettings set \
    --subscription YOUR-SUBSCRIPTION-ID-OR-NAME \
    --resource-group YOUR-RESOURCE-GROUP \
    --name YOUR-RESOURCE_NAME \
    --settings MONGODB_URL=YOUR-CONNECTION-STRING
```
