---
title: 'Passaggio 3: Creare il servizio app da Visual Studio Code'
description: "Passaggio 3 dell'esercitazione: creazione del servizio app dall'estensione VS Code."
ms.topic: conceptual
ms.date: 11/20/2020
ms.custom: devx-track-python, seo-python-october2019
ms.openlocfilehash: 28de4df6b964b985c459af7c5a920330d4eda726
ms.sourcegitcommit: 29930f1593563c5e968b86117945c3452bdefac1
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/23/2020
ms.locfileid: "95485735"
---
# <a name="3-create-the-app-service-from-visual-studio-code"></a>3: Creare il servizio app da Visual Studio Code

[Passaggio precedente: Preparare l'app](tutorial-deploy-app-service-on-linux-02.md)

In questo passaggio viene creata l'**app Web del Servizio app di Azure** in cui distribuire l'app. Nella terminologia del Servizio app di Azure, un'app Web funge da **host** per il codice dell'app Web e non corrisponde al codice dell'app.

Eseguire questo passaggio prima di distribuire il codice per poter configurare un file di avvio personalizzato al passaggio 4.

1. Nell'area **Azure: Servizio app** **selezionare il comando +** per creare un nuovo servizio app:

    ![Creazione di un nuovo servizio app nell'area App Service (Servizio app)](media/deploy-azure/create-new-app-service-in-app-service-explorer.png)

    In alternativa, usare il comando **Servizio app di Azure: Crea nuova app Web** dal riquadro comandi di VS Code.

1. Fornire le informazioni richieste ai prompt visualizzati:

    - **Immettere un nome per l'app**, che deve essere univoco a livello globale in tutto Azure. Per garantire l'univocità, in genere si usa il proprio nome o quello della società, seguito dal nome dell'app.
    - **Selezionare Python 3.7** come runtime.

1. Quando viene visualizzato un messaggio che indica che il nuovo servizio app è stato creato, selezionare **View Output** (Visualizza output) per passare alla finestra **Output** in VS Code:

    ![Messaggio di VS Code con l'URL per il Servizio app di Azure](media/deploy-azure/url-for-your-new-app-service-and-resource-group-and-plan.png)

1. Per verificare la corretta esecuzione del servizio app, espandere la sottoscrizione nell'area **Azure: App Service** (Azure: Servizio app), fare clic con il pulsante destro del mouse sul nome del servizio app e scegliere **Browse website** (Sfoglia sito Web):

    ![Comando Browse Website (Sfoglia sito Web) in un servizio app nell'area App Service (Servizio app)](media/deploy-azure/select-command-to-browse-website-in-app-service.png)

1. Dal momento che il codice personale non è ancora stato distribuito nel servizio app (operazione che verrà eseguita nel passaggio successivo), viene visualizzata solo un'app predefinita:

    ![App Python predefinita nel Servizio app di Azure in Linux](media/deploy-azure/default-python-app-on-app-service-on-linux.png)

---

## <a name="optional-upload-an-environment-variable-definitions-file"></a>(Facoltativo) Caricare un file di definizioni delle variabili di ambiente

Se si ha un file di definizioni delle variabili di ambiente, è possibile usarlo anche per configurare l'ambiente del servizio app. Per altre informazioni su questi file, caratterizzati in genere dall'estensione *env*, vedere l'articolo sugli [ambienti Python in Visual Studio Code](https://code.visualstudio.com/docs/python/environments#environment-variable-definitions-file).

1. Nell'area **Azure: App Service** (Azure: Servizio app) espandere il nodo relativo al servizio app desiderato e quindi fare clic con il pulsante destro del mouse sul nodo **Application Settings** (Impostazioni applicazione) e scegliere **Upload Local Settings** (Carica impostazioni locali).

1. VS Code chiede di specificare il percorso del file con estensione *env* e quindi lo carica nel servizio app.

1. Al termine del caricamento, è possibile espandere il nodo **Application Settings** (Impostazioni applicazione) per visualizzare i singoli valori. È anche possibile visualizzarli nel portale di Azure passando al servizio app e selezionando **Configurazione**.

    Queste impostazioni dell'applicazione vengono visualizzate come variabili di ambiente nel codice.

> [!TIP]
> Se si creano impostazioni direttamente nel portale di Azure, è possibile salvarle in un file di definizioni facendo clic con il pulsante destro del mouse sul nodo **Application Settings** (Impostazioni applicazione) e scegliendo **Download Remote Settings** (Scarica impostazioni remote). Questo processo assicura che le impostazioni siano presenti nel repository e non solo nel portale.

> [!div class="nextstepaction"]
> [Il servizio app è stato creato: procedere con il passaggio 4 >>>](tutorial-deploy-app-service-on-linux-04.md)

[Problemi? Segnalarli](https://aka.ms/FlaskVSCQuickstartHelp).
