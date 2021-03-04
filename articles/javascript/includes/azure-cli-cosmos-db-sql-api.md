---
ms.custom: devx-track-js
ms.topic: include
ms.date: 02/08/2021
ms.openlocfilehash: 4cca83242756b75161331116a4e0a563e0e0f073
ms.sourcegitcommit: 576c878c338d286060010646b96f3ad0fdbcb814
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/04/2021
ms.locfileid: "102118616"
---
## <a name="create-a-cosmos-db-resource-for-sql-api"></a>Creare una risorsa Cosmos DB per l'API SQL

Usare il comando [AZ cosmosdb create](/cli/azure/cosmosdb#az_cosmosdb_create) dell'interfaccia della riga di comando di Azure seguente nel [Azure cloud Shell](https://shell.azure.com) per creare una nuova risorsa Cosmos DB. Il completamento di questo comando può richiedere alcuni minuti. 

```azurecli
az cosmosdb create \
--subscription YOUR-SUBSCRIPTION-ID-OR-NAME \
--resource-group YOUR-RESOURCE-GROUP \
--name YOUR-RESOURCE-NAME \
--kind YOUR-DB-KIND \
--ip-range-filter YOUR-CLIENT-IP
```

Per abilitare l'accesso del firewall dal computer locale alla risorsa, sostituire `123.123.123.123` con l'indirizzo IP del client. Per configurare più indirizzi IP, usare un elenco delimitato da virgole.

[!INCLUDE [Azure CLI command - Cosmos DB Update - firewall IP range](azure-cli-cosmos-db-update-with-firewall.md)]

[!INCLUDE [Azure CLI command - Cosmos DB - get keys](azure-cli-cosmos-db-get-keys.md)]
