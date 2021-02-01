---
title: Creare e usare MongoDB in Azure
description: Creare una risorsa Cosmos DB e usarla per il database MongoDB.
ms.topic: how-to
ms.date: 01/28/2021
ms.custom: seo-javascript-october2019, devx-track-js
ms.openlocfilehash: 608da4261fc4de1e6f8c5329e772a223663cca70
ms.sourcegitcommit: 3f8aa923e4626b31cc533584fe3b66940d384351
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/01/2021
ms.locfileid: "99231555"
---
# <a name="create-and-use-mongodb-on-azure-with-cosmos-db"></a>Creare e usare MongoDB in Azure con Cosmos DB

Una risorsa Cosmos DB fornisce un MongoDB da usare con i pacchetti del provider MongoDB trovati in NPM. 

Cosmos DB è un database NoSQL completamente gestito, con replica geografica e prestazioni elevate, che fornisce un livello di compatibilità per MongoDB. È possibile puntare a un'app JavaScript esistente (o a qualsiasi client/strumento MongoDB) senza dover modificare alcun elemento tranne la stringa di connessione. 

## <a name="create-a-cosmos-db-resource-for-mongodb"></a>Creare una risorsa Cosmos DB per MongoDB

Usare il comando [AZ cosmosdb create](/cli/azure/cosmosdb#az_cosmosdb_create) seguente. Sostituire il segnaposto **-resource_name** con un valore univoco globale. Cosmos DB utilizza questo nome per generare l'URL del server del database.

```azurecli
az cosmosdb create \
    --resource-group YOUR-RESOURCE-GROUP \
    --name YOUR-RESOURCE_NAME \
    --enable-public-network true \
    --locations westus \
    --kind MongoDB
```

## <a name="get-the-mongodb-connection-string-for-your-resource"></a>Ottenere la stringa di connessione MongoDB per la risorsa

Recuperare la stringa di connessione MongoDB per questa istanza con il comando [AZ cosmosdb list-Connection-Strings](/cli/azure/cosmosdb#az_cosmosdb_list_connection_strings) :

```azurecli
az cosmosdb list-connection-strings \
--resource-group YOUR-RESOURCE-GROUP \
--name YOUR-RESOURCE_NAME \
-otsv --query "connectionStrings[0].connectionString"
```

## <a name="configure-your-web-app-with-the-connection-string"></a>Configurare l'app Web con la stringa di connessione

Aggiungere una variabile di ambiente **MONGODB_URL** app Web di Azure con il comando [AZ webapp config appSettings impostato](/cli/azure/webapp/config/appsettings#az_webapp_config_appsettings_set) in modo che l'app Web si connetta alla risorsa Cosmos DB:

```azurecli
az webapp config appsettings set \
    --settings MONGODB_URL=YOUR-CONNECTION-STRING
```

## <a name="next-steps"></a>Passaggi successivi

* [Configurare le impostazioni dell'app Web](../configure-web-app-settings.md)

