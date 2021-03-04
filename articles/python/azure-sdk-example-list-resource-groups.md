---
title: Elencare i gruppi di risorse e le risorse usando le librerie di Azure per Python
description: Usare la libreria di gestione delle risorse di Azure SDK per Python per elencare i gruppi di risorse e le risorse in un gruppo.
ms.date: 01/28/2021
ms.topic: conceptual
ms.custom: devx-track-python
ms.openlocfilehash: ed5ada8332635b6d84a25dcfa70064ae3c9128b1
ms.sourcegitcommit: 576c878c338d286060010646b96f3ad0fdbcb814
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/04/2021
ms.locfileid: "102117815"
---
# <a name="example-use-the-azure-libraries-to-list-resource-groups-and-resources"></a>Esempio: usare le librerie di Azure per elencare i gruppi di risorse e le risorse

Questo esempio illustra come usare le librerie di gestione di Azure SDK in uno script Python per eseguire due attività:

- Elencare tutti i gruppi di risorse in una sottoscrizione di Azure.
- Elencare le risorse entro un gruppo di risorse specifico.

Se non diversamente specificato, tutti i comandi di questo articolo funzionano allo stesso modo nella shell Bash Linux/macOS e nella shell dei comandi di Windows.

Il [comando equivalente dell'interfaccia della riga di comando di Azure](#for-reference-equivalent-azure-cli-commands) viene descritto più avanti in questo articolo.

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



## <a name="3-write-code-to-work-with-resource-groups"></a>3: Scrivere codice per usare i gruppi di risorse

### <a name="3a-list-resource-groups-in-a-subscription"></a>3a. Elencare i gruppi di risorse in una sottoscrizione

Creare un file Python denominato *list_groups.py* con il codice seguente. I commenti spiegano i dettagli:

```python
# Import the needed credential and management objects from the libraries.
from azure.identity import AzureCliCredential
from azure.mgmt.resource import ResourceManagementClient
import os

# Acquire a credential object using CLI-based authentication.
credential = AzureCliCredential()

# Retrieve subscription ID from environment variable.
subscription_id = os.environ["AZURE_SUBSCRIPTION_ID"]

# Obtain the management object for resources.
resource_client = ResourceManagementClient(credential, subscription_id)

# Retrieve the list of resource groups
group_list = resource_client.resource_groups.list()

# Show the groups in formatted output
column_width = 40

print("Resource Group".ljust(column_width) + "Location")
print("-" * (column_width * 2))

for group in list(group_list):
    print(f"{group.name:<{column_width}}{group.location}")
```

### <a name="3b-list-resources-within-a-specific-resource-group"></a>3b. Elencare le risorse entro un gruppo di risorse specifico

Creare un file Python denominato *list_resources.py* con il codice seguente. I commenti spiegano i dettagli:

```python
# Import the needed credential and management objects from the libraries.
from azure.identity import AzureCliCredential
from azure.mgmt.resource import ResourceManagementClient
import os

# Acquire a credential object using CLI-based authentication.
credential = AzureCliCredential()

# Retrieve subscription ID from environment variable.
subscription_id = os.environ["AZURE_SUBSCRIPTION_ID"]

# Obtain the management object for resources.
resource_client = ResourceManagementClient(credential, subscription_id)

# Retrieve the list of resources in "myResourceGroup" (change to any name desired).
# The expand argument includes additional properties in the output.
resource_list = resource_client.resources.list_by_resource_group(
    "myResourceGroup",
    expand = "createdTime,changedTime"
)

# Show the resources in formatted output
column_width = 36

print("Resource".ljust(column_width) + "Type".ljust(column_width)
    + "Create date".ljust(column_width) + "Change date".ljust(column_width))
print("-" * (column_width * 4))

for resource in list(resource_list):
    print(f"{resource.name:<{column_width}}{resource.type:<{column_width}}"
       f"{str(resource.created_time):<{column_width}}{str(resource.changed_time):<{column_width}}")
```

### <a name="authentication-in-the-code"></a>Autenticazione nel codice

[!INCLUDE [cli-auth-note](includes/cli-auth-note.md)]

### <a name="reference-links-for-classes-used-in-the-code"></a>Collegamenti di riferimento per le classi usate nel codice

- [AzureCliCredential (azure.identity)](/python/api/azure-identity/azure.identity.azureclicredential)
- [ResourceManagementClient (azure.mgmt.resource)](/python/api/azure-mgmt-resource/azure.mgmt.resource.resourcemanagementclient)

## <a name="4-run-the-scripts"></a>4: Esecuzione degli script

Elencare tutti i gruppi di risorse nella sottoscrizione:

```cmd
python list_groups.py
```

Elencare tutte le risorse in un gruppo di risorse:

```cmd
python list_resources.py
```

### <a name="for-reference-equivalent-azure-cli-commands"></a>Per riferimento: comandi equivalenti dell'interfaccia della riga di comando di Azure

Il comando seguente dell'interfaccia della riga di comando di Azure elenca i gruppi di risorse in una sottoscrizione usando l'output JSON:

```azurecli
az group list
```

Il comando seguente elenca le risorse in "myResourceGroup" nell'area centralus (Stati Uniti centrali). L'argomento location è necessario per identificare un data center specifico:

```azurecli
az resource list --resource group myResourceGroup --location centralus
```

## <a name="see-also"></a>Vedere anche

- [Esempio: Effettuare il provisioning di un gruppo di risorse](azure-sdk-example-resource-group.md)
- [Esempio: Effettuare il provisioning di Archiviazione di Azure](azure-sdk-example-storage.md)
- [Esempio: Usare Archiviazione di Azure](azure-sdk-example-storage-use.md)
- [Esempio: Effettuare il provisioning di un'app Web e distribuire il codice](azure-sdk-example-web-app.md)
- [Esempio: Effettuare il provisioning ed eseguire query su un database](azure-sdk-example-database.md)
- [Esempio: Effettuare il provisioning di una macchina virtuale](azure-sdk-example-virtual-machines.md)
- [Usare Azure Managed Disks con le macchine virtuali](azure-sdk-samples-managed-disks.md)
- [Completa un breve sondaggio su Azure SDK per Python](https://microsoft.qualtrics.com/jfe/form/SV_bNFX0HECjzPWMiG?Q_CHL=docs)
