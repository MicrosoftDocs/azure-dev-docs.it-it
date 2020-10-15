---
title: Installare il Toolkit di Azure per Eclipse.
description: Informazioni su come installare il plug-in Azure Toolkit for Eclipse per creare e distribuire applicazioni cloud in Azure.
documentationcenter: java
ms.assetid: 9e93ff6a-f42b-4d99-b55b-624136b4a730
ms.date: 08/25/2020
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.custom: devx-track-java
ms.openlocfilehash: be90cbf867cfbbb475e1a80655d2ef925069b90a
ms.sourcegitcommit: f460914ac5843eb7392869a08e3a80af68ab227b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/13/2020
ms.locfileid: "92010206"
---
# <a name="installing-the-azure-toolkit-for-eclipse"></a>Installare il Toolkit di Azure per Eclipse.

Azure Toolkit for Eclipse offre funzionalità che consentono di creare, sviluppare, configurare, testare e sviluppare facilmente app Web Java e processi Spark HDInsight leggeri, a disponibilità e scalabilità elevata in Azure con l'ambiente di sviluppo Eclipse.

> [!NOTE] 
> 
> Azure Toolkit for Eclipse è un progetto Open Source, il cui codice sorgente è disponibile con la licenza MIT dal sito del progetto in GitHub all'indirizzo seguente: 
> 
> <https://github.com/microsoft/azure-tools-for-java> 
> 

[!INCLUDE [basic-prerequisites](includes/basic-prerequisites.md)]

Per installare Azure Toolkit for Eclipse è possibile scegliere se accedere a **Eclipse Marketplace** o usare l'opzione **Install new software** (Installa nuovo software) nel menu Help (Guida). Entrambi i metodi di installazione verranno illustrati nelle sezioni seguenti.

## <a name="eclipse-marketplace"></a>Eclipse Marketplace

La procedura guidata di Eclipse Marketplace disponibile nell'IDE di Eclipse consente agli utenti di esplorare [Eclipse Marketplace](https://marketplace.eclipse.org/) e di installare soluzioni. Per accedere a Eclipse Marketplace, è possibile usare le due opzioni seguenti:

   * Trascinare il pulsante seguente nell'area di lavoro di Eclipse in esecuzione. Questo pulsante apre Eclipse Marketplace con l'opzione Azure Toolkit for Eclipse già selezionata.

      [![Trascinare nell'area di lavoro Eclipse* in esecuzione. *Richiede Eclipse Marketplace Client](https://marketplace.eclipse.org/sites/all/themes/solstice/public/images/marketplace/btn-install.png)](http://marketplace.eclipse.org/marketplace-client-intro?mpc_install=1919278 "Trascinare nell'area di lavoro Eclipse* in esecuzione. *Richiede Eclipse Marketplace Client")

   * Nell'IDE di Eclipse fare clic sul menu **Help** (Guida), passare a **Eclipse Marketplace**, cercare "Azure Toolkit for Eclipse" e fare clic su **Install** (Installa).

      :::image type="content" source="media/installation/eclipse-marketplace-button.png" alt-text="Finestra del Marketplace, menu della Guida."::: 

1. Verrà visualizzata una procedura guidata di Eclipse Marketplace con le istruzioni di installazione, incluso un elenco di componenti che verranno installati. Verificare che tutte le funzionalità siano selezionate e fare clic su **Confirm >** (Conferma >).

   | Funzionalità | Descrizione | 
   |---|---| 
   | **Plug-in di Application Insights per Java** | Consente di usare servizi di analisi e di registrazione dei dati di telemetria di Azure per le applicazioni e le istanze del server. | 
   | **Plug-in Azure Common** | Offre le funzionalità comuni necessarie per gli altri componenti del toolkit. | 
   | **Azure Container Tools for Eclipse** | Consente di compilare e distribuire un file WAR come contenitore Docker in un computer Docker. |
   | **Azure Explorer per Eclipse** | Offre un'interfaccia di tipo Esplora risorse per la gestione delle risorse di Azure. | 
   | **Plug-in Azure HDInsight per Java** | Abilita lo sviluppo di applicazioni Apache Spark in Scala. |
   | **Microsoft JDBC Driver 6.1 per SQL Server** | Fornisce l'API JDBC per SQL Server e il database SQL di Microsoft Azure per Java Platform Enterprise Edition 8. | 
   | **Pacchetto per Librerie di Microsoft Azure per Java** | Fornisce le API per accedere ai servizi di Microsoft Azure, ad esempio l'archiviazione, il bus di servizio, il runtime del servizio e così via. | 
   | **Plug-in WebApp per Eclipse** | Consente di distribuire le applicazioni Web come servizi app Azure. | 

1. Nella finestra di dialogo **Esaminare licenze** , rivedere le condizioni dei contratti di licenza. Se si accettano le condizioni dei contratti di licenza, fare clic su **I accept the terms of the license agreements** (Accetto le condizioni dei contratti di licenza) e quindi fare clic su **Finish** (Fine). 

   > [!NOTE]
   > È possibile controllare lo stato di avanzamento dell'installazione nell'angolo in basso a destra dell'area di lavoro di Eclipse.

4. Al termine dell'installazione verrà chiesto di riavviare l'IDE di Eclipse per applicare l'aggiornamento software. Fare clic su **Riavvia**.

## <a name="install-new-software"></a>Installazione di nuovo software

È possibile installare Azure Toolkit for Eclipse direttamente dal menu *Help* (Guida) sotto forma di nuovo software.

1. Fare clic sul menu **Help** (?) e quindi scegliere **Install New Software** (Installa nuovo software).

   :::image type="content" source="media/installation/eclipse-install-software-button.png" alt-text="Finestra del Marketplace, menu della Guida."::: 

1. Nella finestra di dialogo **Available Software** (Software disponibile) digitare `http://dl.microsoft.com/eclipse/` nella casella di testo **Work with** (Compatibile con).

1. Nel riquadro **Nome**, controllare il **Toolkit di Azure per Java** e deselezionare **Contattare tutti i siti di aggiornamento durante l'installazione per trovare il software richiesto**. Verrà visualizzata una schermata simile alla seguente:

   ![Installare il Toolkit di Azure per Eclipse.][02]

1. Espandendo **Azure Toolkit for Java**, viene visualizzato un elenco di componenti che verranno installati, ad esempio:

   | Funzionalità | Descrizione | 
   |---|---| 
   | **Plug-in di Application Insights per Java** | Consente di usare servizi di analisi e di registrazione dei dati di telemetria di Azure per le applicazioni e le istanze del server. | 
   | **Plug-in Azure Common** | Offre le funzionalità comuni necessarie per gli altri componenti del toolkit. | 
   | **Azure Container Tools for Eclipse** | Consente di compilare e distribuire un file WAR come contenitore Docker in un computer Docker. |
   | **Azure Explorer per Eclipse** | Offre un'interfaccia di tipo Esplora risorse per la gestione delle risorse di Azure. | 
   | **Plug-in Azure HDInsight per Java** | Abilita lo sviluppo di applicazioni Apache Spark in Scala. |
   | **Microsoft JDBC Driver 6.1 per SQL Server** | Fornisce l'API JDBC per SQL Server e il database SQL di Microsoft Azure per Java Platform Enterprise Edition 8. | 
   | **Pacchetto per Librerie di Microsoft Azure per Java** | Fornisce le API per accedere ai servizi di Microsoft Azure, ad esempio l'archiviazione, il bus di servizio, il runtime del servizio e così via. | 
   | **Plug-in WebApp per Eclipse** | Consente di distribuire le applicazioni Web come servizi app Azure. | 

1. Fare clic su **Avanti**. (Se si verificano ritardi insoliti durante l'installazione del toolkit, assicurarsi che **Contattare tutti i siti di aggiornamento durante l'installazione per trovare il software richiesto** sia deselezionato.)

1. Nella finestra di dialogo **Install Details** fare clic su **Next** (Avanti).

1. Nella finestra di dialogo **Esaminare licenze** , rivedere le condizioni dei contratti di licenza. Se si accettano le condizioni dei contratti di licenza, fare clic su **I accept the terms of the license agreements** (Accetto le condizioni dei contratti di licenza) e quindi fare clic su **Finish** (Fine). (I passaggi rimanenti suppongono che le condizioni dei contratti di licenza siano state accettate. Se non si accettano le condizioni dei contratti di licenza, uscire dal processo di installazione.)

   > [!NOTE]
   > È possibile controllare lo stato di avanzamento dell'installazione nell'angolo in basso a destra dell'area di lavoro di Eclipse.

1. Se viene chiesto di riavviare Eclipse per completare l'installazione, fare clic su **Riavvia ora**.

## <a name="next-steps"></a>Passaggi successivi

[!INCLUDE [additional-resources](includes/additional-resources.md)]

<!-- URL List -->

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690946.aspx -->

<!-- IMG List -->
[02]: media/installation/eclipse-installation-02.png
