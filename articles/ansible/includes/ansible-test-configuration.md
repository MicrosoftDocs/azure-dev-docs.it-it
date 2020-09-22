---
author: tomarchermsft
ms.service: ansible
ms.topic: include
ms.date: 09/15/2020
ms.author: tarcher
ms.openlocfilehash: 5351445b63618d072ecf4cf8beeffc9a071bd84d
ms.sourcegitcommit: bfaeacc2fb68f861a9403585d744e51a8f99829c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/16/2020
ms.locfileid: "90682035"
---
Questa sezione illustra come creare un gruppo di risorse di test all'interno della nuova configurazione di Ansible. Se non è necessario eseguire tale operazione, è possibile ignorare questa sezione.

### <a name="create-an-azure-resource-group"></a>Creare un gruppo di risorse di Azure

1. Salvare il codice seguente come `create_rg.yml`.

    ```yaml
    ---
    - hosts: localhost
      connection: local
      tasks:
        - name: Creating resource group - "{{ name }}"
          azure_rm_resourcegroup:
            name: "{{ name }}"
            location: "{{ location }}"
          register: rg
        - debug:
            var: rg
    ```

1. Eseguire il playbook con [ansible-playbook](https://docs.ansible.com/ansible/latest/cli/ansible-playbook.html). Sostituire i segnaposto con il nome e la posizione del gruppo di risorse da creare.

    ```bash
    ansible-playbook create_rg.yml --extra-vars "name=<resource_group_name> location=<resource_group_location>"
    ```

    **Note**:

    - A causa della variabile `register` e della sezione `debug` del playbook, i risultati vengono visualizzati al termine del comando.

### <a name="delete-an-azure-resource-group"></a>Eliminare un gruppo di risorse di Azure

[!INCLUDE [ansible-delete-resource-group.md](ansible-delete-resource-group.md)]
