---
title: Visualizzare i log nel portale di Azure
description: Informazioni su come visualizzare la registrazione con Monitoraggio di Azure e Application Insights.
ms.topic: tutorial
ms.date: 11/13/2020
ms.custom: devx-track-js
ms.openlocfilehash: fd0741cd58e8d2e882a352035cd3c4f9bca956d1
ms.sourcegitcommit: a2a51e0c6530eb5794a2fe667cf4c9a60b2a7470
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/13/2020
ms.locfileid: "94625070"
---
# <a name="6-view-logs-in-azure-portal"></a>6. Visualizzare i log nel portale di Azure

Questa sezione dell'esercitazione contiene informazioni su come visualizzare la registrazione con Monitoraggio di Azure e Application Insights. 

## <a name="count-of-traces-in-logs-with-azure-cli"></a>Numero di tracce nei log con l'interfaccia della riga di comando di Azure

Usare l'interfaccia della riga di comando di Azure per visualizzare rapidamente le parti importanti dei log. Ad esempio, usare il comando seguente per visualizzare il numero di tracce presenti nei log. 

Tenere presente che la traccia è stata aggiunta solo nella route `/trace`. Le chiamate alla radice dell'app Web non produrranno alcun log di traccia. 

```azurecli
az monitor app-insights metrics show \
    --resource-group rg-demo-vm-eastus \
    --app demoWebAppMonitor \
    --metric traces/count
```

La risposta ha un aspetto simile al seguente, e in questo esempio il conteggio totale è di 2 tracce: 

```console
{
  "value": {
    "end": "2020-11-11T21:33:40.311000+00:00",
    "interval": null,
    "segments": null,
    "start": "2020-11-11T20:33:40.311000+00:00",
    "traces/count": {
      "sum": 2
    }
  }
}
```

## <a name="view-application-traces-in-azure-portal"></a>Visualizzare le tracce dell'applicazione nel portale di Azure

Per visualizzare le tracce come un elenco, il metodo più semplice consiste nell'usare il portale di Azure. 

1. Nel Web browser aprire il [portale di Azure](https://ms.portal.azure.com/#blade/HubsExtension/BrowseAll).
1. Filtrare l'elenco di risorse in base al gruppo di risorse `rg-demo-vm-eastus`. 
1. Selezionare la risorsa `demoWebAppMonitor`. 
1. Selezionare **Log** nella sezione **Monitoraggio**. Se viene visualizzata una finestra popup in cui è possibile selezionare le query, selezionare **X** nell'angolo per chiudere il popup.
1. Selezionare l'elemento di **Application Insights** denominato **tracce** facendo doppio clic su di esso. In tal modo verrà aggiunto il nome alla finestra delle query. 
1. Eseguire la query selezionando il pulsante **Esegui**.
1. Le tracce personalizzate di Application Insights di Monitoraggio di Azure, dall'app Web, vengono visualizzate in un elenco.

    :::image type="content" source="../../media/tutorial-vm/azure-portal-application-insights-custom-trace.png" alt-text="Le tracce personalizzate di Application Insights di Monitoraggio di Azure, dall'app Web, vengono visualizzate in un elenco.":::

## <a name="next-step"></a>Passaggio successivo

> [!div class="nextstepaction"]
> [Pulire le risorse di Azure](clean-up-resources.md) 