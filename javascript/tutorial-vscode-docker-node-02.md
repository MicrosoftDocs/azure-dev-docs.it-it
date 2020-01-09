---
title: Usare un registro contenitori da Visual Studio Code
description: Parte 2 dell'esercitazione, usare un registro contenitori
ms.topic: conceptual
ms.date: 09/20/2019
ms.openlocfilehash: c5e9ff3cd803ef4d57408199682c71e4b57f2d77
ms.sourcegitcommit: fc3408b6e153c847dd90026161c4c498aa06e2fc
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/19/2019
ms.locfileid: "75191024"
---
# <a name="use-a-container-registry"></a>Usare un registro contenitori

[Passaggio precedente: Introduzione e prerequisiti](tutorial-vscode-docker-node-01.md)

In questo passaggio viene configurato un registro contenitori idoneo per l'immagine dell'app. I servizi di hosting compatibili con contenitori, come il Servizio app di Azure, eseguono quindi il pull delle immagini dal registro.

Questa esercitazione usa [Registro Azure Container](https://azure.microsoft.com/services/container-registry/), un registro ospitato, sicuro e privato per le immagini. Gli strumenti e i processi illustrati qui, tuttavia, sono compatibili anche con altri registri, ad esempio [Docker Hub](https://hub.docker.com/).

## <a name="create-an-azure-container-registry"></a>Creare un'istanza di Registro Azure Container

1. Accedere al [portale di Azure](https://portal.azure.com), quindi selezionare **Crea una risorsa**.

    ![Creazione di una nuova risorsa nel portale di Azure](media/deploy-containers/portal-01a.png)

1. Nella pagina successiva selezionare **Contenitori** > **Registro Container**.

    ![Creazione di un registro contenitori nel portale di Azure](media/deploy-containers/portal-01b.png)

1. Nel modulo **Crea registro contenitori** che viene visualizzato immettere i valori appropriati:

    - Il **Nome registro** deve essere univoco in Azure e può contenere da 5 a 50 caratteri alfanumerici.
    - In **Sottoscrizione** selezionare la propria sottoscrizione.
    - Il **Gruppo di risorse** deve essere univoco solo all'interno della sottoscrizione.
    - In **Località** selezionare un'area vicina.
    - Impostare **Utente amministratore** su **Abilita**.
    - Per **SKU** selezionare **Basic**.

    ![Valori del modulo del registro contenitori](media/deploy-containers/portal-02.png)

1. Selezionare **Crea** per creare il registro.

1. Dopo aver creato il registro, aprire le notifiche nel portale e selezionare l'opzione **Vai alla risorsa** riferita al registro:

    ![Apertura della risorsa registro appena creata](media/deploy-containers/portal-03.png)

1. Nella pagina del registro selezionare **Chiavi di accesso** e prendere nota delle credenziali di amministratore:

    ![Credenziali per il registro nel portale di Azure](media/deploy-containers/portal-04.png)

1. Al prompt dei comandi o in un terminale accedere a Docker usando il comando seguente, sostituendo `<registry_name>` con il nome del registro e `<username>` e `<password>` con i valori mostrati nel portale di Azure per l'utente amministratore:

    ```bash
    docker login <registry_name>.azurecr.io -u <username> -p <password>
    ```

    Per migliorare la sicurezza, usare `--password-stdin` invece di `-p <password>` e quindi incollare la password quando richiesto.

1. In Visual Studio Code aprire l'area **Docker** e verificare che l'endpoint del registro appena configurato sia visibile in **Registries** (Registri):

    ![Verifica che il registro compaia nell'area Docker](media/deploy-containers/registries.png)

> [!div class="nextstepaction"]
> [Il registro è stato creato](tutorial-vscode-docker-node-03.md) [Si è verificato un problema](https://www.research.net/r/PWZWZ52?tutorial=docker-extension&step=create-registry)
