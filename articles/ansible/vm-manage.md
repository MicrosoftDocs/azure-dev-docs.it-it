---
title: Avvio rapido - Gestire macchine virtuali Linux in Azure tramite Ansible
description: In questo articolo di avvio rapido viene illustrato come gestire una macchina virtuale Linux in Azure con Ansible
keywords: ansible, azure, devops, bash, cloudshell, playbook, bash
ms.topic: quickstart
ms.date: 04/30/2019
ms.custom: devx-track-ansible
ms.openlocfilehash: 896616cff3f2df237de625536981b08d5db9175b
ms.sourcegitcommit: bfaeacc2fb68f861a9403585d744e51a8f99829c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/16/2020
ms.locfileid: "90681925"
---
# <a name="quickstart-manage-linux-virtual-machines-in-azure-using-ansible"></a>Guida introduttiva: Gestire macchine virtuali Linux in Azure tramite Ansible

Ansible consente di automatizzare la distribuzione e la configurazione delle risorse nell'ambiente in uso. In questo articolo si usa un playbook Ansible per avviare e arrestare una macchina virtuale Linux. 

## <a name="prerequisites"></a>Prerequisiti

[!INCLUDE [open-source-devops-prereqs-azure-sub.md](../includes/open-source-devops-prereqs-azure-subscription.md)]
[!INCLUDE [ansible-prereqs-cloudshell-use-or-vm-creation2.md](includes/ansible-prereqs-cloudshell-use-or-vm-creation2.md)]

## <a name="stop-a-virtual-machine"></a>Arrestare una macchina virtuale

In questa sezione si usa Ansible per deallocare (arrestare) una macchina virtuale di Azure.

1. Accedere al [portale di Azure](https://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Aprire [Cloud Shell](/azure/cloud-shell/overview).

1. Creare un file denominato `azure-vm-stop.yml` e aprirlo nell'editor:

    ```bash
    code azure-vm-stop.yml
    ```

1. Incollare il codice di esempio seguente nell'editor:

    ```yaml
    - name: Stop Azure VM
      hosts: localhost
      connection: local
      tasks:
        - name: Stop virtual machine
          azure_rm_virtualmachine:
            resource_group: {{ resource_group_name }}
            name: {{ vm_name }}
            allocated: no
    ```

1. Sostituire i segnaposto `{{ resource_group_name }}` e `{{ vm_name }}` con i valori.

1. Salvare il file e uscire dall'editor.

1. Eseguire il playbook con [ansible-playbook](https://docs.ansible.com/ansible/latest/cli/ansible-playbook.html)

    ```bash
    ansible-playbook azure-vm-stop.yml
    ```

1. Dopo avere eseguito il playbook, viene visualizzato un output simile ai risultati seguenti:

    ```bash
    PLAY [Stop Azure VM] ********************************************************

    TASK [Gathering Facts] ******************************************************
    ok: [localhost]

    TASK [Deallocate the Virtual Machine] ***************************************
    changed: [localhost]

    PLAY RECAP ******************************************************************
    localhost                  : ok=2    changed=1    unreachable=0    failed=0
    ```

## <a name="start-a-virtual-machine"></a>Avviare una macchina virtuale

In questa sezione si usa Ansible per avviare una macchina virtuale di Azure deallocata (arrestata)

1. Accedere al [portale di Azure](https://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Aprire [Cloud Shell](/azure/cloud-shell/overview).

1. Creare un file denominato `azure-vm-start.yml` e aprirlo nell'editor:

    ```bash
    code azure-vm-start.yml
    ```

1. Incollare il codice di esempio seguente nell'editor:

    ```yaml
    - name: Start Azure VM
      hosts: localhost
      connection: local
      tasks:
        - name: Start virtual machine
          azure_rm_virtualmachine:
            resource_group: {{ resource_group_name }}
            name: {{ vm_name }}
    ```

1. Sostituire i segnaposto `{{ resource_group_name }}` e `{{ vm_name }}` con i valori.

1. Salvare il file e uscire dall'editor.

1. Eseguire il playbook con [ansible-playbook](https://docs.ansible.com/ansible/latest/cli/ansible-playbook.html)

    ```bash
    ansible-playbook azure-vm-start.yml
    ```

1. Dopo avere eseguito il playbook, viene visualizzato un output simile ai risultati seguenti:

    ```bash
    PLAY [Start Azure VM] ********************************************************

    TASK [Gathering Facts] ******************************************************
    ok: [localhost]

    TASK [Start the Virtual Machine] ********************************************
    changed: [localhost]

    PLAY RECAP ******************************************************************
    localhost                  : ok=2    changed=1    unreachable=0    failed=0
    ```

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"] 
> [Esercitazione: Gestire gli inventari dinamici di Azure con Ansible](./dynamic-inventory-configure.md)