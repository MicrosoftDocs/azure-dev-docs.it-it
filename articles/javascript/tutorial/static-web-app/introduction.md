---
title: Introduzione e prerequisiti
description: Compilare e distribuire localmente un'applicazione client React/TypeScript in un'app Web statica di Azure con un'azione GitHub.
ms.topic: tutorial
ms.date: 12/17/2020
ms.custom: devx-track-js
ms.openlocfilehash: dfd5803fe79a0a000173d2d4e3fe52b3c4f029c5
ms.sourcegitcommit: 1c508f5ba73a12e4baeacc88ad9a8359301acb50
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/18/2020
ms.locfileid: "97687442"
---
# <a name="1-build-and-deploy-a-static-web-app-to-azure"></a>1. Compilare e distribuire un'app Web statica in Azure

In questa esercitazione, si compila e si distribuisce localmente un'applicazione client React/TypeScript in un'app Web statica di Azure con un'azione GitHub. L'app React consente di analizzare un'immagine con Visione artificiale di Servizi cognitivi.

* [**Codice di esempio**](https://github.com/Azure-Samples/js-e2e-client-cognitive-services)

[!INCLUDE [Create or use existing Azure Subscription ](../../includes/environment-subscription-h2.md)]

## <a name="prerequisites"></a>Prerequisiti

- [Node.js e npm](https://nodejs.org/en/download): installati nel computer locale.
- [Visual Studio Code](https://code.visualstudio.com/): installato nel computer locale. 
    - [App Web statiche di Azure (anteprima)](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurestaticwebapps): usate per distribuire l'app React nell'app Web statica di Azure.
- [Git](https://git-scm.com/downloads): usato per eseguire il push in GitHub, che attiva l'azione GitHub.
- [Account GitHub](https://github.com/join): per creare una copia tramite fork ed eseguire il push in un repository
- Usare [Azure Cloud Shell](/azure/cloud-shell/quickstart) tramite l'ambiente bash.

   [![Incorpora avvio](https://shell.azure.com/images/launchcloudshell.png "Avviare Azure Cloud Shell")](https://shell.azure.com)   
- Se si preferisce, [installare](/cli/azure/install-azure-cli) l'interfaccia della riga di comando di Azure per eseguire i relativi comandi di riferimento.
   - Se si usa un'installazione locale, accedere all'interfaccia della riga di comando usando il comando [az login](/cli/azure/reference-index#az-login).  Per completare il processo di autenticazione, seguire la procedura visualizzata nel terminale.  Per altre opzioni di accesso, vedere [Accedere tramite l'interfaccia della riga di comando di Azure](/cli/azure/authenticate-azure-cli).
  - Quando richiesto, installare le estensioni dell'interfaccia della riga di comando di Azure al primo utilizzo.  Per altre informazioni sulle estensioni, vedere [Usare le estensioni con l'interfaccia della riga di comando di Azure](/cli/azure/azure-cli-extensions-overview).
  - Eseguire [az version](/cli/azure/reference-index?#az_version) per trovare la versione e le librerie dipendenti installate. Per eseguire l'aggiornamento alla versione più recente, eseguire [az upgrade](/cli/azure/reference-index?#az_upgrade).

## <a name="next-step"></a>Passaggio successivo

> [!div class="nextstepaction"]
> [Informazioni sull'architettura delle app client e sul processo di distribuzione con GitHub Actions](./application-architecture.md) 
