---
title: 'Esercitazione: Distribuire contenitori Docker nel servizio app di Azure con Visual Studio Code'
description: "Passaggio 1 dell'esercitazione: introduzione e prerequisiti."
ms.topic: conceptual
ms.date: 09/12/2019
ms.custom: seo-python-october2019
ms.openlocfilehash: 94c6339c57746c84865a370a721fd94dfd30f625
ms.sourcegitcommit: e77f8f652128b798dbf972078a7b460ed21fb5f8
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/25/2019
ms.locfileid: "74466086"
---
# <a name="tutorial-deploy-docker-containers-to-azure-app-service-with-visual-studio-code"></a>Esercitazione: Distribuire contenitori Docker nel servizio app di Azure con Visual Studio Code

Questo articolo descrive come usare Visual Studio Code per distribuire un'immagine del contenitore nel [servizio app di Azure](https://azure.microsoft.com/services/app-service/containers/) da un registro contenitori, il tutto all'interno di Visual Studio Code.

Se si riscontrano problemi con uno qualsiasi dei passaggi descritti in questa esercitazione, è possibile segnalarli. Per inviare feedback, seguire il collegamento **Si è verificato un problema** alla fine di ogni articolo.

## <a name="prerequisites"></a>Prerequisiti

- Un [account Azure](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-docker-extension&mktingSource=vscode-tutorial-docker-extension)
- [Visual Studio Code](https://code.visualstudio.com/)
- Un contenitore idoneo caricato in un registro contenitori. Per informazioni dettagliate sulla creazione di un contenitore con un'app Web Python e sull'aggiunta a un registro, vedere ad esempio [Creare un contenitore](https://code.visualstudio.com/docs/python/tutorial-create-containers).
- [Estensione Servizio app di Azure per VS Code](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice).
- [Estensione Docker per VS Code](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker).

## <a name="sign-in-to-azure"></a>Accedere ad Azure

[!INCLUDE [azure-sign-in](includes/azure-sign-in.md)]

> [!div class="nextstepaction"]
> [L'accesso ad Azure è stato effettuato](tutorial-deploy-containers-02.md)

[Si è verificato un problema](https://www.research.net/r/PWZWZ52?tutorial=vscode-appservice-containers&step=01-verify-prerequisites)
