---
title: "Passaggio 5: Distribuire un'app Web Python nel Servizio app di Azure in Linux con VS Code"
description: Passaggio 5 dell'esercitazione, distribuzione del codice dell'app Web
ms.topic: conceptual
ms.date: 09/12/2019
ms.custom: devx-track-python, seo-python-october2019
ms.openlocfilehash: e7c600314f1535589ca15daaa3bbbd9ffdc69b9d
ms.sourcegitcommit: 815cf2acff71e849735f7afce54723f03ffa5df3
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/17/2020
ms.locfileid: "88501456"
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

1. Mentre è in corso il processo di distribuzione, è possibile visualizzarne lo stato di avanzamento nella finestra **Output** di VS Code.

    ![Stato di avanzamento della distribuzione nella finestra di output di VS Code](media/deploy-azure/view-deployment-progress-in-visual-studio-code-output.png)

1. Quando dopo alcuni minuti (a seconda del numero di dipendenze che devono essere installate) la distribuzione viene completata, viene visualizzato il messaggio seguente. Selezionare il pulsante **Esplora sito Web** per visualizzare il sito in esecuzione.

    ![Distribuzione completata con il pulsante Esplora sito Web](media/deploy-azure/web-app-deployment-complete-with-browse-website-button.png)

    ![Esecuzione corretta dell'app nel servizio app](media/deploy-azure/web-app-running-successfully-on-app-service.png)

1. Per verificare che i file siano stati distribuiti, espandere il servizio app nell'area **Azure: Servizio app**, quindi espandere **File**:

    ![Verifica della distribuzione dei file tramite l'area Servizio app](media/deploy-azure/expand-files-node-to-check-deployment-of-web-app-files.png)

    La cartella *antenv* è quella in cui il servizio app crea un ambiente virtuale con le dipendenze. Se si espande questo nodo, è possibile verificare che i pacchetti specificati in *requirements.txt* sono stati installati in *antenv/lib/python3.7/site-packages*.

> [!div class="nextstepaction"]
> [L'app è stata distribuita: procedere con il passaggio 6 >>>](tutorial-deploy-app-service-on-linux-06.md)

[Problemi? Segnalarli](https://aka.ms/FlaskVSCQuickstartHelp).
