---
title: Usare un registro contenitori da Visual Studio Code
description: Parte 2 dell'esercitazione, usare un registro contenitori
ms.topic: conceptual
ms.date: 09/20/2019
ms.openlocfilehash: 01e35f12e83a7e53d6d5b78c4fcf156d9a5b53f0
ms.sourcegitcommit: 756e4873f904db954a56c20ebb2f1f5116ee4596
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/05/2020
ms.locfileid: "82026174"
---
# <a name="use-a-container-registry"></a>Usare un registro contenitori

[Passaggio precedente: Introduzione e prerequisiti](tutorial-vscode-docker-node-01.md)

In questo passaggio viene configurato un registro contenitori idoneo per l'immagine dell'app. I servizi di hosting compatibili con contenitori, come il servizio app di Azure, eseguono quindi il pull delle immagini dal registro.

Questa esercitazione usa [Registro Azure Container](https://azure.microsoft.com/services/container-registry/), un registro ospitato, sicuro e privato per le immagini. Gli strumenti e i processi illustrati qui, tuttavia, sono compatibili anche con altri registri, ad esempio [Docker Hub](https://hub.docker.com/).

## <a name="create-an-azure-container-registry"></a>Creare un Registro Azure Container

1. In Visual Studio Code premere **F1** per aprire il riquadro comandi.

1. Immettere **registro** nella casella di ricerca. Nei risultati selezionare **Registro Azure Container: Create Registry** (Crea registro).

   ![L'area Docker in Visual Studio Code](media/deploy-containers/docker-create-registry.jpg)

1. Immettere o selezionare i valori seguenti:

    - In **Nome registro**immettere un nome univoco in Azure contenente da 5 a 50 caratteri alfanumerici.
    - In **SKU** selezionare **Basic**.
    - In **Gruppo di risorse** immettere un valore univoco all'interno della sottoscrizione.
    - In **Località** selezionare un'area nelle vicinanze.

    Visual Studio Code inizia il processo di creazione del registro in Azure. Al termine, verrà visualizzata una notifica come quella riportata di seguito. Questa notifica conferma che il registro è stato creato correttamente.

   ![Conferma della creazione del registro in Visual Studio Code](media/deploy-containers/registry-created.jpg)

1. Aprire l'area **Docker**. Verificare che l'endpoint del registro appena configurato sia visibile in **Registri**.

   ![Verifica che il registro compaia nell'area Docker](media/deploy-containers/docker-explorer-registry.jpg)

## <a name="sign-in-to-azure-container-registry"></a>Accedere a Registro Azure Container

Anche se è possibile visualizzare i registri di Azure nell'estensione Docker, non sarà possibile eseguire il push delle immagini fino a quando non si accede a Registro Container.

1. In Visual Studio premere **CTRL**+ **`** per aprire il terminale integrato.

1. Eseguire il seguente comando dell'interfaccia della riga di comando di Azure per accedere a Registro Container. Sostituire `<your-registry-name>` con il nome del registro creato.

    ```bash
    az acr login --name <your-registry-name>
    ```

> [!div class="nextstepaction"]
> [Il registro è stato creato](tutorial-vscode-docker-node-03.md) [Si è verificato un problema](https://www.research.net/r/PWZWZ52?tutorial=docker-extension&step=create-registry)
 