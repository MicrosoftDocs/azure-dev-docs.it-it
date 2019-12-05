---
title: Distribuire contenitori Docker nel Servizio app di Azure da Visual Studio Code
description: Parte 1 dell'esercitazione, introduzione e prerequisiti.
ms.topic: conceptual
ms.date: 09/20/2019
ms.openlocfilehash: 2d6721060281fb73d31576caa47f316f2d078d29
ms.sourcegitcommit: e77f8f652128b798dbf972078a7b460ed21fb5f8
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/25/2019
ms.locfileid: "74467149"
---
# <a name="deploy-containers-to-azure-app-service"></a>Distribuire contenitori nel servizio app di Azure

In questa esercitazione si usa Visual Studio Code per creare un'applicazione Node.js in un contenitore tramite Docker, eseguire il push dell'immagine del contenitore in un registro e quindi distribuire l'immagine nel Servizio app di Azure.

## <a name="prerequisites"></a>Prerequisiti

- Una [sottoscrizione di Azure](#azure-subscription).
- [Visual Studio Code](https://code.visualstudio.com/).
- L'[estensione Docker](vscode:extension/ms-azuretools.vscode-docker)
- L'[estensione Servizio app di Azure](vscode:extension/ms-azuretools.vscode-azureappservice)
- [Node.js e npm](https://nodejs.org/en/download), la gestione pacchetti Node.js.
- [Docker](https://www.docker.com/community-edition).

> <a class="tutorial-install-extension-btn" href="vscode:extension/ms-azuretools.vscode-docker">Installare l'estensione Docker</a>

> <a class="tutorial-install-extension-btn" href="vscode:extension/ms-azuretools.vscode-azureappservice">Installare l'estensione Servizio app di Azure</a>

### <a name="azure-subscription"></a>Sottoscrizione di Azure

Se non si ha una sottoscrizione di Azure, [iscriversi](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-docker-extension&mktingSource=vscode-tutorial-docker-extension) subito per ottenere un account gratuito con 200 USD in crediti di Azure per provare qualsiasi combinazione di servizi.

## <a name="sign-in-to-azure"></a>Accedere ad Azure

[!INCLUDE [azure-sign-in](includes/azure-sign-in.md)]

## <a name="verify-docker-install"></a>Verificare l'installazione di Docker

Verificare che Docker sia stato installato correttamente eseguendo il comando seguente in un terminale o al prompt dei comandi:

```bash
docker --version
```

L'output dovrebbe essere simile al seguente:

```output
Docker Version 17.12.0-ce, build c97c6d6
```

> [!div class="nextstepaction"]
> [L'estensione Docker è stata installata](tutorial-vscode-docker-node-02.md) [Si è verificato un problema](https://www.research.net/r/PWZWZ52?tutorial=docker-extension&step=getting-started)
