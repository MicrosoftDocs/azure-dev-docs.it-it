---
title: Azure SDK per Python
description: Panoramica delle funzionalità e delle funzionalità di Azure SDK per Python che consentono agli sviluppatori di aumentare la produttività quando usano i servizi di Azure.
ms.date: 10/30/2019
ms.topic: conceptual
ms.openlocfilehash: fb81b743de8332d18aeb815d1ed1efa09e6e3305
ms.sourcegitcommit: e77f8f652128b798dbf972078a7b460ed21fb5f8
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/25/2019
ms.locfileid: "74466335"
---
# <a name="azure-sdk-for-python"></a>Azure SDK per Python

Azure SDK per Python semplifica l'uso e la gestione delle risorse di Azure dal codice di applicazione Python. L'SDK supporta Python 2.7 e Python 3.5.3 o versione successiva.

Per installare l'SDK, installare le librerie dei singoli componenti tramite `pip install <library>`. È possibile visualizzare l'elenco delle librerie nell'[indice del pacchetto Azure SDK per Python](https://github.com/Azure/azure-sdk-for-python/blob/master/packages.md).

Per altre istruzioni dettagliate sull'installazione delle librerie e sulla relativa importazione nei progetti, vedere [Installare l'SDK](python-sdk-azure-install.md). Vedere quindi [Introduzione all'SDK](python-sdk-azure-get-started.yml) per configurare l'autenticazione ed eseguire il codice di esempio nella propria sottoscrizione di Azure.

> [!TIP]
> Per informazioni sulle modifiche apportate all'SDK, vedere [Note sulla versione dell'SDK](https://azure.github.io/azure-sdk/).

## <a name="connect-and-use-azure-services"></a>Connettersi ai servizi di Azure e utilizzarli

Una serie di *librerie client* nell'SDK consentono di connettersi alle risorse di Azure esistenti e di usarle nelle app, ad esempio il caricamento di file, l'accesso ai dati di tabelle o l'uso dei vari Servizi cognitivi di Azure. Con l'SDK è possibile usare queste risorse tramite paradigmi di programmazione Python noti anziché usare l'API REST generica del servizio.

Si supponga, ad esempio, di voler caricare un BLOB in un account di Archiviazione di Azure di cui è stato effettuato il provisioning in precedenza. Il primo passaggio consiste nell'installare la libreria appropriata:

```bash
pip install azure-storage-blob
```

Importare quindi la libreria nel codice:

```python
from azure.storage.blob import BlobClient
```

Usare infine l'API della libreria per connettersi ai dati e caricarli. In questo esempio la stringa di connessione e il nome del contenitore sono già stati sottoposti a provisioning nell'account di archiviazione. Il nome del BLOB è il nome assegnato ai dati caricati:

```python
blob = BlobClient.from_connection_string("my_connection_string", container="mycontainer", blob="my_blob")

with open("./SampleSource.txt", "rb") as data:
    blob.upload_blob(data)
```

Per informazioni dettagliate sull'uso di ogni libreria specifica, vedere il file *README.md* o *README.rst* presente nella cartella di progetto della libreria nel nostro [repository GitHub](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk). Vedere anche gli [esempi di Azure](https://docs.microsoft.com/samples/browse/?languages=python) disponibili.

È anche possibile trovare frammenti di codice aggiuntivi nella [documentazione di riferimento](/python/api?view=azure-python).

### <a name="the-azure-core-library"></a>Libreria principale di Azure

Microsoft sta attualmente aggiornando le librerie client Python per condividere le funzionalità principali, ad esempio tentativi, registrazione, protocolli di trasporto e di autenticazione e così via. Questa funzionalità condivida è contenuta nella libreria [azure-core](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/core/azure-core). Per informazioni dettagliate sulla libreria e sulle relative linee guida, vedere [Linee guida di Python: introduzione](https://azure.github.io/azure-sdk/python_introduction.html).

Le librerie che funzionano attualmente con la libreria principale sono le seguenti:

- `azure-storage-blob`
- `azure-storage-queue`
- `azure-keyvault-keys`
- `azure-keyvault-secrets`

## <a name="manage-azure-resources"></a>Gestire le risorse di Azure

In Azure SDK per Python sono incluse anche molte librerie che consentono di creare, effettuare il provisioning e gestire in modo diverso le risorse di Azure. A tali librerie si fa riferimento con il termine *librerie di gestione*. Ogni libreria di gestione è denominata `azure-mgmt-<service name>`. Con le librerie di gestione è possibile scrivere codice Python per eseguire le stesse attività che è possibile eseguire nel [portale di Azure](https://portal.azure.com) o nell'[interfaccia della riga di comando di Azure](https://docs.microsoft.com/cli/azure/install-azure-cli).

Si supponga ad esempio di voler creare un'istanza di SQL Server. Installare prima di tutto la libreria di gestione appropriata:

```bash
pip install azure-mgmt-sql
```

Nel codice Python importare la libreria:

```python
from azure.mgmt.sql import SqlManagementClient

```

Successivamente, creare l'oggetto client di gestione usando le proprie credenziali e l'ID di sottoscrizione di Azure:

```python
sql_client = SqlManagementClient(credentials, subscription_id)
```

Usare infine tale oggetto client per creare la risorsa, usando un nome del gruppo di risorse, il nome del server, la posizione e le credenziali di amministratore appropriati:

```python
server = sql_client.servers.create_or_update(
    'myresourcegroup',
    'myservername',
    {
        'location': 'eastus',
        'version': '12.0', # Required for create
        'administrator_login': 'mysecretname', # Required for create
        'administrator_login_password': 'HusH_Sec4et' # Required for create
    }
)
```

Come per le librerie client, è possibile trovare informazioni dettagliate sull'uso di ogni libreria di gestione nel file *README.md* o *README.rst* presente nella cartella di progetto della libreria nel nostro [repository GitHub](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk).

È anche possibile trovare frammenti di codice aggiuntivi nella [documentazione di riferimento](/python/api?view=azure-python). 

## <a name="get-help-and-give-feedback"></a>Ottenere supporto e inviare commenti

- Visitare la documentazione di [Azure SDK per Python](https://aka.ms/python-docs)
- Pubblicare le domande per la community in [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-sdk-python).
- Aprire i problemi relativi all'SDK in [GitHub](https://github.com/Azure/azure-sdk-for-python/issues)
- Inviare tweet a [@AzureSDK](https://twitter.com/AzureSdk/)
