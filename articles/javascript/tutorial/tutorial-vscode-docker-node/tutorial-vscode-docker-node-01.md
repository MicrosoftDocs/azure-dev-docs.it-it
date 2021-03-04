---
title: Distribuire contenitori Docker nel Servizio app di Azure da Visual Studio Code
description: Parte 1 dell'esercitazione su Docker, introduzione e prerequisiti.
ms.topic: tutorial
ms.date: 03/04/2021
ms.custom: devx-track-js
ms.openlocfilehash: a6058d682a8dec3b06d0854fed09534454270171
ms.sourcegitcommit: 576c878c338d286060010646b96f3ad0fdbcb814
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/04/2021
ms.locfileid: "102117985"
---
# <a name="deploy-containers-to-azure-app-service"></a>Distribuire contenitori nel servizio app di Azure

In questa esercitazione si usa Visual Studio Code per creare un'applicazione Node.js in un contenitore tramite Docker, eseguire il push dell'immagine del contenitore in un registro e quindi distribuire l'immagine nel Servizio app di Azure.

## <a name="walkthrough-video"></a>Video della procedura dettagliata

Guardare questo video per una procedura dettagliata completa del contenuto di questo articolo.

> [!VIDEO https://channel9.msdn.com/Shows/Docs-Azure/Deploy-containers-Azure-App-Service/player]

## <a name="prerequisites"></a>Prerequisiti

- Una [sottoscrizione di Azure](#azure-subscription).
- [Visual Studio Code](https://code.visualstudio.com/).
- Estensione di Visual Studio Code
    - [Estensione account Azure](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account)
    - [Estensione del servizio app Azure](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice).
    - [Estensione risorse di Azure](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureresourcegroups)
    - [Docker](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker).
- [Node.js e npm](https://nodejs.org/en/download), la gestione pacchetti Node.js.
- [Docker](https://www.docker.com/community-edition).

### <a name="azure-subscription"></a>Sottoscrizione di Azure

Se non si ha una sottoscrizione di Azure, [iscriversi](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-docker-extension&mktingSource=vscode-tutorial-docker-extension) subito per ottenere un account gratuito con 200 USD in crediti di Azure per provare qualsiasi combinazione di servizi.

## <a name="sign-in-to-azure"></a>Accedere ad Azure

[!INCLUDE [azure-sign-in](../../includes/azure-sign-in.md)]

## <a name="verify-docker-install"></a>Verificare l'installazione di Docker

Verificare che Docker sia stato installato correttamente eseguendo il comando seguente in un terminale o al prompt dei comandi:

```bash
docker --version
```

L'output dovrebbe essere simile al seguente:

<pre>
Docker Version 17.12.0-ce, build c97c6d6
</pre>

> [!div class="nextstepaction"]
> [L'estensione Docker è stata installata](tutorial-vscode-docker-node-02.md) [Si è verificato un problema](https://www.research.net/r/PWZWZ52?tutorial=docker-extension&step=getting-started)
