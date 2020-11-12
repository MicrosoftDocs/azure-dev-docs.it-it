---
title: file di inclusione azure-sign-in.md
description: file di inclusione azure-sign-in.md
ms.date: 10/13/2020
ms.topic: include
ms.custom: devx-track-javascript
ms.openlocfilehash: c8cf3e2f478265a81742093315e2a4041b2f0ed9
ms.sourcegitcommit: 5f64710b2b0822e789c7f15acba5a3a257c033f9
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/05/2020
ms.locfileid: "93405208"
---
Distribuire il codice nella cartella locale ( *python-docs-hello-world* ) usando il comando `az webapp up`:

```azurecli
az webapp up --sku F1 --name <app-name>
```

- Se il comando `az` non viene riconosciuto, verificare di aver installato l'interfaccia della riga di comando di Azure come descritto in [Configurare l'ambiente iniziale](../../quickstart-python-flask-multipage.yml?tutorial-step=1).
- Se il comando `webapp` non viene riconosciuto, assicurarsi che la versione dell'interfaccia della riga di comando di Azure sia 2.0.80 o successiva. Se non lo è, [installare l'ultima versione](/cli/azure/install-azure-cli).
- Sostituire `<app_name>` con un nome univoco nell'ambito di Azure ( *i caratteri validi sono `a-z`, `0-9` e `-`* ). Un criterio valido consiste nell'usare una combinazione del nome della società e di un identificatore dell'app.
- Con l'argomento `--sku F1` l'app Web viene creata nel piano tariffario Gratuito. Omettere questo argomento per usare un livello Premium più rapido, che però comporta un costo orario.
- Facoltativamente, è possibile includere l'argomento `--location <location-name>`, dove `<location_name>` è un'area di Azure disponibile. Per recuperare un elenco di aree consentite per l'account Azure, è possibile eseguire il comando [`az account list-locations`](/cli/azure/appservice#az-appservice-list-locations).
- Se viene visualizzato un messaggio di errore analogo a "Non è stato possibile rilevare automaticamente lo stack di runtime dell'app", assicurarsi che il comando venga eseguito nella cartella *python-docs-hello-world* (Flask) o *python-docs-hello-django* (Django) che contiene il file *requirements.txt*. Vedere l'articolo sulla [risoluzione dei problemi di rilevamento automatico con az webapp up](https://github.com/Azure/app-service-linux-docs/blob/master/AzWebAppUP/runtime_detection.md) (GitHub).

Il completamento del comando può richiedere alcuni minuti. Durante l'esecuzione, vengono visualizzati messaggi sulla creazione del gruppo di risorse, sul piano di servizio app e l'app di hosting, la configurazione della registrazione e quindi la distribuzione dello ZIP. Viene quindi visualizzato il messaggio che indica che è possibile avviare l'app all'indirizzo http://&lt;nome-app&gt;.azurewebsites.net, ovvero l'URL dell'app in Azure.

![Output di esempio del comando az webapp up](../../media/quickstart-python/az-webapp-up-output.png)

[!include [az webapp up command note](../app-service-web-az-webapp-up-note.md)]