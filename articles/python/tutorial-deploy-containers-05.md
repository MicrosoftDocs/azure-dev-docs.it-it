---
title: 'Passaggio 5: Pulire le risorse di Azure'
description: "Passaggio 5 dell'esercitazione: pulizia delle risorse di Azure per evitare di incorrere in addebiti ricorrenti."
ms.topic: conceptual
ms.date: 09/12/2019
ms.custom: seo-python-october2019
ms.openlocfilehash: df785e68de26fe4414430289800fdabfa8757eef
ms.sourcegitcommit: 1bd9ec6a4115e9162e33b76a933869788e6ab702
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/31/2020
ms.locfileid: "80441856"
---
# <a name="5-clean-up-azure-resources"></a>5: Pulire le risorse di Azure

[Passaggio precedente: Eseguire lo streaming dei log](tutorial-deploy-containers-04.md)

Le risorse di Azure create in questa esercitazione possono comportare addebiti ricorrenti. Per evitare tali costi, eliminare il gruppo che contiene tutte queste risorse.

[!INCLUDE [delete-resource-group](includes/delete-resource-group.md)]

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sulle estensioni di Docker e del Servizio app, visitare i rispettivi repository in GitHub: [vscode-docker](https://github.com/Microsoft/vscode-docker) e [vscode-azureappservice](https://github.com/Microsoft/vscode-azureappservice). Le segnalazioni di problemi e i contributi sono molto apprezzati.

Per altre informazioni sui servizi di Azure che è possibile usare da Python, tra cui l'archiviazione di dati con servizi di intelligenza artificiale e Machine Learning, visitare la pagina dedicata ad [Azure per sviluppatori Python](https://docs.microsoft.com/python/azure/?view=azure-python).

Sono inoltre disponibili altre estensioni di Azure per VS Code che possono risultare utili. È sufficiente cercare "Azure" nell'area delle estensioni:

![Estensioni di Azure per VS Code](media/deploy-containers/azure-extensions-for-visual-studio-code.png)

Alcune estensioni comuni sono:

- [Cosmos DB](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb)
- [Funzioni di Azure](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions)
- [Strumenti dell'interfaccia della riga di comando di Azure](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azurecli)
- [Strumenti di Azure Resource Manager](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools)

> [!div class="nextstepaction"]
> [Fatto](https://docs.microsoft.com/python/azure/?view=azure-python)

[Si è verificato un problema](https://www.research.net/r/PWZWZ52?tutorial=vscode-appservice-containers&step=07-clean-up-resources)