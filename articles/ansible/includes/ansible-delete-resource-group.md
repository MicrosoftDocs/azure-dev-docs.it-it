---
author: tomarchermsft
ms.service: ansible
ms.topic: include
ms.date: 09/15/2020
ms.author: tarcher
ms.openlocfilehash: 885764615beed7a623db03499b4ff6a6ab705ba7
ms.sourcegitcommit: 39f3f69e3be39e30df28421a30747f6711c37a7b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/21/2020
ms.locfileid: "90831172"
---
#### <a name="ansible"></a>[Ansible](#tab/ansible)

1. Salvare il codice seguente come `delete_rg.yml`.

    ```yml
    ---
    - hosts: localhost
      tasks:
        - name: Deleting resource group - "{{ name }}"
          azure_rm_resourcegroup:
            name: "{{ name }}"
            state: absent
          register: rg
        - debug:
            var: rg
    ```

1. Eseguire il playbook usando il comando [ansible-playbook](https://docs.ansible.com/ansible/latest/user_guide/playbooks.html). Sostituire il segnaposto con il nome del gruppo di risorse da eliminare. Tutte le risorse presenti nel gruppo di risorse verranno eliminate.

    ```bash
    ansible-playbook delete_rg.yml --extra-vars "name=<resource_group>"
    ```

    **Note**:

    - A causa della variabile `register` e della sezione `debug` del playbook, i risultati vengono visualizzati al termine del comando.
    
#### <a name="azure-cli"></a>[Interfaccia della riga di comando di Azure](#tab/azure-cli)

1. Eseguire [az group delete](/cli/azure/group#az_group_delete) per eliminare il gruppo di risorse. Tutte le risorse presenti nel gruppo di risorse verranno eliminate.

    ```azurecli
    az group delete --name <resource_group>
    ```

1. Verificare che il gruppo di risorse sia stato eliminato usando [az group show](/cli/azure/group#az_group_show).

    ```azurecli
    az group show --name <resource_group>
    ```

#### <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

1. Eseguire [Remove-AzResourceGroup](/powershell/module/az.resources/Remove-AzResourceGroup) per eliminare il gruppo di risorse. Tutte le risorse presenti nel gruppo di risorse verranno eliminate.

    ```azurepowershell
    Remove-AzResourceGroup -Name <resource_group>
    ```

1. Verificare se il gruppo di risorse è stato eliminato usando [Get-AzResourceGroup](/powershell/module/az.resources/Get-AzResourceGroup).

    ```azurepowershell
    Get-AzResourceGroup -Name <resource_group>
    ```

---