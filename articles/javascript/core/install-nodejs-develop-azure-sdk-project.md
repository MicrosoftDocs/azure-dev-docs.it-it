---
title: Installare Node.js per lo sviluppo di applicazioni di Azure SDK
description: Informazioni su come installare Node.js per scenari di sviluppo comuni con Azure.
ms.topic: how-to
ms.date: 12/04/2020
ms.custom: devx-track-js
ms.openlocfilehash: 5fdaca001f82afca942a22ec9ad971f4af18c8b8
ms.sourcegitcommit: 525c4b41d85aae9c3026a070b07e00c2241ea716
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/14/2020
ms.locfileid: "97394036"
---
# <a name="install-and-manage-nodejs-for-common-azure-development-scenarios"></a>Installare e gestire Node.js per scenari di sviluppo comuni di Azure

Per l'installazione di Node.js per lo sviluppo in Azure, è necessario considerare sia l'ambiente di sviluppo locale che l'ambiente di hosting in cui si intende eseguire la distribuzione. Azure fornisce l'hosting per Node.js in Windows e in Linux nella versione con supporto a lungo termine (LTS). 

## <a name="minimum-version-of-nodejs-8"></a>Versione minima di Node.js 8 e successive

Azure SDK supporta la versione minima 8 di Node.js. 

## <a name="download-and-install-nodejs-based-on-your-intended-use"></a>Scaricare e installare Node.js in base all'uso previsto

È possibile scaricare e installare Node.js in base ai requisiti.
 
* [Pagina di download di Node.js](https://nodejs.org/en/download/) 
* [Immagine Docker ufficiale](https://hub.docker.com/_/node/)

## <a name="managing-versions-of-nodejs"></a>Gestione delle versioni di Node.js

Usare nvm quando è necessario gestire più versioni di Node.js per lo sviluppo in Azure.

* OSX, *nix - [nvm](https://github.com/creationix/nvm)
* Windows - [nvm per Windows](https://github.com/marcelklehr/nodist)

## <a name="next-steps"></a>Passaggi successivi

* [Configurare l'ambiente di sviluppo locale](configure-local-development-environment.md) per l'utilizzo di Azure SDK
