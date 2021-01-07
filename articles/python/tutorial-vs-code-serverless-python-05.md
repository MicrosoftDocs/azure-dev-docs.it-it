---
title: 'Passaggio 5: Distribuire Funzioni di Azure serverless in Python con VS Code'
description: Passaggio 5 dell'esercitazione, distribuzione del codice di funzioni Python serverless in Azure e informazioni sullo streaming di log e sulla sincronizzazione delle impostazioni tra un progetto locale e Azure.
ms.topic: conceptual
ms.date: 11/30/2020
ms.custom: devx-track-python, seo-python-october2019, contperf-fy21q2
ms.openlocfilehash: 24c24aa7a52b756688449eafa0645d0ac971cea3
ms.sourcegitcommit: 4f9ce09cbf9663203c56f5b12ecbf70ea68090ed
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/06/2021
ms.locfileid: "97911381"
---
# <a name="5-deploy-azure-functions-in-python"></a>5: Distribuire Funzioni di Azure in Python

[Passaggio precedente: Eseguire il debug in locale](tutorial-vs-code-serverless-python-04.md)

La distribuzione di una funzione in Azure implica la creazione di un'app per le funzioni in Azure oltre che di tutte le altre risorse di Azure necessarie. Un'app per le funzioni consente di raggruppare le funzioni come un'unità logica per semplificare la gestione, la distribuzione e la condivisione delle risorse.

Un'app per le funzioni richiede un account di archiviazione di Azure per i dati e un [piano di hosting](/azure/azure-functions/functions-scale#hosting-plan-support). Tutte queste risorse sono organizzate all'interno di un singolo gruppo di risorse.

1. Nell'area **Azure: Funzioni** selezionare il comando **Deploy to Function App** (Distribuisci nell'app per le funzioni) oppure aprire il riquadro comandi (**F1**) e selezionare il comando **Funzioni di Azure: Deploy to Function App** (Distribuisci nell'app per le funzioni). Anche in questo caso, l'app per le funzioni è quella in cui viene eseguito il progetto Python in Azure.

    ![Distribuire la funzione Python in un app per le funzioni di Azure](media/tutorial-vs-code-serverless-python/deploy-a-python-fuction-to-azure-function-app.png)

1. Quando richiesto (**Selezionare un'app per le funzioni in Azure**), selezionare **Create New Function App in Azure** (Crea una nuova app per le funzioni in Azure) e specificare un nome univoco in Azure, in genere usando il nome personale o della società insieme ad altri identificatori univoci. È possibile usare lettere, numeri e trattini.

    Se in precedenza è stata creata un'app per le funzioni, il relativo nome viene visualizzato in questo elenco di opzioni.

1. Alle due richieste successive selezionare una versione di Python e una località di Azure.

1. L'estensione esegue le azioni seguenti, che è possibile osservare nei messaggi popup e nella finestra **Output** di Visual Studio Code. Il processo richiede alcuni minuti:

    - Creare un gruppo di risorse nella località selezionata usando il nome assegnato (rimuovendo i trattini).
    - In questo gruppo di risorse creare l'account di archiviazione, il piano di hosting e l'app per le funzioni. Funzioni di Azure usa un [piano a consumo](/azure/azure-functions/functions-scale#consumption-plan) per impostazione predefinita. Per eseguire le funzioni in un piano dedicato, è necessario [abilitare la pubblicazione con le opzioni di creazione avanzate](/azure/azure-functions/functions-develop-vs-code).
    - Distribuire il codice nell'app per le funzioni.

    L'area **Azure: Funzioni** mostra anche lo stato di avanzamento:

    ![Indicatore di stato della distribuzione nell'area Azure: Functions (Azure: Functions)](media/tutorial-vs-code-serverless-python/deployment-progress-indicator-in-azure-function-explorer.png)

1. Al termine della distribuzione, l'estensione Funzioni di Azure visualizza un messaggio con i pulsanti per tre azioni aggiuntive:

    ![Messaggio che indica la corretta distribuzione con azioni aggiuntive](media/tutorial-vs-code-serverless-python/azure-functions-deployment-success-with-additional-actions.png)

    Per **Stream logs** (Streaming di log) e **Upload settings** (Carica impostazioni), vedere le sezioni successive.

1. Selezionare **View output** (Visualizza output) per passare alla finestra **Output**. L'output mostra anche l'endpoint pubblico in Azure (l'URL dell'endpoint specifico corrisponderà al nome specificato per l'app per le funzioni):

    <pre>
    1:38:04 PM vscode-azure-functions: HTTP Trigger Urls:
      HttpExample: https://vscode-azure-functions.azurewebsites.net/api/HttpExample
    </pre>

    Usare questo endpoint per eseguire gli stessi test effettuati localmente, usando i parametri URL e/o le richieste con dati JSON nel corpo della richiesta. I risultati dell'endpoint pubblico devono corrispondere a quelli dell'endpoint locale testato in precedenza nella [parte 4](tutorial-vs-code-serverless-python-04.md).

## <a name="examine-logs-live-metrics"></a>Esaminare i log (Metriche attive)

Il pulsante **Trasmetti log** nel popup con il messaggio sulla distribuzione apre un browser nella sezione **Metriche attive** del portale di Azure per la funzione. È anche possibile connettersi alle metriche nella finestra di esplorazione di **Funzioni di Azure**, facendo clic con il pulsante destro del mouse sul nome del progetto di funzioni e quindi scegliendo **Start streaming logs** (Avvia lo streaming dei log).

## <a name="sync-local-settings-to-azure"></a>Sincronizzare le impostazioni locali con Azure

Il pulsante **Upload settings** (Carica impostazioni) nel popup del messaggio di distribuzione applica le modifiche apportate al file *local.settings.json* in Azure. È anche possibile richiamare il comando nell'area **Funzioni di Azure** espandendo il nodo del progetto Funzioni, facendo clic con il pulsante destro del mouse su **Applications Settings** (Impostazioni applicazioni) e scegliendo **Upload local settings** (Carica impostazioni locali). È inoltre possibile usare il riquadro comandi per selezionare il comando **Funzioni di Azure: Upload local settings** (Carica impostazioni locali).

Il caricamento delle impostazioni aggiorna le impostazioni esistenti e aggiunge quelle nuove definite nel file *local.settings.json*. Il caricamento non rimuove da Azure le impostazioni non presenti nell'elenco del file locale. Per rimuovere queste impostazioni, espandere il nodo **Applications Settings** (Impostazioni applicazioni) nell'area **Funzioni di Azure**, fare clic con il pulsante destro del mouse sull'impostazione e scegliere **Delete Setting** (Elimina impostazione). È anche possibile modificare le impostazioni direttamente nel portale di Azure.

Per applicare le modifiche apportate tramite il portale o tramite **Azure Explorer** nel file *local.settings.json*, fare clic con il pulsante destro del mouse sul nodo **Application Settings** (Impostazioni applicazioni) e scegliere il comando **Download remote settings** (Scarica impostazioni remote). È inoltre possibile usare il riquadro comandi per selezionare il comando **Funzioni di Azure: Download Remote Settings** (Scarica impostazioni remote).

> [!div class="nextstepaction"]
> [Le funzioni sono state distribuite: procedere con il passaggio 6 >>>](tutorial-vs-code-serverless-python-06.md)

[Problemi? Segnalarli](https://aka.ms/python-functions-qs-ms-survey).
