---
title: 'Esercitazione: Usare Azure Key Vault con una macchina virtuale Linux in Ansible'
description: Informazioni su come usare Ansible per configurare la sicurezza delle VM con Azure Key Vault
keywords: ansible, azure, devops, key vault, sicurezza, credenziali, segreti, chiavi, certificati, moduli ansible per azure, gruppo di risorse, azure_rm_resourcegroup,
ms.topic: tutorial
ms.date: 04/20/2020
ms.custom: devx-track-ansible
ms.openlocfilehash: 4891b277f8c1f9fcd7fe4c1d54ed13b39f19d2e4
ms.sourcegitcommit: bfaeacc2fb68f861a9403585d744e51a8f99829c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/16/2020
ms.locfileid: "90682021"
---
# <a name="tutorial-use-azure-key-vault-with-a-linux-virtual-machine-in-ansible"></a>Esercitazione: Usare Azure Key Vault con una macchina virtuale Linux in Ansible

[!INCLUDE [ansible-29-note.md](includes/ansible-29-note.md)]

Questa esercitazione illustra come usare la raccolta Ansible per i moduli di Azure con [Azure Key Vault](/azure/key-vault/general/overview). Azure Key Vault consente di centralizzare l'archiviazione delle credenziali, ad esempio i segreti, le chiavi e i certificati delle applicazioni. Il disaccoppiamento delle credenziali e del codice dell'applicazione consente di migliorare la sicurezza del sistema. Inoltre, l'implementazione di un modello di gestione delle credenziali a rotazione con date di scadenza automatiche diventa molto più gestibile.

> [!div class="checklist"]
>
> * Usare l'interfaccia della riga di comando di Azure per ottenere i valori della sottoscrizione di Azure e delle entità servizio
> * Archiviare i valori delle chiavi come variabili di ambiente Linux
> * Ottenere le variabili di ambiente Linux da un playbook Ansible
> * Creare un insieme di credenziali delle chiavi
> * Impostare un criterio di accesso per un insieme di credenziali delle chiavi
> * Usare il portale di Azure per aggiungere un criterio di accesso di un insieme di credenziali delle chiavi
> * Crea un segreto dell'insieme di credenziali delle chiavi
> * Usare il modulo della shell Ansible per ottenere un segreto dell'insieme di credenziali delle chiavi
> * Creare una macchina virtuale insieme a tutti i relativi componenti costitutivi

## <a name="prerequisites"></a>Prerequisiti

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../includes/open-source-devops-prereqs-azure-subscription.md)]
[!INCLUDE [ansible-prereqs-cloudshell-use-or-vm-creation2.md](includes/ansible-prereqs-cloudshell-use-or-vm-creation2.md)]
[!INCLUDE [ansible-configure-azure-collection.md](includes/ansible-configure-azure-collection.md)]
    
## <a name="get-azure-subscription-info"></a>Ottenere informazioni sulla sottoscrizione di Azure

Usare l'interfaccia della riga di comando di Azure per ottenere le informazioni sulla sottoscrizione di Azure necessarie quando si usano i moduli Ansible per Azure. 

1. Ottenere l'ID sottoscrizione di Azure e l'ID tenant della sottoscrizione di Azure usando il comando `az account show`. Per il segnaposto `<Subscription>` specificare il nome o l'ID della sottoscrizione di Azure. Il comando visualizzerà molti valori delle chiavi associati alla sottoscrizione di Azure predefinita. Se si hanno più sottoscrizioni, potrebbe essere necessario impostare la sottoscrizione corrente tramite il comando [az account set](/cli/azure/account#az-account-set). Nell'output del comando prendere nota dei valori di **ID** e **tenantID**.

    ```azurecli
    az account show --subscription "<Subscription>" --query tenantId
    ```

1. Se non si ha un'entità servizio di Azure per la sottoscrizione di Azure, [crearne una con l'interfaccia della riga di comando di Azure](/cli/azure/create-an-azure-service-principal-azure-cli). Nell'output del comando prendere nota del valore di **appId**.

1. Ottenere l'ID oggetto dell'entità servizio ID usando il comando `az ad sp show`. Per il segnaposto `<ApplicationID>` specificare il valore di appId dell'entità servizio. Il parametro `--query` indica il valore da stampare in *stdout*. In questo caso, si tratta dell'ID oggetto entità servizio.

    ```azurecli
    az ad sp show --id <ApplicationID> --query objectId
    ```
    
1. Archiviare i valori seguenti come variabili di ambiente: ID sottoscrizione di Azure, ID tenant della sottoscrizione e ID oggetto entità servizio. Il modo in cui viene eseguito il playbook, la posizione in cui archiviare i valori di sottoscrizione e credenziali e il modo in cui vengono recuperati questi valori dipendono dall'ambiente specifico in uso. Ai fini di questa demo, è stato usato il servizio Azure Cloud Shell e i valori di Azure necessari sono stati archiviati in `~/.bashrc` aggiungendo le righe seguenti alla fine del file:

    ```bash
    export AZ_SUBSCRIPTION_ID="<subscriptionID>"
    export AZ_TENANT_ID="<tenantID>"
    export AZ_OBJECT_ID="<objectID>"
    export AZ_CLIENT_ID="<appID>"
    ```

## <a name="declare-the-azure-collection"></a>Dichiarare la raccolta di Azure

Dopo aver [scaricato la raccolta di Azure più recente](#prerequisites), specificarne il relativo uso tramite la chiave `collections`.

```yml
- hosts: all
  collections:
    - azure.azcollection
```

## <a name="create-azure-resource-group-for-the-key-vault"></a>Creare un gruppo di risorse di Azure per l'insieme di credenziali delle chiavi

Il frammento di playbook seguente crea un gruppo di risorse con un nome univoco in cui verrà creato l'insieme di credenziali delle chiavi. 

```yml
---
- hosts: localhost
  tasks:
    - name: Prepare random postfix
      set_fact:
        rpfx: "{{ 10000 | random }}"
      run_once: yes
      
- hosts: localhost
  vars:
    kv_rg: kv_rg_{{ rpfx }}
    kv_rg_loc: eastus

  tasks:
    - name: Set facts
      set_fact:
        az_sub_id: "{{ lookup('env', 'AZ_SUBSCRIPTION_ID') }}"

    - name: Create a resource group to hold the key vault
      azure_rm_resourcegroup:
        subscription_id: "{{ az_sub_id }}"
        name: "{{ kv_rg }}"
        location: "{{ kv_rg_loc }}"

    - debug:
        msg: "New resource group = {{ kv_rg }}"
```

**Note:**

- In questa demo l'insieme di credenziali delle chiavi viene creato come unica risorsa di un gruppo di risorse. È prassi comune separare l'insieme di credenziali delle chiavi dalle risorse che lo usano. Questo modello consente di evitare l'eliminazione accidentale dell'insieme di credenziali delle chiavi quando si eliminano altre risorse.
- Poiché il nome dell'insieme di credenziali delle chiavi deve essere univoco in Azure, la demo crea un valore di *suffisso* casuale. Questo valore viene aggiunto al nome del gruppo di risorse dell'insieme di credenziali delle chiavi e all'insieme di credenziali delle chiavi creato nella sezione successiva. Il codice nell'attività `Prepare random postfix` genera il valore di suffisso casuale assegnato alla variabile `rpfx`.
- Nell'attività `Set facts` viene usato il comando `lookup` per recuperare l'ID sottoscrizione di Azure archiviato come variabile di ambiente.
- Per creare il nuovo gruppo di risorse, viene usato il [modulo azure_rm_resourcegroup](https://docs.ansible.com/ansible/latest/modules/azure_rm_resourcegroup_module.html).
- Un'attività `debug` alla fine del playbook visualizza il nome del nuovo gruppo di risorse.

## <a name="create-a-key-vault"></a>Creare un insieme di credenziali delle chiavi

Come indicato nella sezione precedente, il nome dell'insieme di credenziali delle chiavi deve essere univoco in Azure. Di conseguenza, il frammento di playbook seguente assegna alla variabile `kv` la concatenazione del valore letterale `kv` e `rpfx`.

```yml
vars:
  kv: "kv{{ rpfx }}"
  kv_rg: "kv_rg_{{ rpfx }}"

tasks:
  - name: Set facts
    set_fact:
      az_object_id: "{{ lookup('env', 'AZ_OBJECT_ID') }}"
      az_tenant_id: "{{ lookup('env', 'AZ_TENANT_ID') }}"

  - name: Create a key vault
    azure_rm_keyvault:
      subscription_id: "{{ az_sub_id }}"
      resource_group: "{{ kv_rg }}"
      vault_name: "{{ kv }}"
      vault_tenant: "{{ az_tenant_id }}"
      enabled_for_deployment: yes
      sku:
        name: standard
        family: A
      access_policies:
        - object_id: "{{ az_object_id }}"
          tenant_id: "{{ az_tenant_id }}"
          secrets:
            - get
            - list
            - set
  - debug:
      msg: "New key vault name = {{ kv }} within the {{ kv_rg }} resource group"

```

**Note:**

- Per creare l'insieme di credenziali delle chiavi viene usato il [modulo azure_rm_keyvault](https://docs.ansible.com/ansible/latest/modules/azure_rm_keyvault_module.html).
- Quando si creano i criteri di accesso come parte dell'insieme di credenziali delle chiavi, vengono specificati un ID oggetto e un ID tenant. Questi valori definiscono i criteri di accesso per un'entità servizio specifica, associata a una sottoscrizione di Azure. Tuttavia, se si passa al portale di Azure e si prova a visualizzare i segreti dell'insieme di credenziali delle chiavi, è possibile che vengano generati messaggi di errore. Questi messaggi potrebbero essere analoghi a "L'operazione "Elenca"non è abilitata nel criterio di accesso dell'insieme di credenziali delle chiavi" e "Non si è autorizzati a visualizzare questi contenuti". La ricezione di questi messaggi indica che, essendo un utente di Active Directory, non si ha accesso. La sezione successiva illustra come aggiungere un criterio di accesso per se stessi all'insieme di credenziali delle chiavi.
- Un'attività `debug` alla fine del playbook visualizza il nome del nuovo insieme di credenziali delle chiavi.

## <a name="add-yourself-to-key-vault-access-policy"></a>Aggiungere se stessi al criterio di accesso dell'insieme di credenziali delle chiavi

Se si vogliono visualizzare i segreti dell'insieme di credenziali delle chiavi o se si seguono i passaggi della demo, è necessario aggiungere un criterio di accesso per l'ID di Azure Active Directory. La procedura seguente illustra come aggiungere un criterio di accesso all'insieme di credenziali delle chiavi per se stessi come utente:

1. Accedere al [portale di Azure](https://portal.azure.com).

1. Nella casella di ricerca principale della pagina immettere `key vaults`, quindi in **Servizi**selezionare **Insiemi di credenziali delle chiavi**.

1. Selezionare l'insieme di credenziali delle chiavi creato nella sezione precedente. Il nome è stato stampato in stdout dal playbook.

1. Selezionare **Criteri di accesso**.

1. Viene visualizzato un singolo criterio di accesso con l'ID di Azure Active Directory che rappresenta l'entità servizio specificata.

1. Selezionare **Aggiungi criterio di accesso**.

1. Fare clic su **Selezionare un'entità**.

1. Nella casella di ricerca della scheda **Entità** immettere il proprio indirizzo di posta elettronica.

1. Nell'elenco filtrato selezionare l'entità appropriata.

1. Le informazioni per l'utente selezionato vengono copiate nell'elenco **Membri selezionati**. Scegliere **Seleziona**.

1. Selezionare le opzioni appropriate per **Autorizzazioni chiave**, **Autorizzazioni segrete** e **Autorizzazioni del certificato**. Per questa demo, è sufficiente selezionare **Autorizzazioni segrete**, quindi selezionare **Ottieni**, **Elenca** e **Imposta**.

1. Selezionare **Aggiungi**.

1. Nella pagina **Criteri di accesso** viene ora visualizzato il nuovo criterio di accesso per l'utente selezionato.

1. Selezionare **Salva**.

1. Selezionare **Notifiche** nell'angolo in alto a destra del portale. Attendere che il criterio di accesso venga aggiornato prima di continuare con il passaggio successivo.

## <a name="create-a-key-vault-secret"></a>Crea un segreto dell'insieme di credenziali delle chiavi

Il frammento di playbook Ansible seguente illustra come creare un segreto dell'insieme di credenziali delle chiavi:

```yml
  vars:
    kv_secret_name: testsecret
    kv_secret_value: MySecret007$

  tasks:
    - name: Set facts
      set_fact:
        az_client_id: "{{ lookup('env', 'AZ_CLIENT_ID') }}"

    - name: Create a secret
      azure_rm_keyvaultsecret:
        subscription_id: "{{ az_sub_id }}"
        client_id: "{{ az_client_id }}"
        keyvault_uri: "{{ kv_uri }}"
        secret_name: "{{ kv_secret_name }}"
        secret_value: "{{ kv_secret_value }}"
```

**Note:**

- Per creare il segreto dell'insieme di credenziali delle chiavi viene usato il [modulo azure_rm_keyvaultsecret](https://docs.ansible.com/ansible/latest/modules/azure_rm_keyvaultsecret_module.html).
- Per semplicità, la demo include `secret_name` e `secret_value`. Tuttavia, i playbook sono file di infrastruttura distribuita come codice (IaC) come qualsiasi codice sorgente per il progetto. Di conseguenza, i valori come questi non devono essere archiviati in file di testo non crittografato se usati in ambienti di produzione.
- Dopo aver eseguito questo codice, la scheda **Segreti** per l'insieme di credenziali delle chiavi include il segreto appena aggiunto denominato `testsecret`. Per visualizzarlo, selezionare il segreto, selezionare la versione corrente e selezionare **Mostra il valore segreto**.

## <a name="get-a-key-vault-secret"></a>Ottenere un segreto dell'insieme di credenziali delle chiavi

Il frammento di playbook Ansible seguente illustra come ottenere l'ultima versione di un segreto dell'insieme di credenziali delle chiavi:

```yml
  vars:
    kv_secret_name: testsecret
    kv_secret_value: MySecret007$

tasks:
    - name: Get latest version of a secret
      azure_rm_keyvaultsecret_info:
        vault_uri: "{{ kv_uri }}"
        name: "{{ kv_secret_name }}"
      register: output
    - debug:
        var: output['secrets'][0]['secret']
```

**Note:**

- Per ottenere il segreto dell'insieme di credenziali delle chiavi viene usato il **modulo azure_rm_keyvaultsecret_info**. Questo modulo è disponibile solo se si usa la raccolta Ansible per i moduli di Azure. 
- Se si riceve un errore durante l'esecuzione di questo frammento, assicurarsi di aver seguito tutte le istruzioni indicate nella sezione [Prerequisiti](#prerequisites).
- Per semplicità, la demo include `secret_name` e `secret_value`. Tuttavia, i playbook sono file di infrastruttura distribuita come codice (IaC) come qualsiasi codice sorgente per il progetto. Di conseguenza, i valori come questi non devono essere archiviati in file di testo non crittografato se usati in ambienti di produzione.

## <a name="run-the-complete-playbook"></a>Eseguire il playbook completo

Dopo aver creato l'insieme di credenziali delle chiavi e il segreto, è possibile usare tali informazioni per proteggere le risorse di Azure, ad esempio le macchine virtuali. Il playbook Ansible seguente esegue le attività illustrate in questa esercitazione e crea una macchina virtuale completa. 

```yml
---
- hosts: localhost
  tasks:
    - name: Prepare random postfix
      set_fact:
        rpfx: "{{ 10000 | random }}"
      run_once: yes
      
- hosts: localhost
  collections:
    - azure.azcollection
  vars:
    kv_rg: kv_rg_{{ rpfx }}
    kv_rg_loc: eastus
    kv: "kv{{ rpfx }}"
    kv_uri: "https://{{ kv }}.vault.azure.net"
    kv_secret_name: testsecret
    kv_secret_value: MySecret007$

    # Test VM vars
    test_vm_rg: kv_test_vm_rg
    test_vm_rg_loc: eastus
    test_vm: kvtestvm
    test_vm_vnet: "kv_test_vm_vnet"
    test_vm_subnet: kv_test_vm_subnet
    test_vm_public_ip: kv_test_vm_public_ip
    test_vm_nsg: kv_test_vm_nsg
    test_vm_nsg_list: 
      - name: Allow-SSH
        access: Allow
        protocol: Tcp
        direction: Inbound
        priority: 300
        port: 22 
        source_address_prefix: Internet
      - name: Allow-HTTP
        access: Allow
        protocol: Tcp
        direction: Inbound
        priority: 100
        port: 80
        source_address_prefix: Internet 
    test_vm_nic: kv_test_vnic
    admin_username: testadmin

  tasks:
    - name: Set facts
      set_fact:
        az_sub_id: "{{ lookup('env', 'AZ_SUBSCRIPTION_ID') }}"
        az_object_id: "{{ lookup('env', 'AZ_OBJECT_ID') }}"
        az_tenant_id: "{{ lookup('env', 'AZ_TENANT_ID') }}"
        az_client_id: "{{ lookup('env', 'AZ_CLIENT_ID') }}"

    - name: Create a resource group to hold the Key Vault instance
      azure_rm_resourcegroup:
        subscription_id: "{{ az_sub_id }}"
        name: "{{ kv_rg }}"
        location: "{{ kv_rg_loc }}"

    - debug:
        msg: "New resource group = {{ kv_rg }}"

    - name: Create instance of Key Vault
      azure_rm_keyvault:
        subscription_id: "{{ az_sub_id }}"
        resource_group: "{{ kv_rg }}"
        vault_name: "{{ kv }}"
        vault_tenant: "{{ az_tenant_id }}"
        enabled_for_deployment: yes
        sku:
          name: standard
          family: A
        access_policies:
          - object_id: "{{ az_object_id }}"
            tenant_id: "{{ az_tenant_id }}"
            secrets:
              - get
              - list
              - set

    - debug:
        msg: "New Key Vault instance name = {{ kv }} within the {{ kv_rg }} resource group"

    - name: Create a secret
      azure_rm_keyvaultsecret:
        subscription_id: "{{ az_sub_id }}"
        client_id: "{{ az_client_id }}"
        keyvault_uri: "{{ kv_uri }}"
        secret_name: "{{ kv_secret_name }}"
        secret_value: "{{ kv_secret_value }}"

    - name: Register Key Vault provider.
      shell:
        az provider register -n Microsoft.KeyVault

    - name: Get latest version of a secret
      azure_rm_keyvaultsecret_info:
        vault_uri: "{{ kv_uri }}"
        name: "{{ kv_secret_name }}"
      register: output
    - debug:
        var: output['secrets'][0]['secret']

    - name: Create resource group for test VM.
      azure_rm_resourcegroup:
        subscription_id: "{{ az_sub_id }}"
        name: "{{ test_vm_rg }}"
        location: "{{ test_vm_rg_loc }}"

    - name: Create virtual network.
      azure_rm_virtualnetwork:
        subscription_id: "{{ az_sub_id }}"
        resource_group: "{{ test_vm_rg }}"
        name: "{{ test_vm_vnet }}"
        address_prefixes: "172.16.0.0/16"

    - name: Create subset within virtual network.
      azure_rm_subnet:
        subscription_id: "{{ az_sub_id }}"
        resource_group: "{{ test_vm_rg }}"
        virtual_network_name: "{{ test_vm_vnet }}"
        name: "{{ test_vm_subnet }}"
        address_prefix_cidr:  "172.16.10.0/24"

    - name: Create public IP address.
      azure_rm_publicipaddress:
        subscription_id: "{{ az_sub_id }}"
        resource_group: "{{ test_vm_rg }}"
        allocation_method: Static
        name: "{{ test_vm_public_ip }}"

    - name: Create Network Security Group and rules.
      azure_rm_securitygroup:
        subscription_id: "{{ az_sub_id }}"
        resource_group: "{{ test_vm_rg }}"
        name: "{{ test_vm_nsg}}"
        rules:
          - name: "{{ item.name }}"
            access: "{{ item.access }}"
            protocol: "{{ item.protocol }}"
            direction: "{{ item.direction }}"
            destination_port_range: "{{ item.port }}"
            priority: "{{ item.priority }}"
            source_address_prefix: "{{ item.source_address_prefix }}"
      loop: "{{ test_vm_nsg_list }}"

    - name: Create virtual network interface card (NIC).
      azure_rm_networkinterface:
        subscription_id: "{{ az_sub_id }}"
        resource_group: "{{ test_vm_rg }}"
        name: "{{ test_vm_nic }}"
        virtual_network: "{{ test_vm_vnet }}"
        subnet: "{{ test_vm_subnet }}"
        ip_configurations:
          - name: ipconfig
            public_ip_address_name: "{{ test_vm_public_ip }}"
            primary: yes
        security_group: "{{ test_vm_nsg }}"

    - name: Create virtual machine.
      azure_rm_virtualmachine:
        subscription_id: "{{ az_sub_id }}"
        resource_group: "{{ test_vm_rg }}"
        name: "{{ test_vm }}"
        admin_username: " {{ admin_username }} "
        admin_password: " {{ output['secrets'][0]['secret'] }}"
        vm_size: Standard_B1ms
        network_interfaces: "{{ test_vm_nic }}"
        image:
          offer: UbuntuServer
          publisher: Canonical
          sku: 16.04-LTS
          version: latest

```

**Note:**

- La password di *azure_rm_keyvaultsecret_info* per la macchina virtuale è impostata sul segreto dell'insieme di credenziali delle chiavi.
- La possibilità di eseguire l'intero playbook in una sola volta dipende dall'ambiente di test. Potrebbe essere necessario aggiungere manualmente se stessi nel criterio di accesso dell'insieme di credenziali delle chiavi prima di creare la chiave. Questa attività è illustrata nelle sezioni [Creare un insieme di credenziali delle chiavi](#create-a-key-vault) e [Aggiungere se stessi al criterio di accesso dell'insieme di credenziali delle chiavi](#add-yourself-to-key-vault-access-policy).
- Come si può notare, vengono usati molti moduli Ansible diversi per creare una macchina virtuale di Azure e tutti i relativi componenti costitutivi. Per altre informazioni sui vari moduli Ansible usati per creare una macchina virtuale, vedere l'elenco seguente:
    - [Modulo per i gruppi di risorse di Azure (azure_rm_resourcegroup)](https://docs.ansible.com/ansible/latest/modules/azure_rm_resourcegroup_module.html)
    - [Modulo per la rete virtuale di Azure (azure_rm_virtualnetwork)](https://docs.ansible.com/ansible/latest/modules/azure_rm_virtualnetwork_module.html)
    - [Modulo per la subnet di rete virtuale di Azure (azure_rm_subnet)](https://docs.ansible.com/ansible/latest/modules/azure_rm_subnet_module.html)
    - [Modulo per l'IP pubblico di Azure (azure_rm_publicipaddress)](https://docs.ansible.com/ansible/latest/modules/azure_rm_publicipaddress_module.html)
    - [Modulo per il gruppo di sicurezza di rete di Azure (azure_rm_securitygroup)](https://docs.ansible.com/ansible/latest/modules/azure_rm_securitygroup_module.html)
    - [Interfaccia di rete di Azure (azure_rm_networkinterface)](https://docs.ansible.com/ansible/latest/modules/azure_rm_networkinterface_module.html)
    - [Macchina virtuale di Azure (azure_rm_virtualmachine)](https://docs.ansible.com/ansible/latest/modules/azure_rm_virtualmachine_module.html)
    
## <a name="clean-up-resources"></a>Pulire le risorse

Quando non sono più necessarie, eliminare le risorse create in questo articolo. Sostituire il segnaposto `<kv_rg>` con il gruppo di risorse usato per memorizzare l'insieme di credenziali delle chiavi della demo. 

```yml  
- hosts: localhost  
  vars: 
    kv_rg: <kv_rg>  
    test_vm_rg: kv_test_vm_rg   
  tasks:    
    - name: Delete the key vault resource group 
      azure_rm_resourcegroup:   
        name: "{{ kv_rg }}" 
        force_delete_nonempty: yes  
        state: absent   
    - name: Delete the test vm resource group   
      azure_rm_resourcegroup:   
        name: "{{ test_vm_rg }}"    
        force_delete_nonempty: yes  
        state: absent
```

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"] 
> [Panoramica della sicurezza di Azure Key Vault](/azure/key-vault/general/overview-security)
