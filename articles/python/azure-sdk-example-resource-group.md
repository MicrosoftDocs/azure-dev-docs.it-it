---
title: Effettuare il provisioning di un gruppo di risorse con le librerie di Azure per Python
description: Usare la libreria di gestione delle risorse di Azure SDK per Python per creare un gruppo di risorse dal codice Python.
ms.date: 01/28/2021
ms.topic: conceptual
ms.custom: devx-track-python
ms.openlocfilehash: 503ed658fd216edb6308f39b7a721398d6d1fc3a
ms.sourcegitcommit: 576c878c338d286060010646b96f3ad0fdbcb814
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/04/2021
ms.locfileid: "102117805"
---
# <a name="example-use-the-azure-libraries-to-provision-a-resource-group"></a>Esempio: Usare le librerie di Azure per effettuare il provisioning di un gruppo di risorse

Questo esempio illustra come usare le librerie di gestione di Azure SDK in uno script Python per effettuare il provisioning di un gruppo di risorse. Il [comando equivalente dell'interfaccia della riga di comando di Azure](#for-reference-equivalent-azure-cli-commands) viene descritto più avanti in questo articolo. Se si preferisce usare il portale di Azure, vedere [Creare gruppi di risorse](/azure/azure-resource-manager/management/manage-resource-groups-portal).

Se non diversamente specificato, tutti i comandi di questo articolo funzionano allo stesso modo nella shell Bash Linux/macOS e nella shell dei comandi di Windows.

## <a name="1-set-up-your-local-development-environment"></a>1: Configurare un ambiente di sviluppo locale

Se non è già stato fatto, seguire tutte le istruzioni riportate in [Configurare l'ambiente di sviluppo Python locale per Azure](configure-local-development-environment.md).

Assicurarsi di creare e attivare un ambiente virtuale per questo progetto.

## <a name="2-install-the-azure-library-packages"></a>2: Installare i pacchetti di librerie di Azure

Creare un file denominato *requirements.txt* con il contenuto seguente:

```text
azure-mgmt-resource>=1.15.0
azure-identity>=1.5.0
```

Assicurarsi di usare queste versioni delle librerie. Se si usano versioni precedenti, si verificano errori come l'oggetto oggetto "AzureCliCredential" senza attributo "signed_session".

In un terminale o da un prompt dei comandi con l'ambiente virtuale attivato, installare i requisiti:

```cmd
pip install -r requirements.txt
```

## <a name="3-write-code-to-provision-a-resource-group"></a>3: Scrivere codice per effettuare il provisioning di un gruppo di risorse

Creare un file Python denominato *provision_rg.py* con il codice seguente. I commenti spiegano i dettagli:

```python
# Import the needed credential and management objects from the libraries.
from azure.mgmt.resource import ResourceManagementClient
from azure.identity import AzureCliCredential
import os

# Acquire a credential object using CLI-based authentication.
credential = AzureCliCredential()

# Retrieve subscription ID from environment variable.
subscription_id = os.environ["AZURE_SUBSCRIPTION_ID"]

# Obtain the management object for resources.
resource_client = ResourceManagementClient(credential, subscription_id)

# Provision the resource group.
rg_result = resource_client.resource_groups.create_or_update(
    "PythonAzureExample-rg",
    {
        "location": "centralus"
    }
)

# Within the ResourceManagementClient is an object named resource_groups,
# which is of class ResourceGroupsOperations, which contains methods like
# create_or_update.
#
# The second parameter to create_or_update here is technically a ResourceGroup
# object. You can create the object directly using ResourceGroup(location=LOCATION)
# or you can express the object as inline JSON as shown here. For details,
# see Inline JSON pattern for object arguments at
# https://docs.microsoft.com/azure/developer/python/azure-sdk-overview#inline-json-pattern-for-object-arguments.

print(f"Provisioned resource group {rg_result.name} in the {rg_result.location} region")

# The return value is another ResourceGroup object with all the details of the
# new group. In this case the call is synchronous: the resource group has been
# provisioned by the time the call returns.

# Update the resource group with tags
rg_result = resource_client.resource_groups.create_or_update(
    "PythonAzureExample-rg",
    {
        "location": "centralus",
        "tags": { "environment":"test", "department":"tech" }
    }
)

print(f"Updated resource group {rg_result.name} with tags")

# Optional lines to delete the resource group. begin_delete is asynchronous.
# poller = resource_client.resource_groups.begin_delete(rg_result.name)
# result = poller.result()
```

[!INCLUDE [cli-auth-note](includes/cli-auth-note.md)]

### <a name="reference-links-for-classes-used-in-the-code"></a>Collegamenti di riferimento per le classi usate nel codice

- [AzureCliCredential (azure.identity)](/python/api/azure-identity/azure.identity.azureclicredential)
- [ResourceManagementClient (azure.mgmt.resource)](/python/api/azure-mgmt-resource/azure.mgmt.resource.resourcemanagementclient)

## <a name="4-run-the-script"></a>4: Eseguire lo script

```cmd
python provision_rg.py
```

## <a name="5-verify-the-resource-group"></a>5: Verificare il gruppo di risorse

È possibile verificare che il gruppo esista tramite il portale di Azure o l'interfaccia della riga di comando di Azure.

- Portale di Azure: aprire il [portale di Azure](https://portal.azure.com), selezionare **Gruppi di risorse** e verificare che il gruppo sia presente nell'elenco. Se il portale è già aperto, usare il comando **Aggiorna** per aggiornare l'elenco.

- Interfaccia della riga di comando di Azure: eseguire il comando seguente:

    ```azurecli
    az group show -n PythonAzureExample-rg
    ```

## <a name="6-clean-up-resources"></a>6: Pulire le risorse

```azurecli
az group delete -n PythonAzureExample-rg  --no-wait
```

Se non è necessario mantenere il gruppo di risorse di cui è stato effettuato il provisioning in questo esempio, eseguire questo comando. I gruppi di risorse non comportano alcun addebito ricorrente per la sottoscrizione, ma è consigliabile pulire tutti i gruppi che non vengono usati attivamente. Con l'argomento `--no-wait`, il comando restituisce immediatamente il risultato invece di attendere il completamento dell'operazione.

Per eliminare un gruppo di risorse dal codice, è anche possibile usare il metodo [`ResourceManagementClient.resource_groups.delete`](/python/api/azure-mgmt-resource/azure.mgmt.resource.resources.v2019_10_01.operations.resourcegroupsoperations#delete-resource-group-name--custom-headers-none--raw-false--polling-true----operation-config-).

### <a name="for-reference-equivalent-azure-cli-commands"></a>Per riferimento: comandi equivalenti dell'interfaccia della riga di comando di Azure

I seguenti comandi dell'interfaccia della riga di comando di Azure completano gli stessi passaggi di provisioning dello script Python:

```azurecli
az group create -n PythonAzureExample-rg -l centralus
```

## <a name="see-also"></a>Vedere anche

- [Esempio: Elencare i gruppi di risorse in una sottoscrizione](azure-sdk-example-list-resource-groups.md)
- [Esempio: Effettuare il provisioning di Archiviazione di Azure](azure-sdk-example-storage.md)
- [Esempio: Usare Archiviazione di Azure](azure-sdk-example-storage-use.md)
- [Esempio: Effettuare il provisioning di un'app Web e distribuire il codice](azure-sdk-example-web-app.md)
- [Esempio: Effettuare il provisioning ed eseguire query su un database](azure-sdk-example-database.md)
- [Esempio: Effettuare il provisioning di una macchina virtuale](azure-sdk-example-virtual-machines.md)
- [Usare Azure Managed Disks con le macchine virtuali](azure-sdk-samples-managed-disks.md)
- [Completa un breve sondaggio su Azure SDK per Python](https://microsoft.qualtrics.com/jfe/form/SV_bNFX0HECjzPWMiG?Q_CHL=docs)
