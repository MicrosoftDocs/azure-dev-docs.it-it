---
title: Installazione del Toolkit di Azure per IntelliJ
description: Informazioni su come installare il plug-in Azure Toolkit for IntelliJ per creare e distribuire applicazioni cloud in Azure.
documentationcenter: java
ms.assetid: c6817c7b-f28c-4c06-8216-41c7a8117de3
ms.date: 02/01/2018
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.openlocfilehash: 64da0248a34392be88bf83343929b7f4eecc6b9c
ms.sourcegitcommit: 0af39ee9ff27c37ceeeb28ea9d51e32995989591
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/21/2020
ms.locfileid: "81673837"
---
# <a name="installing-the-azure-toolkit-for-intellij"></a>Installazione del Toolkit di Azure per IntelliJ

Azure Toolkit for IntelliJ offre modelli e funzionalità che consentono di creare, sviluppare, testare e distribuire con facilità applicazioni cloud in Azure tramite l'ambiente di sviluppo IntelliJ IDEA.

> [!NOTE] 
> 
> Il Toolkit di Azure per IntelliJ è un progetto Open Source, il cui codice sorgente è disponibile con la licenza MIT dal sito del progetto in GitHub all'indirizzo seguente: 
> 
> <https://github.com/microsoft/azure-tools-for-java> 
> 

Per l'installazione di Azure Toolkit for IntelliJ è possibile procedere in due modi: tramite la finestra di dialogo **Settings** (Impostazioni) o tramite il menu **Configure** (Configura) nella schermata Start. Entrambi i metodi di installazione vengono dimostrati nella procedura seguente.

## <a name="prerequisites"></a>Prerequisiti

Azure Toolkit for IntelliJ richiede i componenti software seguenti:

* Java Development Kit (JDK) 8 o versione successiva installato, ad esempio: [OpenJDK](https://openjdk.java.net/) o [JDK Oracle](https://www.oracle.com/technetwork/java/javase/downloads/index.html)
* [IntelliJ IDEA](https://www.jetbrains.com/idea/download/) Ultimate Edition o Community Edition installato

> [!NOTE]
> 
> Nella pagina [Azure Toolkit for IntelliJ](https://plugins.jetbrains.com/plugin/8053) nel repository dei plug-in JetBrains sono elencate le build compatibili con il toolkit.
> 

<!--
> [!IMPORTANT]
> 
> If you are using the Azure Toolkit for IntelliJ on Windows, the toolkit requires installing the Azure SDK 2.9.6 or later in order to use the Azure emulator. You have two options for installing the Azure SDK:
> 
> * You can download and install the Azure SDK by using the [Web Platform Installer (WebPI)](https://go.microsoft.com/fwlink/?LinkID=252838).
> * If you do not have the Azure SDK installed when you create your first Azure deployment project, you will be prompted to automatically download install the requisite version of the Azure SDK.
> 
> Note that the Azure SDK is only required on Windows.
> 
-->


## <a name="to-install-the-azure-toolkit-for-intellij-from-the-settings-dialog-box"></a>Per installare il Toolkit di Azure per IntelliJ dalla finestra di dialogo Impostazioni

1. Avviare IntelliJ IDEA.

1. Quando si apre IntelliJ IDEA, fare clic su **File**, quindi fare clic su **Settings** (Impostazioni).
   
   ![Aprire la finestra di dialogo Impostazioni di IntelliJ IDEA][01a]

1. Nella finestra di dialogo Settings (Impostazioni) fare clic su **Plugins** (Plug-in) e selezionare **Browse repositories** (Sfoglia repository).
   
   ![Finestra di dialogo Impostazioni di IntelliJ IDEA][02a]

1. Nella finestra di dialogo **Browse Repositories** (Sfoglia repository) digitare "Azure" nella casella di ricerca. Selezionare **Azure Toolkit for IntelliJ** e quindi fare clic su **Install** (Installa).
   
   ![Cercare il Toolkit di Azure per IntelliJ][03]
   
   IntelliJ IDEA mostra lo stato dell'installazione in una finestra di dialogo.
   
   ![Stato dell'installazione][04]

1. Al termine dell'installazione fare clic su **Restart IntelliJ IDEA**(Riavvia IntelliJ IDEA).
   
   ![Restart IntelliJ IDEA][05]

1. Fare clic su **OK** per chiudere la finestra di dialogo.
   
   ![Chiudere la finestra di dialogo Impostazioni di IntelliJ IDEA][06]

1. Quando viene richiesto di riavviare IntelliJ IDEA o posticipare, fare clic su **Restart**(Riavvia).
   
1   ![Restart IntelliJ IDEA][07]

## <a name="to-install-the-azure-toolkit-for-intellij-from-the-start-screen"></a>Per installare il Toolkit di Azure per IntelliJ dalla schermata iniziale

1. Avviare IntelliJ IDEA.

1. Nella schermata iniziale di IntelliJ IDEA fare clic su **Configure** (Configura) e selezionare **Plugins** (plug-in).
   
   ![Installare i plug-in di IntelliJ IDEA][01b]

1. Nella finestra di dialogo **Plugins** (Plug-in) fare clic su **Browse repositories** (Sfoglia repository).
   
   ![Sfogliare i repository dei plug-in di IntelliJ IDEA][02b]

1. Nella finestra di dialogo **Browse Repositories** (Sfoglia repository) digitare "Azure" nella casella di ricerca. Selezionare **Azure Toolkit for IntelliJ** e quindi fare clic su **Install** (Installa).
   
   ![Cercare il Toolkit di Azure per IntelliJ][03]
   
   IntelliJ IDEA mostrerà lo stato dell'installazione in una finestra di dialogo.
   
   ![Stato dell'installazione][04]

1. Al termine dell'installazione fare clic su **Restart IntelliJ IDEA**(Riavvia IntelliJ IDEA).
   
   ![Restart IntelliJ IDEA][05]

1. Quando viene richiesto di riavviare IntelliJ IDEA o posticipare, fare clic su **Restart**(Riavvia).
   
   ![Restart IntelliJ IDEA][07]

## <a name="next-steps"></a>Passaggi successivi

[!INCLUDE [additional-resources](includes/additional-resources.md)]

<!-- URL List -->

<!-- IMG List -->

[01a]: media/installation/01-intellij-file-settings.png
[01b]: media/installation/01-intellij-configure-dropdown.png
[02a]: media/installation/02-intellij-settings-dialog.png
[02b]: media/installation/02-intellij-plugins-dialog.png
[03]: media/installation/03-intellij-browse-repositories.png
[04]: media/installation/04-install-progress.png
[05]: media/installation/05-restart-intellij.png
[06]: media/installation/06-intellij-settings-dialog.png
[07]: media/installation/07-restart-intellij.png
