---
title: file di inclusione azure-sign-in.md
description: file di inclusione azure-sign-in.md
ms.date: 10/13/2020
ms.topic: include
ms.custom: devx-track-javascript
ms.openlocfilehash: 5868697895ad067065627cca99f604d531dcb173
ms.sourcegitcommit: 5f64710b2b0822e789c7f15acba5a3a257c033f9
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/05/2020
ms.locfileid: "93405307"
---
È possibile accedere ai log della console generati dall'interno dell'app e del contenitore in cui è in esecuzione. I log includono tutti gli output generati tramite le istruzioni `print`.

Per trasmettere in streaming i log, eseguire il comando [az webapp log tail](/cli/azure/webapp/log?view=azure-cli-latest&preserve-view=true#az_webapp_log_tail):

```azurecli
az webapp log tail
```

È anche possibile includere il parametro `--logs` con il comando `az webapp up` per aprire automaticamente il flusso di log durante la distribuzione.

Aggiornare l'app nel browser per generare i log della console, che includeranno messaggi che descrivono le richieste HTTP per l'app. Se non viene visualizzato immediatamente un output, riprovare dopo 30 secondi.

È anche possibile esaminare i file di log nel browser all'indirizzo `https://<app-name>.scm.azurewebsites.net/api/logs/docker`.

Per interrompere lo streaming di log in qualsiasi momento, premere **CTRL**+**C** nel terminale.
