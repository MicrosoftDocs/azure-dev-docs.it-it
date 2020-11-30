---
title: Creare la risorsa App Web statica
description: Creare la risorsa App Web statica con l'estensione di codice di Visual Studio Code per tale servizio.
ms.date: 10/13/2020
ms.topic: include
ms.custom: devx-track-javascript
ms.openlocfilehash: c6daa3f2a7ceb7f981cdaf57bdba61a722edbaa3
ms.sourcegitcommit: 4dac39849ba2e48034ecc91ef578d11aab796e58
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/20/2020
ms.locfileid: "94993589"
---
# <a name="4-create-azure-static-web-app-resource"></a>4. Creare la risorsa App Web statica di Azure

In questa sezione dell'esercitazione creare la risorsa App Web statica con un'estensione di Visual Studio Code per tale servizio ed eseguire il push del codice locale nel repository remoto per compilare, quindi distribuire l'app in Azure.

## <a name="create-a-new-branch-dedicated-to-deployment"></a>Creare un nuovo ramo dedicato alla distribuzione

L'app Web statica di Azure riceve una build da un ramo specifico del repository GitHub. Questa esercitazione usa il ramo `main`. In un nuovo terminale di Visual Studio Code creare un ramo di `live` usato solo per la compilazione e la distribuzione dell'app.

```bash
git checkout -b live
```

## <a name="push-the-live-branch-to-github"></a>Eseguire il push del ramo attivo in GitHub

Nel terminale di Visual Studio Code eseguire il push del ramo locale `live` nel repository remoto.

```bash
git push origin live
```

## <a name="create-a-static-web-app-resource"></a>Creare una risorsa App Web statica

1. Selezionare l'icona di **Azure**, quindi fare clic con il pulsante destro del mouse sul servizio **App Web statiche**, infine selezionare **Crea app Web statica...** . 

    :::image type="content" source="../../media/static-web-app/visualstudiocode-storage-extension-create-static-web-resource.png" alt-text="Screenshot di Visual Studio Code con estensione di Visual Studio":::

1. Immettere le informazioni seguenti nei campi successivi, presentati uno alla volta. 

    |Nome campo| Valore|
    |--|--|
    |Un nome per l'app Web statica.|`Demo-ComputerVisionAnalyzer`|
    |Scegliere il ramo per il repository|`live`| 
    |Selezionare il percorso del codice dell'applicazione.|`/`|
    |Selezionare il percorso del codice di Funzioni di Azure.|Selezionare **Ignora per adesso**|
    |Immettere il percorso dell'output di compilazione relativo alla posizione dell'app.|`build`|
    |Selezionare una località per le nuove risorse|Selezionare una località di Azure vicina.|

## <a name="update-the-github-action-with-secret-environment-variables"></a>Aggiornare l'azione GitHub con le variabili di ambiente segrete

La chiave e l'endpoint di Visione artificiale si trovano nella raccolta di segreti del repository, ma non sono ancora presenti nell'azione GitHub. Questo passaggio aggiunge la chiave e l'endpoint all'azione.

1. Rimuovere le modifiche apportate dalla creazione della risorsa di Azure per ottenere il file di azione GitHub.

    ```bash
    git pull origin live
    ```

1. Nell'editor di Visual Studio Code modificare il file di azione GitHub in `./.github/workflows/` per aggiungere i segreti. 

    :::code language="yml" source="~/../js-e2e-client-cognitive-services/.github/workflows/sample-github-workflow.yml" highlight="34-36" :::

    
1. Aggiungere ed eseguire il commit della modifica nel ramo `live` locale.

    ```bash
    git add . && git commit -m "add secrets to action"
    ```

1. Eseguire il push della modifica nel repository remoto, avviando una nuova azione di compilazione e distribuzione nell'app Web statica di Azure.

    ```bash
    git push origin live
    ```

## <a name="view-the-github-action-build-process"></a>Visualizzare il processo di compilazione dell'azione GitHub

1. In un Web browser aprire il repository GitHub per questa esercitazione e selezionare **Actions** (Azioni). 

1. Selezionare la build principale nell'elenco, quindi selezionare **Build and Deploy Job** (Compila e distribuisci processo) nel menu a sinistra per controllare il processo di compilazione. Attendere fino a quando non viene completata la **compilazione e distribuzione**.

    :::image type="content" source="../../media/static-web-app/browser-screenshot-github-action-build-react-computer-vision-app.png" alt-text=" Selezionare la build principale nell'elenco, quindi selezionare &quot;Build and Deploy Job£ (Compila e distribuisci processo) nel menu a sinistra per controllare il processo di compilazione. Attendere che la compilazione venga completata correttamente.":::

## <a name="view-azure-static-web-site-in-browser"></a>Visualizzare il sito Web statico di Azure nel browser

1. In Visual Studio Code selezionare l'icona **Azure** nel menu a destra, quindi selezionare l'app Web statica, fare clic con il pulsante destro del mouse su **Esplorazione del sito**, infine selezionare **Apri** per visualizzare il sito Web statico pubblico. 

:::image type="content" source="../../media/static-web-app/visualstudiocode-browse-static-web-app.png" alt-text="Selezionare &quot;Esplorazione del sito&quot;, quindi selezionare &quot;Apri&quot; per visualizzare il sito Web statico pubblico. ":::

È anche possibile trovare l'URL per il sito dove segue:
* il portale di Azure per la propria risorsa, nella pagina **Panoramica**.
* l'output di compilazione e distribuzione dell'azione GitHub contiene l'URL del sito alla fine dello script 

## <a name="next-step"></a>Passaggio successivo

> [!div class="nextstepaction"]
> [Esaminare il codice React e l'analisi di Visione artificiale di Servizi cognitivi](add-computer-vision-react-app.md)