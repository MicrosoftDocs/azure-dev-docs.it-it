---
title: "Passaggio 5: Distribuire un'app Web Python nel Servizio app di Azure in Linux con VS Code"
description: Passaggio 5 dell'esercitazione, distribuzione del codice dell'app Web
ms.topic: conceptual
ms.date: 11/20/2020
ms.custom: devx-track-python, seo-python-october2019
ms.openlocfilehash: 7b3743d417ed3455c59f5b9887ee54728fb7318a
ms.sourcegitcommit: 29930f1593563c5e968b86117945c3452bdefac1
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/23/2020
ms.locfileid: "95485708"
---
# <a name="5-deploy-your-python-web-app-to-azure-app-service-on-linux"></a>5: Distribuire un'app Web Python nel Servizio app di Azure in Linux

[Passaggio precedente: Configurare un file di avvio personalizzato](tutorial-deploy-app-service-on-linux-04.md)

Usare questa procedura per distribuire l'app Python in un servizio app di Azure.

1. In Visual Studio Code aprire l'area **Azure: Servizio app** e selezionare la freccia verso l'alto blu:

   ![Distribuire l'app Web in un servizio app nell'area App Service (Servizio app)](media/deploy-azure/deploy-web-app-to-app-service-in-app-service-explorer.png)

    In alternativa, è possibile fare clic con il pulsante destro del mouse sul nome del servizio app e scegliere il comando **Distribuisci nell'app Web**.

1. Nei prompt che seguono specificare i dettagli seguenti:

    - Per "Select the folder to deploy" (Selezionare la cartella in cui distribuire) selezionare la cartella dell'app corrente.
    - Per "Select Web App" (Selezionare l'app Web) scegliere il servizio app creato nel passaggio precedente.
    - Se viene richiesto di aggiornare la configurazione della build in modo da eseguire i comandi della build, rispondere **Sì**.
    - Se viene richiesto di sovrascrivere la distribuzione esistente, rispondere **Deploy** (Distribuisci).
    - Se viene richiesto di "distribuire sempre l'area di lavoro", rispondere **Sì**.

1. Mentre è in corso il processo di distribuzione, è possibile visualizzarne lo stato di avanzamento nella finestra **Output** di VS Code.

    ![Stato di avanzamento della distribuzione nella finestra di output di VS Code](media/deploy-azure/view-deployment-progress-in-visual-studio-code-output.png)

1. Quando dopo alcuni minuti (a seconda del numero di dipendenze che devono essere installate) la distribuzione viene completata, viene visualizzato il messaggio seguente. Selezionare il pulsante **Esplora sito Web** per visualizzare il sito in esecuzione.

    ![Distribuzione completata con il pulsante Esplora sito Web](media/deploy-azure/web-app-deployment-complete-with-browse-website-button.png)

    ![Esecuzione corretta dell'app nel servizio app](media/deploy-azure/web-app-running-successfully-on-app-service.png)

1. Se l'app predefinita è ancora visibile, attendere un paio di minuti il riavvio del contenitore dopo la distribuzione e riprovare. Se si sta usando un comando di avvio personalizzato e ne è stata verificata la correttezza, continuare con il passaggio 6 per controllare i log.

1. Per verificare che i file siano stati distribuiti, espandere il servizio app nell'area **Azure: Servizio app**, quindi espandere **File**:

    ![Verifica della distribuzione dei file tramite l'area Servizio app](media/deploy-azure/expand-files-node-to-check-deployment-of-web-app-files.png)

    I file *.deployment*, *antenv.tar.gz* e *oryx-manifest.toml* vengono usati dal sistema di compilazione del servizio app. La pagina predefinita dell'app è *hostingstart.html*.

> [!div class="nextstepaction"]
> [L'app è stata distribuita: procedere con il passaggio 6 >>>](tutorial-deploy-app-service-on-linux-06.md)

[Problemi? Segnalarli](https://aka.ms/FlaskVSCQuickstartHelp).
