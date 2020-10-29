---
title: Usare Archiviazione di Azure con Azure SDK per Python
description: Usare le librerie di Azure SDK per Python per accedere a un contenitore BLOB di cui è già stato effettuato il provisioning in un account di archiviazione di Azure e quindi caricarvi un file.
ms.date: 08/05/2020
ms.topic: conceptual
ms.custom: devx-track-python, devx-track-azurecli
ms.openlocfilehash: f1ada9de2cdf52fac1b4219f1f9b8253d58ca881
ms.sourcegitcommit: 1ddcb0f24d2ae3d1f813ec0f4369865a1c6ef322
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/27/2020
ms.locfileid: "92689234"
---
# <a name="example-access-azure-storage-using-the-azure-libraries-for-python"></a>Esempio: Accedere ad Archiviazione di Azure con le librerie di Azure per Python

Questo esempio illustra come usare le librerie client di Azure nel codice applicativo di Python per caricare un file nel contenitore di archiviazione BLOB. L'esempio presuppone che sia stato effettuato il provisioning delle risorse, come illustrato in [Esempio: Effettuare il provisioning di Archiviazione di Azure](azure-sdk-example-storage.md).

Se non diversamente specificato, tutti i comandi di questo articolo funzionano allo stesso modo nella shell Bash Linux/macOS e nella shell dei comandi di Windows.

## <a name="1-set-up-your-local-development-environment"></a>1: Configurare un ambiente di sviluppo locale

Se non è già stato fatto, seguire tutte le istruzioni riportate in [Configurare l'ambiente di sviluppo Python locale per Azure](configure-local-development-environment.md).

Assicurarsi di creare un'entità servizio per lo sviluppo locale, di impostare le variabili di ambiente per l'entità servizio (vedere di seguito) e di creare e attivare un ambiente virtuale per questo progetto.

Questo esempio presuppone che siano già state impostate le variabili di ambiente seguenti:

| Nome variabile | Valore previsto |
| --- | --- |
| AZURE_SUBSCRIPTION_ID | Il GUID della sottoscrizione di Azure. |
| AZURE_CLIENT_ID | L'ID client dell'entità servizio locale. |
| AZURE_CLIENT_SECRET | Il segreto client dell'entità servizio. |
| AZURE_TENANT_ID | L'ID tenant dell'entità servizio. |

## <a name="2-install-library-packages"></a>2: Installare i pacchetti di librerie

1. Nel file *requirements.txt* aggiungere una riga per il pacchetto di librerie client necessario e salvare il file:

    ```text
    azure-storage-blob
    azure-identity
    ```

1. Nel terminale o dal prompt dei comandi reinstallare i requisiti:

    ```cmd
    pip install -r requirements.txt
    ```

## <a name="3-create-a-file-to-upload"></a>3: Creare un file da caricare

Creare un file di origine denominato *sample-source.txt* , come previsto dal codice, con un contenuto simile al seguente:

```text
Hello there, Azure Storage. I'm a friendly file ready to be stored in a blob.
```

## <a name="4-use-blob-storage-from-app-code"></a>4: Usare l'archiviazione BLOB dal codice dell'app

Le sezioni seguenti (4a e 4b) illustrano due metodi per accedere al contenitore BLOB di cui è stato effettuato il provisioning in [Esempio: Effettuare il provisioning di Archiviazione di Azure](azure-sdk-example-storage.md).

Il [primo metodo (sezione 4a di seguito)](#4a-use-blob-storage-with-authentication) autentica l'app con `DefaultAzureCredential`, come descritto in [Come autenticare le app Python](azure-sdk-authenticate.md#authenticate-with-defaultazurecredential). Con questo metodo è necessario prima di tutto assegnare le autorizzazioni appropriate all'identità dell'app, che è la procedura consigliata.

Il [secondo metodo (sezione 4b di seguito)](#4b-use-blob-storage-with-a-connection-string) usa una stringa di connessione per accedere direttamente all'account di archiviazione. Anche se questo metodo sembra più semplice, presenta due svantaggi significativi:

- Una stringa di connessione autentica intrinsecamente l'agente di connessione con l' *account* di archiviazione invece che con le singole risorse al suo interno. Di conseguenza, una stringa di connessione fornisce autorizzazioni più ampie di quelle che potrebbero essere necessarie.

- Una stringa di connessione contiene una chiave di accesso in testo normale e pertanto presenta potenziali vulnerabilità se è costruita in modo errato o non è adeguatamente protetta. Se tale stringa di connessione viene esposta, può essere usata per accedere a un'ampia gamma di risorse all'interno dell'account di archiviazione.

Per questi motivi, per il codice di produzione è consigliabile usare il metodo di autenticazione.

### <a name="4a-use-blob-storage-with-authentication"></a>4a: Usare l'archiviazione BLOB con l'autenticazione

1. Creare una variabile di ambiente denominata `STORAGE_BLOB_URL`:

    # <a name="cmd"></a>[cmd](#tab/cmd)

    ```cmd
    set STORAGE_BLOB_URL=https://pythonazurestorage12345.blob.core.windows.net
    ```

    # <a name="bash"></a>[Bash](#tab/bash)

    ```bash
    STORAGE_BLOB_URL=https://pythonazurestorage12345.blob.core.windows.net
    ```

    ---

    Sostituire "pythonazurestorage12345" con il nome dell'account di archiviazione specifico.

    Questa variabile di ambiente viene usata solo in questo esempio e non nelle librerie di Azure.

1. Creare un file denominato *use_blob_auth.py* con il codice seguente. I commenti spiegano i passaggi.

    ```python
    import os
    from azure.identity import DefaultAzureCredential

    # Import the client object from the Azure library
    from azure.storage.blob import BlobClient

    credential = DefaultAzureCredential()

    # Retrieve the storage blob service URL, which is of the form
    # https://pythonazurestorage12345.blob.core.windows.net/
    storage_url = os.environ["STORAGE_BLOB_URL"]

    # Create the client object using the storage URL and the credential
    blob_client = BlobClient(storage_url, container_name="blob-container-01", blob_name="sample-blob.txt", credential=credential)

    # Open a local file and upload its contents to Blob Storage
    with open("./sample-source.txt", "rb") as data:
        blob_client.upload_blob(data)
    ```

    Collegamenti di riferimento:
      - [DefaultAzureCredential (azure.identity)](/python/api/azure-identity/azure.identity.defaultazurecredential)
      - [BlobClient (azure.storage.blob)](/python/api/azure-storage-blob/azure.storage.blob.blobclient)

1. Tentativo di eseguire il codice (che non riesce intenzionalmente):

    ```cmd
    python use_blob_auth.py
    ```

    Poiché l'entità servizio locale in uso non ha l'autorizzazione per accedere al contenitore BLOB, viene visualizzato un messaggio di errore analogo al seguente: "L'operazione richiesta non è consentita con questa autorizzazione".

1. Concedere le autorizzazioni per il contenitore all'entità servizio usando il comando [az role assignment create](/cli/azure/role/assignment#az-role-assignment-create) dell'interfaccia della riga di comando di Azure (è molto lungo):

    # <a name="cmd"></a>[cmd](#tab/cmd)

    ```azurecli
    az role assignment create --assignee %AZURE_CLIENT_ID% ^
        --role "Storage Blob Data Contributor" ^
        --scope "/subscriptions/%AZURE_SUBSCRIPTION_ID%/resourceGroups/PythonAzureExample-Storage-rg/providers/Microsoft.Storage/storageAccounts/pythonazurestorage12345/blobServices/default/containers/blob-container-01"
    ```

    # <a name="bash"></a>[Bash](#tab/bash)

    ```azurecli
    az role assignment create --assignee $AZURE_CLIENT_ID \
        --role "Storage Blob Data Contributor" \
        --scope "/subscriptions/$AZURE_SUBSCRIPTION_ID/resourceGroups/PythonAzureExample-Storage-rg/providers/Microsoft.Storage/storageAccounts/pythonazurestorage12345/blobServices/default/containers/blob-container-01"
    ```

    ---

    L'argomento `--scope` identifica dove viene applicata questa assegnazione di ruolo. In questo esempio il ruolo "Collaboratore ai dati BLOB di archiviazione" viene concesso al contenitore *specifico* denominato "blob-container-01".

    Sostituire `pythonazurestorage12345` con il nome esatto del proprio account di archiviazione. Se necessario, è anche possibile modificare il nome del gruppo di risorse e del contenitore BLOB. Se si usa un nome errato, viene visualizzato il messaggio di errore "Non è possibile eseguire l'operazione richiesta su una risorsa annidata. La risorsa padre 'pythonazurestorage12345' non è stata trovata".

    L'argomento `--scope` di questo comando usa anche le variabili di ambiente AZURE_CLIENT_ID e AZURE_SUBSCRIPTION_ID, che devono essere già state impostate nell'ambiente locale per l'entità servizio come descritto in [Configurare l'ambiente di sviluppo Python locale per Azure](configure-local-development-environment.md).

1. Dopo aver atteso uno o due minuti la propagazione delle autorizzazioni, eseguire di nuovo il codice per verificarne il funzionamento. Se l'errore delle autorizzazioni persiste, attendere un po' più a lungo, quindi riprovare a eseguire il codice.

Per altre informazioni sulle assegnazioni di ruolo, vedere [Come assegnare le autorizzazioni per i ruoli con l'interfaccia della riga di comando di Azure](/azure/role-based-access-control/role-assignments-cli).

### <a name="4b-use-blob-storage-with-a-connection-string"></a>4b: Usare archiviazione BLOB con una stringa di connessione

1. Creare una variabile di ambiente denominata `AZURE_STORAGE_CONNECTION_STRING`, il cui valore è la stringa di connessione completa per l'account di archiviazione. Questa variabile di ambiente viene usata anche in vari commenti dell'interfaccia della riga di comando di Azure.

1. Creare un file Python denominato *use_blob_conn_string.py* con il codice seguente. I commenti spiegano i passaggi.

    ```python
    import os

    # Import the client object from the Azure library
    from azure.storage.blob import BlobClient

    # Retrieve the connection string from an environment variable.
    conn_string = os.environ["AZURE_STORAGE_CONNECTION_STRING"]

    # Create the client object for the resource identified by the connection string,
    # indicating also the blob container and the name of the specific blob we want.
    blob_client = BlobClient.from_connection_string(conn_string, container_name="blob-container-01", blob_name="sample-blob.txt")

    # Open a local file and upload its contents to Blob Storage
    with open("./sample-source.txt", "rb") as data:
        blob_client.upload_blob(data)
    ```

1. Eseguire il codice:

    ```bash
    python use_blob_conn_string.py
    ```

Anche in questo caso, sebbene il metodo sia semplice, una stringa di connessione autorizza tutte le operazioni in un account di archiviazione. Con il codice di produzione è preferibile usare autorizzazioni specifiche, come descritto nella sezione precedente.

## <a name="5-verify-blob-creation"></a>5. Verificare la creazione di BLOB

Dopo aver eseguito il codice di uno dei due metodi, nel [portale di Azure](https://portal.azure.com) passare al contenitore BLOB e verificare che esista un nuovo BLOB denominato *sample-blob.txt* con lo stesso contenuto del file *sample-source.txt* :

![Pagina del portale di Azure relativa al contenitore BLOB che mostra il file caricato](media/azure-sdk-example-storage/portal-blob-container-file.png)

## <a name="6-clean-up-resources"></a>6: Pulire le risorse

```azurecli
az group delete -n PythonAzureExample-Storage-rg  --no-wait
```

Se non è necessario mantenere le risorse di cui è stato effettuato il provisioning in questo esempio, eseguire questo comando per evitare addebiti ricorrenti nella sottoscrizione.

[!INCLUDE [resource_group_begin_delete](includes/resource-group-begin-delete.md)]

## <a name="see-also"></a>Vedere anche

- [Esempio: Effettuare il provisioning di un gruppo di risorse](azure-sdk-example-resource-group.md)
- [Esempio: Elencare i gruppi di risorse in una sottoscrizione](azure-sdk-example-list-resource-groups.md)
- [Esempio: Effettuare il provisioning di un'app Web e distribuire il codice](azure-sdk-example-web-app.md)
- [Esempio: Effettuare il provisioning di Archiviazione di Azure](azure-sdk-example-storage.md)
- [Esempio: Effettuare il provisioning ed eseguire query su un database](azure-sdk-example-database.md)
- [Esempio: Effettuare il provisioning di una macchina virtuale](azure-sdk-example-virtual-machines.md)
