---
title: Pulire le risorse di Azure
description: Passaggio 8 dell'esercitazione, pulizia delle risorse di Azure per evitare di incorrere in addebiti ricorrenti.
services: functions
author: kraigb
manager: barbkess
ms.service: azure-functions
ms.topic: conceptual
ms.date: 09/12/2019
ms.author: kraigb
ms.openlocfilehash: 9c0b3b8b4a21975a849531d5c6560a291ed4b7f2
ms.sourcegitcommit: 74e28a479c87a3a53592646420b78e69852dd86a
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/16/2019
ms.locfileid: "71019889"
---
# <a name="clean-up-resources"></a>Pulire le risorse

[Passaggio precedente: Aggiungere un binding di archiviazione](tutorial-vs-code-serverless-python-07.md)

L'app per le funzioni creata include risorse che possono comportare costi minimi (vedere [Prezzi di Funzioni di Azure](https://azure.microsoft.com/pricing/details/functions/)). Per pulire le risorse, fare clic con il pulsante destro del mouse sull'app per le funzioni nell'area **Azure: Funzioni** e scegliere **Delete Function App** (Elimina app per le funzioni).

È anche possibile visitare il [portale di Azure](https://portal.azure.com), selezionare **Gruppi di risorse** nel riquadro di spostamento sinistro, selezionare il gruppo di risorse creato nella procedura di questa esercitazione e quindi usare il comando **Elimina gruppo di risorse**.

## <a name="next-steps"></a>Passaggi successivi

La procedura dettagliata per la distribuzione di codice Python in Funzioni di Azure è stata completata. È ora possibile creare molte altre funzioni serverless.

Come indicato in precedenza, è possibile ottenere altre informazioni sull'estensione Funzioni visitando il relativo repository in GitHub, [vscode-azurefunctions](https://github.com/Microsoft/vscode-azurefunctions). Le segnalazioni di problemi e i contributi sono molto apprezzati.

Per esplorare i vari trigger che è possibile usare, vedere [Introduzione a Funzioni di Azure](/azure/azure-functions/functions-overview.md).

Per altre informazioni sui servizi di Azure che è possibile usare da Python, tra cui l'archiviazione di dati con servizi di intelligenza artificiale e Machine Learning, visitare il [centro per sviluppatori Azure Python](/azure/python/?view=azure-python).

Sono inoltre disponibili altre estensioni di Azure per Visual Studio Code che possono risultare utili. È sufficiente cercare "Azure" nell'area delle estensioni:

![Estensioni di Azure per Visual Studio Code](media/tutorial-vs-code-serverless-python/azure-extensions.png)

Alcune estensioni comuni sono:

- [Cosmos DB](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb)
- [Servizio app di Azure](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice). Vedere l'esercitazione [Distribuire nel servizio app](tutorial-deploy-app-service-on-linux-01.md)
- [Strumenti dell'interfaccia della riga di comando di Azure](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azurecli)
- [Strumenti di Azure Resource Manager](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools)

> [!div class="nextstepaction"]
> [Fatto](https://docs.microsoft.com/python/azure/?view=azure-python)

[Si è verificato un problema](https://www.research.net/r/PWZWZ52?tutorial=vscode-functions-python&step=08-clean-up-resources)