---
ms.custom: devx-track-js
ms.topic: include
ms.date: 02/08/2021
ms.openlocfilehash: 8353ffea6f141b2bf25c726bfadd7f8926e552ce
ms.sourcegitcommit: 576c878c338d286060010646b96f3ad0fdbcb814
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/04/2021
ms.locfileid: "102118613"
---
## <a name="add-firewall-rule-for-your-client-ip-address"></a>Aggiungere una regola del firewall per l'indirizzo IP del client

Se è necessario aggiungere un indirizzo IP client alla risorsa dopo che è stata creata in modo che la connessione client al server con JavaScript abbia esito positivo, usare questa procedura. Usare il comando [AZ cosmosdb Update](/cli/azure/cosmosdb#az_cosmosdb_update) per aggiornare le regole del firewall.


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