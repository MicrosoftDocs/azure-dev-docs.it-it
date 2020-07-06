---
title: Effettuare il provisioning di un gruppo di risorse con le librerie di Azure per Python
description: Usare la libreria di gestione delle risorse di Azure SDK per Python per creare un gruppo di risorse dal codice Python.
ms.date: 05/29/2020
ms.topic: conceptual
ms.openlocfilehash: 38c2f87511c040a04040844fb512fd436d566c15
ms.sourcegitcommit: 499f7275446f006fa43c4eff3b1f0d001e9a98d9
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/05/2020
ms.locfileid: "84453702"
---
# <a name="example-use-the-azure-libraries-to-provision-a-resource-group"></a>Esempio: Usare le librerie di Azure per effettuare il provisioning di un gruppo di risorse

Questo esempio illustra come usare le librerie di gestione di Azure SDK in uno script Python per effettuare il provisioning di un gruppo di risorse. Il [comando equivalente dell'interfaccia della riga di comando di Azure](#for-reference-equivalent-azure-cli-commands) viene descritto più avanti in questo articolo.

Tutti i comandi di questo articolo funzionano allo stesso modo nella shell Bash Linux/Mac OS e nella shell dei comandi di Windows, se non diversamente specificato.

## <a name="1-set-up-your-local-development-environment"></a>1: Configurare un ambiente di sviluppo locale

Se non è già stato fatto, seguire tutte le istruzioni riportate in [Configurare l'ambiente di sviluppo Python locale per Azure](configure-local-development-environment.md).

Assicurarsi di creare un'entità servizio per lo sviluppo locale e di creare e attivare un ambiente virtuale per questo progetto.

## <a name="2-install-the-azure-library-packages"></a>2: Installare i pacchetti di librerie di Azure

Creare un file denominato *requirements.txt* con il contenuto seguente:

```text
azure-mgmt-resource
azure-cli-core
```

In un terminale o da un prompt dei comandi con l'ambiente virtuale attivato, installare i requisiti:

```cmd
pip install -r requirements.txt
```

## <a name="3-write-code-to-provision-a-resource-group"></a>3: Scrivere codice per effettuare il provisioning di un gruppo di risorse

Creare un file Python denominato *provision_rg.py* con il codice seguente. I commenti spiegano i dettagli:

```python
# Import the needed management objects from the libraries. The azure.common library
# is installed automatically with the other libraries.
from azure.common.client_factory import get_client_from_cli_profile
from azure.mgmt.resource import ResourceManagementClient

# Obtain the management object for resources, using the credentials from the CLI login.
resource_client = get_client_from_cli_profile(ResourceManagementClient)

# Provision the resource group.
rg_result = resource_client.resource_groups.create_or_update(
    "PythonAzureExample-ResourceGroup-rg",
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

# Optional line to delete the resource group
#resource_client.resource_groups.delete("PythonAzureExample-ResourceGroup-rg")
```

Questo codice usa i metodi di autenticazione basati sull'interfaccia della riga di comando (`get_client_from_cli_profile`) perché illustra azioni che altrimenti si potrebbero eseguire direttamente con l'interfaccia della riga di comando di Azure. In entrambi i casi si usa la stessa identità per l'autenticazione.

Per usare tale codice in uno script di produzione, è preferibile usare invece `DefaultAzureCredential` (scelta consigliata) o un metodo basato su entità servizio, come descritto in [Come autenticare le app Python con i servizi di Azure](azure-sdk-authenticate.md).

### <a name="reference-links-for-classes-used-in-the-code"></a>Collegamenti di riferimento per le classi usate nel codice

- [ResourceManagementClient (azure.mgmt.resource)](/python/api/azure-mgmt-resource/azure.mgmt.resource.resourcemanagementclient?view=azure-python)

## <a name="4-run-the-script"></a>4: Eseguire lo script

```cmd
python provision_rg.py
```

## <a name="5-verify-the-resource-group"></a>5: Verificare il gruppo di risorse

È possibile verificare che il gruppo esista tramite il portale di Azure o l'interfaccia della riga di comando di Azure.

- Portale di Azure: aprire il [portale di Azure](https://portal.azure.com), selezionare **Gruppi di risorse** e verificare che il gruppo sia presente nell'elenco. Se il portale è già aperto, usare il comando **Aggiorna** per aggiornare l'elenco.

- Interfaccia della riga di comando di Azure: eseguire il comando seguente:

    ```azurecli
    az group show -n PythonAzureExample-ResourceGroup-rg
    ```

## <a name="6-clean-up-resources"></a>6: Pulire le risorse

```azurecli
az group delete -n PythonAzureExample-ResourceGroup-rg
```

Se non è necessario mantenere il gruppo di risorse di cui è stato effettuato il provisioning in questo esempio, eseguire questo comando. I gruppi di risorse non comportano alcun addebito ricorrente per la sottoscrizione, ma è consigliabile pulire tutti i gruppi che non vengono usati attivamente.

Per eliminare un gruppo di risorse dal codice, è anche possibile usare il metodo [`ResourceManagementClient.resource_groups.delete`](/python/api/azure-mgmt-resource/azure.mgmt.resource.resources.v2019_10_01.operations.resourcegroupsoperations?view=azure-python#delete-resource-group-name--custom-headers-none--raw-false--polling-true----operation-config-).

### <a name="for-reference-equivalent-azure-cli-commands"></a>Per riferimento: comandi equivalenti dell'interfaccia della riga di comando di Azure

I seguenti comandi dell'interfaccia della riga di comando di Azure completano gli stessi passaggi di provisioning dello script Python:

```azurecli
az group create -n PythonAzureExample-ResourceGroup-rg -l centralus
```

## <a name="see-also"></a>Vedere anche

- [Esempio: Effettuare il provisioning di Archiviazione di Azure](azure-sdk-example-storage.md)
- [Esempio: Usare Archiviazione di Azure](azure-sdk-example-storage-use.md)
- [Esempio: Effettuare il provisioning di un'app Web e distribuire il codice](azure-sdk-example-web-app.md)
- [Esempio: Effettuare il provisioning ed eseguire query su un database](azure-sdk-example-database.md)
- [Esempio: Effettuare il provisioning di una macchina virtuale](azure-sdk-example-virtual-machines.md)
