---
title: 'Esercitazione: Distribuire app Python nel Servizio app di Azure in Linux da Visual Studio Code'
description: "Passaggio 1 dell'esercitazione: introduzione, prerequisiti e accesso ad Azure."
ms.topic: conceptual
ms.date: 09/12/2019
ms.custom: seo-python-october2019
ms.openlocfilehash: d5eed3d2b1aeea92b3681ada006b3723e67f70c4
ms.sourcegitcommit: e77f8f652128b798dbf972078a7b460ed21fb5f8
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/25/2019
ms.locfileid: "74466271"
---
# <a name="tutorial-deploy-python-apps-to-azure-app-service-on-linux-from-visual-studio-code"></a>Esercitazione: Distribuire app Python nel Servizio app di Azure in Linux da Visual Studio Code

Questo articolo illustra come usare Visual Studio Code per distribuire un'applicazione Python nel Servizio app di Azure in Linux usando l'estensione [Servizio app di Azure](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice).

Se si riscontrano problemi con uno qualsiasi dei passaggi descritti in questa esercitazione, è possibile segnalarli. Per inviare feedback, seguire il collegamento **Si è verificato un problema** alla fine di ogni articolo.

> [!TIP]
> [Servizio app di Azure in Linux](https://docs.microsoft.com/azure/app-service/containers/app-service-linux-intro) esegue il codice sorgente in un contenitore Docker predefinito. Il contenitore esegue le app con Python 3.7 usando il server Web [Gunicorn](https://gunicorn.org). Le caratteristiche di questo contenitore sono descritte in [Configurare app Python per il Servizio app di Azure in Linux](https://docs.microsoft.com/azure/app-service/containers/how-to-configure-python). La definizione del contenitore si trova in [github.com/Azure-App-Service/python](https://github.com/Azure-App-Service/python/tree/master/3.7).

## <a name="prerequisites"></a>Prerequisiti

- Una [sottoscrizione di Azure](#azure-subscription).
- [Visual Studio Code con l'estensione Servizio app di Azure](#visual-studio-code-python-and-the-azure-app-service-extension).
- Ambiente Python

### <a name="azure-subscription"></a>Sottoscrizione di Azure

Se non si ha una sottoscrizione di Azure, [iscriversi](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-appservice-extension&mktingSource=vscode-tutorial-appservice-extension) subito per ottenere un account gratuito con 200 USD in crediti di Azure per provare qualsiasi combinazione di servizi.

### <a name="visual-studio-code-python-and-the-azure-app-service-extension"></a>Visual Studio Code, Python e l'estensione Servizio app di Azure

Installare il software seguente:

- [Visual Studio Code](https://code.visualstudio.com/).
- Python e l'estensione [Python](https://marketplace.visualstudio.com/items?itemName=ms-python.python) come descritto nella sezione dei [prerequisiti dell'esercitazione su Python in VS Code](https://code.visualstudio.com/docs/python/python-tutorial).
- Estensione [Servizio app di Azure](vscode:extension/ms-azuretools.vscode-azureappservice), che consente di interagire con il Servizio app di Azure dall'interno di VS Code. Per informazioni di carattere generale, vedere l'[esercitazione sull'estensione del Servizio app](https://code.visualstudio.com/tutorials/app-service-extension/getting-started) sull'estensione del servizio app e il [repository GitHub vscode-azureappservice](https://github.com/Microsoft/vscode-azureappservice).

## <a name="sign-in-to-azure"></a>Accedere ad Azure

[!INCLUDE [azure-sign-in](includes/azure-sign-in.md)]

> [!div class="nextstepaction"]
> [L'accesso ad Azure è stato effettuato](tutorial-deploy-app-service-on-linux-02.md)

[Si è verificato un problema](https://www.research.net/r/PWZWZ52?tutorial=vscode-appservice-python&step=01-verify-prerequisites)
