---
title: Istruzioni di accesso per Azure Toolkit for IntelliJ
description: Informazioni su come accedere a Microsoft Azure con Azure Toolkit for IntelliJ.
documentationcenter: java
ms.date: 02/01/2018
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.openlocfilehash: 4447c9d2b09d0d004b8c858aceb31e6c7cee9493
ms.sourcegitcommit: 0af39ee9ff27c37ceeeb28ea9d51e32995989591
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/21/2020
ms.locfileid: "81673987"
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

2. Aprire **Azure Explorer** sulla barra laterale e quindi fare clic su **Azure Sign In** (Accesso ad Azure) sulla barra superiore. In alternativa, scegliere **Tools/Azure/Azure Sign in** (Strumenti/Azure/Accesso ad Azure) dal menu IDEA.

   ![Comando di accesso ad Azure in IntelliJ][I01]

3. Nella finestra **Azure Sign In** (Accesso ad Azure), selezionare **Device Login** (Accesso dispositivo) e quindi fare clic su **Sign In** (Accedi).

   ![Finestra di accesso ad Azure con l'accesso dispositivo selezionato][I02]

4. Nella finestra di dialogo **Azure Device Login** (Accesso dispositivo Azure) fare clic su **Copy&Open** (Copia e apri).

   ![Finestra di dialogo di accesso ad Azure][I03]

5. Nel browser incollare il codice dispositivo (copiato facendo clic su **Copy&Open** nell'ultimo passaggio) e quindi fare clic su **Avanti**.

   ![Accesso al dispositivo nel browser][I04]

6. Nella finestra di dialogo **Select Subscriptions** (Seleziona sottoscrizioni) selezionare le sottoscrizioni da usare e quindi fare clic su **OK**.

   ![Finestra di dialogo Seleziona sottoscrizioni][I05]

## <a name="sign-in-to-your-azure-account-by-service-principal"></a>Accedere all'account Azure tramite entità servizio

La sezione seguente illustra come creare un file di credenziali che contiene i dati dell'entità servizio. Al termine di questo processo, IntelliJ usa il file delle credenziali per l'accesso automatico ad Azure quando si apre il progetto.

1. Aprire il progetto con IntelliJ IDEA.

1. Aprire **Azure Explorer** sulla barra laterale e quindi fare clic su **Azure Sign In** (Accesso ad Azure) sulla barra superiore. In alternativa, scegliere **Tools/Azure/Azure Sign in** (Strumenti/Azure/Accesso ad Azure) dal menu IDEA.
   ![Comando di accesso ad Azure in IntelliJ][A01]

1. Nella finestra **Azure Sign In** (Accesso ad Azure) selezionare **Service Principal** (Entità servizio) e quindi fare clic su **New** (Nuovo).

   ![Finestra di accesso ad Azure con l'entità servizio selezionata][A02]

1. Nella finestra di dialogo **Azure Device Login** (Accesso dispositivo Azure) fare clic su **Copy&Open** (Copia e apri).

   ![Finestra di dialogo di accesso ad Azure][A03]

1. Nel browser incollare il codice dispositivo (copiato facendo clic su **Copy&Open** nell'ultimo passaggio) e quindi fare clic su **Avanti**.

   ![Accesso al dispositivo nel browser][A04]

1. Nella finestra **Create authentication files** (Crea file di autenticazione) selezionare le sottoscrizioni da usare, scegliere la directory di destinazione e quindi fare cli su **Start** (Avvio).

   ![Finestra di creazione dei file di autenticazione][A05]

1. Nella finestra di dialogo **Service Principal Creation Status** (Stato creazione entità servizio) fare clic su **OK** al termine della creazione dei file.

   ![Finestra di dialogo sullo stato di creazione dell'entità servizio][A06]

1. Nella finestra **Azure Sign In** (Accesso ad Azure) fare clic su **Sign in** (Accedi). 

   ![Finestra di dialogo di accesso ad Azure][A07]

1. Nella finestra di dialogo **Select Subscriptions** (Seleziona sottoscrizioni) selezionare le sottoscrizioni da usare e quindi fare clic su **OK**.

   ![Finestra di dialogo Seleziona sottoscrizioni][A08]

> Dopo aver creato il file di autenticazione dell'entità servizio, è possibile iniziare dal passaggio 8, scegliere il file di autenticazione ed eseguire l'accesso.

## <a name="sign-out-of-your-azure-account"></a>Disconnettersi dall'account Azure

Dopo la configurazione dell'account con i passaggi precedenti, l'accesso verrà eseguito automaticamente a ogni avvio di IntelliJ IDEA. Se tuttavia ci si vuole disconnettere dall'account Azure, eseguire queste operazioni.

* In IntelliJ IDEA aprire la barra laterale di Azure Explorer, fare clic sull'icona **Azure Sign Out** (Disconnessione da Azure) e confermare. In alternativa, scegliere **Tools/Azure/Azure Sign Out** (Strumenti/Azure/Disconnessione da Azure) dal menu IDEA.

   ![Comando di disconnessione da Azure in IntelliJ][L01]

## <a name="next-steps"></a>Passaggi successivi

[!INCLUDE [additional-resources](includes/additional-resources.md)]

<!-- URL List -->

<!-- IMG List -->

[I01]: media/sign-in-instructions/I01.png
[I02]: media/sign-in-instructions/I02.png
[I03]: media/sign-in-instructions/I03.png
[I04]: media/sign-in-instructions/I04.png
[I05]: media/sign-in-instructions/I05.png

[A01]: media/sign-in-instructions/A01.png
[A02]: media/sign-in-instructions/A02.png
[A03]: media/sign-in-instructions/A03.png
[A04]: media/sign-in-instructions/A04.png
[A05]: media/sign-in-instructions/A05.png
[A06]: media/sign-in-instructions/A06.png
[A07]: media/sign-in-instructions/A07.png
[A08]: media/sign-in-instructions/A08.png
[A09]: media/sign-in-instructions/A09.png

[L01]: media/sign-in-instructions/L01.png
[L02]: media/sign-in-instructions/L02.png
[L03]: media/sign-in-instructions/L03.png
