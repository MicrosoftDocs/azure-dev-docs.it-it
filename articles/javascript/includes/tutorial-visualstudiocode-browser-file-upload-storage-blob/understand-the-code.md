---
ms.topic: include
ms.date: 10/13/2020
ms.custom: devx-track-javascript
title: file di inclusione understand-the-code.md
description: file di inclusione understand-the-code.md
ms.openlocfilehash: fd9d2eb2c6c8dd3a39d737cdcdf609b9cf04cf7e
ms.sourcegitcommit: ced8331ba36b28e6e2eacd23a64b39ddc7ffe6ab
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/21/2020
ms.locfileid: "92344170"
---
In questa sezione dell'esercitazione viene spiegato il codice dell'esempio che indica all'app Web di caricare un file.

## <a name="sample-created-with-create-react-app"></a>Esempio creato con create-react-app

L'[esempio](https://github.com/Azure-Samples/js-e2e-browser-file-upload-storage-blob) è un'app React di base creata con [create-react-app](https://create-react-app.dev/docs/adding-typescript/) con il modello TypeScript.

```typescript
npx create-react-app my-app --template typescript
```

Il framework create-react-app è utile per:
* consentire la creazione automatica di bundle di file, che è un requisito per l'uso delle librerie client di Azure nel browser
* fornire script di transpilazione e compilazione 

## <a name="upload-button-functionality"></a>Funzionalità del pulsante di caricamento

Il file `src/app` viene fornito come parte della creazione dell'app con create-react-app. Il file è stato modificato per fornire il pulsante di selezione file e il pulsante di caricamento e il codice di supporto per fornire tale funzionalità. 

Il codice che si connette al codice di Archiviazione BLOB di Azure è evidenziato. La chiamata a `uploadFileToBlob` restituisce tutti i BLOB (file) nel contenitore in forma di elenco semplice. L'elenco viene visualizzato con la funzione `DisplayImagesFromContainer`.

:::code language="typescript" source="~/../js-e2e-browser-file-upload-storage-blob/src/App.tsx" highlight="3,28":::

## <a name="upload-file-to-azure-storage-blob-with-azure-sdk-client-library"></a>Caricare il file nel BLOB di Archiviazione di Azure con la libreria client di Azure SDK

Il codice per caricare il file in Archiviazione di Azure è indipendente dal framework. Poiché il codice è stato compilato per un'esercitazione, sono state fatte alcune scelte all'insegna della semplicità e della comprensione. Queste scelte sono spiegate. Rivedere il progetto per l'uso intenzionale, la sicurezza e l'efficienza. 

L'esempio crea e usa un contenitore accessibile pubblicamente e i relativi file. Se si vogliono proteggere i file del proprio progetto, è possibile farlo a diversi livelli, dall'impostazione dell'autenticazione generale per le risorse alla configurazione di autorizzazioni molto specifiche per ogni oggetto BLOB. 

### <a name="dependencies-and-variables"></a>Dipendenze e variabili

Il file `uploadToBlob.ts` carica le dipendenze ed esegue il pull delle variabili richieste dalle variabili di ambiente o dalle stringhe hardcoded.

| Variabile | Descrizione |
|--|--|
|`sasToken`|Al token di firma di accesso condiviso creato con il portale di Azure è anteposto un `?`. Rimuoverlo prima di impostarlo nella variabile `sasToken`.| 
|`container`|Nome del contenitore nell'archiviazione BLOB. Può essere considerato equivalente a una cartella o una directory per un file system.|
|`storageAccountName`|Nome della risorsa.|

:::code language="typescript" source="~/../js-e2e-browser-file-upload-storage-blob/src/uploadToBlob.ts" highlight="2,5,16" id="snippet_package":::

### <a name="security-for-azure-credentials"></a>Sicurezza per le credenziali di Azure

Nel progetto considerare la posizione in cui archiviare i segreti, ad esempio un token di firma di accesso condiviso. Se un requisito dell'applicazione è la protezione delle informazioni di Azure, è consigliabile ospitare questo codice di archiviazione in una [funzione di Azure](/azure/azure-functions/) invece che nel client, quindi chiamare la funzione di Azure dall'app React.  

### <a name="create-storage-client-and-manage-steps"></a>Creare un client di archiviazione e gestire i passaggi

La funzione `uploadFileToBlob` è la funzione principale del file. Crea l'oggetto client per il servizio di archiviazione, quindi crea il client nell'oggetto contenitore, carica il file e infine ottiene un elenco di tutti i BLOB nel contenitore. 

:::code language="typescript" source="~/../js-e2e-browser-file-upload-storage-blob/src/uploadToBlob.ts" highlight="5,6,7" id="snippet_uploadFileToBlob":::

### <a name="upload-file-to-blob"></a>Caricare il file in un BLOB

La funzione `createBlobInContainer` carica il file nel contenitore con il metodo della libreria client `uploadBrowserData`. Il tipo di contenuto deve essere inviato con la richiesta se si intende usare la funzionalità del browser, che dipende dal tipo di file, ad esempio la visualizzazione di un'immagine. 

:::code language="typescript" source="~/../js-e2e-browser-file-upload-storage-blob/src/uploadToBlob.ts" highlight="10" id="snippet_createBlobInContainer":::

### <a name="get-list-of-blobs"></a>Ottenere l'elenco dei BLOB

La funzione `getBlobsInContainer` ottiene un elenco degli URL dei BLOB nel contenitore. Gli URL sono costruiti in modo da essere usati come `src` della visualizzazione di un'immagine in HTML: `<img src={item} alt={item} height="200" />`. 

:::code language="typescript" source="~/../js-e2e-browser-file-upload-storage-blob/src/uploadToBlob.ts" highlight="10" id="snippet_getBlobsInContainer":::

