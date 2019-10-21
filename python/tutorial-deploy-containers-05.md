---
title: 'Esercitazione: Pulire le risorse di Azure'
description: "Passaggio 5 dell'esercitazione: pulizia delle risorse di Azure per evitare di incorrere in addebiti ricorrenti."
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/12/2019
ms.author: kraigb
ms.custom: seo-python-october2019
ms.openlocfilehash: 0a3e04759573769d1ed00e59a294caddfc4ef0cc
ms.sourcegitcommit: 6012460ad8d6ff112226b8f9ea6da397ef77712d
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/11/2019
ms.locfileid: "72278726"
---
# <a name="tutorial-clean-up-azure-resources"></a>Esercitazione: Pulire le risorse di Azure

[Passaggio precedente: Eseguire lo streaming dei log](tutorial-deploy-containers-04.md)

Questo articolo illustra come rimuovere le risorse di Azure create durante la distribuzione di un'app nel servizio app di Azure con Visual Studio Code.

Le diverse risorse di Azure create in questa esercitazione possono comportare addebiti ricorrenti. Per pulirle, è opportuno visitare il [portale di Azure](https://portal.azure.com), selezionare **Gruppi di risorse** nel riquadro di spostamento sinistro, selezionare il gruppo di risorse creato nella procedura di questa esercitazione e quindi usare il comando **Elimina gruppo di risorse**.

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sulle estensioni di Docker e del Servizio app, visitare i rispettivi repository in GitHub: [vscode-docker](https://github.com/Microsoft/vscode-docker) e [vscode-azureappservice](https://github.com/Microsoft/vscode-azureappservice). Le segnalazioni di problemi e i contributi sono inoltre molto apprezzati.

Per altre informazioni sui servizi di Azure che è possibile usare da Python, tra cui l'archiviazione di dati con servizi di intelligenza artificiale e Machine Learning, visitare la pagina dedicata ad [Azure per sviluppatori Python](https://docs.microsoft.com/python/azure/?view=azure-python).

Sono inoltre disponibili altre estensioni di Azure per VS Code che possono risultare utili. È sufficiente cercare "Azure" nella finestra di esplorazione delle estensioni:

![Estensioni di Azure per VS Code](media/deploy-containers/azure-extensions-for-visual-studio-code.png)

Alcune estensioni comuni sono:

- [Cosmos DB](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb)
- [Funzioni di Azure](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions)
- [Strumenti dell'interfaccia della riga di comando di Azure](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azurecli)
- [Strumenti di Azure Resource Manager](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools)

> [!div class="nextstepaction"]
> [Fatto](https://docs.microsoft.com/python/azure/?view=azure-python)

[Si è verificato un problema](https://www.research.net/r/PWZWZ52?tutorial=vscode-appservice-containers&step=07-clean-up-resources)