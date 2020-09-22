---
title: Istruzioni di accesso per Azure Toolkit for IntelliJ
description: Informazioni su come accedere a Microsoft Azure con Azure Toolkit for IntelliJ.
documentationcenter: java
ms.date: 02/01/2018
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.custom: devx-track-java
ms.openlocfilehash: 2891b0c09c43b652fd7dd41e354290c2821bad46
ms.sourcegitcommit: a139e25190960ba89c9e31f861f0996a6067cd6c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/15/2020
ms.locfileid: "90534567"
---
# <a name="sign-in-instructions-for-the-azure-toolkit-for-intellij"></a>Istruzioni di accesso per Azure Toolkit for IntelliJ

Dopo l'[installazione](https://www.jetbrains.com/help/idea/managing-plugins.html), [Azure Toolkit for IntelliJ](https://plugins.jetbrains.com/plugin/8053) consente di accedere all'account Azure in due modi diversi:

  - [Accedere all'account Azure tramite accesso dispositivo](#sign-in-to-your-azure-account-by-device-login)
  - [Accedere all'account Azure tramite entità servizio](#sign-in-to-your-azure-account-by-service-principal)

Sono anche disponibili metodi di [**disconnessione**](#sign-out-of-your-azure-account).

[!INCLUDE [basic-prerequisites](includes/basic-prerequisites.md)]

## <a name="sign-in-to-your-azure-account-by-device-login"></a>Accedere all'account Azure tramite accesso dispositivo

Per accedere ad Azure tramite accesso dispositivo, procedere come segue:

1. Aprire il progetto con IntelliJ IDEA.

1. Aprire **Azure Explorer** sulla barra laterale e quindi fare clic sull'icona **Azure Sign In** (Accesso ad Azure) sulla barra in alto. In alternativa, nel menu IntelliJ, passare a **Tools > Azure > Azure Sign in** (Strumenti > Azure > Accesso ad Azure).

   ![Comando di accesso ad Azure in IntelliJ][I01]

1. Nella finestra **Azure Sign In** (Accesso ad Azure), selezionare **Device Login** (Accesso dispositivo) e quindi fare clic su **Sign In** (Accedi).

   ![Finestra di accesso ad Azure con l'accesso dispositivo selezionato][I02]

1. Nella finestra di dialogo **Azure Device Login** (Accesso dispositivo Azure) fare clic su **Copy&Open** (Copia e apri).

1. Nel browser incollare il codice dispositivo (copiato facendo clic su **Copy&Open** nell'ultimo passaggio) e quindi fare clic su **Avanti**.

1. Selezionare l'account Azure e completare le procedure di autenticazione necessarie per eseguire l'accesso.

1. Nella finestra di dialogo **Select Subscriptions** (Seleziona sottoscrizioni) selezionare le sottoscrizioni da usare e quindi fare clic su **OK**.


## <a name="sign-in-to-your-azure-account-by-service-principal"></a>Accedere all'account Azure tramite entità servizio

La sezione seguente illustra come creare un file di credenziali che contiene i dati dell'entità servizio. Al termine di questo processo, IntelliJ usa il file delle credenziali per l'accesso automatico ad Azure quando si apre il progetto.

1. Aprire il progetto con IntelliJ IDEA.

1. Aprire **Azure Explorer** sulla barra laterale e quindi fare clic sull'icona **Azure Sign In** (Accesso ad Azure) sulla barra in alto. In alternativa, nel menu IntelliJ, passare a **Tools > Azure > Azure Sign in** (Strumenti > Azure > Accesso ad Azure).

   ![Comando di accesso ad Azure in IntelliJ][I01]

1. Nella finestra **Azure Sign In** (Accesso ad Azure) selezionare **Service Principal** (Entità servizio) e quindi fare clic su **New** (Nuovo).

   ![Finestra di accesso ad Azure con l'entità servizio selezionata][A02]

1. Nella finestra di dialogo **Azure Device Login** (Accesso dispositivo Azure) fare clic su **Copy&Open** (Copia e apri).

1. Nel browser incollare il codice dispositivo (copiato facendo clic su **Copy&Open** nell'ultimo passaggio) e quindi fare clic su **Avanti**.

1. Selezionare l'account Azure e completare le procedure di autenticazione necessarie per eseguire l'accesso. Dopo l'autenticazione chiudere il browser e tornare a IntelliJ.

1. Nella finestra **Create authentication files** (Crea file di autenticazione) selezionare le sottoscrizioni da usare, scegliere la directory di destinazione e quindi fare cli su **Start** (Avvio).

1. Nella finestra di dialogo **Service Principal Creation Status** (Stato creazione entità servizio) fare clic su **OK** al termine della creazione dei file.

1. Nella finestra **Azure Sign In** (Accesso ad Azure) fare clic su **Sign in** (Accedi). 

1. Nella finestra di dialogo **Select Subscriptions** (Seleziona sottoscrizioni) selezionare le sottoscrizioni da usare e quindi fare clic su **OK**.

   > [!TIP]
   > Dopo aver creato il file di autenticazione dell'entità servizio, è possibile iniziare dal passaggio 3, scegliere il file di autenticazione ed eseguire l'accesso.

## <a name="sign-out-of-your-azure-account"></a>Disconnettersi dall'account Azure

Dopo la configurazione dell'account con i passaggi precedenti, l'accesso verrà eseguito automaticamente a ogni avvio di IntelliJ IDEA. 

Per disconnettersi dall'account Azure, però, passare alla barra laterale di Azure Explorer, fare clic sull'icona **Azure Sign Out** (Disconnessione da Azure). In alternativa, nel menu IntelliJ passare a **Tools > Azure > Azure Sign Out** (Strumenti > Azure > Disconnessione da Azure).


## <a name="next-steps"></a>Passaggi successivi

[!INCLUDE [additional-resources](includes/additional-resources.md)]

<!-- URL List -->

<!-- IMG List -->

[I01]: media/sign-in-instructions/I01.png
[I02]: media/sign-in-instructions/I02.png

[A02]: media/sign-in-instructions/A02.png

