---
title: file di inclusione azure-sign-in.md
description: file di inclusione azure-sign-in.md
ms.date: 10/13/2020
ms.topic: include
ms.custom: devx-track-javascript
ms.openlocfilehash: cc541400419bf3269a7bb31b36fab788271539f4
ms.sourcegitcommit: 5f64710b2b0822e789c7f15acba5a3a257c033f9
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/05/2020
ms.locfileid: "93405229"
---
Nei passaggi precedenti sono state create risorse di Azure in un gruppo di risorse. Il gruppo di risorse ha un nome simile a "appsvc_rg_Linux_CentralUS", a seconda della località scelta. Se si usa uno SKU del servizio app diverso dal livello F1 gratuito, per queste risorse verranno addebitati costi su base continuativa. Vedere [Prezzi di Servizio app](https://azure.microsoft.com/pricing/details/app-service/linux/).

Se non si prevede di usare queste risorse in futuro, eliminare i gruppi di risorse eseguendo questo comando:

```azurecli
az group delete --no-wait
```

Il comando usa il nome del gruppo di risorse memorizzato nella cache nel file *.azure/config*.

Con l'argomento `--no-wait`, il comando restituisce il risultato prima del completamento dell'operazione.