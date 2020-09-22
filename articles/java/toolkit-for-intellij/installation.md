---
title: Installazione del Toolkit di Azure per IntelliJ
description: Informazioni su come installare il plug-in Azure Toolkit for IntelliJ per creare e distribuire applicazioni cloud in Azure.
documentationcenter: java
ms.assetid: c6817c7b-f28c-4c06-8216-41c7a8117de3
ms.date: 09/09/2020
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.custom: devx-track-java
ms.openlocfilehash: fe8b07257ff3a9fc5523d13dd13e19982103ab05
ms.sourcegitcommit: a139e25190960ba89c9e31f861f0996a6067cd6c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/15/2020
ms.locfileid: "90534468"
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


## <a name="from-the-settings-dialog-box"></a>Dalla finestra di dialogo delle impostazioni

1. Sulla barra degli strumenti di IntelliJ fare clic su **File** e quindi fare clic su **Settings** (Impostazioni).

1. Nel menu di spostamento di sinistra della finestra di dialogo Settings (Impostazioni) fare clic su **Plugins** (Plug-in).

1. Nella barra di ricerca del **Marketplace** digitare "Azure" per filtrare l'elenco dei plug-in. Selezionare **Azure Toolkit for IntelliJ** e quindi fare clic su **Install** (Installa). Leggere il documento *Third-party Plugins Privacy Note* (Informativa sulla privacy per i plug-in di terze parti) di IntelliJ e fare clic su **Accept** (Accetta).

   :::image type="content" source="media/installation/03-intellij-search-plugin.png" alt-text="Cercare il plug-in Azure Toolkit for IntelliJ."::: 

1. Al termine dell'installazione fare clic su **Restart IDE**(Riavvia IDE).

1. Quando viene richiesto di riavviare IntelliJ IDEA, fare clic su **Restart**(Riavvia).
   
   :::image type="content" source="media/installation/07-restart-intellij.png" alt-text="Riavviare IntelliJ IDEA."::: 

## <a name="from-the-start-screen"></a>Dalla schermata iniziale

1. Nella schermata iniziale di IntelliJ IDEA fare clic su **Configure** (Configura) e selezionare **Plugins** (Plug-in).

   :::image type="content" source="media/installation/01-intellij-configure-dropdown.png" alt-text="Plug-in dalla schermata iniziale."::: 

1. Nella barra di ricerca del **Marketplace** digitare "Azure" per filtrare l'elenco dei plug-in. Selezionare **Azure Toolkit for IntelliJ** e quindi fare clic su **Install** (Installa). Leggere il documento *Third-party Plugins Privacy Note* (Informativa sulla privacy per i plug-in di terze parti) di IntelliJ e fare clic su **Accept** (Accetta).

   :::image type="content" source="media/installation/01-intellij-start-screen-marketplace.png" alt-text="Marketplace dei plug-in dalla schermata iniziale.":::

1. Al termine dell'installazione fare clic su **Restart IDE**(Riavvia IDE).

1. Quando viene richiesto di riavviare IntelliJ IDEA, fare clic su **Restart**(Riavvia).
   
   :::image type="content" source="media/installation/01-intellij-start-screen-marketplace-restart.png" alt-text="Riavviare per installare dalla schermata iniziale.":::

## <a name="next-steps"></a>Passaggi successivi

[!INCLUDE [additional-resources](includes/additional-resources.md)]

