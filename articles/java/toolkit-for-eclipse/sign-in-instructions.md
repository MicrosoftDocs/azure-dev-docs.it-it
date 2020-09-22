---
title: Istruzioni di accesso per Azure Toolkit for Eclipse
description: Informazioni su come accedere a Microsoft Azure con il Toolkit di Azure per Eclipse.
documentationcenter: java
ms.date: 02/01/2018
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.custom: devx-track-java
ms.openlocfilehash: d0dbc16a16ca3a5ff367e6c67fceabcb37e2cce6
ms.sourcegitcommit: a139e25190960ba89c9e31f861f0996a6067cd6c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/15/2020
ms.locfileid: "90534425"
---
# <a name="sign-in-instructions-for-the-azure-toolkit-for-eclipse"></a>Istruzioni di accesso per Azure Toolkit for Eclipse

Il Toolkit di Azure per Eclipse consente di accedere all'account Azure in due metodi diversi:

  - [Accedere all'account Azure tramite accesso dispositivo](#sign-in-to-your-azure-account-by-device-login)
  - [Accedere all'account Azure tramite entità servizio](#sign-in-to-your-azure-account-by-service-principal)

Sono anche disponibili metodi di [**disconnessione**](#sign-out-of-your-azure-account).

[!INCLUDE [prerequisites](includes/prerequisites.md)]

## <a name="sign-in-to-your-azure-account-by-device-login"></a>Accedere all'account Azure tramite accesso dispositivo

Questa sezione illustra il processo di accesso ad Azure con l'accesso dispositivo.

1. Aprire il progetto con Eclipse.

1. Fare clic su **Tools** (Strumenti), su **Azure** e quindi fare clic su **Sign In** (Accedi).

      :::image type="content" source="media/sign-in-instructions/eclipse-azure-signin.png" alt-text="Accesso ad Azure nell'IDE di Eclipse.":::

1. Nella finestra **Azure Sign In** (Accesso ad Azure), selezionare **Device Login** (Accesso dispositivo) e quindi fare clic su **Sign In** (Accedi).

   ![Finestra di accesso ad Azure con l'accesso dispositivo selezionato][I02]

1. Nella finestra di dialogo **Azure Device Login** (Accesso dispositivo Azure) fare clic su **Copy&Open** (Copia e apri).

> [!NOTE]
>
> Se il browser non si apre, configurare Eclipse per l'uso di un browser esterno, come Internet Explorer, Firefox o Chrome:
>
> 1. Aprire Preferences -> General -> Web Browser -> Use external web browser in Eclipse (Preferenze -> Generale -> Web Browser -> Usa Web browser esterno in Eclipse)
>
> 2. Selezionare il browser che si preferisce usare
>

1. Nel browser incollare il codice dispositivo (copiato facendo clic su **Copy&Open** nell'ultimo passaggio) e quindi fare clic su **Avanti**.

1. Selezionare l'account Azure e completare le procedure di autenticazione necessarie per eseguire l'accesso.

1. Dopo l'accesso, chiudere il browser e tornare all'IDE di Eclipse. Nella finestra di dialogo **Select Subscriptions** (Seleziona sottoscrizioni) selezionare le sottoscrizioni da usare e quindi fare clic su **OK**.

## <a name="sign-in-to-your-azure-account-by-service-principal"></a>Accedere all'account Azure tramite entità servizio

La sezione seguente illustra come creare un file di credenziali che contiene i dati dell'entità servizio. Dopo avere completato questo processo, Eclipse usa il file di credenziali per l'accesso automatico ad Azure quando si apre il progetto.

1. Aprire il progetto con Eclipse.

2. Fare clic su **Tools** (Strumenti), su **Azure** e quindi fare clic su **Sign In** (Accedi).

      :::image type="content" source="media/sign-in-instructions/eclipse-azure-signin.png" alt-text="Accesso ad Azure nell'IDE di Eclipse.":::

3. Nella finestra **Azure Sign In** (Accesso ad Azure) selezionare **Service Principal** (Entità servizio). Se il file di autenticazione dell'entità servizio non è già disponibile, fare clic su **New** (Nuovo) per crearne uno. Altrimenti è possibile fare clic su **Browse** (Sfoglia) per aprirlo e procedere con il passaggio 8.

   ![Finestra di accesso ad Azure con l'entità servizio selezionata][A02]

4. Nella finestra di dialogo **Azure Device Login** (Accesso dispositivo Azure) fare clic su **Copy&Open** (Copia e apri).

> [!NOTE]
>
> Se il browser non si apre, configurare Eclipse per l'uso di un browser esterno, come IE o Chrome:
>
> 1. Aprire Preferences -> General -> Web Browser -> Use external web browser in Eclipse (Preferenze -> Generale -> Web Browser -> Usa Web browser esterno in Eclipse)
>
> 2. Selezionare il browser che si preferisce usare
>

5. Nel browser incollare il codice dispositivo (copiato facendo clic su **Copy&Open** nell'ultimo passaggio) e quindi fare clic su **Avanti**.

6. Nella finestra **Create authentication files** (Crea file di autenticazione) selezionare le sottoscrizioni da usare, scegliere la directory di destinazione e quindi fare cli su **Start** (Avvio).

7. Nella finestra di dialogo **Service Principal Creation Status** (Stato creazione entità servizio) fare clic su **OK** al termine della creazione dei file.

8. L'indirizzo del file creato verrà inserito automaticamente nella finestra **Azure Sign In** (Accesso ad Azure). Fare clic su **Sign in** (Accedi).

   ![Finestra di dialogo di accesso ad Azure][A06]

9. Infine, nella finestra di dialogo **Select Subscriptions** (Seleziona sottoscrizioni) selezionare le sottoscrizioni da usare e quindi fare clic su **OK**.


## <a name="sign-out-of-your-azure-account"></a>Disconnettersi dall'account Azure

Dopo avere configurato l'account usando i passaggi precedenti, l'accesso verrà eseguito automaticamente ogni volta che si avvia Eclipse. Se tuttavia ci si vuole disconnettere dall'account Azure, eseguire queste operazioni.

1. In Eclipse fare clic su **Tools** (Strumenti), su **Azure** e quindi su **Sign Out** (Disconnetti).

2. Quando viene visualizzata la finestra di dialogo **Azure Sign Out** (Disconnessione da Azure), fare clic su **Yes** (Sì).

## <a name="next-steps"></a>Passaggi successivi

[!INCLUDE [additional-resources](includes/additional-resources.md)]

<!-- URL List -->


<!-- IMG List -->

[I02]: media/sign-in-instructions/I02.png

[A02]: media/sign-in-instructions/A02.png
[A06]: media/sign-in-instructions/A06.png