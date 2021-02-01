---
title: Esercitazione - Configurare i set di scalabilità di macchine virtuali in Azure con Ansible
description: Informazioni su come usare Ansible per creare e configurare set di scalabilità di macchine virtuali in Azure
keywords: ansible, azure, devops, bash, playbook, macchina virtuale, set di scalabilità di macchine virtuali, vmss
ms.topic: tutorial
ms.date: 04/30/2019
ms.custom: devx-track-ansible
ms.openlocfilehash: 88b8c32f4eb1a7422cc6aa55e8a2520a01476656
ms.sourcegitcommit: 3f8aa923e4626b31cc533584fe3b66940d384351
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/01/2021
ms.locfileid: "99224715"
---
# <a name="tutorial-configure-virtual-machine-scale-sets-in-azure-using-ansible"></a>Esercitazione: Configurare set di scalabilità di macchine virtuali in Azure tramite Ansible

[!INCLUDE [ansible-29-note.md](includes/ansible-29-note.md)]

[!INCLUDE [open-source-devops-intro-vm-scale-set.md](../includes/open-source-devops-intro-vm-scale-set.md)]

[!INCLUDE [ansible-tutorial-goals.md](includes/ansible-tutorial-goals.md)]

> [!div class="checklist"]
>
> * Configurare le risorse per una macchina virtuale
> * Configurare un set di scalabilità
> * Ridimensionare il set di scalabilità aumentando le relative istanze di macchina virtuale 

## <a name="prerequisites"></a>Prerequisiti

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../includes/open-source-devops-prereqs-azure-subscription.md)]
[!INCLUDE [ansible-prereqs-cloudshell-use-or-vm-creation2.md](includes/ansible-prereqs-cloudshell-use-or-vm-creation2.md)]

## <a name="configure-a-scale-set"></a>Configurare un set di scalabilità

Il codice del playbook in questa sezione definisce le risorse seguenti:

* **Gruppo di risorse** in cui verranno distribuite tutte le risorse.
* **Rete virtuale** nello spazio indirizzi 10.0.0.0/16
* **Subnet** all'interno della rete virtuale
* **Indirizzo IP pubblico** che consente di accedere alle risorse in Internet
* **Gruppo di sicurezza di rete** che controlla il flusso del traffico di rete in ingresso e in uscita dal set di scalabilità
* **Bilanciamento del carico** che distribuisce il traffico in un set di macchine virtuali definite usando regole di bilanciamento del carico
* **Set di scalabilità di macchine virtuali** che usa tutte le risorse create

È possibile ottenere il playbook di esempio in due modi:

* [Scaricare il playbook](https://github.com/Azure-Samples/ansible-playbooks/blob/master/vmss/vmss-create.yml) e salvarlo in `vmss-create.yml`.
* Creare un nuovo file denominato `vmss-create.yml` e copiarvi il contenuto seguente:

```yml
- hosts: localhost
  vars:
    resource_group: myResourceGroup
    vmss_lb_name: myScaleSetLb
    location: eastus
    admin_username: azureuser
    admin_password: "{{ admin_password }}"

  tasks:
    - name: Create a resource group
      azure_rm_resourcegroup:
        name: "{{ resource_group }}"
        location: "{{ location }}"
    - name: Create virtual network
      azure_rm_virtualnetwork:
        resource_group: "{{ resource_group }}"
        name: "{{ vmss_name }}"
        address_prefixes: "10.0.0.0/16"
    - name: Add subnet
      azure_rm_subnet:
        resource_group: "{{ resource_group }}"
        name: "{{ vmss_name }}"
        address_prefix: "10.0.1.0/24"
        virtual_network: "{{ vmss_name }}"
    - name: Create public IP address
      azure_rm_publicipaddress:
        resource_group: "{{ resource_group }}"
        allocation_method: Static
        name: "{{ vmss_name }}"
    - name: Create Network Security Group that allows SSH
      azure_rm_securitygroup:
        resource_group: "{{ resource_group }}"
        name: "{{ vmss_name }}"
        rules:
          - name: SSH
            protocol: Tcp
            destination_port_range: 22
            access: Allow
            priority: 1001
            direction: Inbound

    - name: Create a load balancer
      azure_rm_loadbalancer:
        resource_group: "{{ resource_group }}"
        name: "{{ vmss_name }}lb"
        location: "{{ location }}"
        frontend_ip_configurations:
          - name: "{{ vmss_name }}front-config"
            public_ip_address: "{{ vmss_name }}"
        backend_address_pools:
          - name: "{{ vmss_name }}backend-pool"
        probes:
          - name: "{{ vmss_name }}prob0"
            port: 8080
            interval: 10
            fail_count: 3
        inbound_nat_pools:
          - name: "{{ vmss_name }}nat-pool"
            frontend_ip_configuration_name: "{{ vmss_name }}front-config"
            protocol: Tcp
            frontend_port_range_start: 50000
            frontend_port_range_end: 50040
            backend_port: 22
        load_balancing_rules:
          - name: "{{ vmss_name }}lb-rules"
            frontend_ip_configuration: "{{ vmss_name }}front-config"
            backend_address_pool: "{{ vmss_name }}backend-pool"
            frontend_port: 80
            backend_port: 8080
            load_distribution: Default
            probe: "{{ vmss_name }}prob0"

    - name: Create VMSS
      no_log: true
      azure_rm_virtualmachinescaleset:
        resource_group: "{{ resource_group }}"
        name: "{{ vmss_name }}"
        vm_size: Standard_DS1_v2
        admin_username: "{{ admin_username }}"
        admin_password: "{{ admin_password }}"
        ssh_password_enabled: true
        capacity: 2
        virtual_network_name: "{{ vmss_name }}"
        subnet_name: "{{ vmss_name }}"
        upgrade_policy: Manual
        tier: Standard
        managed_disk_type: Standard_LRS
        os_disk_caching: ReadWrite
        image:
          offer: UbuntuServer
          publisher: Canonical
          sku: 16.04-LTS
          version: latest
        load_balancer: "{{ vmss_name }}lb"
        data_disks:
          - lun: 0
            disk_size_gb: 20
            managed_disk_type: Standard_LRS
            caching: ReadOnly
          - lun: 1
            disk_size_gb: 30
            managed_disk_type: Standard_LRS
            caching: ReadOnly

```

Prima di eseguire il playbook, vedere le note seguenti:

* Nella sezione `vars` sostituire il segnaposto `{{ admin_password }}` con la propria password.

Eseguire il playbook con [ansible-playbook](https://docs.ansible.com/ansible/latest/cli/ansible-playbook.html)

```bash
ansible-playbook vmss-create.yml
```

Dopo avere eseguito il playbook, viene visualizzato un output simile ai risultati seguenti:

```Output
PLAY [localhost] 

TASK [Gathering Facts] 
ok: [localhost]

TASK [Create a resource group] 
changed: [localhost]

TASK [Create virtual network] 
changed: [localhost]

TASK [Add subnet] 
changed: [localhost]

TASK [Create public IP address] 
changed: [localhost]

TASK [Create Network Security Group that allows SSH] 
changed: [localhost]

TASK [Create a load balancer] 
changed: [localhost]

TASK [Create Scale Set] 
changed: [localhost]

PLAY RECAP 
localhost                  : ok=8    changed=7    unreachable=0    failed=0

```

## <a name="view-the-number-of-vm-instances"></a>Visualizzare il numero di istanze di macchina virtuale

Il [set di scalabilità configurato](#configure-a-scale-set) include attualmente due istanze. Per confermare tale valore, seguire questa procedura:

1. Accedere al [portale di Azure](https://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Passare al set di scalabilità configurato.

1. Viene visualizzato il nome del set di scalabilità con il numero di istanze tra parentesi: `Standard_DS1_v2 (2 instances)`

1. È anche possibile verificare il numero di istanze con [Azure Cloud Shell](https://shell.azure.com/) eseguendo questo comando:

    ```azurecli-interactive
    az vmss show -n myScaleSet -g myResourceGroup --query '{"capacity":sku.capacity}' 
    ```

    I risultati dell'esecuzione del comando dell'interfaccia della riga di comando di Azure in Cloud Shell mostrano che esistono due istanze:

    ```bash
    {
      "capacity": 2,
    }
    ```

## <a name="scale-out-a-scale-set"></a>Aumentare il numero di istanze per un set di scalabilità

Il codice del playbook in questa sezione consente di recuperare informazioni sul set di scalabilità e di modificarne la capacità da due a tre.

È possibile ottenere il playbook di esempio in due modi:

* [Scaricare il playbook](https://github.com/Azure-Samples/ansible-playbooks/blob/master/vmss/vmss-scale-out.yml) e salvarlo in `vmss-scale-out.yml`.
* Creare un nuovo file denominato `vmss-scale-out.yml` e copiarvi il contenuto seguente:

```yml
---
- hosts: localhost
  gather_facts: false
  
  vars:
    resource_group: myTestRG
    vmss_name: myTestVMSS
  
  tasks:

    - name: Get scaleset info
      azure_rm_virtualmachine_scaleset_facts:
        resource_group: "{{ resource_group }}"
        name: "{{ vmss_name }}"
        format: curated
      register: output_scaleset

    - name: set image fact
      set_fact:
        vmss_image: "{{ output_scaleset.vmss[0].image }}"

    - name: Create VMSS
      no_log: true
      azure_rm_virtualmachinescaleset:
        resource_group: "{{ resource_group }}"
        name: "{{ vmss_name }}"
        capacity: 3
        image: "{{ vmss_image }}"
```

Eseguire il playbook con [ansible-playbook](https://docs.ansible.com/ansible/latest/cli/ansible-playbook.html)

```bash
ansible-playbook vmss-scale-out.yml
```

Dopo avere eseguito il playbook, viene visualizzato un output simile ai risultati seguenti:

```Output
PLAY [localhost] 

TASK [Gathering Facts] 
ok: [localhost]

TASK [Get scaleset info] 
ok: [localhost]

TASK [Set image fact] 
ok: [localhost]

TASK [Change VMSS capacity] 
changed: [localhost]

PLAY RECAP 
localhost                  : ok=3    changed=1    unreachable=0    failed=0
```

## <a name="verify-the-results"></a>Verificare i risultati

Verificare i risultati delle operazioni eseguite tramite il portale di Azure:

1. Accedere al [portale di Azure](https://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Passare al set di scalabilità configurato.

1. Viene visualizzato il nome del set di scalabilità con il numero di istanze tra parentesi: `Standard_DS1_v2 (3 instances)`

1. È anche possibile verificare la modifica con [Azure Cloud Shell](https://shell.azure.com/) eseguendo questo comando:

    ```azurecli-interactive
    az vmss show -n myScaleSet -g myResourceGroup --query '{"capacity":sku.capacity}' 
    ```

    I risultati dell'esecuzione del comando dell'interfaccia della riga di comando di Azure in Cloud Shell indicano che ora sono disponibili tre istanze: 

    ```bash
    {
      "capacity": 3,
    }
    ```

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"]
> [Esercitazione: Distribuire app in set di scalabilità di macchine virtuali in Azure tramite Ansible](./vm-scale-set-deploy-app.md)
