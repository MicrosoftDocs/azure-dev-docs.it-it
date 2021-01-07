---
title: 'Passaggio 8: Pulire le risorse usate con il codice Python serverless in Funzioni di Azure'
description: Passaggio 8 dell'esercitazione, pulizia delle risorse di Azure per evitare di incorrere in addebiti ricorrenti.
ms.topic: conceptual
ms.date: 11/30/2020
ms.custom: devx-track-python, seo-python-october2019, contperf-fy21q2
ms.openlocfilehash: 499d738e9f979a249b421287644b089530e5ef27
ms.sourcegitcommit: 4f9ce09cbf9663203c56f5b12ecbf70ea68090ed
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/06/2021
ms.locfileid: "97911431"
---
# <a name="8-clean-up-azure-resources-for-azure-functions"></a>8: Pulire le risorse di Azure per Funzioni di Azure

[Passaggio precedente: Aggiungere un binding di archiviazione](tutorial-vs-code-serverless-python-07.md)

L'app per le funzioni di Azure creata con Visual Studio Code durante questa esercitazione include risorse che possono comportare costi minimi. Per altre informazioni, vedere [Prezzi di Funzioni di Azure](https://azure.microsoft.com/pricing/details/functions/).

Il modo migliore per pulire le risorse consiste nell'eliminare il gruppo che contiene tutte le singole risorse usate in questa esercitazione. Le risorse includono l'app per le funzioni, l'account di archiviazione e il piano di servizio app di supporto.

[!INCLUDE [delete-resource-group](includes/delete-resource-group.md)]

In Visual Studio Code si può notare che il menu di scelta rapida nell'app per le funzioni nell'area **Azure: Funzioni** include il comando **Delete Function App** (Elimina app per le funzioni). Il comando elimina solo l'app per le funzioni, ma lascia invariate le altre risorse, che possono comportare costi ricorrenti.

## <a name="next-steps"></a>Passaggi successivi

La procedura dettagliata per la distribuzione di codice Python in Funzioni di Azure è stata completata. È ora possibile creare molte altre funzioni serverless.

Come indicato in precedenza, è possibile ottenere altre informazioni sull'estensione Funzioni visitando il relativo repository in GitHub, [vscode-azurefunctions](https://github.com/Microsoft/vscode-azurefunctions). Le segnalazioni di problemi e i contributi sono molto apprezzati.

Per esplorare i vari trigger che è possibile usare, vedere [Introduzione a Funzioni di Azure](/azure/azure-functions/functions-overview).

Per altre informazioni sui servizi di Azure che è possibile usare da Python, tra cui l'archiviazione di dati con servizi di intelligenza artificiale e Machine Learning, visitare la pagina dedicata ad [Azure per sviluppatori Python](./index.yml).

Sono inoltre disponibili altre estensioni di Azure per Visual Studio Code che possono risultare utili. È sufficiente cercare "Azure" nell'area delle estensioni:

![Estensioni di Azure per Visual Studio Code](media/tutorial-vs-code-serverless-python/azure-extensions-for-visual-studio-code.png)

Alcune estensioni comuni sono:

- [Cosmos DB](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb)
- [Servizio app di Azure](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice). Vedere l'esercitazione [Distribuire nel servizio app](tutorial-deploy-app-service-on-linux-01.md)
- [Strumenti dell'interfaccia della riga di comando di Azure](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azurecli)
- [Strumenti di Azure Resource Manager](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools)

> [!div class="nextstepaction"]
> [Fatto](/python/azure/?preserve-view=true&view=azure-python)

[Problemi? Segnalarli](https://aka.ms/python-functions-qs-ms-survey).
