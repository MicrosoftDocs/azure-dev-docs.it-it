---
title: Creare un'app Web Hello World per Servizio app di Azure con Eclipse
description: Questa esercitazione descrive come usare il toolkit di Azure per Eclipse per creare un'app Web Hello World per Azure.
services: app-service
keywords: java, eclipse, app web, servizio app di azure, hello world, avvio rapido
documentationcenter: java
author: selvasingh
ms.assetid: 20d41e88-9eab-462e-8ee3-89da71e7a33f
ms.reviewer: asirveda
ms.date: 08/25/2020
ms.service: app-service
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: web
ms.custom: devx-track-java
ms.openlocfilehash: 57cb6f2b731ee6d9522787d70efbaf35e029c632
ms.sourcegitcommit: 717e32b68fc5f4c986f16b2790f4211967c0524b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/30/2020
ms.locfileid: "91586149"
---
# <a name="create-a-hello-world-web-app-for-azure-app-service-using-eclipse"></a>Creare un'app Web Hello World per Servizio app di Azure con Eclipse

Questo articolo illustra i passaggi necessari per creare un'app Web Hello World di base e pubblicarla in Servizio app di Azure usando [Azure Toolkit for Eclipse](https://marketplace.eclipse.org/content/azure-toolkit-eclipse).

> [!NOTE]
>
> Se si preferisce usare IntelliJ IDEA, vedere l'[esercitazione simile per IntelliJ][intellij-hello-world].
>
>[!INCLUDE [quickstarts-free-trial-note](includes/quickstarts-free-trial-note.md)]
>
> Dopo aver completato questa esercitazione, non dimenticare di pulire le risorse. In tal modo, con l'esecuzione di questa guida non si supererà la quota dell'account gratuito.
>

[!INCLUDE [basic-prerequisites](includes/basic-prerequisites.md)]

## <a name="installation-and-sign-in"></a>Installazione e accesso

La procedura seguente illustra il processo di accesso ad Azure nell'ambiente di sviluppo Eclipse.

1. Se non è stato installato il plug-in, vedere [Installazione di Azure Toolkit for Eclipse](installation.md).

1. Per accedere all'account Azure, fare clic su **Tools** (Strumenti), su **Azure** e quindi su **Sign In** (Accedi).

   :::image type="content" source="media/sign-in-instructions/eclipse-azure-signin.png" alt-text="Accesso ad Azure nell'IDE di Eclipse.":::

1. Nella finestra **Azure Sign In** (Accesso ad Azure) selezionare **Device Login** (Accesso dispositivo) e quindi fare clic su **Sign in** (Accedi) ([altre opzioni di accesso](sign-in-instructions.md)).

1. Nella finestra di dialogo **Azure Device Login** (Accesso dispositivo Azure) fare clic su **Copy&Open** (Copia e apri).

1. Nel browser incollare il codice dispositivo (copiato facendo clic su **Copy&Open** nell'ultimo passaggio) e quindi fare clic su **Avanti**.

1. Selezionare l'account Azure e completare le procedure di autenticazione necessarie per eseguire l'accesso.

1. Dopo l'accesso, chiudere il browser e tornare all'IDE di Eclipse. Nella finestra di dialogo **Select Subscriptions** (Seleziona sottoscrizioni) selezionare le sottoscrizioni da usare e quindi fare clic su **OK**.

### <a name="install-required-software-optional"></a>Installare il software necessario *(facoltativo)*

Per assicurarsi che i componenti necessari funzionino con i progetti di app Web, seguire questa procedura:

1. Fare clic sul menu **Help** (?) e quindi scegliere **Install New Software** (Installa nuovo software).

1. Nella finestra di dialogo **Available Software** (Software disponibile) fare clic su **Manage** (Gestisci) e assicurarsi che sia selezionata la versione più recente di Eclipse, ad esempio *2020-06*.

1. Fare clic su **Apply and Close** (Applica e chiudi). Espandere il menu a discesa *Work with* (Compatibile con) per visualizzare i siti suggeriti. Selezionare il sito della versione di Eclipse più recente per eseguire una query sul software disponibile.

1. Scorrere l'elenco verso il basso e selezionare l'elemento **Web, XML, Java EE and OSGi Enterprise Development** (Web, XML, Java EE e OSGi Enterprise Development). Fare clic su **Avanti**.

1. Nella finestra Install Details (Dettagli installazione) fare clic su **Next** (Avanti).

1. Nella finestra di dialogo Revisione licenze leggere le condizioni dei contratti di licenza. Se si accettano le condizioni dei contratti di licenza, fare clic su **I accept the terms of the license agreements** (Accetto le condizioni dei contratti di licenza) e quindi fare clic su **Finish** (Fine). 

   > [!NOTE]
   > È possibile controllare lo stato di avanzamento dell'installazione nell'angolo in basso a destra dell'area di lavoro di Eclipse.

1. Se viene chiesto di riavviare Eclipse per completare l'installazione, fare clic su **Riavvia ora**.

## <a name="creating-a-web-app-project"></a>Creazione di un progetto di app Web

1. Fare clic su **File**, espandere **New** (Nuovo) e quindi fare clic su **Project** (Progetto). Nella finestra di dialogo New Project (Nuovo progetto) espandere **Web**, selezionare **Dynamic Web Project** (Progetto Web dinamico) e fare clic su **Next** (Avanti).

   > [!TIP]
   > Se **Web** non è indicato nell'elenco tra i progetti disponibili, vedere [questa sezione](#install-required-software-optional) per assicurarsi che il software Eclipse richiesto sia disponibile.

1. Ai fini di questa esercitazione, denominare il progetto **MyWebApp**. L'aspetto della schermata sarà simile al seguente:
   
   ![Proprietà del nuovo progetto Web dinamico][dynamic-web-project-properties]

1. Fare clic su **Fine**.

1. Nel riquadro di esplorazione pacchetti a sinistra espandere **MyWebApp**. Fare clic con il pulsante destro del mouse su **WebContent**, passare con il puntatore del mouse su **New** (Nuovo) e quindi fare clic su **Other** (Altro).

1. Espandere **Web** per trovare l'opzione **JSP File** (File JSP). Fare clic su **Avanti**.

1. Nella finestra di dialogo **New JSP File** (Nuovo file JSP) denominare il file **index.jsp**, mantenere il nome **MyWebApp/WebContent** per la cartella padre, quindi fare clic su **Next** (Avanti).

   ![Finestra di dialogo New JSP File (Nuovo file JSP)][new-jsp-file-dialog]

1. Per le finalità di questa esercitazione, nella finestra di dialogo **Select JSP Template** (Seleziona modello JSP) selezionare **New JSP File (html 5)** (Nuovo file JSP - html 5) e quindi fare clic su **Finish** (Fine).

1. Quando in Eclipse viene aperto il file index.jsp, aggiungere testo in modo da visualizzare dinamicamente **Hello World!** all'interno dell'elemento `<body>` esistente. Il contenuto `<body>` aggiornato deve avere un aspetto simile all'esempio seguente:
   
   ```jsp
   <body>
   <b><% out.println("Hello World!"); %></b>
   </body>
   ```
1. Salvare index.jsp.

## <a name="deploying-the-web-app-to-azure"></a>Distribuzione dell'app Web in Azure

1. Nel riquadro di esplorazione pacchetti a sinistra fare clic con il pulsante destro del mouse sul progetto e scegliere **Azure** e quindi fare clic su **Publish as Azure Web App** (Pubblica come app Web di Azure).
   
   ![Publish as Azure Web App][publish-as-azure-web-app]

1. Quando viene visualizzata la finestra di dialogo **Deploy Web App** (Distribuisci app Web) è possibile scegliere una delle opzioni seguenti:

   * Selezionare un'app Web esistente, se presente.

   * Se non esiste già un'app Web, fare clic su **Create** (Crea).

      In questa finestra di dialogo è possibile configurare l'ambiente di runtime, il gruppo di risorse del piano di servizio dell'app e le impostazioni dell'app. Se necessario, creare nuove risorse.

      Specificare le informazioni necessarie per l'app Web nella finestra di dialogo **Create App Service** (Crea servizio app) e quindi fare clic su **Create** (Crea).

1. Selezionare l'app Web e quindi fare clic su **Deploy** (Distribuisci).

1. Dopo aver distribuito l'app Web, il toolkit visualizza lo stato come **Pubblicato** sotto la scheda **Log attività di Azure**, come collegamento ipertestuale all'URL dell'app Web distribuita.

1. È possibile passare all'app Web usando il collegamento contenuto nel messaggio di stato.

   ![Collegamento all'app Web][browse-web-app]

## <a name="cleaning-up-resources"></a>Pulizia delle risorse

1. Dopo aver pubblicato l'app Web in Azure, è possibile gestirla facendo clic con il pulsante destro del mouse in Azure Explorer e scegliendo una delle opzioni del menu di scelta rapida. È ad esempio possibile **eliminare** l'app Web per pulire le risorse per l'esercitazione.

   ![Gestire il servizio app][manage-app-service]

[!INCLUDE [show-azure-explorer](includes/show-azure-explorer.md)]

## <a name="next-steps"></a>Passaggi successivi

[!INCLUDE [additional-resources](includes/additional-resources.md)]

Per altre informazioni sulla creazione di App Web di Azure, vedere la [Panoramica delle App Web].

<!-- URL List -->

[Azure Toolkit for Eclipse]: /azure/developer/java/tookit-for-eclipse
[Azure Toolkit for IntelliJ]: ../toolkit-for-intellij
[intellij-hello-world]: ../toolkit-for-intellij/create-hello-world-web-app.md
[Panoramica delle app Web]: /azure/app-service/app-service-web-overview
[Apache Tomcat]: http://tomcat.apache.org/
[Jetty]: http://www.eclipse.org/jetty/

<!-- IMG List -->

[browse-web-app]: media/create-hello-world-web-app/browse-web-app.png
[dynamic-web-project-properties]: media/create-hello-world-web-app/dynamic-web-project-properties.png
[new-jsp-file-dialog]: media/create-hello-world-web-app/new-jsp-file-dialog.png
[publish-as-azure-web-app]: media/create-hello-world-web-app/publish-as-azure-web-app.png
[publish-status]: media/create-hello-world-web-app/publish-status.png
[manage-app-service]: media/create-hello-world-web-app/manage-app-service.png