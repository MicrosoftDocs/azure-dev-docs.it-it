---
title: Distribuire app Node.js nel Servizio app di Azure da Visual Studio Code
description: Distribuire un'applicazione Express.js Node.js nel servizio app di Azure con l'estensione Servizio app di Visual Studio Code.
ms.topic: tutorial
ms.date: 01/11/2021
ms.custom: devx-track-js
ms.openlocfilehash: 2c019cc9ae13b81ecde934faee6d7d7a9fadf07a
ms.sourcegitcommit: 657f43a5048cd17b080b40b5090d575c8d7f5eaf
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/13/2021
ms.locfileid: "98173251"
---
# <a name="deploy-nodejs-to-azure-app-service-using-visual-studio-code"></a>Distribuire Node.js nel Servizio app di Azure con Visual Studio Code

Distribuire un'applicazione Node.js nel servizio app di Azure (in Linux o Windows) con l'[estensione Servizio app](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice) di Visual Studio Code.

Distribuire un'app Node.js in Azure usando Git e l'estensione Servizio app di Azure. A tale scopo:

* Creare un'app Express.js
* Inizializzare un repository Git locale
* Creare una risorsa app Web in cui ospitare l'app
* Distribuire l'app nella risorsa
* Visualizzare i log remoti in locale

## <a name="walkthrough-video"></a>Video della procedura dettagliata

Guardare questo video per una procedura dettagliata completa del contenuto di questo articolo.

> [!VIDEO https://channel9.msdn.com/Shows/Docs-Azure/Deploy-to-Azure-App-Service-using-Visual-Studio-Code/player]

## <a name="1-set-up-your-development-environment"></a>1. Configurazione dell'ambiente di sviluppo

- Un account Azure con una sottoscrizione attiva. [È possibile crearne uno gratuitamente](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-appservice-extension&mktingSource=vscode-tutorial-appservice-extension).
- [Visual Studio Code](https://code.visualstudio.com/).
- L'[estensione Servizio app di Azure per VS Code](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice) (installata dall'interno di VS Code).
- [Node.js 12 e versioni successive e npm](https://nodejs.org/en/download).

## <a name="2-sign-in-to-azure"></a>2. Accedere ad Azure

[!INCLUDE [azure-sign-in](../includes/azure-sign-in.md)]

## <a name="3-create-a-local-expressjs-app"></a>3. Creare un'app Express.js locale

[!INCLUDE [Create a local Express.js app](../includes/create-node-app.md)]

## <a name="4-run-your-local-expressjs-app"></a>4. Eseguire l'app Express.js locale

[!INCLUDE [Run your local Express.js app](../includes/run-node-app.md)]

## <a name="5-initialize-git-in-visual-studio-code-for-current-app"></a>5. Inizializzare Git in Visual Studio Code per l'app corrente

1. Nel terminale assicurarsi di trovarsi nella cartella *expressApp1*, quindi avviare Visual Studio Code con il comando seguente:

    ```bash
    code .
    ```

1. In Visual Studio Code selezionare l'icona del controllo del codice sorgente per aprire l'area **Controllo del codice sorgente** e quindi selezionare **Inizializza repository** per inizializzare un repository Git locale:

    ![Inizializzare repository git](../media/deploy-azure/git-init.png)

1. Ai prompt scegliere *expressApp1* come cartella dell'area di lavoro.

1. Dopo l'inizializzazione del repository, immettere il messaggio "Initial commit" e selezionare il segno di spunta per creare il commit iniziale dei file di origine.

    ![Completare un commit iniziale nel repository](../media/deploy-azure/initial-commit.png)

## <a name="6-create-app-service-resource-in-visual-studio-code"></a>6. Creare una risorsa Servizio app in Visual Studio Code

1. Nel riquadro comandi (**CTRL**+**MAIUSC**+**P**), digitare "create web" e selezionare **Servizio app di Azure: Crea una nuova app Web - Avanzate**. Usare il comando Avanzate per avere il controllo completo sulla distribuzione, tra cui il gruppo di risorse, il piano di servizio app e il sistema operativo, invece di usare le impostazioni predefinite di Linux.

1. Rispondere alle richieste come segue:

    - Selezionare l'account **Sottoscrizione**.
    - Per **Enter a globally unique name** (Immettere un nome univoco globale) immettere un nome univoco in tutto Azure. Usare solo caratteri alfanumerici ('A-Z', 'a-z' e '0-9') e trattini ('-')
    - Selezionare **Crea nuovo gruppo di risorse** e specificare un nome, ad esempio `AppServiceTutorial-rg`.
    - Selezionare un sistema operativo (Windows o Linux)
    - Solo Linux: selezionare una versione di Node.js. Per Windows, la versione viene specificata con un'impostazione dell'app più avanti.
    - Selezionare **Crea un nuovo piano di servizio app**, specificare un nome, ad esempio `AppServiceTutorial-plan`, quindi selezionare il [piano tariffario](../core/what-is-azure-for-javascript-development.md#free-tier-resources) **F1 Gratuito**.
    - Per la risorsa di Application Insight, selezionare **Ignora per adesso**.
    - Selezionare una località nelle vicinanze.

1. Dopo un breve periodo di tempo, VS Code informa che la creazione è stata completata. Chiudere la notifica con il pulsante **X**:

    ![Notifica sul completamento della creazione dell'app Web](../media/deploy-azure/creation-complete.png)

1. Dopo aver creato l'app Web, indicare a VS Code di distribuire il codice dal repository Git locale. Selezionare l'icona di Azure per aprire l'area **Servizio app di Azure**, espandere il nodo della sottoscrizione, fare clic con il pulsante destro del mouse sul nome dell'app Web appena creata e scegliere **Configure Deployment Source** (Configura l'origine della distribuzione).

    ![Comando per configurare l'origine della distribuzione per un'app Web](../media/deploy-azure/configure-deployment-source.png)

1. Quando richiesto, selezionare **LocalGit**.

1. Se si esegue la distribuzione nel servizio app in Windows, è necessario specificare due impostazioni prima della distribuzione:

    1. In VS Code espandere il nodo del nuovo servizio app, quindi fare clic con il pulsante destro del mouse su **Impostazioni applicazione** e scegliere **Aggiungi nuova impostazione**:

        ![Comando per l'aggiunta di un'impostazione dell'app](../media/deploy-azure/add-setting.png)

    1. Immettere `WEBSITE_NODE_DEFAULT_VERSION` per la chiave e `10.15.2` per il valore dell'impostazione. Questa impostazione specifica la versione di Node.js.
    1. Ripetere la procedura per creare una chiave per `SCM_DO_BUILD_DURING_DEPLOYMENT` con il valore `1`. Questa impostazione forza il server a eseguire `npm install` al momento della distribuzione.
    1. Espandere il nodo **Impostazioni applicazione** per verificare se le impostazioni sono attive.

1. Selezionare l'icona della freccia in su blu per distribuire il codice in Azure:

    ![Icona per la distribuzione nell'app Web](../media/deploy-azure/deploy.png)

1. Ai prompt selezionare la cartella *expressApp1*, selezionare di nuovo l'account della **sottoscrizione** e quindi selezionare il nome dell'app Web creata in precedenza.

1. Per la distribuzione in Linux, selezionare **Sì** quando viene richiesto di aggiornare la configurazione per eseguire `npm install` nel server di destinazione.

    ![Richiesta di aggiornamento della configurazione nel server Linux di destinazione](../media/deploy-azure/server-build.png)

1. Per Linux e Windows, quando richiesto, selezionare **Sì** per **Distribuisci sempre l'area di lavoro "nodejs-docs-hello-world" in (nome app)"** . Selezionando **Sì** si indica VS Code di impostare automaticamente come destinazione la stessa app Web del servizio app con le distribuzioni successive.

1. Una volta completata la distribuzione, selezionare **Esplora sito Web** nel messaggio per visualizzare l'app Web appena distribuita. Nel browser verrà visualizzato il testo "Hello World!"

1. (Facoltativo): È possibile apportare modifiche ai file di codice, quindi usare di nuovo il pulsante di distribuzione per aggiornare l'app Web.

## <a name="7-stream-remote-service-logs-in-visual-studio-code"></a>7. Trasmettere i log remoti del servizio in Visual Studio Code

Visualizzare l'output generato dall'app in esecuzione tramite chiamate a `console.log`. Questo output viene visualizzato nella finestra **Output** in Visual Studio Code.

1. Nell'area **Servizio app di Azure** fare clic con il pulsante destro del mouse sul nodo dell'app e scegliere **Start Streaming Logs** (Avvia lo streaming dei log).

    ![Comando Visualizza log in streaming](../media/deploy-azure/start-streaming-logs.png)

1. Quando viene richiesto, scegliere di abilitare la registrazione e riavviare l'applicazione.

    ![Richiesta di abilitare la registrazione e riavviare](../media/deploy-azure/enable-restart.png)

1. Una volta riavviata l'app, verrà aperta la finestra **Output** di VS Code con una connessione al flusso di log che mostra l'output.

    <pre>
    Connecting to log stream...
    2020-03-04T19:29:44  Welcome, you are now connected to log-streaming service. The default timeout is 2 hours.
    Change the timeout with the App Setting SCM_LOGSTREAM_TIMEOUT (in seconds).
    </pre>

1. Aggiornare la pagina Web alcune volte nel browser per visualizzare altro output dei log.

## <a name="8-make-changes-and-redeploy"></a>8. Apportare modifiche e ripetere la distribuzione

Apportare alcune modifiche e [ridistribuire](../how-to/deploy-web-app.md#deploy-or-redeploy-to-app-service-with-visual-studio-code) l'app usando l'estensione del servizio app. 

## <a name="9-clean-up-resources"></a>9. Pulire le risorse

Per pulire le risorse, fare clic con il pulsante destro del mouse sul servizio app nell'estensione Servizio app di Visual Studio Code, quindi scegliere **Elimina**.

:::image type="content" source="../media/deploy-azure/delete-azure-app-service-with-visual-studio-code-extension.png" alt-text="Per pulire le risorse, fare clic con il pulsante destro del mouse sul servizio app nell'estensione Servizio app di Visual Studio Code, quindi scegliere **Elimina**":::.

## <a name="next-steps"></a>Passaggi successivi

* [Informazioni su come configurare le impostazioni dell'app](../how-to/configure-web-app-settings.md)

[!INCLUDE [tutorial-next-steps](../includes/tutorial-next-steps.md)]
