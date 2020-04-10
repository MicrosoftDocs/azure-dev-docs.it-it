---
title: Creare un'immagine del contenitore per un'app Node.js da Visual Studio Code
description: Parte 4 dell'esercitazione, creare un'immagine di applicazione Node.js
ms.topic: conceptual
ms.date: 09/20/2019
ms.openlocfilehash: a8659edb4d0b3664c7704fd0bedde0c274562f3c
ms.sourcegitcommit: f89c59f772364ec717e751fb59105039e6fab60c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2020
ms.locfileid: "80740686"
---
# <a name="create-your-nodejs-application-image"></a>Creare l'immagine dell'applicazione Node.js

[Passaggio precedente: Creare ed eseguire un'app Node.js in locale](tutorial-vscode-docker-node-03.md)

## <a name="add-docker-files"></a>Aggiungere i file Docker

1. In Visual Studio Code aprire il **riquadro comandi** (**F1**), digitare `add docker files to workspace`, quindi selezionare il comando **Docker: Add Docker files to workspace** (Aggiungi i file Docker all'area di lavoro).

1. Quando richiesto, selezionare **Node.js** come tipo di applicazione, rispondere **No** per i file Docker Compose, quindi selezionare la porta in cui l'applicazione resta in ascolto (dovrebbe essere 3000).

1. Il comando crea un `Dockerfile` insieme ad alcuni file di configurazione per Docker Compose e un file `.dockerignore`.

    > [!TIP]
    > VS Code prevede un ampio supporto per i file Docker. Per informazioni sulle funzionalità avanzate del linguaggio, come suggerimenti intelligenti, completamenti e rilevamento di errori, vedere [Uso di Docker](https://code.visualstudio.com/docs/azure/docker) nella documentazione di VS Code.

## <a name="build-a-docker-image"></a>Creare un'immagine Docker

Il `Dockerfile` descrive l'ambiente per l'app, tra cui la posizione dei file di origine e il comando per avviare l'app all'interno di un contenitore.

> [!TIP]
> Differenza tra contenitori e immagini: il contenitore è un'istanza di un'immagine.

1. Aprire il **riquadro comandi** (**F1**) ed eseguire **Docker Images: Build Image** (Immagini Docker: Compila immagine) per compilare l'immagine. Visual Studio Code usa Dockerfile nella cartella corrente e assegna all'immagine lo stesso nome della cartella corrente.

1. Al termine, viene visualizzato il pannello **Terminale** di Visual Studio Code per eseguire il comando `docker build`. L'output mostra anche ogni passaggio, o livello, che costituisce l'ambiente dell'app.

1. Una volta compilata, l'immagine viene visualizzata nell'area **DOCKER** in **Images** (Immagini).

    ![Elenco di immagini Docker in Visual Studio Code](media/deploy-containers/image-list.png)

## <a name="push-the-image-to-a-registry"></a>Eseguire il push dell'immagine in un registro

1. Per eseguire il push dell'immagine in un registro, è necessario prima di tutto contrassegnarla con il nome del registro. Nello strumento di esplorazione di **DOCKER** fare clic con il pulsante destro del mouse sull'immagine **Più recenti**.

    ![Comando Tag immagine in Visual Studio Code](media/deploy-containers/tag-command.png)

1. Nella richiesta visualizzata completare i tag e premere **INVIO**.

    Per convenzione, viene usato il formato seguente per i tag:

    `[registry or username]/[image name]:[tag]`

    Se si usa il Registro Azure Container, il nome dell'immagine sarà analogo al seguente:

    `msdocsvscodereg.azurecr.io/myexpressapp:latest`

    Se si usa Docker Hub, usare il nome utente di Docker Hub. Ad esempio:

    `fiveisprime/myexpressapp:latest`

1. L'immagine appena contrassegnata viene ora visualizzata in un nodo in **Images** (Immagini) che include il nome del registro. Espandere il nodo, fare clic con il pulsante destro del mouse su **Più recenti**, quindi selezionare **Push**.

    ![Comando Push immagine in Visual Studio Code](media/deploy-containers/push-command.png)

1. Il pannello **Terminale** visualizza i comandi `docker push` usati per questa operazione. Il registro di destinazione viene determinato in base al registro specificato nel nome dell'immagine.

   > [!IMPORTANT]
   > Se l'output visualizza un messaggio analogo a "Autenticazione obbligatoria", eseguire `az acr login --name <your registry name>` nel terminale.

1. Al termine, espandere il nodo **Registries** (Registri) nell'area dell'estensione Docker per visualizzare l'immagine nel registro.

    ![Immagine sottoposta a push visualizzata in Registro Azure Container](media/deploy-containers/image-in-acr.png)

> [!div class="nextstepaction"]
> [È stata creata un'immagine per l'applicazione](tutorial-vscode-docker-node-05.md) [Si è verificato un problema](https://www.research.net/r/PWZWZ52?tutorial=docker-extension&step=containerize-app)
