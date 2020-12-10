---
title: Introduzione e prerequisiti
description: Compilare e distribuire localmente un'applicazione client React in un'app Web statica di Azure con un'azione GitHub.
ms.topic: tutorial
ms.date: 11/13/2020
ms.custom: devx-track-js
ms.openlocfilehash: 1a034d2746fae453019325d01f20c7073a6ce9a3
ms.sourcegitcommit: 09b4a2dbe13601fdf16fcc4082a5075b46ad3459
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/03/2020
ms.locfileid: "96559256"
---
# <a name="1-build-and-deploy-a-static-web-app-to-azure"></a>1. Compilare e distribuire un'app Web statica in Azure

In questa esercitazione, compilare e distribuire localmente un'applicazione client React in un'app Web statica di Azure con un'azione GitHub. 

Il client React (create-react-app) fornisce le funzionalità seguenti: 
* Visualizzare il messaggio se la chiave di Azure e l'endpoint per Visione artificiale di Servizi cognitivi non è stato trovato
* Consente di analizzare un'immagine con Visione artificiale di Servizi cognitivi
    * Immettere un URL di immagine pubblico o analizzare l'immagine dalla raccolta
    * Al termine dell'analisi
        * Immagine visualizzata
        * Visualizzare i risultati JSON di Visione artificiale 

L'azione GitHub inizia quando viene eseguito un push in un ramo specifico:
* Inserisce i segreti GitHub per la chiave e l'endpoint di Visione artificiale nella build
* Compila il client React (create-react-app)
* Sposta i file risultanti nella risorsa dell'app Web statica di Azure

[!INCLUDE [Create or use existing Azure Subscription ](../../includes/environment-subscription-h2.md)]

## <a name="what-is-an-azure-static-web-app"></a>Che cos'è un'app Web statica di Azure

Quando si compilano app Web statiche, sono disponibili diverse opzioni in Azure, in base al grado di funzionalità e al controllo a cui si è interessati. Questa esercitazione è incentrata sul servizio più semplice con molte delle scelte effettuate per l'utente, per potersi concentrare sul codice front-end e non sull'ambiente host.

## <a name="prerequisites"></a>Prerequisiti

- [Installare l'interfaccia della riga di comando di Azure](/cli/azure/install-azure-cli) o usare [Azure Cloud Shell](https://shell.azure.com)
- [Node.js e npm](https://nodejs.org/en/download): installati nel computer locale.
- [Visual Studio Code](https://code.visualstudio.com/): installato nel computer locale. 
    - [App Web statiche di Azure (anteprima)](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurestaticwebapps): usate per distribuire l'app React nell'app Web statica di Azure.
- [Git](https://git-scm.com/downloads): usato per eseguire il push in GitHub, che attiva l'azione GitHub.
- [Account GitHub](https://github.com/join): per creare una copia tramite fork ed eseguire il push in un repository

## <a name="next-step"></a>Passaggio successivo

> [!div class="nextstepaction"]
> [Scaricare ed eseguire l'app React Image Analyzer di Servizi cognitivi in locale](run-the-react-cognitive-services-image-analyzer-app-locally.md) 