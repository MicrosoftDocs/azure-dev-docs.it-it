---
title: Express.js App converte il testo in sintesi vocale con servizi cognitivi
description: Usare il riconoscimento vocale di servizi cognitivi per convertire il testo in sintesi vocale, illustrato nel client e nel server.
ms.topic: tutorial
ms.date: 01/20/2021
ms.custom: languages:JavaScript, devx-track-javascript
ms.openlocfilehash: 911d38854856f2add28958454f7ce020c1cf2a31
ms.sourcegitcommit: 6fbf9e489b194586887a2c11152044be5b3a2b99
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/25/2021
ms.locfileid: "98760346"
---
# <a name="expressjs-app-converts-text-to-speech-with-cognitive-services-speech"></a>Express.js App converte il testo in sintesi vocale con servizi cognitivi

In questa esercitazione aggiungere il riconoscimento vocale di servizi cognitivi a un'app Express.js esistente per aggiungere la conversione da testo a riconoscimento vocale usando il servizio di riconoscimento vocale di servizi cognitivi. La conversione di testo in sintesi vocale consente di fornire audio senza il costo di generare manualmente l'audio. 

Questa esercitazione illustra tre modi diversi per convertire il testo in sintesi vocale dal riconoscimento vocale di servizi cognitivi di Azure:

* JavaScript client ottiene direttamente l'audio 
* Il server JavaScript Ottiene l'audio dal file (*. MP3
* Il server JavaScript Ottiene l'audio da arrayBuffer in memoria

## <a name="application-architecture"></a>Architettura dell'applicazione

Nell'esercitazione viene accettata un'app Express.js minima e viene aggiunta la funzionalità utilizzando una combinazione di:

* nuova route per l'API del server per fornire la conversione da testo a riconoscimento vocale, restituendo un flusso MP3
* nuova route per un modulo HTML che consente di immettere le informazioni
* il nuovo modulo HTML, con JavaScript, fornisce una chiamata sul lato client al servizio di riconoscimento vocale

Questa applicazione fornisce tre diverse chiamate per convertire la voce in testo:

* La prima chiamata al server crea un file nel server, quindi lo restituisce al client. Questa operazione viene in genere utilizzata per un testo più lungo o un testo che si conosce più volte. 
* La seconda chiamata al server è per il testo a breve termine e è la Guida in memoria prima di essere restituita al client. 
* La chiamata del client illustra una chiamata diretta al servizio di riconoscimento vocale usando l'SDK. È possibile scegliere di effettuare questa chiamata se si dispone di un'applicazione solo client senza un server. 

## <a name="prerequisites"></a>Prerequisiti


- [Node.js 10.1 + e NPM](https://nodejs.org/en/download) -installato nel computer locale.
- [Visual Studio Code](https://code.visualstudio.com/): installato nel computer locale. 
- L'[estensione Servizio app di Azure per VS Code](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice) (installata dall'interno di VS Code).
- [Git](https://git-scm.com/downloads): usato per eseguire il push in GitHub, che attiva l'azione GitHub.
- Usare [Azure cloud Shell](/azure/cloud-shell/quickstart) usando il [![lancio di incorporamento](../../includes/media/cloud-shell-try-it/hdi-launch-cloud-shell.png "Avviare Azure Cloud Shell")](https://shell.azure.com) bash   
- Se si preferisce, [installare](/cli/azure/install-azure-cli) l'interfaccia della riga di comando di Azure per eseguire i relativi comandi di riferimento.
   - Se si usa un'installazione locale, accedere all'interfaccia della riga di comando usando il comando [az login](/cli/azure/reference-index#az-login).  Per completare il processo di autenticazione, seguire la procedura visualizzata nel terminale.  Per altre opzioni di accesso, vedere [accedere con l'interfaccia della](/cli/azure/authenticate-azure-cli) riga di comando di Azure.
  - Quando richiesto, installare le estensioni dell'interfaccia della riga di comando di Azure al primo utilizzo.  Per altre informazioni sulle estensioni, vedere [Usare le estensioni con l'interfaccia della riga di comando di Azure](/cli/azure/azure-cli-extensions-overview).
  - Eseguire [az version](/cli/azure/reference-index?#az_version) per trovare la versione e le librerie dipendenti installate. Per eseguire l'aggiornamento alla versione più recente, eseguire [az upgrade](/cli/azure/reference-index?#az_upgrade).

## <a name="download-sample-expressjs-repo"></a>Scaricare il repository Express.js di esempio 

1. Usando git, clonare il repository di esempio Express.js nel computer locale. 

    ```bash
    git clone https://github.com/Azure-Samples/js-e2e-express-server
    ```

1. Passare alla nuova directory per l'esempio.

    ```bash
    cd js-e2e-express-server
    ```

1. Aprire il progetto in Visual Studio Code.

    ```bash
    code .
    ```

1. Aprire un nuovo terminale in Visual Studio Code e installare le dipendenze del progetto.

    ```bash
    npm install
    ```

## <a name="install-cognitive-services-speech-sdk-for-javascript"></a>Installare cognitive Services Speech SDK per JavaScript

Dal terminale Visual Studio Code installare l'SDK per la sintesi vocale dei servizi cognitivi di Azure.

```bash
npm install microsoft-cognitiveservices-speech-sdk
```

## <a name="create-a-speech-module-for-the-expressjs-app"></a>Creare un modulo di riconoscimento vocale per l'app Express.js

1. Per integrare l'SDK di riconoscimento vocale nell'applicazione Express.js, creare un file nella `src` cartella denominata `azure-cognitiveservices-speech.js` .
1. Aggiungere il codice seguente per eseguire il pull delle dipendenze e creare una funzione per convertire il testo in sintesi vocale.

    :::code language="javascript" source="~/../js-e2e-express-server-cognitive-services/text-to-speech/src/azure-cognitiveservices-speech.js" highlight="3,21,32" :::

    * Parameters: il file estrae le dipendenze per l'uso dell'SDK, dei flussi, dei buffer e del file system (FS). La `textToSpeech` funzione accetta quattro argomenti. Se viene inviato un nome file con percorso locale, il testo viene convertito in un file audio. Se non viene inviato un nome file, viene creato un flusso audio in memoria. 
    * Metodo dell'SDK per la sintesi vocale. il metodo del sintetizzatore di riconoscimento vocale [. speakTextAsync](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/speechsynthesizer#speakTextAsync_string___e__SpeechSynthesisResult_____void___e__string_____void__AudioOutputStream___PushAudioOutputStreamCallback___PathLike_) restituisce tipi diversi, in base alla configurazione ricevuta. 
        Il metodo restituisce il risultato, che differisce a seconda di ciò che è stato richiesto al metodo:
        * Crea file 
        * Creare un flusso in memoria come matrice di buffer
    * Formato audio: il formato audio selezionato è MP3, ma esistono [altri formati](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/speechsynthesisoutputformat?preserve-view=true&view=azure-node-latest) , insieme ad altri [metodi di configurazione audio](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/audioconfig?preserve-view=true&view=azure-node-latest#methods). 

    Il metodo locale, `textToSpeech` , esegue il wrapping e converte la funzione di callback SDK in un suggerimento. 

## <a name="create-a-new-route-for-the-expressjs-app"></a>Creare una nuova route per l'app Express.js

1. Aprire il file `src/server.js`. 
1. Aggiungere il `azure-cognitiveservices-speech.js` modulo come dipendenza all'inizio del file:

    :::code language="javascript" source="~/../js-e2e-express-server-cognitive-services/text-to-speech/src/server.js" range="3"  :::
    

1. Aggiungere una nuova route API per chiamare il metodo **textToSpeech** creato nella sezione precedente dell'esercitazione. 

    :::code language="javascript" source="~/../js-e2e-express-server-cognitive-services/text-to-speech/src/server.js" range="30-51" highlight="45-50" :::

    Questo metodo accetta i parametri obbligatori e facoltativi per il `textToSpeech` Metodo dalla QueryString. Se è necessario creare un file, viene sviluppato un nome file univoco. Il `textToSpeech` metodo viene chiamato in modo asincrono e invia tramite pipe il risultato all' `res` oggetto Response (). 

## <a name="update-the-client-web-page-with-a-form"></a>Aggiornare la pagina Web del client con un modulo 

Aggiornare la pagina Web HTML del client con un modulo che raccoglie i parametri richiesti. Il parametro facoltativo viene passato in base al controllo audio selezionato dall'utente. Poiché questa esercitazione fornisce un meccanismo per chiamare il servizio riconoscimento vocale di Azure dal client, viene fornito anche il codice JavaScript. 

Aprire il `/public/client.html` file e sostituirne il contenuto con il codice seguente:

:::code language="html" source="~/../js-e2e-express-server-cognitive-services/text-to-speech/public/client.html" highlight="75, 102 137" :::

Righe evidenziate nel file: 

* Riga 74: l'SDK di riconoscimento vocale di Azure viene inserito nella libreria client, usando il `cdn.jsdelivr.net` sito per distribuire il pacchetto NPM. 
* Riga 102: il `updateSrc` metodo aggiorna l'URL dei controlli audio `src` con la QueryString, incluse la chiave, l'area e il testo. 
* Riga 137: se un utente seleziona il `Get directly from Azure` pulsante, la pagina Web chiama direttamente in Azure dalla pagina client ed elabora il risultato. 

## <a name="create-cognitive-services-speech-resource"></a>Creare una risorsa vocale di servizi cognitivi

Creare la risorsa vocale con i comandi dell'interfaccia della riga di comando di Azure in un Azure Cloud Shell.


1. Accedere al [Azure cloud Shell](https://shell.azure.com). A questo scopo, è necessario eseguire l'autenticazione in un browser con l'account, che dispone dell'autorizzazione per una sottoscrizione di Azure valida. 
1. Creare un gruppo di risorse per la risorsa di riconoscimento vocale. 

    ```azurecli
    az group create \
        --location eastus \
        --name tutorial-resource-group-eastus
    ```

1. Creare una risorsa vocale nel gruppo di risorse.

    ```azurecli
    az cognitiveservices account create \
        --kind SpeechServices \
        --location eastus \
        --name tutorial-speech \
        --resource-group tutorial-resource-group-eastus \
        --sku F0
    ```

    Questo comando avrà esito negativo se è già stata creata la risorsa di sintesi vocale gratuita. 

1. Usare il comando per ottenere i valori della chiave per la nuova risorsa di riconoscimento vocale. 

    ```azurecli
    az cognitiveservices account keys list \
        --name tutorial-speech \
        --resource-group tutorial-resource-group-eastus \
        --output table
    ```

1. Copiare una delle chiavi. 

    Usare la chiave nel modulo Web per l'autenticazione al servizio riconoscimento vocale di Azure.

## <a name="run-the-expressjs-app-to-convert-text-to-speech"></a>Eseguire l'app Express.js per convertire il testo in sintesi vocale

1. Avviare l'app con il comando bash seguente.

    ```bash
    npm start
    ```

1. Aprire l'app Web in un browser.

    ```
    http://localhost:3000    
    ```

1. Incollare la chiave vocale nella casella di testo evidenziato. 

    :::image type="content" source="../media/speech-tutorial/expressjs-webapp-form-with-speech-key-field-highlighted.png" alt-text="Screenshot del browser del Web Form con il campo di input della chiave vocale evidenziato.":::

1. Facoltativamente, modificare il testo in un nuovo elemento. 

1. Selezionare uno dei tre pulsanti per iniziare la conversione al formato audio:
    * Ottenere direttamente dalla chiamata lato client Azure ad Azure
    * Controllo audio per audio da file
    * Controllo audio per audio da buffer

    È possibile notare un piccolo ritardo tra la selezione del controllo e la riproduzione audio. 

## <a name="create-new-azure-app-service-in-visual-studio-code"></a>Crea nuovo servizio app Azure in Visual Studio Code

1. Nel riquadro comandi (**CTRL**+**MAIUSC**+**P**), digitare "create web" e selezionare **Servizio app di Azure: Crea una nuova app Web - Avanzate**. Usare il comando Avanzate per avere il controllo completo sulla distribuzione, tra cui il gruppo di risorse, il piano di servizio app e il sistema operativo, invece di usare le impostazioni predefinite di Linux.

1. Rispondere alle richieste come segue:

    - Selezionare l'account **Sottoscrizione**.
    - Per **immettere un nome univoco globale** come `my-text-to-speech-app` . 
        - Immettere un nome univoco in tutte le aree di Azure. Usare solo caratteri alfanumerici ('A-Z', 'a-z' e '0-9') e trattini ('-')
    - Selezionare `tutorial-resource-group-eastus` per il gruppo di risorse.
    - **Selezionare uno stack di runtime** di `Node 10.1` o più recente. 
    - Selezionare il sistema operativo Linux.
    - Selezionare **Crea un nuovo piano di servizio app**, specificare un nome, ad esempio `my-text-to-speech-app-plan`, quindi selezionare il [piano tariffario](../core/what-is-azure-for-javascript-development.md#free-tier-resources) **F1 Gratuito**.
    - Selezionare il piano tariffario gratuito **F1** .
    - Per la risorsa di Application Insight, selezionare **Ignora per adesso**.
    - Selezionare la `eastus` località. 

1. Dopo un breve periodo di tempo, Visual Studio Code informa che la creazione è stata completata. Chiudere la notifica con il pulsante **X** .

## <a name="deploy-local-expressjs-app-to-remote-app-service-in-visual-studio-code"></a>Distribuire l'app Express.js locale al servizio app remoto in Visual Studio Code

1. Con l'app Web sul posto, distribuire il codice dal computer locale. Selezionare l'icona di Azure per aprire Esplora **servizi app Azure** , espandere il nodo sottoscrizione, fare clic con il pulsante destro del mouse sul nome dell'app Web appena creata e scegliere **Distribuisci in app Web**.

1. Al prompt selezionare la cartella radice dell'app Express.js, selezionare di nuovo l'account della **sottoscrizione** e quindi selezionare il nome dell'app Web, `my-text-to-speech-app` , creato in precedenza.

1. Quando si esegue la distribuzione in Linux, selezionare **Sì** se viene richiesto di aggiornare la configurazione per `npm install` l'esecuzione nel server di destinazione.

    ![Richiesta di aggiornamento della configurazione nel server Linux di destinazione](../media/deploy-azure/server-build.png)

1. Una volta completata la distribuzione, selezionare **Esplora sito Web** nel messaggio per visualizzare l'app Web appena distribuita. 

1. (Facoltativo): è possibile apportare modifiche ai file di codice, quindi usare l' **app Distribuisci in Web** nell'estensione del servizio app Azure per aggiornare l'app Web.

## <a name="stream-remote-service-logs-in-visual-studio-code"></a>Flussi di log del servizio remoto in Visual Studio Code

Visualizzare l'output generato dall'app in esecuzione tramite chiamate a `console.log`. Questo output viene visualizzato nella finestra **Output** in Visual Studio Code.

1. In **app Azure Service** Explorer fare clic con il pulsante destro del mouse sul nuovo nodo app e scegliere **Avvia log in streaming**.

    <pre>
    Starting Live Log Stream ---
    </pre>

1. Aggiornare la pagina Web alcune volte nel browser per visualizzare altro output dei log.


## <a name="clean-up-resources-by-removing-resource-group"></a>Pulire le risorse rimuovendo il gruppo di risorse

Al termine di questa esercitazione, è necessario rimuovere il gruppo di risorse, che include la risorsa, per assicurarsi che non venga addebitato altro utilizzo. 

Nel Azure Cloud Shell usare il comando dell' [interfaccia della riga di comando di Azure](/cli/azure/group#az_group_delete) per eliminare il gruppo di risorse:

```azurecli
az group delete --name tutorial-resource-group-eastus  -y
```

L'esecuzione del comando può richiedere alcuni minuti. 

## <a name="next-steps"></a>Passaggi successivi

* [Distribuire l'app MongoDB Express.js nel servizio app da Visual Studio Code](deploy-nodejs-mongodb-app-service-from-visual-studio-code.md)