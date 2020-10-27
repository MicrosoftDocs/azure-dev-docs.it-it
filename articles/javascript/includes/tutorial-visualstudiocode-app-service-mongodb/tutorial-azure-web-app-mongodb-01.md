---
title: include file tutorial-azure-web-app-mongodb-01.md
description: include file tutorial-azure-web-app-mongodb-01.md
ms.date: 10/13/2020
ms.topic: include
ms.custom: devx-track-javascript
ms.openlocfilehash: a8c9fcca1b7374ae5122cfabb3fcbfc6ba8cdd07
ms.sourcegitcommit: 8a2a7df568c69fff2080ffab248409040efda1ac
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/19/2020
ms.locfileid: "92183846"
---
Per questa sezione dell'esercitazione, è necessario avere una sottoscrizione di Azure e il software indicato.

## <a name="create-or-use-existing-azure-subscription"></a>Creare una sottoscrizione di Azure o usarne una esistente 

* Un account Azure con una sottoscrizione attiva. [È possibile crearne uno gratuitamente](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-appservice-extension&mktingSource=vscode-tutorial-appservice-extension).

## <a name="install-software"></a>Installazione del software

- [Node.js e npm](https://nodejs.org/en/download), lo strumento di gestione pacchetti Node.js, installato nel computer locale.
- [Docker](https://docs.docker.com/get-docker/), usato per fornire un database MongoDB locale senza dover installare MongoDB. 
    - Se è necessario usare Docker per ottenere un database MongoDB locale, è anche necessario usare:
        -  I [contenitori di sviluppo](https://code.visualstudio.com/docs/remote/containers) di Visual Studio includono diversi contenitori comuni per lo sviluppo in JavaScript. 
        - [Contenitori remoti](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)
    - Se è già presente un database MongoDB locale e non si vuole installare Docker, è comunque possibile eseguire questo passaggio. Tutti i passaggi che usano il contenitore di sviluppo per accedere a un database MongoDB in esecuzione in locale possono essere riconfigurati per usare il database MongoDB locale, purché sia disponibile l'URL MongoDB seguente: 
        - `mongodb://localhost:27017`
- [Visual Studio Code](https://code.visualstudio.com/) installato nel computer locale. 
- Estensioni per Visual Studio Code:
    - [Estensione del servizio app di Azure](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice) per Visual Studio Code (installata da Visual Studio Code).
    - [Database di Azure](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb)

## <a name="want-to-know-more"></a>Per saperne di più 

Estensioni per Visual Studio Code facoltative:
* [MongoDB per Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=mongodb.mongodb-vscode)
* [Docker](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker)