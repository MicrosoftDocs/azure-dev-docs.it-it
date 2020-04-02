---
title: Azure SDK per Python
description: Panoramica delle caratteristiche e delle funzionalità di Azure SDK per Python che consentono agli sviluppatori di aumentare la produttività per il provisioning, l'uso e la gestione di risorse di Azure.
ms.date: 03/17/2020
ms.topic: conceptual
ms.openlocfilehash: a7fa744884972c6e25ac85b69c6b8317d7cb5190
ms.sourcegitcommit: 1bd9ec6a4115e9162e33b76a933869788e6ab702
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/31/2020
ms.locfileid: "80441676"
---
# <a name="azure-sdk-for-python"></a>Azure SDK per Python

Azure SDK per Python open source semplifica il provisioning, l'uso e la gestione di risorse di Azure dal codice di applicazione Python. L'SDK supporta Python 2.7 e Python 3.5.3 o versione successiva.

L'SDK è costituito da singole librerie di componenti, ciascuna delle quali viene installata usando `pip install <library>`. Per istruzioni più dettagliate, vedere [Installare l'SDK](azure-sdk-install.md). La [documentazione di Azure SDK per Python](https://azure.github.io/azure-sdk-for-python/) fornisce i nomi delle librerie.

È anche possibile seguire la procedura dettagliata [Introduzione ad Azure SDK per Python](azure-sdk-get-started.yml) per sperimentare autonomamente con le librerie.

> [!TIP]
> Per informazioni sulle modifiche apportate all'SDK, vedere [Note sulla versione dell'SDK](https://azure.github.io/azure-sdk/).

[!INCLUDE [chrome-note](includes/chrome-note.md)]

## <a name="management-and-client-libraries"></a>Librerie client e di gestione

Azure SDK per Python contiene due tipi di librerie:

- Le librerie di *gestione* consentono di effettuare il provisioning e gestire le risorse di Azure come si farebbe tramite il [portale di Azure](https://portal.azure.com) o con strumenti da riga di comando come l'[interfaccia della riga di comando di Azure](https://docs.microsoft.com/cli/azure/install-azure-cli) e [Azure PowerShell](https://docs.microsoft.com/powershell/azure/). Le librerie di gestione vengono in genere usate negli script di configurazione e distribuzione. La stessa interfaccia della riga di comando di Azure, infatti, viene scritta con le librerie di gestione Python.

- Le librerie *client* consentono l'interazione tra il codice dell'applicazione e i servizi con provisioning usando idiomi naturali di Python. Dietro le quinte, le librerie client usano le API REST di Azure, ma con l'SDK non bisogna preoccuparsi dei dettagli di REST.

Le sezioni seguenti includono altre informazioni dettagliate su ogni categoria.

## <a name="manage-azure-resources-with-management-libraries"></a>Gestire le risorse di Azure con le librerie di gestione

Le librerie di gestione di Azure SDK per Python, ognuna denominata `azure-mgmt-<service name>`, consentono di creare, effettuare il provisioning e gestire in altro modo le risorse di Azure.

Si supponga ad esempio di voler creare un'istanza di SQL Server. Installare prima di tutto la libreria di gestione appropriata:

```bash
pip install azure-mgmt-sql
```

Nel codice Python importare la libreria:

```python
from azure.mgmt.sql import SqlManagementClient
```

Successivamente, creare l'oggetto client di gestione usando le proprie credenziali (vedere [Eseguire l'autenticazione con l'SDK](azure-sdk-authenticate.md)) e l'ID di sottoscrizione di Azure:

```python
sql_client = SqlManagementClient(credentials, subscription_id)
```

Usare infine l'oggetto client di gestione per creare la risorsa, usando i valori appropriati per nome del gruppo di gestione, nome del server, località e credenziali di amministratore:

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

Per informazioni dettagliate sull'uso di ogni libreria specifica, vedere il file *README.md* o *README.rst* disponibile nella cartella di progetto della libreria nel [repository GitHub](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk) dell'SDK. È anche possibile trovare frammenti di codice aggiuntivi nella [documentazione di riferimento](/python/api?view=azure-python) e negli [esempi di Azure](https://docs.microsoft.com/samples/browse/?languages=python&products=azure).

## <a name="connect-and-use-azure-services-with-client-libraries"></a>Connettersi e usare i servizi di Azure con le librerie client

Le *librerie client* di Azure SDK consentono di connettersi alle risorse di Azure esistenti e di usarle nelle app, ad esempio il caricamento di file, l'accesso ai dati di tabelle o l'uso dei vari Servizi cognitivi di Azure. Con l'SDK è possibile usare queste risorse tramite paradigmi di programmazione Python noti anziché usare l'API REST generica del servizio. I servizi che non hanno un'API REST non sono rappresentati da una libreria client.

Si supponga, ad esempio, di aver distribuito un'app Web nel servizio app di Azure e di voler aggiungere la possibilità di caricare un file in un account di archiviazione di Azure. Il primo passaggio consiste nell'installare la libreria appropriata:

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

Per informazioni dettagliate sull'uso di ogni libreria specifica, vedere il file *README.md* o *README.rst* disponibile nella cartella di progetto della libreria nel [repository GitHub](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk) dell'SDK. È anche possibile trovare frammenti di codice aggiuntivi nella [documentazione di riferimento](/python/api?view=azure-python) e negli [esempi di Azure](https://docs.microsoft.com/samples/browse/?languages=python&products=azure).

### <a name="the-azure-core-library"></a>Libreria principale di Azure

È in corso l'aggiornamento delle librerie client di Azure SDK per Python per condividere modelli cloud comuni come protocolli di autenticazione, registrazione, traccia, protocolli di trasporto, risposte memorizzate nel buffer e tentativi. Questa funzionalità condivida è contenuta nella libreria [azure-core](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/core/azure-core). Le librerie che attualmente funzionano con la libreria Core sono elencate nella pagina di [versioni più recenti di Azure SDK](https://azure.github.io/azure-sdk/releases/latest/#python-packages).

Per informazioni dettagliate sulla libreria Azure Core e sulle relative linee guida, vedere [Linee guida di Python: introduzione](https://azure.github.io/azure-sdk/python_introduction.html).

## <a name="get-help-and-give-feedback"></a>Ottenere supporto e inviare commenti

- Visitare la documentazione di [Azure SDK per Python](https://aka.ms/python-docs)
- Pubblicare le domande per la community in [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-sdk-python).
- Aprire i problemi relativi all'SDK in [GitHub](https://github.com/Azure/azure-sdk-for-python/issues)
- Menzionare [@AzureSDK](https://twitter.com/AzureSdk/) su Twitter
