---
title: Effettuare il provisioning di una macchina virtuale con le librerie di Azure SDK per Python
description: Come effettuare il provisioning di una macchina virtuale di Azure usando Python e le librerie di gestione di Azure SDK.
ms.date: 10/05/2020
ms.topic: conceptual
ms.custom: devx-track-python
ms.openlocfilehash: e01121047d42200e956345df611f82706b1e081e
ms.sourcegitcommit: f460914ac5843eb7392869a08e3a80af68ab227b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/13/2020
ms.locfileid: "92010226"
---
# <a name="example-use-the-azure-libraries-to-provision-a-virtual-machine"></a>Esempio: Usare le librerie di Azure per effettuare il provisioning di una macchina virtuale

Questo esempio illustra come usare le librerie di gestione di Azure SDK in uno script Python per creare un gruppo di risorse che contiene una macchina virtuale Linux. I [comandi equivalenti dell'interfaccia della riga di comando di Azure](#for-reference-equivalent-azure-cli-commands) vengono descritti più avanti in questo articolo. Se si preferisce usare il portale di Azure, vedere [Creare una macchina virtuale Linux](/azure/virtual-machines/linux/quick-create-portal) e [Creare una macchina virtuale Windows](/azure/virtual-machines/windows/quick-create-portal).

Se non diversamente specificato, tutti i comandi di questo articolo funzionano allo stesso modo nella shell Bash Linux/macOS e nella shell dei comandi di Windows.

> [!NOTE]
> Il provisioning di una macchina virtuale tramite codice è un processo in più passaggi che prevede il provisioning di una serie di altre risorse richieste dalla macchina virtuale. Se questo codice viene semplicemente eseguito dalla riga di comando, è molto più facile usare il comando [`az vm create`](/cli/azure/vm#az-vm-create), che effettua automaticamente il provisioning di queste risorse secondarie con i valori predefiniti di qualsiasi impostazione che si sceglie di omettere. Gli unici argomenti obbligatori sono un gruppo di risorse, il nome della macchina virtuale, il nome dell'immagine e le credenziali di accesso. Per altre informazioni, vedere [Creare rapidamente una macchina virtuale con l'interfaccia della riga di comando di Azure](/azure/virtual-machines/scripts/virtual-machines-windows-cli-sample-create-vm-quick-create).

## <a name="1-set-up-your-local-development-environment"></a>1: Configurare un ambiente di sviluppo locale

Se non è già stato fatto, seguire tutte le istruzioni riportate in [Configurare l'ambiente di sviluppo Python locale per Azure](configure-local-development-environment.md).

Assicurarsi di creare un'entità servizio per lo sviluppo locale e di creare e attivare un ambiente virtuale per questo progetto.

## <a name="2-install-the-needed-azure-library-packages"></a>2: Installare i pacchetti delle librerie di Azure necessari

1. Creare un file *requirements.txt* in cui elencare le librerie di gestione usate in questo esempio:

    ```txt
    azure-mgmt-resource
    azure-mgmt-compute
    azure-mgmt-network
    azure-identity
    ```

1. In un terminale o da un prompt dei comandi con l'ambiente virtuale attivato installare le librerie di gestione elencate nel file *requirements.txt*:

    ```cmd
    pip install -r requirements.txt
    ```

## <a name="3-write-code-to-provision-a-virtual-machine"></a>3: Scrivere il codice per effettuare il provisioning di una macchina virtuale

Creare un file Python denominato *provision_vm.py* con il codice seguente. I commenti spiegano i dettagli:

```python
# Import the needed credential and management objects from the libraries.
from azure.identity import AzureCliCredential
from azure.mgmt.resource import ResourceManagementClient
from azure.mgmt.network import NetworkManagementClient
from azure.mgmt.compute import ComputeManagementClient
import os

print(f"Provisioning a virtual machine...some operations might take a minute or two.")

# Acquire a credential object using CLI-based authentication.
credential = AzureCliCredential()

# Retrieve subscription ID from environment variable.
subscription_id = os.environ["AZURE_SUBSCRIPTION_ID"]


# Step 1: Provision a resource group

# Obtain the management object for resources, using the credentials from the CLI login.
resource_client = ResourceManagementClient(credential, subscription_id)

# Constants we need in multiple places: the resource group name and the region
# in which we provision resources. You can change these values however you want.
RESOURCE_GROUP_NAME = "PythonAzureExample-VM-rg"
LOCATION = "centralus"

# Provision the resource group.
rg_result = resource_client.resource_groups.create_or_update(RESOURCE_GROUP_NAME,
    {
        "location": LOCATION
    }
)


print(f"Provisioned resource group {rg_result.name} in the {rg_result.location} region")

# For details on the previous code, see Example: Provision a resource group
# at https://docs.microsoft.com/azure/developer/python/azure-sdk-example-resource-group


# Step 2: provision a virtual network

# A virtual machine requires a network interface client (NIC). A NIC requires
# a virtual network and subnet along with an IP address. Therefore we must provision
# these downstream components first, then provision the NIC, after which we
# can provision the VM.

# Network and IP address names
VNET_NAME = "python-example-vnet"
SUBNET_NAME = "python-example-subnet"
IP_NAME = "python-example-ip"
IP_CONFIG_NAME = "python-example-ip-config"
NIC_NAME = "python-example-nic"

# Obtain the management object for networks
network_client = NetworkManagementClient(credential, subscription_id)

# Provision the virtual network and wait for completion
poller = network_client.virtual_networks.begin_create_or_update(RESOURCE_GROUP_NAME,
    VNET_NAME,
    {
        "location": LOCATION,
        "address_space": {
            "address_prefixes": ["10.0.0.0/16"]
        }
    }
)

vnet_result = poller.result()

print(f"Provisioned virtual network {vnet_result.name} with address prefixes {vnet_result.address_space.address_prefixes}")

# Step 3: Provision the subnet and wait for completion
poller = network_client.subnets.begin_create_or_update(RESOURCE_GROUP_NAME, 
    VNET_NAME, SUBNET_NAME,
    { "address_prefix": "10.0.0.0/24" }
)
subnet_result = poller.result()

print(f"Provisioned virtual subnet {subnet_result.name} with address prefix {subnet_result.address_prefix}")

# Step 4: Provision an IP address and wait for completion
poller = network_client.public_ip_addresses.begin_create_or_update(RESOURCE_GROUP_NAME,
    IP_NAME,
    {
        "location": LOCATION,
        "sku": { "name": "Standard" },
        "public_ip_allocation_method": "Static",
        "public_ip_address_version" : "IPV4"
    }
)

ip_address_result = poller.result()

print(f"Provisioned public IP address {ip_address_result.name} with address {ip_address_result.ip_address}")

# Step 5: Provision the network interface client
poller = network_client.network_interfaces.begin_create_or_update(RESOURCE_GROUP_NAME,
    NIC_NAME, 
    {
        "location": LOCATION,
        "ip_configurations": [ {
            "name": IP_CONFIG_NAME,
            "subnet": { "id": subnet_result.id },
            "public_ip_address": {"id": ip_address_result.id }
        }]
    }
)

nic_result = poller.result()

print(f"Provisioned network interface client {nic_result.name}")

# Step 6: Provision the virtual machine

# Obtain the management object for virtual machines
compute_client = ComputeManagementClient(credential, subscription_id)

VM_NAME = "ExampleVM"
USERNAME = "azureuser"
PASSWORD = "ChangePa$$w0rd24"

print(f"Provisioning virtual machine {VM_NAME}; this operation might take a few minutes.")

# Provision the VM specifying only minimal arguments, which defaults to an Ubuntu 18.04 VM
# on a Standard DS1 v2 plan with a public IP address and a default virtual network/subnet.

poller = compute_client.virtual_machines.begin_create_or_update(RESOURCE_GROUP_NAME, VM_NAME,
    {
        "location": LOCATION,
        "storage_profile": {
            "image_reference": {
                "publisher": 'Canonical',
                "offer": "UbuntuServer",
                "sku": "16.04.0-LTS",
                "version": "latest"
            }
        },
        "hardware_profile": {
            "vm_size": "Standard_DS1_v2"
        },
        "os_profile": {
            "computer_name": VM_NAME,
            "admin_username": USERNAME,
            "admin_password": PASSWORD
        },
        "network_profile": {
            "network_interfaces": [{
                "id": nic_result.id,
            }]
        }
    }
)

vm_result = poller.result()

print(f"Provisioned virtual machine {vm_result.name}")
```

[!INCLUDE [cli-auth-note](includes/cli-auth-note.md)]

### <a name="reference-links-for-classes-used-in-the-code"></a>Collegamenti di riferimento per le classi usate nel codice

- [AzureCliCredential (azure.identity)](/python/api/azure-identity/azure.identity.azureclicredential)
- [ResourceManagementClient (azure.mgmt.resource)](/python/api/azure-mgmt-resource/azure.mgmt.resource.resourcemanagementclient)
- [NetworkManagementClient (azure.mgmt.network)](/python/api/azure-mgmt-network/azure.mgmt.network.networkmanagementclient)
- [ComputeManagementClient (azure.mgmt.compute)](/python/api/azure-mgmt-compute/azure.mgmt.compute.computemanagementclient)

## <a name="4-run-the-script"></a>4. Eseguire lo script

```cmd
python provision_vm.py
```

Il processo di provisioning richiede alcuni minuti.

## <a name="5-verify-the-resources"></a>5. Verificare le risorse

Aprire il [portale di Azure](https://portal.azure.com), passare al gruppo di risorse "PythonAzureExample-VM-rg" e osservare la macchina virtuale, il disco virtuale, il gruppo di sicurezza di rete, l'indirizzo IP pubblico, l'interfaccia di rete e la rete virtuale:

![Pagina del portale di Azure relativa al nuovo gruppo di risorse che mostra la macchina virtuale e le risorse correlate](media/azure-sdk-example-virtual-machines/portal-vm-resources.png)

### <a name="for-reference-equivalent-azure-cli-commands"></a>Per riferimento: comandi equivalenti dell'interfaccia della riga di comando di Azure

# <a name="cmd"></a>[cmd](#tab/cmd)

```azurecli
rem Provision the resource group

az group create -n PythonAzureExample-VM-rg -l centralus

rem Provision a virtual network and subnet

az network vnet create -g PythonAzureExample-VM-rg -n python-example-vnet ^
    --address-prefix 10.0.0.0/16 --subnet-name python-example-subnet ^
    --subnet-prefix 10.0.0.0/24

rem Provision a public IP address

az network public-ip create -g PythonAzureExample-VM-rg -n python-example-ip ^
    --allocation-method Dynamic --version IPv4

rem Provision a network interface client

az network nic create -g PythonAzureExample-VM-rg --vnet-name python-example-vnet ^
    --subnet python-example-subnet -n python-example-nic ^
    --public-ip-address python-example-ip

rem Provision the virtual machine

az vm create -g PythonAzureExample-VM-rg -n ExampleVM -l "centralus" ^
    --nics python-example-nic --image UbuntuLTS ^
    --admin-username azureuser --admin-password ChangePa$$w0rd24
```

# <a name="bash"></a>[Bash](#tab/bash)

```azurecli
# Provision the resource group

az group create -n PythonAzureExample-VM-rg -l centralus

# Provision a virtual network and subnet

az network vnet create -g PythonAzureExample-VM-rg -n python-example-vnet \
    --address-prefix 10.0.0.0/16 --subnet-name python-example-subnet \
    --subnet-prefix 10.0.0.0/24

# Provision a public IP address

az network public-ip create -g PythonAzureExample-VM-rg -n python-example-ip \
    --allocation-method Dynamic --version IPv4

# Provision a network interface client

az network nic create -g PythonAzureExample-VM-rg --vnet-name python-example-vnet \
    --subnet python-example-subnet -n python-example-nic \
    --public-ip-address python-example-ip

# Provision the virtual machine

az vm create -g PythonAzureExample-VM-rg -n ExampleVM -l "centralus" \
    --nics python-example-nic --image UbuntuLTS \
    --admin-username azureuser --admin-password ChangePa$$w0rd24

```

---

## <a name="6-clean-up-resources"></a>6: Pulire le risorse

```azurecli
az group delete -n PythonAzureExample-VM-rg  --no-wait
```

Eseguire questo comando se non è necessario mantenere le risorse create in questo esempio per evitare addebiti ricorrenti nella sottoscrizione.

[!INCLUDE [resource_group_begin_delete](includes/resource-group-begin-delete.md)]

## <a name="see-also"></a>Vedere anche

- [Esempio: Effettuare il provisioning di un gruppo di risorse](azure-sdk-example-resource-group.md)
- [Esempio: Elencare i gruppi di risorse in una sottoscrizione](azure-sdk-example-list-resource-groups.md)
- [Esempio: Effettuare il provisioning di Archiviazione di Azure](azure-sdk-example-storage.md)
- [Esempio: Usare Archiviazione di Azure](azure-sdk-example-storage-use.md)
- [Esempio: Effettuare il provisioning di un'app Web e distribuire il codice](azure-sdk-example-web-app.md)
- [Esempio: Effettuare il provisioning ed eseguire query su un database](azure-sdk-example-database.md)

Il contenitore di risorse seguente illustra esempi più completi che usano Python per creare una macchina virtuale:

- [Creare e gestire macchine virtuali Windows in Azure usando Python](/azure/virtual-machines/windows/python). È possibile usare questo esempio per creare VM Linux cambiando il parametro `storage_profile`.
- [Esempi di gestione delle macchine virtuali di Azure - Python](https://github.com/Azure-Samples/virtual-machines-python-manage) (GitHub). Questo esempio illustra altre operazioni di gestione, come l'avvio e il riavvio di una VM, l'arresto e l'eliminazione di una VM, l'aumento delle dimensioni del disco e la gestione di dischi dati.
