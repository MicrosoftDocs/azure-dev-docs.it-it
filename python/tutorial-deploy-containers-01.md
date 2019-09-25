---
title: Distribuire contenitori Docker nel servizio app di Azure con Visual Studio Code
description: Passaggio 1 dell'esercitazione, introduzione e prerequisiti.
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/12/2019
ms.author: kraigb
ms.openlocfilehash: 37df8bf4a84d13d6fc4e25ea6c91ef14b6f6f502
ms.sourcegitcommit: 74e28a479c87a3a53592646420b78e69852dd86a
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/16/2019
ms.locfileid: "71019489"
---
# <a name="deploy-containers-to-azure-app-service"></a>Distribuire contenitori nel servizio app di Azure

Questa esercitazione descrive come usare Visual Studio Code per distribuire un'immagine del contenitore nel [servizio app di Azure](https://azure.microsoft.com/services/app-service/containers/) da un registro contenitori, il tutto all'interno di Visual Studio Code.

Se si riscontrano problemi con uno qualsiasi dei passaggi descritti in questa esercitazione, è possibile segnalarli. Per inviare feedback, seguire il collegamento **Si è verificato un problema** alla fine di ogni articolo.

## <a name="prerequisites"></a>Prerequisiti

- Un [account Azure](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-docker-extension&mktingSource=vscode-tutorial-docker-extension)
- [Visual Studio Code](https://code.visualstudio.com/)
- Un contenitore idoneo caricato in un registro contenitori. Per informazioni dettagliate sulla creazione di un contenitore con un'app Web Python e sull'aggiunta a un registro, vedere ad esempio [Creare un contenitore](https://code.visualstudio.com/python/tutorial-create-containers).
- [Estensione Servizio app di Azure per VS Code](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice).
- [Estensione Docker per VS Code](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker).

## <a name="sign-in-to-azure"></a>Accedere ad Azure

[!INCLUDE [azure-sign-in](includes/azure-sign-in.md)]

> [!div class="nextstepaction"]
> [L'accesso ad Azure è stato effettuato](tutorial-deploy-containers-02.md)

[Si è verificato un problema](https://www.research.net/r/PWZWZ52?tutorial=vscode-appservice-containers&step=01-verify-prerequisites)