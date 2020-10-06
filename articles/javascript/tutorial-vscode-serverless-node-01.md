---
title: Distribuire Funzioni di Azure in Node.js da Visual Studio Code
description: Parte 1 dell'esercitazione serverless, introduzione e prerequisiti.
ms.topic: tutorial
ms.date: 09/23/2019
ms.custom: devx-track-js
ms.openlocfilehash: 1d0c1795b7bec0138507ceadea966c421ca12a13
ms.sourcegitcommit: 4dd392ea864be52421d0239e59198bc44b0a5a16
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/25/2020
ms.locfileid: "91364644"
---
# <a name="deploy-azure-functions-from-visual-studio-code"></a>Distribuire Funzioni di Azure da Visual Studio Code

In questa esercitazione si usano Visual Studio Code e l'estensione Funzioni di Azure per creare e distribuire un'applicazione di Funzioni di Azure scritta in JavaScript.

## <a name="walkthrough-video"></a>Video della procedura dettagliata

Guardare questo video per una procedura dettagliata completa del contenuto di questo articolo.

> [!VIDEO https://channel9.msdn.com/Shows/Docs-Azure/Deploy-Azure-Functions-Visual-Studio-Code/player]

## <a name="prerequisites"></a>Prerequisiti

- Una [sottoscrizione di Azure](#azure-subscription).
- [Visual Studio Code](https://code.visualstudio.com/).
- L'[estensione Funzioni di Azure](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions)
- [Node.js e npm](https://nodejs.org/en/download), la gestione pacchetti Node.js.

> <a class="tutorial-install-extension-btn" href="https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions">Installare l'estensione Funzioni di Azure</a>

### <a name="azure-subscription"></a>Sottoscrizione di Azure

Se non si ha una sottoscrizione di Azure, [iscriversi](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-functions-extension&mktingSource=vscode-tutorial-functions-extension) subito per ottenere un account gratuito con 200 USD in crediti di Azure per provare qualsiasi combinazione di servizi.

## <a name="sign-in-to-azure"></a>Accedere ad Azure

[!INCLUDE [azure-sign-in](includes/azure-sign-in.md)]

## <a name="install-the-azure-functions-core-tools"></a>Installare gli strumenti di base per Funzioni di Azure

Per abilitare il debug locale, è necessario installare [Azure Functions Core Tools](https://github.com/Azure/azure-functions-core-tools). Questa operazione può essere eseguita direttamente in Visual Studio Code.

1. Avviare Visual Studio Code.

1. Aprire il **riquadro comandi** (**F1**) e immettere **Funzioni di Azure: Install or Update Azure Functions Core Tools** (Installa o aggiorna Azure Functions Core Tools), quindi premere **INVIO** per eseguire il comando.

1. Per verificare l'installazione, selezionare il comando di menu **Terminale** > **Nuovo terminale** in VS Code, quindi eseguire il comando `func`. Il comando dovrebbe mostrare un output simile al seguente, insieme alle informazioni sull'utilizzo.

    <pre>
                      %%%%%%
                     %%%%%%
                @   %%%%%%    @
              @@   %%%%%%      @@
           @@@    %%%%%%%%%%%    @@@
         @@      %%%%%%%%%%        @@
           @@         %%%%       @@
             @@      %%%       @@
               @@    %%      @@
                    %%
                    %

    Azure Functions Core Tools (2.4.419 Commit hash: c9c1724d002bd90b2e6b41393915ea3a26bcf0ce)
    Function Runtime Version: 2.0.12332.0
    </pre>

> [!div class="nextstepaction"]
> [I prerequisiti sono stati installati](tutorial-vscode-serverless-node-02.md) [Si è verificato un problema](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azurefunctions&step=getting-started)
