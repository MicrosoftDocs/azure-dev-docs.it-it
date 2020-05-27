---
title: Effettuare il provisioning e usare Archiviazione di Azure con Azure SDK per Python
description: Usare le librerie di Azure SDK per Python per effettuare il provisioning di un contenitore BLOB in un account di archiviazione di Azure e quindi caricarvi un file.
ms.date: 05/12/2020
ms.topic: conceptual
ms.openlocfilehash: 9b26dd4c5708231c807ea57979bed6bdcd9de25f
ms.sourcegitcommit: 2cdf597e5368a870b0c51b598add91c129f4e0e2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/14/2020
ms.locfileid: "83405104"
---
# <a name="example-use-the-azure-sdk-with-azure-storage"></a>Esempio: Usare Azure SDK con Archiviazione di Azure

Questo articolo illustra come usare le librerie di gestione di Azure SDK in uno script Python per effettuare il provisioning di un gruppo di risorse contenente l'account di archiviazione di Azure e un contenitore di archiviazione BLOB. Verrà quindi descritto come usare le librerie client di Azure SDK nel codice applicativo Python per caricare un file nel contenitore di archiviazione BLOB.

Tutti i comandi di questo articolo funzionano allo stesso modo nella shell Bash Linux/Mac OS e nella shell dei comandi di Windows, se non diversamente specificato.

## <a name="1-set-up-your-local-development-environment"></a>1: Configurare un ambiente di sviluppo locale

Se non è già stato fatto, seguire tutte le istruzioni riportate in [Configurare l'ambiente di sviluppo Python locale per Azure](configure-local-development-environment.md).

Assicurarsi di creare un'entità servizio per lo sviluppo locale e di creare e attivare un ambiente virtuale per questo progetto.

## <a name="2-install-the-needed-management-libraries"></a>2: Installare le librerie di gestione necessarie

1. Creare un file *requirements.txt* in cui elencare le librerie di gestione usate in questo esempio:

    ```txt
    azure-mgmt-resource
    azure-mgmt-storage
    azure-cli-core
    ```

1. Nel terminale con l'ambiente virtuale attivato installare i requisiti:

    ```bash
    pip install -r requirements.txt
    ```

## <a name="3-write-code-to-provision-storage-resources"></a>3: Scrivere codice per effettuare il provisioning delle risorse di archiviazione

Creare un file Python denominato *provision_blob.py* con il codice seguente. I commenti spiegano i dettagli:

```python
import os, random

# Import the needed management objects from the libraries. The azure.common library
# is installed automatically with the other libraries.
from azure.common.client_factory import get_client_from_cli_profile
from azure.mgmt.resource import ResourceManagementClient
from azure.mgmt.storage import StorageManagementClient

# Obtain the management object for resources, using the credentials from the CLI login.
resource_client = get_client_from_cli_profile(ResourceManagementClient)

# Constants we need in multiple places: the resource group name and the region
# in which we provision resources. You can change these values however you want.
RESOURCE_GROUP_NAME = "PythonSDKExample-Storage-rg"
LOCATION = "centralus"

# Step 1: Provision the resource group.
rg_result = resource_client.resource_groups.create_or_update(RESOURCE_GROUP_NAME,
    { "location": LOCATION })

print(f"Provisioned resource group {rg_result.name}")

# For details on the previous code, see Example: Provision a resource group
# at https://docs.microsoft.com/azure/developer/python/azure-sdk-example-resource-group

# Step 2: Provision the storage account, starting with a management object.

storage_client = get_client_from_cli_profile(StorageManagementClient)

# This example uses the CLI profile credentials because we assume the script
# is being used to provision the resource in the same way the Azure CLI would be used.

STORAGE_ACCOUNT_NAME = f"pythonsdkstorage{random.randint(1,100000):05}"

# You can replace the storage account here with any unique name. A random number is used
# by default, but note that the name changes every time you run this script.
# The name must be 3-24 lower case letters and numbers only.


# Check if the account name is available. Storage account names must be unique across
# Azure because they're used in URLs.
availability_result = storage_client.storage_accounts.check_name_availability(STORAGE_ACCOUNT_NAME)

if not availability_result.name_available:
    print(f"Storage name {STORAGE_ACCOUNT_NAME} is already in use. Try another name.")
    exit()

# The name is available, so provision the account
poller = storage_client.storage_accounts.create(RESOURCE_GROUP_NAME, STORAGE_ACCOUNT_NAME,
    {
        "location" : LOCATION,
        "kind": "StorageV2",
        "sku": {"name": "Standard_LRS"}
    }
)

# Long-running operations return a poller object; calling poller.result()
# waits for completion.
account_result = poller.result()
print(f"Provisioned storage account {account_result.name}")

# Step 3: Retrieve the account's primary access key and generate a connection string.
keys = storage_client.storage_accounts.list_keys(RESOURCE_GROUP_NAME, STORAGE_ACCOUNT_NAME)

print(f"Primary key for storage account: {keys.keys[0].value}")

conn_string = f"DefaultEndpointsProtocol=https;EndpointSuffix=core.windows.net;AccountName={STORAGE_ACCOUNT_NAME};AccountKey={keys.keys[0].value}"

print(f"Connection string: {conn_string}")

# Step 4: Provision the blob container in the account (this call is synchronous)
CONTAINER_NAME = "blob-container-01"
container = storage_client.blob_containers.create(RESOURCE_GROUP_NAME, STORAGE_ACCOUNT_NAME, CONTAINER_NAME, {})

# The fourth argument is a required BlobContainer object, but because we don't need any
# special values there, so we just pass empty JSON.

print(f"Provisioned blob container {container.name}")

```

Questo codice usa i metodi di autenticazione basati sull'interfaccia della riga di comando (`get_client_from_cli_profile`) perché illustra azioni che altrimenti si potrebbero eseguire direttamente con l'interfaccia della riga di comando di Azure. In entrambi i casi si usa la stessa identità per l'autenticazione.

Per usare tale codice in uno script di produzione, è preferibile usare invece `DefaultAzureCredential` (scelta consigliata) o un metodo basato su entità servizio, come descritto in [Come autenticare le app Python con i servizi di Azure](azure-sdk-authenticate.md).

## <a name="4-run-the-script"></a>4. Eseguire lo script:

```bash
python provision_blob.py
```

Il completamento dello script richiederà uno o due minuti.

## <a name="5-verify-the-resources"></a>5: Verificare le risorse

1. Aprire il [portale di Azure](https://portal.azure.com) per verificare che il provisioning del gruppo di risorse e dell'account di archiviazione sia stato effettuato come previsto. Può essere necessario selezionare **Mostra tipi nascosti** nel gruppo di risorse per vedere un account di archiviazione di cui è stato effettuato il provisioning da uno script Python:

    ![Pagina del portale di Azure relativa al nuovo gruppo di risorse che mostra l'account di archiviazione](media/azure-sdk-example-storage/portal-show-hidden-types.png)

1. Selezionare l'account di archiviazione, quindi selezionare **Servizio BLOB** > **Contenitori** nel menu a sinistra per verificare che venga visualizzato "blob-container-01":

    ![Pagina del portale di Azure relativa all'account di archiviazione che mostra il contenitore BLOB](media/azure-sdk-example-storage/portal-show-blob-containers.png)

Per un altro esempio dell'uso della libreria di gestione di Archiviazione di Azure, vedere l'[esempio di gestione dell'archiviazione in Python](https://docs.microsoft.com/samples/azure-samples/storage-python-manage/storage-python-manage/).

### <a name="for-reference-equivalent-azure-cli-commands"></a>Per riferimento: comandi equivalenti dell'interfaccia della riga di comando di Azure

I seguenti comandi dell'interfaccia della riga di comando di Azure completano gli stessi passaggi di provisioning dello script Python:

# <a name="bash"></a>[Bash](#tab/bash)

```azurecli
# Provision the resource group

az group create -n PythonSDKExample-Storage-rg -l centralus

# Provision the storage account

az storage account create -g PythonSDKExample-Storage-rg -l centralus \
    -n pythonsdkstorage12345 --kind StorageV2 --sku Standard_LRS

# Retrieve the connection string

az storage account show-connection-string -g PythonSDKExample-Storage-rg \
    -n pythonsdkstorage12345

# Provision the blob container; NOTE: this command assumes you have an environment variable
# named AZURE_STORAGE_CONNECTION_STRING with the connection string for the storage account.

AZURE_STORAGE_CONNECTION_STRING=<connection_string>
az storage container create --account-name pythonsdkstorage12345 -n blob-container-01
```

# <a name="cmd"></a>[Cmd](#tab/cmd)

```azurecli
# Provision the resource group

az group create -n PythonSDKExample-Storage-rg -l centralus

# Provision the storage account

az storage account create -g PythonSDKExample-Storage-rg -l centralus ^
    -n pythonsdkstorage12345 --kind StorageV2 --sku Standard_LRS

# Retrieve the connection string

az storage account show-connection-string -g PythonSDKExample-Storage-rg ^
    -n pythonsdkstorage12345

# Provision the blob container; NOTE: this command assumes you have an environment variable
# named AZURE_STORAGE_CONNECTION_STRING with the connection string for the storage account.

set AZURE_STORAGE_CONNECTION_STRING=<connection_string>
az storage container create --account-name pythonsdkstorage12345 -n blob-container-01
```

---

## <a name="6-use-resources-through-the-sdk-client-libraries"></a>6: Usare le risorse tramite le librerie client dell'SDK

Le sezioni seguenti illustrano due modi per accedere al contenitore BLOB di cui è stato effettuato il provisioning nella sezione precedente. Questi esempi caricano in particolare un BLOB di file in tale contenitore usando le librerie client appropriate dell'SDK.

Seguire i [passaggi comuni](#common-steps-for-both-methods) per provare il codice.

Il primo metodo consente di autenticare l'app con `DefaultAzureCredential`, come descritto in [Come autenticare le app Python](azure-sdk-authenticate.md#authenticate-with-defaultazurecredential). Con questo metodo è necessario prima di tutto assegnare le autorizzazioni appropriate all'identità dell'app, che è la procedura consigliata.

Il secondo metodo usa una stringa di connessione per accedere direttamente all'account di archiviazione. Anche se questo metodo sembra più semplice, presenta due svantaggi significativi:

- Una stringa di connessione autentica intrinsecamente l'agente di connessione con l'*account* di archiviazione invece che con le singole risorse al suo interno. Di conseguenza, una stringa di connessione fornisce autorizzazioni più ampie di quelle che potrebbero essere necessarie.

- Una stringa di connessione contiene una chiave di accesso in testo normale e pertanto presenta potenziali vulnerabilità se è costruita in modo errato o non è adeguatamente protetta. Se tale stringa di connessione viene esposta, può essere usata per accedere a un'ampia gamma di risorse all'interno dell'account di archiviazione.

Per questi motivi, per il codice di produzione è consigliabile usare il metodo di autenticazione. Per la sperimentazione, tuttavia, l'uso della stringa di connessione è una scelta valida.

### <a name="common-steps-for-both-methods"></a>Passaggi comuni per entrambi i metodi

1. Nel file *requirements.txt* aggiungere una riga per le librerie client necessarie e salvare il file:

    ```text
    azure-storage-blob
    azure-identity
    ```

1. Nel terminale o dal prompt dei comandi reinstallare i requisiti:

    ```bash
    pip install -r requirements.txt
    ```

1. Creare un file di origine denominato *sample-source.txt*, come previsto dal codice, con un contenuto simile al seguente:

    ```text
    Hello there, Azure Storage. I'm a friendly file ready to be stored in a blob.
    ```

### <a name="use-blob-storage-with-authentication"></a>Usare l'archiviazione BLOB con l'autenticazione

1. Creare una variabile di ambiente denominata `AZURE_STORAGE_BLOB_URL`:

    # <a name="bash"></a>[Bash](#tab/bash)

    ```bash
    AZURE_STORAGE_BLOB_URL=https://pythonsdkstorage12345.blob.core.windows.net
    ```

    # <a name="cmd"></a>[Cmd](#tab/cmd)

    ```cmd
    set AZURE_STORAGE_BLOB_URL=https://pythonsdkstorage12345.blob.core.windows.net
    ```

    ---

    Sostituire "pythonsdkstorage12345" con il nome dell'account di archiviazione specifico.

1. Creare un file denominato *use_blob_auth.py* con il codice seguente. I commenti spiegano i passaggi.

    ```python
    import os
    from azure.identity import DefaultAzureCredential

    # Import the client object from the SDK library
    from azure.storage.blob import BlobClient

    credential = DefaultAzureCredential()

    # Retrieve the storage blob service URL, which is of the form
    # https://pythonsdkstorage12345.blob.core.windows.net/
    storage_url = os.environ["AZURE_STORAGE_BLOB_URL"]

    # Create the client object using the storage URL and the credential
    blob_client = BlobClient(storage_url, container_name="blob-container-01", blob_name="sample-blob.txt", credential=credential)

    # Open a local file and upload its contents to Blob Storage
    with open("./sample-source.txt", "rb") as data:
        blob_client.upload_blob(data)
    ```

1. Provare a eseguire il codice:

    ```bash
    python use_blob_auth.py
    ```

    Poiché l'entità servizio locale in uso non ha l'autorizzazione per accedere al contenitore BLOB, viene visualizzato un messaggio di errore analogo al seguente: "L'operazione richiesta non è consentita con questa autorizzazione".

1. Per concedere le autorizzazioni per il contenitore all'entità servizio, usare il comando [az role assignment create](/cli/azure/role/assignment?view=azure-cli-latest#az-role-assignment-create) dell'interfaccia della riga di comando di Azure (è molto lungo):

    # <a name="bash"></a>[Bash](#tab/bash)

    ```azurecli
    az role assignment create --assignee $AZURE_CLIENT_ID \
        --role "Storage Blob Data Contributor" \
        --scope "/subscriptions/$AZURE_SUBSCRIPTION_ID/resourceGroups/PythonSDKExample-Storage-rg/providers/Microsoft.Storage/storageAccounts/pythonsdkstorage12345/blobServices/default/containers/blob-container-01"
    ```

    # <a name="cmd"></a>[Cmd](#tab/cmd)

    ```azurecli
    az role assignment create --assignee %AZURE_CLIENT_ID% ^
        --role "Storage Blob Data Contributor" ^
        --scope "/subscriptions/%AZURE_SUBSCRIPTION_ID%/resourceGroups/PythonSDKExample-Storage-rg/providers/Microsoft.Storage/storageAccounts/pythonsdkstorage12345/blobServices/default/containers/blob-container-01"
    ```

    ---

    L'argomento `--scope` identifica dove viene applicata questa assegnazione di ruolo. In questo esempio il ruolo "Collaboratore ai dati BLOB di archiviazione" viene concesso al contenitore *specifico* denominato "blob-container-01".

    Sostituire `pythonsdkstorage12345` con il nome esatto del proprio account di archiviazione. Se necessario, è anche possibile modificare il nome del gruppo di risorse e del contenitore BLOB. Se si usa un nome errato, viene visualizzato il messaggio di errore "Non è possibile eseguire l'operazione richiesta su una risorsa annidata. La risorsa padre 'pythonsdkstorage12345' non è stata trovata".

    L'argomento `--scope` di questo comando usa anche le variabili di ambiente AZURE_CLIENT_ID e AZURE_SUBSCRIPTION_ID, che devono essere già state impostate nell'ambiente locale per l'entità servizio come descritto in [Configurare l'ambiente di sviluppo Python locale per Azure](configure-local-development-environment.md).

1. Eseguire di nuovo il codice per verificare se adesso funziona. Se viene visualizzato di nuovo il messaggio di errore sulle autorizzazioni, attendere un minuto che le autorizzazioni vengano propagate e quindi riprovare a eseguire il codice.

Per altre informazioni su ambiti e assegnazioni di ruolo, vedere [Come assegnare le autorizzazioni per i ruoli](how-to-assign-role-permissions.md).

### <a name="use-blob-storage-with-a-connection-string"></a>Usare archiviazione BLOB con una stringa di connessione

1. Creare un file Python denominato *use_blob_conn_string.py* con il codice seguente. I commenti spiegano i passaggi.

    ```python
    import os

    # Import the client object from the SDK library
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

### <a name="verify-blob-creation"></a>Verificare la creazione di BLOB

Dopo aver eseguito il codice di uno dei due metodi, nel [portale di Azure](https://portal.azure.com) passare al contenitore BLOB e verificare che esista un nuovo BLOB denominato *sample-blob.txt* con lo stesso contenuto del file *sample-source.txt*:

![Pagina del portale di Azure relativa al contenitore BLOB che mostra il file caricato](media/azure-sdk-example-storage/portal-blob-container-file.png)

## <a name="7-clean-up-resources"></a>7: Pulire le risorse

```azurecli
az group delete -n PythonSDKExample-Storage-rg
```

Se non è necessario mantenere le risorse di cui è stato effettuato il provisioning in questo esempio, eseguire questo comando per evitare addebiti ricorrenti nella sottoscrizione.

Per eliminare un gruppo di risorse dal codice, è anche possibile usare il metodo [`ResourceManagementClient.resource_groups.delete`](/python/api/azure-mgmt-resource/azure.mgmt.resource.resources.v2019_10_01.operations.resourcegroupsoperations?view=azure-python#delete-resource-group-name--custom-headers-none--raw-false--polling-true----operation-config-).

## <a name="next-step"></a>Passaggio successivo

> [!div class="nextstepaction"]
> [Esempio: Effettuare il provisioning di un app Web e distribuire il codice >>>](azure-sdk-example-web-app.md)
