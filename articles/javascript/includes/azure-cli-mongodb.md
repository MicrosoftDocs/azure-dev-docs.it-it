---
ms.custom: devx-track-js
ms.topic: include
ms.date: 02/02/2021
ms.openlocfilehash: fa874c825531f75bda57cb1e98f66b71b7b8a4ca
ms.sourcegitcommit: 54f976887d218aaabd94371e24809716da8cf86e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/04/2021
ms.locfileid: "99554217"
---
## <a name="create-a-cosmos-db-resource-for-mongodb"></a>Creare una risorsa Cosmos DB per MongoDB

Usare il comando [AZ cosmosdb create](/cli/azure/cosmosdb#az_cosmosdb_create) dell'interfaccia della riga di comando di Azure seguente nel [Azure cloud Shell](https://shell.azure.com) per creare una nuova risorsa Cosmosdb per un database MongoDB. 

```azurecli
az cosmosdb create \
    --subscription YOUR-SUBSCRIPTION-ID-OR-NAME \
    --resource-group YOUR-RESOURCE-GROUP \
    --name YOUR-RESOURCE_NAME \
    --enable-public-network true \
    --locations regionName=eastus \
    --kind MongoDB
```

Il completamento di questo comando può richiedere alcuni minuti e creare una risorsa disponibile pubblicamente nell' `eastus` area. 

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
