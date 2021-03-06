---
title: Esercitazione - Configurare inventari dinamici delle risorse di Azure con Ansible
description: Informazioni su come usare Ansible per gestire gli inventari dinamici di Azure
keywords: ansible, azure, devops, bash, cloud shell, inventario dinamico
ms.topic: tutorial
ms.date: 10/30/2020
ms.custom: devx-track-ansible, devx-track-azurecli
ms.openlocfilehash: ff23b6d4d363e8b83e33414c6518560fa82b8ee0
ms.sourcegitcommit: b380f6e637b47e6e3822b364136853e1d342d5cd
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/14/2021
ms.locfileid: "100395266"
---
# <a name="tutorial-configure-dynamic-inventories-of-your-azure-resources-using-ansible"></a>Esercitazione: Configurare gli inventari dinamici delle risorse di Azure tramite Ansible

[!INCLUDE [ansible-28-note.md](includes/ansible-28-note.md)]

Ansible può essere usato per eseguire il pull delle informazioni degli inventari da diverse origini (incluse origini cloud, ad esempio Azure) in un *inventario dinamico*. 

[!INCLUDE [ansible-tutorial-goals.md](includes/ansible-tutorial-goals.md)]

> [!div class="checklist"]
>
> * Configurare due macchine virtuali di test.
> * Contrassegnare una delle macchine virtuali
> * Installare Nginx nelle macchine virtuali contrassegnate
> * Configurare un inventario dinamico che include le risorse di Azure configurate

## <a name="prerequisites"></a>Prerequisiti

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../includes/open-source-devops-prereqs-azure-subscription.md)]
[!INCLUDE [open-source-devops-prereqs-create-service-principal.md](../includes/open-source-devops-prereqs-create-service-principal.md)]
[!INCLUDE [ansible-prereqs-cloudshell-use-or-vm-creation2.md](includes/ansible-prereqs-cloudshell-use-or-vm-creation2.md)]

## <a name="create-the-test-vms"></a>Creare le macchine virtuali di test

1. Accedere al [portale di Azure](https://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Aprire [Cloud Shell](/azure/cloud-shell/overview).

1. Creare un gruppo di risorse di Azure in cui includere le macchine virtuali per questa esercitazione.

    > [!IMPORTANT]
    > Il nome del gruppo di risorse di Azure creato in questo passaggio deve essere interamente in lettere minuscole. In caso contrario, la generazione dell'inventario dinamico avrà esito negativo.

    ```azurecli
    az group create --resource-group ansible-inventory-test-rg --location eastus
    ```

1. Creare due macchine virtuali Linux in Azure con una delle tecniche seguenti:

    - **Playbook di Ansible**: l'articolo [Creare una macchina virtuale di base in Azure con Ansible](./vm-configure.md) illustra come creare una macchina virtuale da un playbook di Ansible. Se si usa un playbook per definire una o entrambe le macchine virtuali, verificare che venga usata la connessione SSH invece di una password.

    - **Interfaccia della riga di comando di Azure**: eseguire ognuno dei comandi seguenti in Cloud Shell per creare le due macchine virtuali:

        ```azurecli
        az vm create --resource-group ansible-inventory-test-rg \
                     --name ansible-inventory-test-vm1 \
                     --image UbuntuLTS --generate-ssh-keys
        ```

        ```azurecli
        az vm create --resource-group ansible-inventory-test-rg \
                     --name ansible-inventory-test-vm2 \
                     --image UbuntuLTS --generate-ssh-keys
        ```

## <a name="tag-a-vm"></a>Assegnare un tag a una VM

È possibile [usare i tag per organizzare le risorse di Azure](/azure/azure-resource-manager/resource-group-using-tags#azure-cli) in base a categorie definite dall'utente.

Immettere il comando [az resource tag](/cli/azure/resource#az-resource-tag) seguente per contrassegnare la macchina virtuale `ansible-inventory-test-vm1` con la chiave `Ansible=nginx`:

```azurecli
az resource tag --tags Ansible=nginx --id /subscriptions/<YourAzureSubscriptionID>/resourceGroups/ansible-inventory-test-rg/providers/Microsoft.Compute/virtualMachines/ansible-inventory-test-vm1
```

Sostituire `<YourAzureSubscriptionID>` con SubscriptionId.

## <a name="generate-a-dynamic-inventory"></a>Generare un inventario dinamico

Dopo avere definito (e contrassegnato) le macchine virtuali, è necessario generare l'inventario dinamico.

Ansible fornisce un [plug-in di inventario dinamico di Azure](https://github.com/ansible/ansible/blob/stable-2.9/lib/ansible/plugins/inventory/azure_rm.py). La procedura seguente illustra come usare il plug-in:

1. L'inventario dinamico deve terminare con `azure_rm` un'estensione di `yml` o `yaml` altrimenti Ansible non rileverà il plug-in di inventario appropriato. Per l'esempio in questa esercitazione, salvare il playbook seguente come `myazure_rm.yml`:

    ```yml
        plugin: azure_rm
        include_vm_resource_groups:
        - ansible-inventory-test-rg
        auth_source: auto
    
        keyed_groups:
        - prefix: tag
          key: tags
    ```

1. Eseguire il comando seguente per eseguire il ping delle macchine virtuali nel gruppo di risorse:

    ```bash
    ansible all -m ping -i ./myazure_rm.yml
    ```

1. Per impostazione predefinita, il controllo della chiave host è abilitato, che può causare l'errore seguente.

    ```output
    Failed to connect to the host via ssh: Host key verification failed.
    ```

    Disabilitare la verifica della chiave host impostando la `ANSIBLE_HOST_KEY_CHECKING` variabile di ambiente su `False` .

    ```bash
    export ANSIBLE_HOST_KEY_CHECKING=False
    ```

1. Quando si esegue il playbook viene visualizzato un output simile al seguente:
  
    ```output
    ansible-inventory-test-vm1_0324 : ok=1    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
    ansible-inventory-test-vm2_8971 : ok=1    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
    ```

## <a name="enable-the-vm-tag"></a>Abilitare il tag della macchina virtuale

- Eseguire il comando `ansible-inventory -i myazure_rm.yml --graph` per ottenere l'output seguente:

    ```output
        @all:
          |--@tag_Ansible_nginx:
          |  |--ansible-inventory-test-vm1_9e2f
          |--@ungrouped:
          |  |--ansible-inventory-test-vm2_7ba9
    ```

- È anche possibile eseguire il comando seguente per testare la connessione alla macchina virtuale Nginx:
  
    ```bash
    ansible -i ./myazure_rm.yml -m ping tag_Ansible_nginx
    ```

## <a name="set-up-nginx-on-the-tagged-vm"></a>Configurare Nginx nella VM contrassegnata

Lo scopo dei tag è consentire un uso facile e rapido dei sottogruppi delle macchine virtuali. Si supponga, ad esempio, che sia necessario installare Nginx solo nelle macchine virtuali a cui è stato assegnato il tag `nginx`. La procedura seguente illustrata come questa operazione sia semplice:

1. Creare un file denominato `nginx.yml`:

   ```console
   code nginx.yml
   ```

1. Incollare il codice di esempio seguente nell'editor:

    ```yml
        ---
        - name: Install and start Nginx on an Azure virtual machine
          hosts: all
          become: yes
          tasks:
          - name: install nginx
            apt: pkg=nginx state=present
            notify:
            - start nginx
    
          handlers:
            - name: start nginx
              service: name=nginx state=started
    ```

1. Salvare il file e uscire dall'editor.

1. Eseguire il playbook con [ansible-playbook](https://docs.ansible.com/ansible/latest/cli/ansible-playbook.html)

     ```bash
     ansible-playbook  -i ./myazure_rm.yml  nginx.yml --limit=tag_Ansible_nginx
     ```

1. Dopo avere eseguito il playbook, viene visualizzato un output simile ai risultati seguenti:

    ```output
    PLAY [Install and start Nginx on an Azure virtual machine] 

    TASK [Gathering Facts] 
    ok: [ansible-inventory-test-vm1]

    TASK [install nginx] 
    changed: [ansible-inventory-test-vm1]

    RUNNING HANDLER [start nginx] 
    ok: [ansible-inventory-test-vm1]

    PLAY RECAP 
    ansible-inventory-test-vm1 : ok=3    changed=1    unreachable=0    failed=0
    ```

## <a name="test-nginx-installation"></a>Testare l'installazione di Nginx

Questa sezione illustra una tecnica per testare l'installazione di Nginx nella macchina virtuale.

1. Usare il comando [az vm list-ip-addresses](/cli/azure/vm#az-vm-list-ip-addresses) per recuperare l'indirizzo IP della macchina virtuale `ansible-inventory-test-vm1`. Il valore restituito (l'indirizzo IP della macchina virtuale) viene quindi usato come parametro per il comando SSH per la connessione alla macchina virtuale.

    ```azurecli
    ssh `az vm list-ip-addresses \
    -n ansible-inventory-test-vm1 \
    --query [0].virtualMachine.network.publicIpAddresses[0].ipAddress -o tsv`
    ```

1. Durante la connessione alla macchina virtuale `ansible-inventory-test-vm1`, eseguire il comando [nginx -v](https://nginx.org/en/docs/switches.html) per determinare se Nginx è installato.

    ```console
    nginx -v
    ```

1. Dopo avere eseguito il comando `nginx -v`, viene visualizzata la versione di Nginx (seconda riga) che indica che Nginx è installato.

    ```output
    tom@ansible-inventory-test-vm1:~$ nginx -v

    nginx version: nginx/1.10.3 (Ubuntu)

    tom@ansible-inventory-test-vm1:~$
    ```

1. Premere la combinazione di tasti `<Ctrl>D` per disconnettere la sessione SSH.

1. Eseguendo i passaggi precedenti per la macchina virtuale `ansible-inventory-test-vm2`, viene visualizzato un messaggio informativo che indica dove è possibile ottenere Nginx (il che implica che non è ancora installato):

    ```output
    tom@ansible-inventory-test-vm2:~$ nginx -v
    The program 'nginx' can be found in the following packages:
    * nginx-core
    * nginx-extras
    * nginx-full
    * nginx-lightTry: sudo apt install <selected package>
    tom@ansible-inventory-test-vm2:~$
    ```

## <a name="clean-up-resources"></a>Pulire le risorse

[!INCLUDE [ansible-delete-resource-group.md](includes/ansible-delete-resource-group.md)]

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"]
> [Avvio rapido: Configurare macchine virtuali Linux in Azure tramite Ansible](./vm-configure.md)
