---
title: Pulire le risorse dopo la distribuzione nel Servizio app di Azure in Linux da Visual Studio Code
description: "Passaggio 7 dell'esercitazione: pulizia delle risorse di Azure"
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/12/2019
ms.author: kraigb
ms.openlocfilehash: a86207477deb4652ef1d98c1130329bc3e0c91fc
ms.sourcegitcommit: 74e28a479c87a3a53592646420b78e69852dd86a
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/16/2019
ms.locfileid: "71019609"
---
# <a name="clean-up-resources"></a>Pulire le risorse

[Passaggio precedente: Eseguire lo streaming dei log](tutorial-deploy-app-service-on-linux-06.md)

Il servizio app creato include un piano di servizio app sottostante che può comportare costi. Per pulire le risorse, fare clic con il pulsante destro del mouse sul servizio app nell'area **Azure: App Service** (Azure: Servizio app) e selezionare **Delete** (Elimina).

È anche possibile visitare il [portale di Azure](https://portal.azure.com), selezionare **Gruppi di risorse** nel riquadro di spostamento sinistro, selezionare il gruppo di risorse creato nella procedura di questa esercitazione e quindi usare il comando **Elimina gruppo di risorse**.

## <a name="next-steps"></a>Passaggi successivi

La procedura dettagliata per la distribuzione di codice Python nel Servizio app di Azure in Linux è stata completata.

Come indicato in precedenza, è possibile ottenere altre informazioni sull'estensione Servizio app visitando il relativo repository in GitHub, [vscode-azureappservice](https://github.com/Microsoft/vscode-azureappservice). Le segnalazioni di problemi e i contributi sono inoltre molto apprezzati.

Per altre informazioni sui servizi di Azure che è possibile usare da Python, tra cui l'archiviazione di dati con servizi di intelligenza artificiale e Machine Learning, visitare la pagina dedicata ad [Azure per sviluppatori Python](https://docs.microsoft.com/python/azure/?view=azure-python).

Sono inoltre disponibili altre estensioni di Azure per VS Code che possono risultare utili. È sufficiente cercare "Azure" nella finestra di esplorazione delle estensioni:

![Estensioni di Azure per VS Code](media/deploy-containers/azure-extensions.png)

Alcune estensioni comuni sono:

- [Cosmos DB](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb)
- [Funzioni di Azure](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions)
- [Strumenti dell'interfaccia della riga di comando di Azure](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azurecli)
- [Azure Resource Manager Tools](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools)

> [!div class="nextstepaction"]
> [Fatto](https://docs.microsoft.com/python/azure/?view=azure-python) 

[Si è verificato un problema](https://www.research.net/r/PWZWZ52?tutorial=vscode-appservice-python&step=07-clean-up-resources)
