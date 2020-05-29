---
title: Effettuare il provisioning di una macchina virtuale con Azure SDK per Python
description: Come effettuare il provisioning di una macchina virtuale di Azure usando Python e le librerie di gestione di Azure SDK.
ms.date: 05/12/2020
ms.topic: conceptual
ms.openlocfilehash: f21495cc42f3bb228e460f1c591c9aa037dd8123
ms.sourcegitcommit: 9330d5af796b4b114466bbe75b8e18a9206f218e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/26/2020
ms.locfileid: "83862784"
---
# <a name="example-use-the-azure-sdk-to-provision-a-virtual-machine"></a>Esempio: Usare Azure SDK per effettuare il provisioning di una macchina virtuale

Questo esempio illustra come usare le librerie di gestione di Azure SDK in uno script Python per creare un gruppo di risorse che contiene una macchina virtuale Linux.

In questo esempio non sono presenti librerie client perché le macchine virtuali hanno solo un'interfaccia di gestione.

Tutti i comandi di questo articolo funzionano allo stesso modo nella shell Bash Linux/Mac OS e nella shell dei comandi di Windows, se non diversamente specificato.

> [!NOTE]
> Il provisioning di una macchina virtuale tramite codice è un processo in più passaggi che prevede il provisioning di una serie di altre risorse richieste dalla macchina virtuale. Se questo codice viene semplicemente eseguito dalla riga di comando, è molto più facile usare il comando [`az vm create`](/cli/azure/vm?view=azure-cli-latest#az-vm-create), che effettua automaticamente il provisioning di queste risorse secondarie con i valori predefiniti di qualsiasi impostazione che si sceglie di omettere. Gli unici argomenti obbligatori sono un gruppo di risorse, il nome della macchina virtuale, il nome dell'immagine e le credenziali di accesso. Per altre informazioni, vedere [Creare rapidamente una macchina virtuale con l'interfaccia della riga di comando di Azure](/azure/virtual-machines/scripts/virtual-machines-windows-cli-sample-create-vm-quick-create).

## <a name="1-set-up-your-local-development-environment"></a>1: Configurare un ambiente di sviluppo locale

Se non è già stato fatto, seguire tutte le istruzioni riportate in [Configurare l'ambiente di sviluppo Python locale per Azure](configure-local-development-environment.md).

Assicurarsi di creare un'entità servizio per lo sviluppo locale e di creare e attivare un ambiente virtuale per questo progetto.

## <a name="2-install-the-needed-sdk-libraries"></a>2: Installare le librerie necessarie dell'SDK

1. Creare un file *requirements.txt* in cui elencare le librerie di gestione usate in questo esempio:

    ```txt
    azure-mgmt-resource
    azure-mgmt-network
    azure-mgmt-compute
    azure-cli-core
    ```

1. In un terminale o da un prompt dei comandi con l'ambiente virtuale attivato installare le librerie di gestione elencate nel file *requirements.txt*:

    ```bash
    pip install -r requirements.txt
    ```

## <a name="3-write-code-to-provision-a-virtual-machine"></a>3: Scrivere il codice per effettuare il provisioning di una macchina virtuale

Creare un file Python denominato *provision_vm.py* con il codice seguente. I commenti spiegano i dettagli:

```python
# Import the needed management objects from the libraries. The azure.common library
# is installed automatically with the other libraries.
from azure.common.client_factory import get_client_from_cli_profile
from azure.mgmt.resource import ResourceManagementClient
from azure.mgmt.network import NetworkManagementClient
from azure.mgmt.compute import ComputeManagementClient

print(f"Provisioning a virtual machine...some operations might take a minute or two.")

# Step 1: Provision a resource group

# Obtain the management object for resources, using the credentials from the CLI login.
resource_client = get_client_from_cli_profile(ResourceManagementClient)

# Constants we need in multiple places: the resource group name and the region
# in which we provision resources. You can change these values however you want.
RESOURCE_GROUP_NAME = "PythonSDKExample-VM-rg"
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
network_client = get_client_from_cli_profile(NetworkManagementClient)

# Provision the virtual network and wait for completion
poller = network_client.virtual_networks.create_or_update(RESOURCE_GROUP_NAME,
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
poller = network_client.subnets.create_or_update(RESOURCE_GROUP_NAME, 
    VNET_NAME, SUBNET_NAME,
    { "address_prefix": "10.0.0.0/24" }
)
subnet_result = poller.result()

print(f"Provisioned virtual subnet {subnet_result.name} with address prefix {subnet_result.address_prefix}")

# Step 4: Provision an IP address and wait for completion
poller = network_client.public_ip_addresses.create_or_update(RESOURCE_GROUP_NAME,
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
poller = network_client.network_interfaces.create_or_update(RESOURCE_GROUP_NAME,
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
compute_client = get_client_from_cli_profile(ComputeManagementClient)

VM_NAME = "ExampleVM"
USERNAME = "azureuser"
PASSWORD = "ChangePa$$w0rd24"

print(f"Provisioning virtual machine {VM_NAME}; this operation might take a few minutes.")

# Provision the VM specifying only minimal arguments, which defaults to an Ubuntu 18.04 VM
# on a Standard DS1 v2 plan with a public IP address and a default virtual network/subnet.

poller = compute_client.virtual_machines.create_or_update(RESOURCE_GROUP_NAME, VM_NAME,
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

Questo codice usa i metodi di autenticazione basati sull'interfaccia della riga di comando (`get_client_from_cli_profile`) perché illustra azioni che altrimenti si potrebbero eseguire direttamente con l'interfaccia della riga di comando di Azure. In entrambi i casi si usa la stessa identità per l'autenticazione.

Per usare tale codice in uno script di produzione (ad esempio per automatizzare la gestione delle VM), invece `DefaultAzureCredential` (scelta consigliata) o un metodo basato su entità servizio come descritto in [Come autenticare le app Python con i servizi di Azure](azure-sdk-authenticate.md).

## <a name="4-run-the-script"></a>4. Eseguire lo script

```bash
python provision_vm.py
```

Il processo di provisioning richiede alcuni minuti.

## <a name="5-verify-the-resources"></a>5. Verificare le risorse

Aprire il [portale di Azure](https://portal.azure.com), passare al gruppo di risorse "PythonSDKExample-VM-rg" e osservare la macchina virtuale, il disco virtuale, il gruppo di sicurezza di rete, l'indirizzo IP pubblico, l'interfaccia di rete e la rete virtuale:

![Pagina del portale di Azure relativa al nuovo gruppo di risorse che mostra la macchina virtuale e le risorse correlate](media/azure-sdk-example-virtual-machines/portal-vm-resources.png)

### <a name="for-reference-equivalent-azure-cli-commands"></a>Per riferimento: comandi equivalenti dell'interfaccia della riga di comando di Azure

# <a name="bash"></a>[Bash](#tab/bash)

```azurecli
# Provision the resource group

az group create -n PythonSDKExample-VM-rg -l centralus

# Provision a virtual network and subnet

az network vnet create -g PythonSDKExample-VM-rg -n python-example-vnet \
    --address-prefix 10.0.0.0/16 --subnet-name python-example-subnet \
    --subnet-prefix 10.0.0.0/24

# Provision a public IP address

az network public-ip create -g PythonSDKExample-VM-rg -n python-example-ip \
    --allocation-method Dynamic --version IPv4

# Provision a network interface client

az network nic create -g PythonSDKExample-VM-rg --vnet-name python-example-vnet \
    --subnet python-example-subnet -n python-example-nic \
    --public-ip-address python-example-ip

# Provision the virtual machine

az vm create -g PythonSDKExample-VM-rg -n ExampleVM -l "centralus" \
    --nics python-example-nic --image UbuntuLTS \
    --admin-username azureuser --admin-password ChangePa$$w0rd24

```

# <a name="cmd"></a>[Cmd](#tab/cmd)

```azurecli
# Provision the resource group

az group create -n PythonSDKExample-VM-rg -l centralus

# Provision a virtual network and subnet

az network vnet create -g PythonSDKExample-VM-rg -n python-example-vnet ^
    --address-prefix 10.0.0.0/16 --subnet-name python-example-subnet ^
    --subnet-prefix 10.0.0.0/24

# Provision a public IP address

az network public-ip create -g PythonSDKExample-VM-rg -n python-example-ip ^
    --allocation-method Dynamic --version IPv4

# Provision a network interface client

az network nic create -g PythonSDKExample-VM-rg --vnet-name python-example-vnet ^
    --subnet python-example-subnet -n python-example-nic ^
    --public-ip-address python-example-ip

# Provision the virtual machine

az vm create -g PythonSDKExample-VM-rg -n ExampleVM -l "centralus" ^
    --nics python-example-nic --image UbuntuLTS ^
    --admin-username azureuser --admin-password ChangePa$$w0rd24
```

---

## <a name="6-clean-up-resources"></a>6: Pulire le risorse

```azurecli
az group delete -n PythonSDKExample-VM-rg
```

Eseguire questo comando se non è necessario mantenere le risorse create in questo esempio per evitare addebiti ricorrenti nella sottoscrizione.

## <a name="see-also"></a>Vedere anche

Il contenitore di risorse seguente illustra esempi più completi che usano Python per creare una macchina virtuale:

- [Creare e gestire macchine virtuali Windows in Azure usando Python](/azure/virtual-machines/windows/python). È possibile usare questo esempio per creare VM Linux cambiando il parametro `storage_profile`.
- [Esempi di gestione delle macchine virtuali di Azure - Python](https://github.com/Azure-Samples/virtual-machines-python-manage) (GitHub). Questo esempio illustra altre operazioni di gestione, come l'avvio e il riavvio di una VM, l'arresto e l'eliminazione di una VM, l'aumento delle dimensioni del disco e la gestione di dischi dati.