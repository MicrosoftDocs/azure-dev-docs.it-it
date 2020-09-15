---
title: 'Esercitazione: Distribuire contenitori Docker nel servizio app di Azure con Visual Studio Code'
description: Passaggio 1 dell'esercitazione, uso dei contenitori, introduzione e prerequisiti.
ms.topic: conceptual
ms.date: 09/12/2019
ms.custom: devx-track-python, seo-python-october2019
ms.openlocfilehash: f0fb983a596ca1828809d1d829af5517e8af66df
ms.sourcegitcommit: 9e282fc2ec967bee181c3034e7e70b28ae308905
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/04/2020
ms.locfileid: "89473556"
---
# <a name="tutorial-deploy-docker-containers-to-azure-app-service-with-visual-studio-code"></a>Esercitazione: Distribuire contenitori Docker nel servizio app di Azure con Visual Studio Code

Questo articolo descrive come usare Visual Studio Code per distribuire un'immagine del contenitore nel [servizio app di Azure](/azure/app-service/) da un registro contenitori, il tutto all'interno di Visual Studio Code.

Se si riscontrano problemi con uno qualsiasi dei passaggi descritti in questa esercitazione, è possibile segnalarli. Per inviare feedback, seguire il collegamento **Si è verificato un problema** alla fine di ogni articolo.

Per una dimostrazione, vedere il video che illustra le <a href="https://www.youtube.com/watch?v=t79HDLC5kQA&feature=youtu.be&ocid=AID3006292" target="_blank">app Django nei contenitori per lo sviluppo VS Code</a> (youtube.com) tratto dalla conferenza virtuale PyCon 2020.

## <a name="prerequisites"></a>Prerequisiti

- Un [account Azure](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-docker-extension&mktingSource=vscode-tutorial-docker-extension)
- [Visual Studio Code](https://code.visualstudio.com/)
- Un contenitore idoneo caricato in un registro contenitori. Le informazioni su come creare un contenitore con un'app Web Phyton sono disponibili nell'articolo [Python nei contenitori](https://code.visualstudio.com/docs/containers/quickstart-python).
- [Estensione Servizio app di Azure per VS Code](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice).
- [Estensione Docker per VS Code](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker).

## <a name="sign-in-to-azure"></a>Accedere ad Azure

[!INCLUDE [azure-sign-in](includes/azure-sign-in.md)]

> [!div class="nextstepaction"]
> [L'accesso ad Azure è stato eseguito: procedere con il passaggio 2 >>>](tutorial-deploy-containers-02.md)

o problemi Inviare un problema GitHub usando il feedback "Questa pagina" nella parte inferiore della pagina.
