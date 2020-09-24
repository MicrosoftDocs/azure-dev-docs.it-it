---
title: 'Passaggio 5: Pulire le risorse di Azure'
description: "Passaggio 5 dell'esercitazione: pulizia delle risorse di Azure per evitare di incorrere in addebiti ricorrenti."
ms.topic: conceptual
ms.date: 09/12/2019
ms.custom: devx-track-python, seo-python-october2019
ms.openlocfilehash: 9140f5b695fe4b20e1e9417fe1d5e6ad115bb4eb
ms.sourcegitcommit: 39f3f69e3be39e30df28421a30747f6711c37a7b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/21/2020
ms.locfileid: "90831417"
---
# <a name="5-clean-up-azure-resources"></a>5: Pulire le risorse di Azure

[Passaggio precedente: Eseguire lo streaming dei log](tutorial-deploy-containers-04.md)

Le risorse di Azure create in questa esercitazione possono comportare addebiti ricorrenti. Per evitare tali costi, eliminare il gruppo che contiene tutte queste risorse.

[!INCLUDE [delete-resource-group](includes/delete-resource-group.md)]

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sulle estensioni di Docker e del Servizio app, visitare i rispettivi repository in GitHub: [vscode-docker](https://github.com/Microsoft/vscode-docker) e [vscode-azureappservice](https://github.com/Microsoft/vscode-azureappservice). Le segnalazioni di problemi e i contributi sono molto apprezzati.

Per altre informazioni sui servizi di Azure che è possibile usare da Python, tra cui l'archiviazione di dati con servizi di intelligenza artificiale e Machine Learning, visitare la pagina dedicata ad [Azure per sviluppatori Python](/python/azure/?preserve-view=true&view=azure-python).

Sono inoltre disponibili altre estensioni di Azure per VS Code che possono risultare utili. È sufficiente cercare "Azure" nell'area delle estensioni:

![Estensioni di Azure per VS Code](media/deploy-containers/azure-extensions-for-visual-studio-code.png)

Alcune estensioni comuni sono:

- [Cosmos DB](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb)
- [Funzioni di Azure](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions)
- [Strumenti dell'interfaccia della riga di comando di Azure](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azurecli)
- [Strumenti di Azure Resource Manager](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools)

> [!div class="nextstepaction"]
> [Fatto](/python/azure/?preserve-view=true&view=azure-python)