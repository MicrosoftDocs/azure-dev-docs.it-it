---
title: Distribuire app Node.js nel Servizio app di Azure da Visual Studio Code
description: Parte 3 dell'esercitazione, distribuire il sito Web
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/20/2019
ms.author: kraigb
ms.openlocfilehash: 5891c5a9dafe87987f725b38147957fb6a961ca5
ms.sourcegitcommit: c04984b6367e922dbc5973af44f8cd0ca81ce157
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/30/2019
ms.locfileid: "71686256"
---
# <a name="deploy-the-website"></a>Distribuire il sito Web

[Passaggio precedente: Creare l'app](tutorial-vscode-azure-app-service-node-02.md)

In questa sezione si distribuisce il sito Web Node.js usando Visual Studio Code e l'estensione Servizio app di Azure. Questa esercitazione usa il modello di distribuzione più semplice in cui l'app viene compressa e distribuita nel Servizio app di Azure in Linux.

1. Prima di distribuire l'app, assicurarsi che sia in ascolto sulla porta specificata dalla variabile di ambiente `PORT`: `process.env.PORT`.

1. Nella cartella dell'app, ad esempio `myExpressApp` del passaggio precedente, avviare VS Code usando il comando seguente:

    ```bash
    code .
    ```

1. In VS Code selezionare l'icona di Azure per aprire l'area **Servizio app di Azure**, quindi selezionare l'icona della freccia verso l'alto blu per distribuire l'app in Azure:

    ![Eseguire la distribuzione nell'app Web](media/deploy-azure/deploy.png)

    > [!TIP]
    > In alternativa, aprire il **riquadro comandi** (**F1**), digitare 'deploy to web app' e selezionare il comando **Servizio app di Azure: Distribuisci nell'app Web**.

1. Quando richiesto, immettere le informazioni seguenti:

    a. Selezionare la cartella corrente per l'app (in questa esercitazione, `myExpressApp`).

    a. Selezionare **Crea una nuova app Web**.

    a. Immettere un nome univoco globale per l'app e premere **INVIO**. I caratteri validi per il nome dell'app sono "a-z", "0-9" e "-".

    a. Scegliere una posizione in un'area di Azure, come illustrato in [Aree](https://azure.microsoft.com/regions/), vicina alla propria posizione o ad altri servizi a cui può essere necessario accedere.

    a. Scegliere la **versione di Node.js**; l'opzione consigliata è LTS.

1. Mentre l'estensione crea l'app, nella finestra **Output** di VS Code viene visualizzato lo stato di avanzamento. Può essere necessario selezionare l'output **Servizio app di Azure**.

    ![Finestra dell'output di Visual Studio Code per il Servizio app di Azure](media/deploy-azure/output-window.png)

1. Selezionare **Sì** quando viene chiesto di aggiornare la configurazione per eseguire `npm install` nel server di destinazione. L'app viene quindi distribuita.

    ![Distribuzione configurata](media/deploy-azure/server-build.png)

1. All'avvio della distribuzione, viene richiesto di aggiornare l'area di lavoro in modo che tutte le distribuzioni successive usino automaticamente come destinazione la stessa app Web del Servizio app. Scegliere **Sì** per assicurarsi che le modifiche vengano distribuite nell'app corretta.

    ![Distribuzione configurata](media/deploy-azure/save-configuration.png)

1. Una volta completata la distribuzione, selezionare **Esplora sito Web** nel messaggio per visualizzare il sito Web appena distribuito.

## <a name="troubleshooting"></a>risoluzione dei problemi

Se viene visualizzato il messaggio di errore "Non si dispone delle autorizzazioni necessarie per visualizzare la directory o la pagina", è probabile che l'app non sia stata avviata correttamente. La visualizzazione dei log, come descritto nel [passaggio successivo](tutorial-vscode-azure-app-service-node-04.md), può risultare utile per diagnosticare e risolvere il problema. Se non è possibile risolvere il problema, contattare il supporto facendo clic sul pulsante **Si è verificato un problema** sotto. Microsoft sarà lieta di fornire aiuto.

## <a name="updating-the-website"></a>Aggiornamento del sito Web

È possibile distribuire le modifiche all'app usando lo stesso processo e scegliendo l'app esistente invece di crearne una nuova.

----

> [!div class="nextstepaction"]
> [Il sito si trova in Azure](tutorial-vscode-azure-app-service-node-04.md) [Si è verificato un problema](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azureappservice&step=deploy-app)
