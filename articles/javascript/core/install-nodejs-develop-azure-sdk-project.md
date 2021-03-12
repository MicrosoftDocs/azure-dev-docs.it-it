---
title: Installare Node.js per lo sviluppo di applicazioni di Azure SDK
description: Informazioni su come installare Node.js per scenari di sviluppo comuni con Azure.
ms.topic: how-to
ms.date: 03/11/2021
ms.custom: devx-track-js
ms.openlocfilehash: 1e346c42b34eb0e38ec7869196dce4c171c897a0
ms.sourcegitcommit: 3536f174735cd3bb7da7e4b266fbf43349a22b67
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/12/2021
ms.locfileid: "103193426"
---
# <a name="install-and-manage-nodejs-for-azure-development"></a>Installare e gestire Node.js per lo sviluppo in Azure

Per l'installazione di Node.js per lo sviluppo in Azure, è necessario considerare sia l'ambiente di sviluppo locale che l'ambiente di hosting in cui si intende eseguire la distribuzione. Azure fornisce l'hosting per Node.js in Windows e in Linux nella versione con supporto a lungo termine (LTS). 

## <a name="minimum-version-of-nodejs-for-azure-sdk"></a>Versione minima di Node.js per Azure SDK

Azure SDK supporta una versione minima di:

* Node.js 8. 

## <a name="minimum-version-of-nodejs-for-azure-services"></a>Versione minima di Node.js per i servizi di Azure

Se si intende ospitare l'applicazione in Azure, senza ospitarla all'interno di un contenitore, è necessario controllare la versione minima di Node.js supportata per il servizio con cui si ospita:

* [App Web statiche di Azure](/azure/static-web-apps/) -client e API
* [Funzioni di Azure](/azure/azure-functions/) -solo API
* [App di Azure](/azure/app-service/) -server

## <a name="manage-versions-of-nodejs"></a>Gestire le versioni di Node.js

Quando è necessario gestire più di una versione di Node.js negli ambienti locali e remoti, usare una delle opzioni seguenti:

* [NVM](#manage-nodejs-version-with-nvm) -gestione versioni del nodo
* [Contenitori](#manage-nodejs-version-with-containers) : usare un contenitore con una specifica Node.js versione minima

## <a name="manage-nodejs-version-with-nvm"></a>Gestire la versione di Node.js con NVM

Usare nvm quando è necessario gestire più versioni di Node.js per lo sviluppo in Azure.

* OSX, *nix - [nvm](https://github.com/creationix/nvm)
* Windows - [nvm per Windows](https://github.com/marcelklehr/nodist)

## <a name="manage-nodejs-version-with-containers"></a>Gestire la versione di Node.js con i contenitori

È possibile gestire la versione di Node.js in diversi ambienti usando i contenitori. L'estensione dei [contenitori remoti](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers) di Visual Studio Code rende molto semplice l'uso dei contenitori. 

Dopo aver installato [Docker](https://www.docker.com/)e aver aperto il progetto, usare l'estensione per caricare il progetto in un contenitore e connettersi al contenitore per eseguire il debug.  

## <a name="download-and-install-nodejs-based-on-your-intended-use"></a>Scaricare e installare Node.js in base all'uso previsto

È possibile scaricare e installare Node.js in base ai requisiti.
 
* [Pagina di download di Node.js](https://nodejs.org/en/download/) 
* [Immagine Docker ufficiale](https://hub.docker.com/_/node/)

## <a name="next-steps"></a>Passaggi successivi

* [Configurare l'ambiente di sviluppo locale](configure-local-development-environment.md) per l'utilizzo di Azure SDK
