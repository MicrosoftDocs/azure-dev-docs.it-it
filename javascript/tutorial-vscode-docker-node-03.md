---
title: Creare un'immagine del contenitore per un'app Node.js da Visual Studio Code
description: Parte 3 dell'esercitazione, creare un'immagine di applicazione Node.js
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/20/2019
ms.author: kraigb
ms.openlocfilehash: 1b79f84bd69853578796b4485ca669be98f41006
ms.sourcegitcommit: c04984b6367e922dbc5973af44f8cd0ca81ce157
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/30/2019
ms.locfileid: "71686101"
---
# <a name="create-your-nodejs-application-image"></a>Creare l'immagine dell'applicazione Node.js

[Passaggio precedente: Usare un registro contenitori](tutorial-vscode-docker-node-02.md)

In questo passaggio si usa l'estensione Docker in Visual Studio Code per aggiungere i file necessari per creare un'immagine per l'app, compilarla ed eseguirne il push in un registro.

Se non si ha già un'app per questa procedura dettagliata, usare l'app dell'esercitazione su [Node.js in Visual Studio Code](https://code.visualstudio.com/docs/nodejs/nodejs-tutorial).

## <a name="add-docker-files"></a>Aggiungere i file Docker

1. In Visual Studio Code aprire il **riquadro comandi** (**F1**), digitare `add docker files to workspace`, quindi selezionare il comando **Docker: Add Docker files to workspace** (Aggiungi i file Docker all'area di lavoro).

1. Quando richiesto, selezionare **Node.js** come tipo di applicazione, quindi selezionare la porta in cui l'applicazione resta in ascolto.

1. Il comando crea un `Dockerfile` insieme ad alcuni file di configurazione per Docker Compose e un file `.dockerignore`.

    > [!TIP]
    > VS Code prevede un ampio supporto per i file Docker. Per informazioni sulle funzionalità avanzate del linguaggio, come suggerimenti intelligenti, completamenti e rilevamento di errori, vedere [Uso di Docker](https://code.visualstudio.com/docs/azure/docker) nella documentazione di VS Code.

## <a name="build-a-docker-image"></a>Creare un'immagine Docker

Il `Dockerfile` descrive l'ambiente per l'app, tra cui la posizione dei file di origine e il comando per avviare l'app all'interno di un contenitore.

> [!TIP]
> Differenza tra contenitori e immagini: il contenitore è un'istanza di un'immagine.

1. Aprire il **riquadro comandi** (**F1**) ed eseguire **Docker Images: Build Image** (Immagini Docker: Compila immagine) per compilare l'immagine.

1. Quando richiesto, scegliere il `Dockerfile` appena creato e assegnare un nome all'immagine. Il nome deve includere il registro di destinazione o l'account di Docker Hub:

    `[registry or username]/[image name]:[tag]`

    In questa esercitazione, in cui si usa Registro Azure Container, il nome dell'immagine è il seguente:

    `msdocsvscodereg.azurecr.io/myexpressapp:latest`

    Se si usa Docker Hub, usare il nome utente di Docker Hub. Ad esempio:

    `fiveisprime/myexpressapp:latest`

1. Al termine, viene visualizzato il pannello **Terminale** di Visual Studio Code per eseguire il comando `docker build`. L'output mostra anche ogni passaggio, o livello, che costituisce l'ambiente dell'app.

1. Una volta compilata, l'immagine viene visualizzata nell'area **DOCKER** in **Images** (Immagini).

    ![Elenco di immagini Docker in Visual Studio Code](media/deploy-containers/image-list.png)

## <a name="push-the-image-to-a-registry"></a>Eseguire il push dell'immagine in un registro

1. In Visual Studio Code aprire il **riquadro comandi** (**F1**) ed eseguire **Docker Images: Push** (Immagini Docker: Esegui il push), quindi scegliere l'immagine appena compilata. Il pannello **Terminale** visualizza i comandi `docker push` usati per questa operazione.

1. Al termine, espandere il nodo **Images** (Immagini) nell'area dell'estensione Docker per visualizzare l'immagine.

    ![Immagine sottoposta a push visualizzata in Registro Azure Container](media/deploy-containers/image-in-acr.png)

> [!div class="nextstepaction"]
> [È stata creata un'immagine per l'applicazione](tutorial-vscode-docker-node-04.md) [Si è verificato un problema](https://www.research.net/r/PWZWZ52?tutorial=docker-extension&step=containerize-app)
