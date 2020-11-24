---
title: file di inclusione create-storage-resource.md
description: file di inclusione create-storage-resource.md
ms.date: 11/13/2020
ms.topic: include
ms.custom: devx-track-javascript, devx-track-azurecli
ms.openlocfilehash: 19a21dbf557c31f7eeae6afdb4722bfed35c86fe
ms.sourcegitcommit: dc74b60217abce66fe6cc93923e869e63ac86a8f
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/18/2020
ms.locfileid: "94884714"
---
In questa sezione dell'esercitazione viene creata la risorsa di Archiviazione di Azure con un'estensione di Visual Studio, che quindi viene configurata nel portale di Azure. 

## <a name="sign-in-to-azure"></a>Accedere ad Azure

[!INCLUDE [azure-sign-in](azure-sign-in.md)]

## <a name="create-storage-resource"></a>Creare la risorsa di archiviazione 

Usare l'estensione di Visual Studio Code per creare una risorsa di archiviazione. 

1. Passare all'estensione Archiviazione di Azure. Fare clic con il pulsante destro del mouse sulla sottoscrizione e quindi scegliere `Create Storage Account...`.

    :::image type="content" source="../../media/tutorial-browser-file-upload/visualstudiocode-storage-extension-create-resource.png" alt-text="Passare all'estensione Archiviazione di Azure. Fare clic con il pulsante destro del mouse sulla sottoscrizione e quindi scegliere 'Crea account di archiviazione'.":::

1. Seguire le istruzioni riportate nella tabella seguente per informazioni su come vengono usati i valori.

    |Proprietà|Valore|
    |--|--|
    |Immettere un nome univoco globale per la nuova app Web.| Immettere un valore, ad esempio `fileuploadyourname`, per la risorsa di archiviazione. Sostituire `yourname` con il nome in minuscolo o l'ID univoco. Questo nome univoco viene usato anche come parte dell'URL per accedere alla risorsa in un browser. Usare solo un massimo di 24 caratteri e numeri. Questo **nome dell'account** sarà necessario più avanti.|

1. Al termine del processo di creazione dell'app, viene visualizzata una notifica con le informazioni sulla nuova risorsa. 

    :::image type="content" source="../../media/tutorial-browser-file-upload/visualstudiocode-storage-extension-create-resource-complete.png" alt-text="Al termine del processo di creazione dell'app, viene visualizzata una notifica con le informazioni sulla nuova risorsa.":::

## <a name="set-storage-account-name-in-code-file"></a>Impostare il nome dell'account di archiviazione nel file di codice

Impostare il nome della risorsa in `src/uploadToBlob.ts` per il valore `storageAccountName` aggiungendo il nome della chiave di archiviazione nella stringa vuota. Lasciare il resto del codice invariato. 

```typescript
const storageAccountName = process.env.storageresourcename || ""; 
```

## <a name="generate-your-shared-access-signature-sas-token"></a>Generare il token di firma di accesso condiviso 

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
    |Data/ora di inizio e di scadenza|Accettare la data e l'ora di inizio e impostare la data e l'ora di fine su 24 ore in futuro. Il token di firma di accesso condiviso è valido solo per 24 ore.|
    |Solo HTTPS|Selezionato|
    |Livello di routing preferito|Basic|
    |Chiave di firma|Key1 selezionata|

    :::image type="content" source="../../media/tutorial-browser-file-upload/azure-portal-storage-blob-generate-sas-token.png" alt-text="Configurare il token di firma di accesso condiviso come illustrato nell'immagine. Le impostazioni vengono spiegate sotto l'immagine.":::

1.  Selezionare **Genera firma di accesso condiviso e stringa di connessione**. Copiare immediatamente il token di firma di accesso condiviso. Non sarà possibile visualizzare questo token, quindi se non viene copiato sarà necessario crearne uno nuovo. 

## <a name="set-sas-token-in-code-file"></a>Impostare il token di firma di accesso condiviso nel file di codice

Il valore del token di firma di accesso condiviso è una stringa di query parziale e viene usato nell'URL quando vengono eseguite query sulla risorsa basata sul cloud.

Il formato del token dipende dallo strumento usato per crearlo: 
* **Portale di Azure**: se viene creato nel portale di Azure, il token di firma di accesso condiviso include `?` come primo carattere della stringa.
* **Interfaccia della riga di comando di Azure**: se viene creato nell'interfaccia della riga di comando di Azure, il token di firma di accesso condiviso include `?` come primo carattere della stringa. 

1. Rimuovere `?`, se è il primo carattere del token. Il file di codice fornisce il carattere `?`, quindi non è necessario nel token.

1. Impostare il token di firma di accesso condiviso in `src/uploadToBlob.ts` per il valore sasToken aggiungendolo nella stringa vuota. Lasciare il resto del codice invariato. 

```typescript
// remove `?` if it is first character of token
const sasToken = process.env.storagesastoken || "";
```

## <a name="configure-your-azure-storage-resource-for-cors-with-azure-cli"></a>Configurare la risorsa di Archiviazione Azure per CORS con l'interfaccia della riga di comando di Azure

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

    :::image type="content" source="../../media/tutorial-browser-file-upload/azure-portal-storage-blob-cors.png" alt-text="Configurare CORS come mostrato nell'immagine. Le impostazioni vengono spiegate sotto l'immagine.":::

1. Selezionare **Salva** sopra le impostazioni per salvarle nella risorsa. Il codice non richiede modifiche per l'uso di queste impostazioni CORS. 

## <a name="run-project-locally-to-verify-connection-to-storage-account"></a>Eseguire il progetto in locale per verificare la connessione all'account di archiviazione

Il token di firma di accesso condiviso e il nome dell'account di archiviazione vengono impostati nel file `src/uploadToBlob.ts`, quindi è ora possibile eseguire l'applicazione.

1. Nel terminale di Visual Studio Code immettere il comando seguente:

    ```javascript
    npm start
    ```

1. Quando il terminale visualizza l'URL, ad esempio `http://localhost:3000`, l'app è pronta. Aprire un browser e immettere l'URL. Verrà visualizzato il sito Web connesso ai BLOB di archiviazione di Azure con un pulsante di selezione file e un pulsante di caricamento file. 

    :::image type="content" source="../../media/tutorial-browser-file-upload/browser-react-app-azure-storage-resource-configured-upload-button-displayed.png" alt-text="Verrà visualizzato il sito Web React connesso ai BLOB di archiviazione di Azure con un pulsante di selezione file e un pulsante di caricamento file.":::

1. Selezionare un'immagine nella cartella `images` da caricare. `spring-flowers.jpg` è un oggetto visivo efficace per questo test. Quindi selezionare **Carica**. . 

    Il codice client front-end React chiama `src/uploadToBlob.ts` per l'autenticazione in Azure, quindi creare un contenitore di archiviazione (se non esiste già) e caricarvi il file. 

## <a name="troubleshooting"></a>Risoluzione dei problemi

Se è stato ricevuto un errore o il file non viene caricato nel contenitore, verificare quanto segue:

* Ricreare il token di firma di accesso condiviso, assicurandosi che venga creato a livello di Risorsa di archiviazione e non a livello di contenitore. Copiare il nuovo token nel codice nella posizione corretta.
* Verificare che la stringa di token copiata nel codice non contenga il `?` (punto interrogativo) all'inizio della stringa.
* Verificare l'impostazione di CORS per la Risorsa di archiviazione.

## <a name="want-to-know-more"></a>Per saperne di più 

Altri modi per configurare l'account di archiviazione includono:
* Token di firma di accesso condiviso con [PowerShell](/powershell/module/azure.storage/new-azurestorageblobsastoken)
* Token di firma di accesso condiviso con il portale
* CORS con [PowerShell](/powershell/module/azure.storage/set-azurestoragecorsrule)
* CORS con il portale

Altre informazioni sulle [firme di accesso condiviso](/azure/storage/common/storage-sas-overview).
