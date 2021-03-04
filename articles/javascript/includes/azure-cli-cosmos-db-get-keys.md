---
ms.custom: devx-track-js
ms.topic: include
ms.date: 02/08/2021
ms.openlocfilehash: e3a7f10e6db4a805c136e0d04289da5333e77bc7
ms.sourcegitcommit: 576c878c338d286060010646b96f3ad0fdbcb814
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/04/2021
ms.locfileid: "102118614"
---
## <a name="get-the-cosmos-db-keys-for-your-resource"></a>Ottenere le chiavi di Cosmos DB per la risorsa

Recuperare le chiavi per questa istanza con il comando [AZ cosmosdb keys List](/cli/azure/cosmosdb/keys#az_cosmosdb_keys_list) :

```azurecli
az cosmosdb keys list \
    --subscription YOUR-SUBSCRIPTION-ID-OR-NAME \
    --resource-group YOUR-RESOURCE-GROUP \
    --name YOUR-RESOURCE-NAME
```

Restituisce 4 chiavi, 2 di lettura/scrittura e 2 di sola lettura. Ci sono due in modo che sia possibile assegnare a due diversi sistemi o sviluppatori una chiave da usare e riciclare singolarmente. 