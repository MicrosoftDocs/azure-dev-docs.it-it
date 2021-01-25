---
title: Caricare l'immagine nell'archiviazione BLOB con Visual Studio Code - Servizio app/CosmosDB
description: Usare un'app React/TypeScript per caricare un file in BLOB del servizio di archiviazione di Azure. Questa esercitazione è incentrata sull'uso di ambienti locali e remoti con le estensioni di Visual Studio Code.
ms.topic: tutorial
ms.date: 12/17/2020
ms.custom: scenarios:getting-started, languages:JavaScript, devx-track-javascript, azure-sdk-storage-blob-typescript-version-12.2.1
ms.openlocfilehash: 19fc63272ff579ca0bb04802b04a7d44e9114ba4
ms.sourcegitcommit: 6fbf9e489b194586887a2c11152044be5b3a2b99
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/25/2021
ms.locfileid: "98759566"
---
# <a name="upload-an-image-to-an-azure-storage-blob"></a>Caricare un'immagine in un BLOB del servizio di archiviazione di Azure

Usare un'app React lato client per caricare un file di immagine in un BLOB del servizio di archiviazione di Azure usando un pacchetto npm di Archiviazione di Azure. 

L'attività di programmazione TypeScript viene eseguita automaticamente. Questa esercitazione è incentrata sull'uso corretto degli ambienti di Azure locali e remoti all'interno Visual Studio Code con le estensioni di Azure.

* [**Codice di esempio**](https://github.com/Azure-Samples/js-e2e-browser-file-upload-storage-blob)

## <a name="application-architecture-and-functionality"></a>Architettura e funzionalità dell'applicazione

Questa esercitazione include diverse attività principali di Azure per sviluppatori JavaScript:

* Eseguire un'app React/TypeScript in locale con Visual Studio Code
* Creare una risorsa **BLOB del servizio di archiviazione di Azure** e configurarla per il caricamento di file
    * Configurare CORS
    * Creare un token di firma di accesso condiviso
* Configurare il codice per la libreria client di Azure SDK per l'uso del token di firma di accesso condiviso per l'autenticazione nel servizio

L'app React di esempio, [disponibile in GitHub](https://github.com/Azure-Samples/js-e2e-browser-file-upload-storage-blob), è costituita dagli elementi seguenti:

* **App React** ospitata sulla porta 3000
* **Script della libreria client Azure SDK** per il caricamento in BLOB del servizio di archiviazione

:::image type="content" source="../media/tutorial-browser-file-upload/browser-react-app-azure-storage-resource-image-uploaded-displayed.png" alt-text="Semplice app React connessa ai BLOB di archiviazione di Azure.":::

## <a name="1-set-up-development-environment"></a>1. Configurare l'ambiente di sviluppo

- Un account utente di Azure con una sottoscrizione attiva. [È possibile crearne uno gratuitamente](https://azure.microsoft.com/free/).
- [Node.js 10 e npm](https://nodejs.org/en/download), il sistema di gestione pacchetti di Node.js installato nel computer locale.
- [Visual Studio Code](https://code.visualstudio.com/) installato nel computer locale. 
- Estensioni di Visual Studio Code:
    - [Archiviazione di Azure](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurestorage), per visualizzare la risorsa di archiviazione

## <a name="2-clone-and-run-the-initial-react-app"></a>2. Clonare ed eseguire l'app React iniziale

1. Aprire Visual Studio Code.
1. Clonare il repository GitHub selezionando l'icona Git, quindi **Clona repository**. Per eseguire questa procedura, è necessario accedere a GitHub. Usare l'URL del repository: 

    `https://github.com/Azure-Samples/js-e2e-browser-file-upload-storage-blob`

1. Selezionare una cartella nel computer locale in cui clonare l'esempio. Quando richiesto, aprire il repository clonato. 

    :::image type="content" source="../media/tutorial-browser-file-upload/vscode-git-clone-repository.png" alt-text="Clonare il repository GitHub selezionando l'icona Git e quindi 'Clona repository'.":::

1. In Visual Studio Code aprire una finestra del terminale ed eseguire il comando seguente per installare le dipendenze dell'esempio.

    ```javascript
    npm install
    ```

1. Nella stessa finestra del terminale eseguire il comando per eseguire l'app Web.

    ```javascript
    npm start
    ```

1. Aprire un Web browser e usare l'URL seguente per visualizzare l'app Web nel computer locale.

    ```url
    http://localhost:3000/
    ```

    Se nel browser viene visualizzata la semplice app Web con il messaggio che indica che l'archiviazione non è configurata, questa sezione dell'esercitazione è stata completata correttamente.

    :::image type="content" source="../media/tutorial-browser-file-upload/browser-react-app-no-azure-storage-resource-configured.png" alt-text="Semplice app Node.js connessa al database MongoDB.":::

1. Arrestare il codice premendo CTRL+C nel terminale Visual Studio Code.

## <a name="3-create-storage-resource-with-visual-studio-extension"></a>3. Creare la risorsa di archiviazione con l'estensione di Visual Studio

1. Passare all'estensione Archiviazione di Azure. Fare clic con il pulsante destro del mouse sulla sottoscrizione e quindi scegliere `Create Storage Account...`.

    :::image type="content" source="../media/tutorial-browser-file-upload/visualstudiocode-storage-extension-create-resource.png" alt-text="Passare all'estensione Archiviazione di Azure. Fare clic con il pulsante destro del mouse sulla sottoscrizione e quindi scegliere 'Crea account di archiviazione'.":::

1. Seguire le istruzioni riportate nella tabella seguente per informazioni su come creare la risorsa di archiviazione.

    |Proprietà|Valore|
    |--|--|
    |Immettere un nome univoco globale per la nuova app Web.| Immettere un valore, ad esempio `fileuploaddemo`, per la risorsa di archiviazione.<br><br> Questo nome univoco è il **nome della risorsa** usato nella sezione successiva. Usare un massimo di 24 caratteri e numeri. Questo **nome dell'account** sarà necessario più avanti.|

1. Al termine del processo di creazione dell'app, viene visualizzata una notifica con le informazioni sulla nuova risorsa. 

    :::image type="content" source="../media/tutorial-browser-file-upload/visualstudiocode-storage-extension-create-resource-complete.png" alt-text="Al termine del processo di creazione dell'app, viene visualizzata una notifica con le informazioni sulla nuova risorsa.":::

## <a name="4-set-storage-account-name-in-code-file"></a>4. Impostare il nome dell'account di archiviazione nel file di codice

Impostare il nome della risorsa del passaggio precedente in `src/azure-storage-blob.ts`. 

```typescript
const storageAccountName = process.env.storageresourcename || "fileuploaddemo"; 
```

## <a name="5-generate-your-shared-access-signature-sas-token"></a>5. Generare il token di firma di accesso condiviso 

Generare il token di firma di accesso condiviso prima di configurare CORS. 

1. Nell'estensione di Visual Studio Code per la risorsa di archiviazione, fare clic con il pulsante del mouse sulla risorsa e quindi scegliere **Apri nel portale**. Viene aperto il portale di Azure con la risorsa di archiviazione.
1. Nella sezione **Impostazioni** selezionare **Firma di accesso condiviso**. 
1. Configurare il token di firma di accesso condiviso con le impostazioni seguenti. 

    | Proprietà|valore|
    |--|--|
    |Servizi consentiti|BLOB|
    |Tipi di risorse consentiti|Servizio, contenitore, oggetto|
    |Autorizzazioni consentite|Lettura, scrittura, eliminazione, visualizzazione elenco, aggiunta, creazione|
    |Abilitare le eliminazioni della versione|Selezionata|
    |Data/ora di inizio e di scadenza|Accettare la data/ora di inizio e **impostare la data e l'ora di fine su 24 ore in futuro**. Il token di firma di accesso condiviso è valido solo per 24 ore.|
    |Solo HTTPS|Selezionato|
    |Livello di routing preferito|Basic|
    |Chiave di firma|Key1 selezionata|

    :::image type="content" source="../media/tutorial-browser-file-upload/azure-portal-storage-blob-generate-sas-token.png" alt-text="Configurare il token di firma di accesso condiviso come illustrato nell'immagine. Le impostazioni vengono spiegate sotto l'immagine.":::

1. Selezionare **Genera firma di accesso condiviso e stringa di connessione**. Copiare immediatamente il token di firma di accesso condiviso. Non sarà possibile visualizzare questo token, quindi se non viene copiato sarà necessario crearne uno nuovo. 

## <a name="set-sas-token-in-code-file"></a>Impostare il token di firma di accesso condiviso nel file di codice

Il token di firma di accesso condiviso viene usato quando vengono eseguite query sulla risorsa basata sul cloud.

1. Se il token inizia con un punto interrogativo, rimuovere il carattere `?`. Il file di codice fornisce il carattere `?`, quindi non è necessario nel token.

1. Impostare il token di firma di accesso condiviso in `src/azure-storage-blob.ts` per il valore sasToken aggiungendolo nella stringa vuota. Lasciare il resto del codice invariato. 

```typescript
// remove `?` if it is first character of token
const sasToken = process.env.storagesastoken || "";
```

## <a name="6-configure-cors-for-azure-storage-resource"></a>6. Configurare CORS per una risorsa di Archiviazione di Azure

Configurare CORS per la risorsa in modo che il codice React sul lato client possa accedere all'account di archiviazione. 

1. Sempre nel portale di Azure, nella sezione Impostazioni selezionare **CORS**. 
1. Configurare CORS come mostrato nell'immagine. Le impostazioni vengono spiegate sotto l'immagine. 

    | Proprietà|Valore|
    |--|--|
    |Origini consentite|`*`|
    |Metodi consentiti|Tutti ad eccezione di patch.|
    |Intestazioni consentite|`*`|
    |Intestazioni esposte|`*`|
    |Validità massima|86400|

    :::image type="content" source="../media/tutorial-browser-file-upload/azure-portal-storage-blob-cors.png" alt-text="Configurare CORS come mostrato nell'immagine. Le impostazioni vengono spiegate sotto l'immagine.":::

1. Selezionare **Salva** sopra le impostazioni per salvarle nella risorsa. Il codice non richiede modifiche per l'uso di queste impostazioni CORS. 

## <a name="7-run-project-locally-to-verify-connection-to-storage-account"></a>7. Eseguire il progetto in locale per verificare la connessione all'account di archiviazione

Il token di firma di accesso condiviso e il nome dell'account di archiviazione vengono impostati nel file `src/azure-storage-blob.ts`, quindi è ora possibile eseguire l'applicazione.

1. Se l'app non è in esecuzione, riavviarla:

    ```javascript
    npm start
    ```

1. Aprire l'URL seguente in un browser:

    `http://localhost:3000` 

    :::image type="content" source="../media/tutorial-browser-file-upload/browser-react-app-azure-storage-resource-configured-upload-button-displayed.png" alt-text="Verrà visualizzato il sito Web React connesso ai BLOB di archiviazione di Azure con un pulsante di selezione file e un pulsante di caricamento file.":::

1. Selezionare un'immagine nella cartella `images` da caricare, quindi selezionare il pulsante **Carica**. . 

    Il codice client front-end React chiama `src/azure-storage-blob.ts` per l'autenticazione in Azure, quindi creare un contenitore di archiviazione (se non esiste già) e caricarvi il file. 

    I passaggi di questa esercitazione sono stati completati. Le sezioni rimanenti illustrano l'app e consentono di risolvere i problemi.

## <a name="troubleshoot-local-connection-to-storage-account"></a>Risolvere i problemi di una connessione locale all'account di archiviazione

Se è stato ricevuto un errore o il file non viene caricato nel contenitore, verificare quanto segue:

* Ricreare il token di firma di accesso condiviso, assicurandosi che venga creato a livello di Risorsa di archiviazione e non a livello di contenitore. Copiare il nuovo token nel codice nella posizione corretta.
* Verificare che la stringa di token copiata nel codice non contenga il `?` (punto interrogativo) all'inizio della stringa.
* Verificare l'impostazione di CORS per la Risorsa di archiviazione.

## <a name="upload-button-functionality"></a>Funzionalità del pulsante di caricamento

Il file TypeScript `src/App.tsx` viene fornito come parte della creazione dell'app con create-react-app. Il file è stato modificato per fornire il pulsante di selezione file e il pulsante di caricamento e il codice di supporto per fornire tale funzionalità. 

Il codice che si connette al codice di archiviazione BLOB di Azure è evidenziato. La chiamata a `uploadFileToBlob` restituisce tutti i BLOB (file) nel contenitore in forma di elenco semplice. L'elenco viene visualizzato con la funzione `DisplayImagesFromContainer`.

:::code language="typescript" source="~/../js-e2e-browser-file-upload-storage-blob/src/App.tsx" highlight="3,28":::

## <a name="upload-file-to-azure-storage-blob-with-azure-sdk-client-library"></a>Caricare il file nel BLOB di Archiviazione di Azure con la libreria client di Azure SDK

Il codice per caricare il file in Archiviazione di Azure è indipendente dal framework. Poiché il codice è stato compilato per un'esercitazione, sono state fatte alcune scelte all'insegna della semplicità e della comprensione. Queste scelte sono spiegate. Rivedere il progetto per l'uso intenzionale, la sicurezza e l'efficienza. 

L'esempio crea e usa un contenitore accessibile pubblicamente e i relativi file. Se si vogliono proteggere i file del proprio progetto, è possibile farlo a diversi livelli, dall'impostazione dell'autenticazione generale per le risorse alla configurazione di autorizzazioni molto specifiche per ogni oggetto BLOB. 

### <a name="dependencies-and-variables"></a>Dipendenze e variabili

Il file TypeScript `azure-storage-blob.ts` carica le dipendenze ed esegue il pull delle variabili richieste dalle variabili di ambiente o dalle stringhe hardcoded.

| Variabile | Descrizione |
|--|--|
|`sasToken`|Al token di firma di accesso condiviso creato con il portale di Azure è anteposto un `?`. Rimuoverlo prima di impostarlo nella variabile `sasToken`.| 
|`container`|Nome del contenitore nell'archiviazione BLOB. Può essere considerato equivalente a una cartella o una directory per un file system.|
|`storageAccountName`|Nome della risorsa.|

:::code language="typescript" source="~/../js-e2e-browser-file-upload-storage-blob/src/azure-storage-blob.ts" highlight="2,5,16" id="snippet_package":::

### <a name="security-for-azure-credentials"></a>Sicurezza per le credenziali di Azure

Nel progetto considerare la posizione in cui archiviare i segreti, ad esempio un token di firma di accesso condiviso. Se un requisito dell'applicazione è la protezione delle informazioni di Azure, è consigliabile ospitare questo codice di archiviazione in una [funzione di Azure](/azure/azure-functions/) invece che nel client, quindi chiamare la funzione di Azure dall'app React.  

### <a name="create-storage-client-and-manage-steps"></a>Creare un client di archiviazione e gestire i passaggi

La funzione `uploadFileToBlob` è la funzione principale del file. Crea l'oggetto client per il servizio di archiviazione, quindi crea il client nell'oggetto contenitore, carica il file e infine ottiene un elenco di tutti i BLOB nel contenitore. 

:::code language="typescript" source="~/../js-e2e-browser-file-upload-storage-blob/src/azure-storage-blob.ts" highlight="5,6,7" id="snippet_uploadFileToBlob":::

### <a name="upload-file-to-blob"></a>Caricare il file in un BLOB

La funzione `createBlobInContainer` carica il file nel contenitore con il metodo della libreria client `uploadBrowserData`. Il tipo di contenuto deve essere inviato con la richiesta se si intende usare la funzionalità del browser, che dipende dal tipo di file, ad esempio la visualizzazione di un'immagine. 

:::code language="typescript" source="~/../js-e2e-browser-file-upload-storage-blob/src/azure-storage-blob.ts" highlight="10" id="snippet_createBlobInContainer":::

### <a name="get-list-of-blobs"></a>Ottenere l'elenco dei BLOB

La funzione `getBlobsInContainer` ottiene un elenco degli URL dei BLOB nel contenitore. Gli URL sono costruiti in modo da essere usati come `src` della visualizzazione di un'immagine in HTML: `<img src={item} alt={item} height="200" />`. 

:::code language="typescript" source="~/../js-e2e-browser-file-upload-storage-blob/src/azure-storage-blob.ts" highlight="10" id="snippet_getBlobsInContainer":::

## <a name="clean-up-resources"></a>Pulire le risorse

In Visual Studio Code usare l'area Azure per Archiviazione, fare clic on il pulsante destro del mouse sulla risorsa, quindi scegliere **Elimina account di archiviazione**.

## <a name="next-steps"></a>Passaggi successivi

Per continuare con questa app, si apprenderà come distribuire l'app in Azure per l'hosting con una delle opzioni seguenti:

* [Caricare come app Web statica](/azure/static-web-apps/getting-started?tabs=vanilla-javascript)
* Caricare in una risorsa dell'app Web usando l'[estensione di codice di Visual Studio per il servizio app](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice)
* [Caricare un'app in una macchina virtuale di Azure](nodejs-virtual-machine-vm/introduction.md)
