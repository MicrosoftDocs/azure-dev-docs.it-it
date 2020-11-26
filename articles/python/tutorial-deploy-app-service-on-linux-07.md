---
title: 'Passaggio 7: Pulire le risorse dopo la distribuzione nel Servizio app di Azure in Linux da Visual Studio Code'
description: "Passaggio 7 dell'esercitazione: pulizia delle risorse di Azure"
ms.topic: conceptual
ms.date: 11/20/2020
ms.custom: devx-track-python, seo-python-october2019
ms.openlocfilehash: e9fe39d97487cb58523e2fd33a8acb74c51aa00b
ms.sourcegitcommit: 29930f1593563c5e968b86117945c3452bdefac1
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/23/2020
ms.locfileid: "96035489"
---
# <a name="7-clean-up-resources-after-deploying-to-azure-app-service-on-linux-from-visual-studio-code"></a>7: Pulire le risorse dopo la distribuzione nel Servizio app di Azure in Linux da Visual Studio Code

[Passaggio precedente: Eseguire lo streaming dei log](tutorial-deploy-app-service-on-linux-06.md)

Il servizio app di Azure creato include un piano di servizio app di supporto che può comportare costi. Per evitare tali costi, eliminare il gruppo che contiene tutte queste risorse.

[!INCLUDE [delete-resource-group](includes/delete-resource-group.md)]

## <a name="next-steps"></a>Passaggi successivi

La procedura dettagliata per la distribuzione di codice Python nel Servizio app di Azure in Linux è stata completata.

Come indicato in precedenza, è possibile ottenere altre informazioni sull'estensione Servizio app visitando il relativo repository in GitHub, [vscode-azureappservice](https://github.com/Microsoft/vscode-azureappservice). Le segnalazioni di problemi e i contributi sono molto apprezzati.

Per altre informazioni sui servizi di Azure che è possibile usare da Python, tra cui l'archiviazione di dati con servizi di intelligenza artificiale e Machine Learning, visitare la pagina dedicata ad [Azure per sviluppatori Python](/python/azure/).

Sono inoltre disponibili altre estensioni di Azure per VS Code che possono risultare utili. È sufficiente cercare "Azure" nell'area delle estensioni:

![Estensioni di Azure per Visual Studio Code](media/deploy-azure/azure-extensions-for-visual-studio-code.png)

Alcune estensioni comuni sono:

- [Cosmos DB](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb)
- [Funzioni di Azure](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions)
- [Strumenti dell'interfaccia della riga di comando di Azure](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azurecli)
- [Azure Resource Manager Tools](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools)

> [!div class="nextstepaction"]
> [Fatto](/python/azure/) 

[Problemi? Segnalarli](https://aka.ms/FlaskVSCQuickstartHelp).
