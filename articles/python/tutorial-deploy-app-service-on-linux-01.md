---
title: 'Esercitazione: Distribuire app Python nel Servizio app di Azure in Linux da Visual Studio Code'
description: "Passaggio 1 dell'esercitazione: configurare l'ambiente per il servizio app"
ms.topic: conceptual
ms.date: 09/12/2019
ms.custom: devx-track-python, seo-python-october2019
ms.openlocfilehash: b35fc41707b31bec8e889d2b60becdad56f4e7d9
ms.sourcegitcommit: 69933dcce571b2686897b295b7822e207d944617
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/18/2020
ms.locfileid: "90773074"
---
# <a name="tutorial-deploy-python-apps-to-azure-app-service-on-linux-from-visual-studio-code"></a>Esercitazione: Distribuire app Python nel Servizio app di Azure in Linux da Visual Studio Code

Questo articolo illustra come usare Visual Studio Code per distribuire un'applicazione Python nel Servizio app di Azure in Linux usando l'estensione [Servizio app di Azure](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice).

Se si riscontrano problemi con uno qualsiasi dei passaggi descritti in questa esercitazione, è possibile segnalarli. Usare il collegamento **Problemi? Segnalarli**. alla fine di ogni articolo per inviare feedback.

Per una dimostrazione, vedere il video che spiega come <a href="https://www.youtube.com/watch?v=dNVvFttc-sA&feature=youtu.be&ocid=AID3006292" target="_blank">compilare app Web con VS Code e il Servizio app di Azure</a> (youtube.com) tratto dalla conferenza virtuale PyCon 2020.

> [!NOTE]
> Se si preferisce distribuire le app tramite l'interfaccia della riga di comando, vedere **[Avvio rapido: Creare un'app Python nel Servizio app di Azure in Linux](/azure/app-service/quickstart-python)** .

> [!TIP]
> [Servizio app di Azure in Linux](/azure/app-service/overview#app-service-on-linux) esegue il codice sorgente in un contenitore Docker predefinito. Il contenitore esegue le app con Python 3.7 usando il server Web [Gunicorn](https://gunicorn.org). Le caratteristiche di questo contenitore sono descritte in [Configurare app Python per il Servizio app di Azure in Linux](/azure/app-service/configure-language-python). La definizione del contenitore si trova in [github.com/Azure-App-Service/python](https://github.com/Azure-App-Service/python/tree/master/3.7).

## <a name="configure-your-environment"></a>Configurare l'ambiente

- Una [sottoscrizione di Azure](#azure-subscription).
- [Visual Studio Code con l'estensione Servizio app di Azure](#visual-studio-code-python-and-the-azure-app-service-extension).
- Ambiente Python

### <a name="azure-subscription"></a>Sottoscrizione di Azure

Se non si ha una sottoscrizione di Azure, [iscriversi](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-appservice-extension&mktingSource=vscode-tutorial-appservice-extension) subito per ottenere un account gratuito con 200 USD in crediti di Azure per provare qualsiasi combinazione di servizi.

### <a name="visual-studio-code-python-and-the-azure-app-service-extension"></a>Visual Studio Code, Python e l'estensione Servizio app di Azure

Installare il software seguente:

- [Visual Studio Code](https://code.visualstudio.com/).
- Python e l'estensione [Python](https://marketplace.visualstudio.com/items?itemName=ms-python.python) come descritto nella sezione dei [prerequisiti dell'esercitazione su Python in VS Code](https://code.visualstudio.com/docs/python/python-tutorial).
- Estensione [Servizio app di Azure](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice), che consente di interagire con il Servizio app di Azure dall'interno di VS Code. Per informazioni di carattere generale, vedere l'[esercitazione sull'estensione del Servizio app](https://code.visualstudio.com/tutorials/app-service-extension/getting-started) sull'estensione del servizio app e il [repository GitHub vscode-azureappservice](https://github.com/Microsoft/vscode-azureappservice).

## <a name="sign-in-to-azure"></a>Accedere ad Azure

[!INCLUDE [azure-sign-in](includes/azure-sign-in.md)]

> [!div class="nextstepaction"]
> [L'accesso ad Azure è stato eseguito: procedere con il passaggio 2 >>>](tutorial-deploy-app-service-on-linux-02.md)

[Problemi? Segnalarli](https://aka.ms/FlaskVSCQuickstartHelp).
