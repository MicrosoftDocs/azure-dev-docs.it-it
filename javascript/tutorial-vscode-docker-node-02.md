---
title: Usare un registro contenitori da Visual Studio Code
description: Parte 2 dell'esercitazione, usare un registro contenitori
ms.topic: conceptual
ms.date: 09/20/2019
ms.openlocfilehash: e6dde135a2e6482284488fb83d9f811b02249c4d
ms.sourcegitcommit: f89c59f772364ec717e751fb59105039e6fab60c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2020
ms.locfileid: "80740526"
---
# <a name="use-a-container-registry"></a>Usare un registro contenitori

[Passaggio precedente: Introduzione e prerequisiti](tutorial-vscode-docker-node-01.md)

In questo passaggio viene configurato un registro contenitori idoneo per l'immagine dell'app. I servizi di hosting compatibili con contenitori, come il Servizio app di Azure, eseguono quindi il pull delle immagini dal registro.

Questa esercitazione usa [Registro Azure Container](https://azure.microsoft.com/services/container-registry/), un registro ospitato, sicuro e privato per le immagini. Gli strumenti e i processi illustrati qui, tuttavia, sono compatibili anche con altri registri, ad esempio [Docker Hub](https://hub.docker.com/).

## <a name="create-an-azure-container-registry"></a>Creare un'istanza di Registro Azure Container

1. In Visual Studio Code premere <kbd>F1</kbd> per aprire il **riquadro comandi**.

1. Digitare "registry" nella casella di ricerca e selezionare **Registro Azure Container: Create Registry** (Crea registro).

   ![L'area Docker in VS Code](media/deploy-containers/docker-create-registry.jpg)

1. Quando richiesto, specificare i valori seguenti.

    - Il **Nome registro** deve essere univoco in Azure e può contenere da 5 a 50 caratteri alfanumerici.
    - Per **SKU** selezionare **Basic**.
    - Il **Gruppo di risorse** deve essere univoco solo all'interno della sottoscrizione.
    - In **Località** selezionare un'area vicina.

    Visual Studio Code inizierà il processo di creazione del registro in Azure. Al termine, verrà visualizzata una notifica simile a quella riportata di seguito, che conferma che il registro è stato creato correttamente.

   ![Conferma in Visual Studio Code che il registro è stato creato](media/deploy-containers/registry-created.jpg)

1. Aprire l'area **Docker** e verificare che l'endpoint del registro appena configurato sia visibile in **Registries** (Registri):

   ![Verifica che il registro compaia nell'area Docker](media/deploy-containers/docker-explorer-registry.jpg)

## <a name="sign-in-to-azure-container-registry"></a>Accedere a Registro Azure Container

Anche se è possibile visualizzare i registri di Azure nell'estensione Docker, non sarà possibile eseguire il push delle immagini fino a quando non si accede a Registro Azure Container.

1. Premere <kbd>CTRL+'</kbd> per aprire il **terminale integrato** in VS Code.

1. Eseguire il comando dell'interfaccia della riga di comando di Azure seguente per accedere a Registro Azure Container. Sostituire "<your-registry-name>" con il nome del registro appena creato.

    ```bash
    az acr login --name <your-registry-name>
    ```

> [!div class="nextstepaction"]
> [Il registro è stato creato](tutorial-vscode-docker-node-03.md) [Si è verificato un problema](https://www.research.net/r/PWZWZ52?tutorial=docker-extension&step=create-registry)
