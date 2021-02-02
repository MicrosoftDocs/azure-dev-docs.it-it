---
title: Configurare la registrazione nelle librerie di Azure per Python
description: Le librerie di Azure usano il modulo di registrazione Python standard, che è configurato per singola libreria o per singola operazione.
ms.date: 02/01/2021
ms.topic: conceptual
ms.custom: devx-track-python
ms.openlocfilehash: f42941ec54876fec5854a0a82cee1cbcf30506ce
ms.sourcegitcommit: b09d3aa79113af04a245b05cec2f810e43062152
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/02/2021
ms.locfileid: "99476447"
---
# <a name="configure-logging-in-the-azure-libraries-for-python"></a>Configurare la registrazione nelle librerie di Azure per Python

Le librerie di Azure per Python [basate su azure.core](azure-sdk-library-package-index.md#libraries-using-azurecore) forniscono l'output di registrazione usando la libreria [logging](https://docs.python.org/3/library/logging.html) di Python standard.

Il processo generale per lavorare con la registrazione è il seguente:

1. Acquisire l'oggetto di registrazione per la libreria desiderata e impostare il livello di registrazione.
1. Registrare un gestore per il flusso di registrazione.
1. Per includere informazioni HTTP, passare un `logging_enable=True` parametro a un costruttore di oggetto client, un costruttore di oggetto credenziale o a un metodo specifico.

I dettagli sono disponibili nelle sezioni rimanenti di questo articolo.

Come regola generale, la risorsa migliore per comprendere l'utilizzo della registrazione all'interno delle librerie è il codice sorgente dell'SDK disponibile all'indirizzo [github.com/Azure/azure-sdk-for-python](https://github.com/Azure/azure-sdk-for-python). È consigliabile clonare il repository in locale, in modo da poter cercare facilmente i dettagli quando necessario, come suggerito nelle sezioni seguenti.

## <a name="set-logging-levels"></a>Impostare i livelli di registrazione

```python
import logging

# ...

# Acquire the logger for a library (azure.storage.blob in this example)
logger = logging.getLogger('azure.storage.blob')

# Set the desired logging level
logger.setLevel(logging.DEBUG)
```

- Questo esempio acquisisce il logger per la libreria `azure.storage.blob`, quindi imposta il livello di registrazione su `logging.DEBUG`.

- È possibile chiamare `logger.setLevel` in qualsiasi momento per cambiare il livello di registrazione per i diversi segmenti di codice.

Per impostare un livello per una libreria diversa, usare il nome della libreria nella chiamata a `logging.getLogger`. Ad esempio, la libreria azure-eventhubs fornisce un logger denominato `azure.eventhubs`, la libreria azure-storage-queue fornisce un logger denominato `azure.storage.queue` e così via. Il codice sorgente dell'SDK usa spesso l'istruzione `logging.getLogger(__name__)`, che acquisisce un logger usando il nome del modulo che lo contiene.

Si possono anche usare spazi dei nomi più generali. Ad esempio:

```python
import logging

# Set the logging level for all azure-storage-* libraries
logger = logging.getLogger('azure.storage')
logger.setLevel(logging.INFO)

# Set the logging level for all azure-* libraries
logger = logging.getLogger('azure')
logger.setLevel(logging.ERROR)
```

È possibile usare il metodo `logger.isEnabledFor` per verificare se è abilitato un determinato livello di registrazione:

```python
print(f"Logger enabled for ERROR={logger.isEnabledFor(logging.ERROR)}, " \
    f"WARNING={logger.isEnabledFor(logging.WARNING)}, " \
    f"INFO={logger.isEnabledFor(logging.INFO)}, " \
    f"DEBUG={logger.isEnabledFor(logging.DEBUG)}")
```

I livelli di registrazione corrispondono ai [livelli della libreria logging standard](https://docs.python.org/3/library/logging.html#levels). La tabella seguente descrive l'uso generale di questi livelli di registrazione nelle librerie di Azure per Python:

| Livello di registrazione             | Uso tipico |
| ---                       | ---         |
| logging.ERROR             | Errori da cui è improbabile che venga ripristinato lo stato normale dell'applicazione (ad esempio memoria insufficiente). |
| logging.WARNING (predefinito) | Una funzione non riesce a eseguire l'attività prevista (ma non quando la funzione può essere ripristinata, come nel caso di un nuovo tentativo di chiamata all'API REST). In genere le funzioni registrano un avviso in caso di generazione di eccezioni. Il livello di avviso abilita automaticamente il livello di errore. |
| logging.INFO              | La funzione funziona normalmente o una chiamata al servizio viene annullata. Gli eventi informativi includono in genere richieste, risposte e intestazioni. Il livello di informazioni abilita automaticamente i livelli di errore e avviso. |
| logging.DEBUG             | Informazioni dettagliate comunemente utilizzate per la risoluzione dei problemi e che includono un'analisi dello stack per le eccezioni. Il livello di debug abilita automaticamente i livelli di informazioni, avviso ed errore. Attenzione: se si imposta anche `logging_enable=True` , il livello di debug include informazioni riservate, ad esempio le chiavi dell'account nelle intestazioni e altre credenziali. Assicurarsi di proteggere questi log per evitare di compromettere la sicurezza. |
| logging.NOTSET            | Disabilita tutte le registrazioni. |

### <a name="library-specific-logging-level-behavior"></a>Comportamento del livello di registrazione specifico della libreria

L'esatto comportamento di registrazione per ogni livello dipende dalla libreria in questione. Alcune librerie, come azure.eventhub, eseguono attività di registrazione su vasta scala, mentre altre sono molto limitate in questo ambito.

Il modo migliore per esaminare la portata esatta della registrazione per una libreria consiste nel cercare i livelli di registrazione nel codice sorgente di Azure SDK per Python:

1. Nella cartella repository passare alla cartella *sdk*, quindi passare alla cartella del servizio specifico di interesse.

1. In questa cartella cercare una delle stringhe seguenti:

    - `_LOGGER.error`
    - `_LOGGER.warning`
    - `_LOGGER.info`
    - `_LOGGER.debug`

## <a name="register-a-log-stream-handler"></a>Registrare un gestore del flusso di registrazione

Per acquisire l'output di registrazione, è necessario registrare almeno un gestore del flusso di registrazione nel codice:

```python
import logging

# Direct logging output to stdout. Without adding a handler,
# no logging output is captured.
handler = logging.StreamHandler(stream=sys.stdout)
logger.addHandler(handler)
```

Questo esempio registra un gestore che indirizza l'output di registrazione a stdout. È possibile usare altri tipi di gestori come descritto in [Logging. Handlers](https://docs.python.org/3/library/logging.handlers.html) nella documentazione di Python o usare il metodo standard [Logging. basicConfig](https://docs.python.org/3/library/logging.html#logging.basicConfig) .

## <a name="enable-http-logging-for-a-client-object-or-operation"></a>Abilitare la registrazione HTTP per un oggetto o un'operazione client

Per impostazione predefinita, la registrazione nelle librerie di Azure non include informazioni HTTP. Per includere informazioni HTTP nell'output del log, ad esempio il livello di DEBUG, è necessario passare in modo specifico `logging_enable=True` a un costruttore di oggetti client o credenziali o a un metodo specifico.

**Attenzione**: la registrazione http può rivelare le informazioni riservate, ad esempio le chiavi dell'account nelle intestazioni e altre credenziali. Assicurarsi di proteggere questi log per evitare di compromettere la sicurezza.

### <a name="enable-http-logging-for-a-client-object-debug-level"></a>Abilitare la registrazione HTTP per un oggetto client (livello di DEBUG)

```python
from azure.storage.blob import BlobClient
from azure.identity import DefaultAzureCredential

# Enable HTTP logging on the client object when using DEBUG level
# endpoint is the Blob storage URL.
client = BlobClient(endpoint, DefaultAzureCredential(), logging_enable=True)
```

L'abilitazione della registrazione HTTP per un oggetto client consente la registrazione per tutte le operazioni richiamate tramite tale oggetto.

### <a name="enable-http-logging-for-a-credential-object-debug-level"></a>Abilitare la registrazione HTTP per un oggetto credenziale (livello di DEBUG)

```python
from azure.storage.blob import BlobClient
from azure.identity import DefaultAzureCredential

# Enable HTTP logging on the credential object when using DEBUG level
credential = DefaultAzureCredential(logging_enable=True)

# endpoint is the Blob storage URL.
client = BlobClient(endpoint, credential)
```

L'abilitazione della registrazione HTTP per un oggetto credenziale consente la registrazione per tutte le operazioni richiamate tramite tale oggetto, in particolare, ma non per le operazioni in un oggetto client che non implicano l'autenticazione.

### <a name="enable-logging-for-an-individual-method-debug-level"></a>Abilitare la registrazione per un singolo metodo (livello di DEBUG)

```python
from azure.storage.blob import BlobClient
from azure.identity import DefaultAzureCredential

# endpoint is the Blob storage URL.
client = BlobClient(endpoint, DefaultAzureCredential())

# Enable HTTP logging for only this operation when using DEBUG level
client.create_container("container01", logging_enable=True)
```

## <a name="example-logging-output"></a>Esempio di output di registrazione

Il codice seguente è illustrato nell' [esempio: usare un account di archiviazione](azure-sdk-example-storage-use.md) con l'aggiunta di abilitazione del debug e della registrazione http (Commenti omessi per brevità):

```python
import os, sys, logging
from azure.identity import DefaultAzureCredential
from azure.storage.blob import BlobClient

logger = logging.getLogger('azure.storage.blob')
logger.setLevel(logging.DEBUG)

handler = logging.StreamHandler(stream=sys.stdout)
logger.addHandler(handler)

credential = DefaultAzureCredential()
storage_url = os.environ["AZURE_STORAGE_BLOB_URL"]

blob_client = BlobClient(storage_url, container_name="blob-container-01",
    blob_name="sample-blob.txt", credential=credential)

with open("./sample-source.txt", "rb") as data:
    blob_client.upload_blob(data, logging_enable=True)
```

L'output di registrazione è simile al seguente:

<pre>
Request URL: 'https://pythonsdkstorage12345.blob.core.windows.net/blob-container-01/sample-blob.txt'
Request method: 'PUT'
Request headers:
    'Content-Type': 'application/octet-stream'
    'Content-Length': '79'
    'x-ms-version': '2019-07-07'
    'x-ms-blob-type': 'BlockBlob'
    'If-None-Match': '*'
    'x-ms-date': 'Mon, 01 Jun 2020 22:54:14 GMT'
    'x-ms-client-request-id': 'd081f88e-a45a-11ea-b9eb-0c5415dfd03a'
    'User-Agent': 'azsdk-python-storage-blob/12.3.1 Python/3.8.3 (Windows-10-10.0.18362-SP0)'
    'Authorization': '*****'
Request body:
b"Hello there, Azure Storage. I'm a friendly file ready to be stored in a blob.\r\n"
Response status: 201
Response headers:
    'Content-Length': '0'
    'Content-MD5': 'kvMIzjEi6O8EqTVnZJNakQ=='
    'Last-Modified': 'Mon, 01 Jun 2020 22:54:14 GMT'
    'ETag': '"0x8D8067EB52FF7BC"'
    'Server': 'Windows-Azure-Blob/1.0 Microsoft-HTTPAPI/2.0'
    'x-ms-request-id': '5df479b1-f01e-00d0-5b67-382916000000'
    'x-ms-client-request-id': 'd081f88e-a45a-11ea-b9eb-0c5415dfd03a'
    'x-ms-version': '2019-07-07'
    'x-ms-content-crc64': 'QmecNePSHnY='
    'x-ms-request-server-encrypted': 'true'
    'Date': 'Mon, 01 Jun 2020 22:54:14 GMT'
Response content:
</pre>
