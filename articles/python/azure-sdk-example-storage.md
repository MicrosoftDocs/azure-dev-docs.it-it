---
title: Effettuare il provisioning di Archiviazione di Azure con le librerie di Azure per Python
description: Usare le librerie di Azure SDK per Python per effettuare il provisioning di un contenitore BLOB in un account di archiviazione di Azure e quindi caricarvi un file.
ms.date: 10/05/2020
ms.topic: conceptual
ms.custom: devx-track-python, devx-track-azurecli
ms.openlocfilehash: 56d40c013456cb56e38970afb9d72933b5fa1f26
ms.sourcegitcommit: 6fbf9e489b194586887a2c11152044be5b3a2b99
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/25/2021
ms.locfileid: "98759394"
---
# <a name="example-provision-azure-storage-using-the-azure-libraries-for-python"></a>Esempio: Effettuare il provisioning di Archiviazione di Azure con le librerie di Azure per Python

Questo articolo illustra come usare le librerie di gestione di Azure in uno script Python per effettuare il provisioning di un gruppo di risorse contenente l'account di archiviazione di Azure e un contenitore di archiviazione BLOB. I [comandi equivalenti dell'interfaccia della riga di comando di Azure](#for-reference-equivalent-azure-cli-commands) vengono descritti più avanti in questo articolo. Se si preferisce usare il portale di Azure, vedere [Creare un account di archiviazione di Azure](/azure/storage/common/storage-account-create?tabs=azure-portal) e [Creare un contenitore BLOB](/azure/storage/blobs/storage-quickstart-blobs-portal).

Dopo aver effettuato il provisioning delle risorse, vedere [Esempio: Usare Archiviazione di Azure](azure-sdk-example-storage-use.md) usare le librerie client di Azure nel codice dell'applicazione Python per caricare un file nel contenitore di archiviazione BLOB.

Se non diversamente specificato, tutti i comandi di questo articolo funzionano allo stesso modo nella shell Bash Linux/macOS e nella shell dei comandi di Windows.

## <a name="1-set-up-your-local-development-environment"></a>1: Configurare un ambiente di sviluppo locale

Se non è già stato fatto, seguire tutte le istruzioni riportate in [Configurare l'ambiente di sviluppo Python locale per Azure](configure-local-development-environment.md).

Assicurarsi di creare un'entità servizio per lo sviluppo locale e di creare e attivare un ambiente virtuale per questo progetto.

## <a name="2-install-the-needed-azure-library-packages"></a>2: Installare i pacchetti delle librerie di Azure necessari

1. Creare un file *requirements.txt* in cui elencare le librerie di gestione usate in questo esempio:

    ```txt
    azure-mgmt-resource
    azure-mgmt-storage
    azure-identity
    ```

1. Nel terminale con l'ambiente virtuale attivato installare i requisiti:

    ```cmd
    pip install -r requirements.txt
    ```

## <a name="3-write-code-to-provision-storage-resources"></a>3: Scrivere codice per effettuare il provisioning delle risorse di archiviazione

Questa sezione descrive come effettuare il provisioning delle risorse di archiviazione da codice Python. Se si preferisce, è anche possibile effettuare il provisioning delle risorse tramite il portale di Azure o i [comandi equivalenti dell'interfaccia della riga di comando di Azure](#for-reference-equivalent-azure-cli-commands).

Creare un file Python denominato *provision_blob.py* con il codice seguente. I commenti spiegano i dettagli:

```python
import os, random

# Import the needed management objects from the libraries. The azure.common library
# is installed automatically with the other libraries.
from azure.identity import AzureCliCredential
from azure.mgmt.resource import ResourceManagementClient
from azure.mgmt.storage import StorageManagementClient

# Acquire a credential object using CLI-based authentication.
credential = AzureCliCredential()

# Retrieve subscription ID from environment variable.
subscription_id = os.environ["AZURE_SUBSCRIPTION_ID"]

# Obtain the management object for resources.
resource_client = ResourceManagementClient(credential, subscription_id)

# Constants we need in multiple places: the resource group name and the region
# in which we provision resources. You can change these values however you want.
RESOURCE_GROUP_NAME = "PythonAzureExample-Storage-rg"
LOCATION = "centralus"

# Step 1: Provision the resource group.

rg_result = resource_client.resource_groups.create_or_update(RESOURCE_GROUP_NAME,
    { "location": LOCATION })

print(f"Provisioned resource group {rg_result.name}")

# For details on the previous code, see Example: Provision a resource group
# at https://docs.microsoft.com/azure/developer/python/azure-sdk-example-resource-group


# Step 2: Provision the storage account, starting with a management object.

storage_client = StorageManagementClient(credential, subscription_id)

# This example uses the CLI profile credentials because we assume the script
# is being used to provision the resource in the same way the Azure CLI would be used.

STORAGE_ACCOUNT_NAME = f"pythonazurestorage{random.randint(1,100000):05}"

# You can replace the storage account here with any unique name. A random number is used
# by default, but note that the name changes every time you run this script.
# The name must be 3-24 lower case letters and numbers only.


# Check if the account name is available. Storage account names must be unique across
# Azure because they're used in URLs.
availability_result = storage_client.storage_accounts.check_name_availability(
    { "name": STORAGE_ACCOUNT_NAME }
)

if not availability_result.name_available:
    print(f"Storage name {STORAGE_ACCOUNT_NAME} is already in use. Try another name.")
    exit()

# The name is available, so provision the account
poller = storage_client.storage_accounts.begin_create(RESOURCE_GROUP_NAME, STORAGE_ACCOUNT_NAME,
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

[!INCLUDE [cli-auth-note](includes/cli-auth-note.md)]

### <a name="reference-links-for-classes-used-in-the-code"></a>Collegamenti di riferimento per le classi usate nel codice

- [AzureCliCredential (azure.identity)](/python/api/azure-identity/azure.identity.azureclicredential)
- [ResourceManagementClient (azure.mgmt.resource)](/python/api/azure-mgmt-resource/azure.mgmt.resource.resourcemanagementclient)
- [StorageManagementClient (azure.mgmt.storage)](/python/api/azure-mgmt-storage/azure.mgmt.storage.storagemanagementclient)

## <a name="4-run-the-script"></a>4. Eseguire lo script

```cmd
python provision_blob.py
```

Il completamento dello script richiederà uno o due minuti.

## <a name="5-verify-the-resources"></a>5: Verificare le risorse

1. Aprire il [portale di Azure](https://portal.azure.com) per verificare che il provisioning del gruppo di risorse e dell'account di archiviazione sia stato effettuato come previsto. Può essere necessario selezionare **Mostra tipi nascosti** nel gruppo di risorse per vedere un account di archiviazione di cui è stato effettuato il provisioning da uno script Python:

    ![Pagina del portale di Azure relativa al nuovo gruppo di risorse che mostra l'account di archiviazione](media/azure-sdk-example-storage/portal-show-hidden-types.png)

1. Selezionare l'account di archiviazione, quindi selezionare **Servizio BLOB** > **Contenitori** nel menu a sinistra per verificare che venga visualizzato "blob-container-01":

    ![Pagina del portale di Azure relativa all'account di archiviazione che mostra il contenitore BLOB](media/azure-sdk-example-storage/portal-show-blob-containers.png)

1. Per provare a usare queste risorse di cui è stato effettuato il provisioning dal codice dell'applicazione, continuare con [Esempio: Usare Archiviazione di Azure](azure-sdk-example-storage-use.md).

Per un altro esempio relativo all'uso della libreria di gestione di Archiviazione di Azure, vedere l'[esempio di gestione dell'archiviazione in Python](/samples/azure-samples/storage-python-manage/storage-python-manage/).

### <a name="for-reference-equivalent-azure-cli-commands"></a>Per riferimento: comandi equivalenti dell'interfaccia della riga di comando di Azure

I seguenti comandi dell'interfaccia della riga di comando di Azure completano gli stessi passaggi di provisioning dello script Python:

# <a name="cmd"></a>[cmd](#tab/cmd)

```azurecli
rem Provision the resource group

az group create -n PythonAzureExample-Storage-rg -l centralus

rem Provision the storage account

az storage account create -g PythonAzureExample-Storage-rg -l centralus ^
    -n pythonazurestorage12345 --kind StorageV2 --sku Standard_LRS

rem Retrieve the connection string

az storage account show-connection-string -g PythonAzureExample-Storage-rg ^
    -n pythonazurestorage12345

rem Provision the blob container; NOTE: this command assumes you have an environment variable
rem named AZURE_STORAGE_CONNECTION_STRING with the connection string for the storage account.

set AZURE_STORAGE_CONNECTION_STRING=<connection_string>
az storage container create --account-name pythonazurestorage12345 -n blob-container-01
```

# <a name="bash"></a>[Bash](#tab/bash)

```azurecli
# Provision the resource group

az group create -n PythonAzureExample-Storage-rg -l centralus

# Provision the storage account

az storage account create -g PythonAzureExample-Storage-rg -l centralus \
    -n pythonazurestorage12345 --kind StorageV2 --sku Standard_LRS

# Retrieve the connection string

az storage account show-connection-string -g PythonAzureExample-Storage-rg \
    -n pythonazurestorage12345

# Provision the blob container; NOTE: this command assumes you have an environment variable
# named AZURE_STORAGE_CONNECTION_STRING with the connection string for the storage account.

AZURE_STORAGE_CONNECTION_STRING=<connection_string>
az storage container create --account-name pythonazurestorage12345 -n blob-container-01
```

---

## <a name="6-clean-up-resources"></a>6: Pulire le risorse

Non pulire le risorse se si vuole seguire l'articolo [Esempio: Usare Archiviazione di Azure](azure-sdk-example-storage-use.md) per usare queste risorse nel codice dell'app.

In caso contrario, eseguire il comando seguente per evitare ulteriori addebiti nella sottoscrizione.

```azurecli
az group delete -n PythonAzureExample-Storage-rg  --no-wait
```

[!INCLUDE [resource_group_begin_delete](includes/resource-group-begin-delete.md)]

## <a name="see-also"></a>Vedere anche

- [Esempio: Usare Archiviazione di Azure](azure-sdk-example-storage-use.md)
- [Esempio: Effettuare il provisioning di un gruppo di risorse](azure-sdk-example-resource-group.md)
- [Esempio: Elencare i gruppi di risorse in una sottoscrizione](azure-sdk-example-list-resource-groups.md)
- [Esempio: Effettuare il provisioning di un'app Web e distribuire il codice](azure-sdk-example-web-app.md)
- [Esempio: Effettuare il provisioning ed eseguire query su un database](azure-sdk-example-database.md)
- [Esempio: Effettuare il provisioning di una macchina virtuale](azure-sdk-example-virtual-machines.md)
- [Usare Azure Managed Disks con le macchine virtuali](azure-sdk-samples-managed-disks.md)
- [Completa un breve sondaggio su Azure SDK per Python](https://microsoft.qualtrics.com/jfe/form/SV_bNFX0HECjzPWMiG?Q_CHL=docs)
