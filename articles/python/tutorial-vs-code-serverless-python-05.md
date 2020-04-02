---
title: 'Passaggio 5: Distribuire Funzioni di Azure in Python con VS Code'
description: Passaggio 5 dell'esercitazione, distribuzione del codice di funzioni Python in Azure e informazioni sullo streaming di log e sulla sincronizzazione delle impostazioni tra un progetto locale e Azure.
ms.topic: conceptual
ms.date: 09/02/2019
ms.custom: seo-python-october2019
ms.openlocfilehash: 425fb745cec74672cfabc6c3c5eab96821a43224
ms.sourcegitcommit: 1bd9ec6a4115e9162e33b76a933869788e6ab702
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/31/2020
ms.locfileid: "80441166"
---
# <a name="5-deploy-azure-functions-in-python"></a>5: Distribuire Funzioni di Azure in Python

[Passaggio precedente: Eseguire il debug in locale](tutorial-vs-code-serverless-python-04.md)

In questo articolo si userà l'estensione Funzioni di Azure per creare un'app per le funzioni in Azure, oltre ad altre risorse di Azure necessarie. Un'app per le funzioni consente di raggruppare le funzioni come un'unità logica per semplificare la gestione, la distribuzione e la condivisione delle risorse.

Un'app per le funzioni richiede un account di archiviazione di Azure per i dati e un [piano di hosting](/azure/azure-functions/functions-scale#hosting-plan-support). Tutte queste risorse sono organizzate all'interno di un singolo gruppo di risorse.

1. Nell'area **Azure: Funzioni** selezionare il comando **Deploy to Function App** (Distribuisci nell'app per le funzioni) oppure aprire il riquadro comandi (**F1**) e selezionare il comando **Funzioni di Azure: Deploy to Function App** (Distribuisci nell'app per le funzioni). Anche in questo caso, l'app per le funzioni è quella in cui viene eseguito il progetto Python in Azure.

    ![Distribuire la funzione Python in un app per le funzioni di Azure](media/tutorial-vs-code-serverless-python/deploy-a-python-fuction-to-azure-function-app.png)

1. Quando richiesto, selezionare **Create New Function App in Azure** (Crea una nuova app per le funzioni in Azure) e specificare un nome univoco in Azure, in genere usando il nome personale o della società insieme ad altri identificatori univoci. È possibile usare lettere, numeri e trattini. Se in precedenza è stata creata un'app per le funzioni, il relativo nome viene visualizzato in questo elenco di opzioni.

1. L'estensione esegue le azioni seguenti, che è possibile osservare nei messaggi popup e nella finestra **Output** di Visual Studio Code. Il processo richiede alcuni minuti:

    - Creare un gruppo di risorse usando il nome assegnato (rimuovendo i trattini).
    - In questo gruppo di risorse creare l'account di archiviazione, il piano di hosting e l'app per le funzioni. Per impostazione predefinita, viene creato un [piano a consumo](/azure/azure-functions/functions-scale#consumption-plan). Per eseguire le funzioni in un piano dedicato, è necessario [abilitare la pubblicazione con le opzioni di creazione avanzate](/azure/azure-functions/functions-develop-vs-code).
    - Distribuire il codice nell'app per le funzioni.

    L'area **Azure: Funzioni** mostra anche lo stato di avanzamento:

    ![Indicatore di stato della distribuzione nell'area Azure: Functions (Azure: Functions)](media/tutorial-vs-code-serverless-python/deployment-progress-indicator-in-azure-function-explorer.png)

1. Al termine della distribuzione, l'estensione Funzioni di Azure visualizza un messaggio con i pulsanti per tre azioni aggiuntive:

    ![Messaggio che indica la corretta distribuzione con azioni aggiuntive](media/tutorial-vs-code-serverless-python/azure-functions-deployment-success-with-additional-actions.png)

    Per **Stream logs** (Streaming di log) e **Upload settings** (Carica impostazioni), vedere le sezioni successive. Per **View output** (Visualizza output), vedere il passaggio 5 seguente.

1. Dopo la distribuzione, la finestra **Output** visualizza anche l'endpoint pubblico in Azure (l'URL dell'endpoint specifico corrisponderà al nome specificato per l'app per le funzioni):

    <pre>
    HTTP Trigger Urls:

          HttpExample: https://vscode-azure-functions.azurewebsites.net/api/HttpExample
    </pre>

    Usare questo endpoint per eseguire gli stessi test effettuati localmente, usando i parametri URL e/o le richieste con dati JSON nel corpo della richiesta. I risultati dell'endpoint pubblico devono corrispondere a quelli dell'endpoint locale testato in precedenza nella [parte 4](tutorial-vs-code-serverless-python-04.md).

## <a name="stream-logs"></a>Eseguire lo streaming dei log

Il supporto per lo streaming dei log è attualmente in fase di sviluppo, come descritto nel [problema 589](https://github.com/microsoft/vscode-azurefunctions/issues/589) per l'estensione Funzioni di Azure. Il pulsante **Stream logs** (Streaming dei log) nel popup del messaggio di distribuzione alla fine collegherà l'output dei log in Azure a Visual Studio Code. Sarà anche possibile avviare e interrompere lo streaming dei log nell'area **Funzioni di Azure** facendo clic con il pulsante destro del mouse sul progetto Funzioni e scegliendo **Start streaming logs** (Avvia lo streaming dei log) o **Stop streaming logs** (Interrompi lo streaming dei log).

Attualmente, tuttavia, questi comandi non sono ancora operativi. Lo streaming dei log è invece disponibile in un browser eseguendo il comando seguente, sostituendo `<app_name>` con il nome dell'app per le funzioni in Azure:

```
# Replace <app_name> with the name of your Functions app on Azure
func azure functionapp logstream <app_name> --browser
```

## <a name="sync-local-settings-to-azure"></a>Sincronizzare le impostazioni locali con Azure

Il pulsante **Upload settings** (Carica impostazioni) nel popup del messaggio di distribuzione applica le modifiche apportate al file *local.settings.json* in Azure. È anche possibile richiamare il comando nell'area **Funzioni di Azure** espandendo il nodo del progetto Funzioni, facendo clic con il pulsante destro del mouse su **Applications Settings** (Impostazioni applicazioni) e scegliendo **Upload local settings** (Carica impostazioni locali). È inoltre possibile usare il riquadro comandi per selezionare il comando **Funzioni di Azure: Upload local settings** (Carica impostazioni locali).

Il caricamento delle impostazioni aggiorna le impostazioni esistenti e aggiunge quelle nuove definite nel file *local.settings.json*. Il caricamento non rimuove da Azure le impostazioni non presenti nell'elenco del file locale. Per rimuovere queste impostazioni, espandere il nodo **Applications Settings** (Impostazioni applicazioni) nell'area **Funzioni di Azure**, fare clic con il pulsante destro del mouse sull'impostazione e scegliere **Delete Setting** (Elimina impostazione). È anche possibile modificare le impostazioni direttamente nel portale di Azure.

Per applicare le modifiche apportate tramite il portale o tramite **Azure Explorer** nel file *local.settings.json*, fare clic con il pulsante destro del mouse sul nodo **Application Settings** (Impostazioni applicazioni) e scegliere il comando **Download remote settings** (Scarica impostazioni remote). È inoltre possibile usare il riquadro comandi per selezionare il comando **Funzioni di Azure: Download Remote Settings** (Scarica impostazioni remote).

> [!div class="nextstepaction"]
> [Le funzioni sono state distribuite: procedere con il passaggio 6 >>>](tutorial-vs-code-serverless-python-06.md)

[Si è verificato un problema](https://www.research.net/r/PWZWZ52?tutorial=vscode-functions-python&step=05-deploy)