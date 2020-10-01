---
title: Creare un'app Web Hello World per Servizio app di Azure con IntelliJ
description: Questa esercitazione spiega come usare Azure Toolkit per IntelliJ per creare un'app Web Hello World per Azure.
services: app-service
keywords: java, intellij, app web, servizio app di azure, hello world, avvio rapido
documentationcenter: java
author: selvasingh
ms.assetid: 75ce7b36-e3ae-491d-8305-4b42ce37db4e
ms.reviewer: asirveda
ms.date: 09/09/2020
ms.service: app-service
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: web
ms.custom: devx-track-java
ms.openlocfilehash: af85a31f39f87c38e378fc1cf4254053447b9dbd
ms.sourcegitcommit: 717e32b68fc5f4c986f16b2790f4211967c0524b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/30/2020
ms.locfileid: "91586174"
---
# <a name="create-a-hello-world-web-app-for-azure-app-service-using-intellij"></a>Creare un'app Web Hello World per Servizio app di Azure con IntelliJ

Questo articolo illustra i passaggi necessari per creare un'app Web Hello World di base e pubblicarla in Servizio app di Azure usando [Azure Toolkit for IntelliJ](https://plugins.jetbrains.com/plugin/8053).

> [!NOTE]
>
> Se si preferisce usare Eclipse, vedere l'[esercitazione simile per Eclipse][eclipse-hello-world].
>
>[!INCLUDE [quickstarts-free-trial-note](includes/quickstarts-free-trial-note.md)]
>
> Dopo aver completato questa esercitazione, non dimenticare di pulire le risorse. In tal modo, con l'esecuzione di questa guida non si supererà la quota dell'account gratuito.
>

[!INCLUDE [basic-prerequisites](includes/basic-prerequisites.md)]

## <a name="installation-and-sign-in"></a>Installazione e accesso

La procedura seguente illustra il processo di accesso ad Azure nell'ambiente di sviluppo IntelliJ.

1. Se non è stato installato il plug-in, vedere [Installazione di Azure Toolkit for IntelliJ](installation.md).

1. Per accedere all'account Azure, passare alla barra laterale sinistra di **Azure Explorer** e quindi fare clic sull'icona **Azure Sign In** (Accesso ad Azure). In alternativa, è possibile passare a **Tools** (Strumenti), espandere **Azure** e fare clic su **Azure Sign in** (Accesso ad Azure).

   :::image type="content" source="media/sign-in-instructions/I01.png" alt-text="Accesso ad Azure in IntelliJ."::: 

1. Nella finestra **Azure Sign In** (Accesso ad Azure) selezionare **Device Login** (Accesso dispositivo) e quindi fare clic su **Sign in** (Accedi) ([altre opzioni di accesso](sign-in-instructions.md)).

1. Nella finestra di dialogo **Azure Device Login** (Accesso dispositivo Azure) fare clic su **Copy&Open** (Copia e apri).

1. Nel browser incollare il codice dispositivo (copiato facendo clic su **Copy&Open** nell'ultimo passaggio) e quindi fare clic su **Avanti**.

1. Selezionare l'account Azure e completare le procedure di autenticazione necessarie per eseguire l'accesso.

1. Dopo l'accesso, chiudere il browser e tornare all'IDE di IntelliJ. Nella finestra di dialogo **Select Subscriptions** (Seleziona sottoscrizioni) selezionare le sottoscrizioni da usare e quindi fare clic su **OK**.

## <a name="creating-a-new-web-app-project"></a>Creazione di un nuovo progetto di app Web

1. Fare clic su **File**, espandere **New** (Nuovo) e quindi fare clic su **Project** (Progetto).

1. Nella finestra di dialogo **New Project** (Nuovo progetto) selezionare **Maven** e assicurarsi che l'opzione **Create from Archetype** (Crea da archetipo) sia selezionata. Nell'elenco selezionare **maven-archetype-webapp** e quindi fare clic su **Next** (Avanti).

   :::image type="content" source="media/create-hello-world-web-app/maven-archetype-webapp.png" alt-text="Accesso ad Azure in IntelliJ."::: 

1. Espandere l'elenco a discesa **Artifact Coordinates** (Coordinate artefatto) per visualizzare tutti i campi di input, specificare le informazioni seguenti per la nuova app Web e fare clic su **Next** (Avanti):

   * **Name**: nome dell'app Web. Questo valore verrà inserito automaticamente nel campo **ArtifactId** dell'app Web.
   * **GroupId** (ID gruppo): nome del gruppo di artefatti, in genere un dominio aziendale, ad esempio *com.microsoft.azure*.
   * **Versione**: mantenere la versione predefinita, ovvero *1.0-SNAPSHOT*.

1. Personalizzare eventuali impostazioni di Maven o accettare quelle predefinite e quindi fare clic su **Finish** (Fine).

1. Passare al progetto nella scheda **Project** (Progetto) a sinistra e aprire il file **src/main/webapp/WEB-INF/index.jsp**. Sostituire il codice con quello seguente e **salvare le modifiche**:

   ```html
   <html>
    <body>
      <b><% out.println("Hello World!"); %></b>
    </body>
   </html>
   ```
   :::image type="content" source="media/create-hello-world-web-app/open-index-page.png" alt-text="Accesso ad Azure in IntelliJ.":::

## <a name="deploying-web-app-to-azure"></a>Distribuzione dell'app Web in Azure

1. Nella visualizzazione Project Explorer (Esplora progetti) fare clic con il pulsante destro del mouse sul progetto, espandere **Azure** e quindi fare clic su **Deploy to Azure Web Apps** (Distribuisci in App Web di Azure).

1. Nella finestra di dialogo Deploy to Azure è possibile distribuire l'applicazione in un'app Web Tomcat esistente oppure crearne una nuova.

   a. Fare clic su **No available function, click to create a new one** (Nessuna funzione disponibile, fare clic per crearne una) per creare una nuova app Web. In caso contrario, scegliere **Create New WebApp** (Crea nuova app Web) dal menu a discesa WebApp (App Web) se nella sottoscrizione sono presenti app Web.

      :::image type="content" source="media/create-hello-world-web-app/deploy-to-azure-webapps.png" alt-text="Accesso ad Azure in IntelliJ.":::

   Nella finestra di dialogo popup **Create WebApp** (Crea app Web) specificare le informazioni seguenti e fare clic su **OK**: 

      * **Name**: stringa del nome di dominio dell'app Web.
      * **Sottoscrizione** specifica la sottoscrizione di Azure da usare per la nuova app Web.
      * **Platform**: selezionare *Linux*.
      * **Web Container** (Contenitore Web): selezionare *TOMCAT 9.0-jre8* o il valore appropriato.
      * **Gruppo di risorse**: specifica il gruppo di risorse per l'app Web. È possibile selezionare un gruppo di risorse esistente associato all'account Azure o crearne uno nuovo.
      * **App Service Plan** (Piano di servizio app): specifica il piano di servizio app per l'app Web. È possibile selezionare un piano esistente associato all'account Azure o crearne uno nuovo.

   b. Per eseguire la distribuzione in un'app Web esistente, scegliere l'app Web dal menu WebApp e quindi fare clic su **Run** (Esegui).

1. Dopo la distribuzione dell'app Web, il toolkit visualizzerà un messaggio di stato insieme all'URL dell'app Web distribuita, se l'operazione è riuscita.

1. È possibile passare all'app Web usando il collegamento contenuto nel messaggio di stato.

   :::image type="content" source="media/create-hello-world-web-app/browse-web-app.png" alt-text="Accesso ad Azure in IntelliJ.":::

## <a name="managing-deploy-configurations"></a>Gestione delle configurazioni della distribuzione

> [!TIP]
> Dopo aver pubblicato l'app Web, sarà possibile eseguire la distribuzione facendo clic sull'icona della freccia verde sulla barra degli strumenti.

1. Prima di eseguire la distribuzione dell'app Web, è possibile modificare le impostazioni predefinite facendo clic sul menu a discesa per l'app Web e quindi selezionando **Edit Configurations** (Modifica configurazioni).

   :::image type="content" source="media/create-hello-world-web-app/edit-configuration-menu.png" alt-text="Accesso ad Azure in IntelliJ.":::

1. Nella finestra di dialogo **Run/Debug Configurations** (Esecuzione/debug configurazioni) è possibile modificare qualsiasi impostazione predefinita. Fare clic su **OK** per salvare le impostazioni.

## <a name="cleaning-up-resources"></a>Pulizia delle risorse

1. Per eliminare l'app Web, passare alla barra laterale sinistra di **Azure Explorer** e individuare l'elemento **Web Apps** (App Web). 

   > [!NOTE]
   > Se la voce di menu Web Apps (App Web) non viene espansa, aggiornare manualmente l'elenco facendo clic sull'icona **Refresh** (Aggiorna) sulla barra degli strumenti di Azure Explorer oppure facendo clic con il pulsante destro del mouse sulla voce di menu Web Apps e scegliendo **Refresh**.

1. Fare clic con il pulsante destro del mouse sull'app Web da eliminare, quindi scegliere **Delete** (Elimina).

1. Per eliminare il piano di servizio app o il gruppo di risorse, visitare il [portale di Azure](https://portal.azure.com) ed eliminare manualmente le risorse nella sottoscrizione.

## <a name="next-steps"></a>Passaggi successivi

[!INCLUDE [additional-resources](includes/additional-resources.md)]

Per altre informazioni sulla creazione di App Web di Azure, vedere la [Panoramica delle App Web].

<!-- URL List -->

[Azure Toolkit for IntelliJ]: /azure/developer/java/tookit-for-intellij
[Azure Toolkit for Eclipse]: /azure/developer/java/tookit-for-eclipse
[eclipse-hello-world]: ../toolkit-for-eclipse/create-hello-world-web-app.md
[Panoramica delle app Web]: /azure/app-service/app-service-web-overview
[Apache Tomcat]: http://tomcat.apache.org/
[Jetty]: http://www.eclipse.org/jetty/
[intelliJ-sign-in-instructions]: sign-in-instructions.md

<!-- IMG List -->
[marketplace]:media/create-hello-world-web-app/marketplace.png
[file-new-project]: media/create-hello-world-web-app/file-new-project.png
[maven-archetype-webapp]: media/create-hello-world-web-app/maven-archetype-webapp.png
[groupid-and-artifactid]: media/create-hello-world-web-app/groupid-and-artifactid.png
[maven-options]: media/create-hello-world-web-app/maven-options.png
[project-name]: media/create-hello-world-web-app/project-name.png
[open-index-page]: media/create-hello-world-web-app/open-index-page.png
[edit-index-page]: media/create-hello-world-web-app/edit-index-page.png
[deploy-to-azure-menu]: media/create-hello-world-web-app/run-on-web-app-menu.png
[deploy-to-azure-dialog]: media/create-hello-world-web-app/run-on-web-app-dialog.png
[deploy-to-existing-webapp]: media/create-hello-world-web-app/deploy-to-existing-webapp.png
[create-new-web-app-dialog]: media/create-hello-world-web-app/create-new-web-app-dialog.png
[successfully-deployed]: media/create-hello-world-web-app/successfully-deployed.png
[browse-web-app]: media/create-hello-world-web-app/browse-web-app.png
[edit-configuration-menu]: media/create-hello-world-web-app/edit-configuration-menu.png
[edit-configuration-dialog]: media/create-hello-world-web-app/edit-configuration-dialog.png
[clean-resources]: media/create-hello-world-web-app/clean-resource.png