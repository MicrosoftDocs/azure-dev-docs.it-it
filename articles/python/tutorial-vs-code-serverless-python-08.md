---
title: 'Passaggio 8: Pulire le risorse usate con il codice Python in Funzioni di Azure'
description: Passaggio 8 dell'esercitazione, pulizia delle risorse di Azure per evitare di incorrere in addebiti ricorrenti.
ms.topic: conceptual
ms.date: 01/15/2020
ms.custom: seo-python-october2019
ms.openlocfilehash: 1e8735f8cb1a3955fda50365a70274ae6c3c5230
ms.sourcegitcommit: 1bd9ec6a4115e9162e33b76a933869788e6ab702
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/31/2020
ms.locfileid: "80442146"
---
# <a name="8-clean-up-azure-resources-for-azure-functions"></a>8: Pulire le risorse di Azure per Funzioni di Azure

[Passaggio precedente: Aggiungere un binding di archiviazione](tutorial-vs-code-serverless-python-07.md)

Questo articolo illustra come rimuovere le risorse di Azure create in questa esercitazione. L'app per le funzioni di Azure creata con Visual Studio Code include risorse che possono comportare costi minimi. Per altre informazioni, vedere [Prezzi di Funzioni di Azure](https://azure.microsoft.com/pricing/details/functions/).

Il modo migliore per pulire le risorse consiste nell'eliminare il gruppo che contiene tutte le singole risorse usate in questa esercitazione. Le risorse includono l'app per le funzioni, l'account di archiviazione e il piano di servizio app di supporto.

[!INCLUDE [delete-resource-group](includes/delete-resource-group.md)]

In Visual Studio Code si può notare che il menu di scelta rapida nell'app per le funzioni nell'area **Azure: Funzioni** include il comando **Delete Function App** (Elimina app per le funzioni). Il comando elimina solo l'app per le funzioni, ma lascia invariate le altre risorse, che possono comportare costi ricorrenti.

## <a name="next-steps"></a>Passaggi successivi

La procedura dettagliata per la distribuzione di codice Python in Funzioni di Azure è stata completata. È ora possibile creare molte altre funzioni serverless.

Come indicato in precedenza, è possibile ottenere altre informazioni sull'estensione Funzioni visitando il relativo repository in GitHub, [vscode-azurefunctions](https://github.com/Microsoft/vscode-azurefunctions). Le segnalazioni di problemi e i contributi sono molto apprezzati.

Per esplorare i vari trigger che è possibile usare, vedere [Introduzione a Funzioni di Azure](/azure/azure-functions/functions-overview).

Per altre informazioni sui servizi di Azure che è possibile usare da Python, tra cui l'archiviazione di dati con servizi di intelligenza artificiale e Machine Learning, visitare la pagina dedicata ad [Azure per sviluppatori Python](/azure/python/?view=azure-python).

Sono inoltre disponibili altre estensioni di Azure per Visual Studio Code che possono risultare utili. È sufficiente cercare "Azure" nell'area delle estensioni:

![Estensioni di Azure per Visual Studio Code](media/tutorial-vs-code-serverless-python/azure-extensions-for-visual-studio-code.png)

Alcune estensioni comuni sono:

- [Cosmos DB](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb)
- [Servizio app di Azure](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice). Vedere l'esercitazione [Distribuire nel servizio app](tutorial-deploy-app-service-on-linux-01.md)
- [Strumenti dell'interfaccia della riga di comando di Azure](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azurecli)
- [Strumenti di Azure Resource Manager](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools)

> [!div class="nextstepaction"]
> [Fatto](https://docs.microsoft.com/python/azure/?view=azure-python)

[Si è verificato un problema](https://www.research.net/r/PWZWZ52?tutorial=vscode-functions-python&step=08-clean-up-resources)