---
title: 'Esercitazione: Distribuire app Python nel Servizio app di Azure in Linux da Visual Studio Code'
description: "Passaggio 1 dell'esercitazione: configurare l'ambiente per il servizio app"
ms.topic: conceptual
ms.date: 11/20/2020
ms.custom: devx-track-python, seo-python-october2019
adobe-target: true
ms.openlocfilehash: f73f968849cdf6e9324074427f6156a706d65dc0
ms.sourcegitcommit: b380f6e637b47e6e3822b364136853e1d342d5cd
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/14/2021
ms.locfileid: "100395346"
---
# <a name="tutorial-deploy-python-apps-to-azure-app-service-on-linux-from-visual-studio-code"></a>Esercitazione: Distribuire app Python nel Servizio app di Azure in Linux da Visual Studio Code

Questo articolo illustra come usare Visual Studio Code per distribuire un'applicazione Python nel Servizio app di Azure in Linux usando l'estensione [Servizio app di Azure](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice).

Se si riscontrano problemi con uno qualsiasi dei passaggi descritti in questa esercitazione, è possibile segnalarli. Usare il collegamento **Problemi? Segnalarli**. alla fine di ogni articolo per inviare feedback.

Per una dimostrazione, vedere il video che spiega come <a href="https://www.youtube.com/watch?v=dNVvFttc-sA&feature=youtu.be&ocid=AID3006292" target="_blank">compilare app Web con VS Code e il Servizio app di Azure</a> (youtube.com) tratto dalla conferenza virtuale PyCon 2020.

> [!NOTE]
> Se si preferisce distribuire le app tramite l'interfaccia della riga di comando, vedere **[Avvio rapido: Creare un'app Python nel Servizio app di Azure in Linux](/azure/app-service/quickstart-python)** .

> [!TIP]
> [Servizio app di Azure in Linux](/azure/app-service/overview#app-service-on-linux) esegue il codice sorgente in un contenitore Docker predefinito. Il contenitore esegue le app con Python 3.6 usando il server Web [Gunicorn](https://gunicorn.org). Le caratteristiche di questo contenitore sono descritte in [Configurare app Python per il Servizio app di Azure in Linux](/azure/app-service/configure-language-python). Le definizioni del contenitore si trovano in [github.com/Azure-App-Service/python](https://github.com/Azure-App-Service/python/tree/master/).

## <a name="configure-your-environment"></a>Configurare l'ambiente

- Se non si ha un account Azure con una sottoscrizione attiva, è possibile [crearne uno gratuito](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-appservice-extension&mktingSource=vscode-tutorial-appservice-extension).

- Assicurarsi di avere un'[installazione locale di Python 3.7 o 3.8](https://python.org/downloads). Per verificare la versione, eseguire il comando seguente:

    ```bash
    python --version
    ```

- Installare il software seguente:
  - [Visual Studio Code](https://code.visualstudio.com/).
  - Python e l'estensione [Python](https://marketplace.visualstudio.com/items?itemName=ms-python.python) come descritto nella sezione dei [prerequisiti dell'esercitazione su Python in VS Code](https://code.visualstudio.com/docs/python/python-tutorial).
  - Estensione [Servizio app di Azure](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice), che consente di interagire con il Servizio app di Azure dall'interno di VS Code. Per informazioni di carattere generale, vedere l'[esercitazione sull'estensione del Servizio app](https://code.visualstudio.com/tutorials/app-service-extension/getting-started) sull'estensione del servizio app e il [repository GitHub vscode-azureappservice](https://github.com/Microsoft/vscode-azureappservice).

## <a name="sign-in-to-azure"></a>Accedere ad Azure

[!INCLUDE [azure-sign-in](includes/azure-sign-in.md)]

> [!div class="nextstepaction"]
> [L'accesso ad Azure è stato eseguito: procedere con il passaggio 2 >>>](tutorial-deploy-app-service-on-linux-02.md)

[Problemi? Segnalarli](https://aka.ms/FlaskVSCQuickstartHelp).
