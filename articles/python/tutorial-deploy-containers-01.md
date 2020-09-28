---
title: 'Esercitazione: Distribuire contenitori Docker nel servizio app di Azure con Visual Studio Code'
description: "Passaggio 1 dell'esercitazione: configurare l'ambiente per i contenitori"
ms.topic: conceptual
ms.date: 09/17/2020
ms.custom: devx-track-python, seo-python-october2019
ms.openlocfilehash: 447643662ef3c839823d81d29cfb55cc2ee442b2
ms.sourcegitcommit: 69933dcce571b2686897b295b7822e207d944617
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/18/2020
ms.locfileid: "90772609"
---
# <a name="tutorial-deploy-docker-containers-to-azure-app-service-with-visual-studio-code"></a>Esercitazione: Distribuire contenitori Docker nel servizio app di Azure con Visual Studio Code

Questo articolo descrive come usare Visual Studio Code per distribuire un'immagine del contenitore in nel [servizio app di Azure](/azure/app-service/) da un registro contenitori.

Se si riscontrano problemi con uno qualsiasi dei passaggi descritti in questa esercitazione, è possibile segnalarli. Usare il pulsante di feedback **Questa pagina** alla fine di ogni articolo.

Per una dimostrazione, vedere il video che illustra le <a href="https://www.youtube.com/watch?v=t79HDLC5kQA&feature=youtu.be&ocid=AID3006292" target="_blank">app Django nei contenitori per lo sviluppo VS Code</a> (youtube.com) tratto dalla conferenza virtuale PyCon 2020.

## <a name="configure-your-environment"></a>Configurare l'ambiente

- Un [account Azure](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-docker-extension&mktingSource=vscode-tutorial-docker-extension)
- [Visual Studio Code](https://code.visualstudio.com/)
- Un contenitore idoneo caricato in un registro contenitori. Le informazioni su come creare un contenitore con un'app Web Phyton sono disponibili nell'articolo [Python nei contenitori](https://code.visualstudio.com/docs/containers/quickstart-python).
- [Estensione Servizio app di Azure per VS Code](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice).
- [Estensione Docker per VS Code](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker).

## <a name="sign-in-to-azure"></a>Accedere ad Azure

[!INCLUDE [azure-sign-in](includes/azure-sign-in.md)]

> [!div class="nextstepaction"]
> [L'accesso ad Azure è stato eseguito: procedere con il passaggio 2 >>>](tutorial-deploy-containers-02.md)
