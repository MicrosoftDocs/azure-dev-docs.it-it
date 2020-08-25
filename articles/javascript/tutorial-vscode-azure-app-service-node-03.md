---
title: Distribuire app nel Servizio app di Azure da Visual Studio Code
description: Parte 3 dell'esercitazione, distribuire il sito Web
ms.topic: conceptual
ms.date: 03/04/2020
ms.custom: devx-track-javascript
ms.openlocfilehash: 5d812f9aa6efb308cafcb5d3e3ccb1ce852e93f1
ms.sourcegitcommit: 815cf2acff71e849735f7afce54723f03ffa5df3
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/17/2020
ms.locfileid: "88501506"
---
# <a name="deploy-the-app-to-azure"></a>Distribuire l'app in Azure

[Passaggio precedente: Creare l'app](tutorial-vscode-azure-app-service-node-02.md)

In questo passaggio l'app Node.js viene distribuita in Azure usando Git deploy tramite VS Code e l'estensione Servizio app di Azure. A questo scopo, è necessario prima inizializzare un repository Git locale, quindi creare un'app Web in Azure e infine configurare VS Code per l'uso di Git deploy.

1. Nel terminale assicurarsi di trovarsi nella cartella *expressApp1*, quindi avviare Visual Studio Code con il comando seguente:

    ```bash
    code .
    ```

1. In VS Code selezionare l'icona del controllo del codice sorgente per aprire l'area di esplorazione **Controllo del codice sorgente** e quindi selezionare **+** per inizializzare un repository Git locale:

    ![Inizializzare repository git](media/deploy-azure/git-init.png)

1. Ai prompt scegliere *expressApp1* come cartella dell'area di lavoro.

1. Dopo l'inizializzazione del repository, immettere il messaggio "Initial commit" e selezionare il segno di spunta per creare il commit iniziale dei file di origine.

    ![Completare un commit iniziale nel repository](media/deploy-azure/initial-commit.png)

1. Nel riquadro comandi (**CTRL**+**MAIUSC**+**P**), digitare "create web" e selezionare **Servizio app di Azure: Crea una nuova app Web - Avanzate**. Usare il comando Avanzate per avere il controllo completo sulla distribuzione, tra cui il gruppo di risorse, il piano di servizio app e il sistema operativo, invece di usare le impostazioni predefinite di Linux.

1. Rispondere alle richieste come segue:

    - Selezionare l'account **Sottoscrizione**.
    - Per **Enter a globally unique name** (Immettere un nome univoco globale) immettere un nome univoco in tutto Azure. Usare solo caratteri alfanumerici ('A-Z', 'a-z' e '0-9') e trattini ('-')
    - Selezionare **Crea nuovo gruppo di risorse** e specificare un nome, ad esempio `AppServiceTutorial-rg`.
    - Selezionare un sistema operativo (Windows o Linux)
    - Solo Linux: selezionare una versione di Node.js. Per Windows, la versione viene specificata con un'impostazione dell'app più avanti.
    - Selezionare **Crea un nuovo piano di servizio app**, specificare un nome, ad esempio `AppServiceTutorial-plan`, quindi selezionare il piano tariffario **F1 Gratuito**.
    - Per la risorsa di Application Insight, selezionare **Ignora per adesso**.
    - Selezionare una località nelle vicinanze.

1. Dopo un breve periodo di tempo, VS Code informa che la creazione è stata completata. Chiudere la notifica con il pulsante **X**:

    ![Notifica sul completamento della creazione dell'app Web](media/deploy-azure/creation-complete.png)

1. Dopo aver creato l'app Web, indicare a VS Code di distribuire il codice dal repository Git locale. Selezionare l'icona di Azure per aprire l'area **Servizio app di Azure**, espandere il nodo della sottoscrizione, fare clic con il pulsante destro del mouse sul nome dell'app Web appena creata e scegliere **Configure Deployment Source** (Configura l'origine della distribuzione).

    ![Comando per configurare l'origine della distribuzione per un'app Web](media/deploy-azure/configure-deployment-source.png)

1. Quando richiesto, selezionare **LocalGit**.

1. Se si esegue la distribuzione nel servizio app in Windows, è necessario specificare due impostazioni prima della distribuzione:

    1. In VS Code espandere il nodo del nuovo servizio app, quindi fare clic con il pulsante destro del mouse su **Impostazioni applicazione** e scegliere **Aggiungi nuova impostazione**:

        ![Comando per l'aggiunta di un'impostazione dell'app](media/deploy-azure/add-setting.png)

    1. Immettere `WEBSITE_NODE_DEFAULT_VERSION` per la chiave e `10.15.2` per il valore dell'impostazione. Questa impostazione specifica la versione di Node.js.
    1. Ripetere la procedura per creare una chiave per `SCM_DO_BUILD_DURING_DEPLOYMENT` con il valore `1`. Questa impostazione forza il server a eseguire `npm install` al momento della distribuzione.
    1. Espandere il nodo **Impostazioni applicazione** per verificare se le impostazioni sono attive.

1. Selezionare l'icona della freccia in su blu per distribuire il codice in Azure:

    ![Icona per la distribuzione nell'app Web](media/deploy-azure/deploy.png)

1. Ai prompt selezionare la cartella *expressApp1*, selezionare di nuovo l'account della **sottoscrizione** e quindi selezionare il nome dell'app Web creata in precedenza.

1. Per la distribuzione in Linux, selezionare **Sì** quando viene richiesto di aggiornare la configurazione per eseguire `npm install` nel server di destinazione.

    ![Richiesta di aggiornamento della configurazione nel server Linux di destinazione](media/deploy-azure/server-build.png)

1. Per Linux e Windows, quando richiesto, selezionare **Sì** per **Distribuisci sempre l'area di lavoro "nodejs-docs-hello-world" in (nome app)"** . Selezionando **Sì** si indica VS Code di impostare automaticamente come destinazione la stessa app Web del servizio app con le distribuzioni successive.

1. Una volta completata la distribuzione, selezionare **Esplora sito Web** nel messaggio per visualizzare l'app Web appena distribuita. Nel browser verrà visualizzato il testo "Hello World!"

1. (Facoltativo): È possibile apportare modifiche ai file di codice, quindi usare di nuovo il pulsante di distribuzione per aggiornare l'app Web.

> [!div class="nextstepaction"]
> [Il sito si trova in Azure](tutorial-vscode-azure-app-service-node-04.md) [Si è verificato un problema](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azureappservice&step=deploy-app)
