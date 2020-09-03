---
title: Avvio rapido - Configurare Ansible con Azure Cloud Shell
description: In questo argomento di avvio rapido viene illustrato come eseguire diverse attività di Ansible con Bash in Azure Cloud Shell
keywords: ansible, azure, devops, bash, cloudshell, playbook, bash
ms.topic: quickstart
ms.date: 08/31/2020
ms.custom: devx-track-ansible
ms.openlocfilehash: 42d7b57d9890bce3a432fba3c6fdcf3080ddb63c
ms.sourcegitcommit: 2f98cf2a394d4fd82ddc917ac1041c1dc08473b6
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/01/2020
ms.locfileid: "89275160"
---
# <a name="quickstart-configure-ansible-using-azure-cloud-shell"></a>Avvio rapido: Configurare Ansible con Azure Cloud Shell

[!INCLUDE [annsible-intro.md](includes/ansible-intro.md)]

Questo articolo descrive come iniziare a usare Ansible dall'ambiente [Azure Cloud Shell](/azure/cloud-shell/overview).

## <a name="configure-your-environment"></a>Configurare l'ambiente

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../includes/open-source-devops-prereqs-azure-subscription.md)]
- **Configurare Azure Cloud Shell**: se non si ha familiarità con Azure Cloud Shell, vedere [Avvio rapido per Bash in Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/quickstart).

[!INCLUDE [open-cloud-shell.md](../includes/open-cloud-shell.md)]

## <a name="automatic-credential-configuration"></a>Configurazione automatica delle credenziali

Quando si è connessi a Cloud Shell per gestire l'infrastruttura, Ansible viene autenticato in Azure senza la necessità di configurazioni aggiuntive. 

Quando si usano più sottoscrizioni, specificare la sottoscrizione usata da Ansible esportando la variabile di ambiente `AZURE_SUBSCRIPTION_ID`. 

Per visualizzare un elenco di tutte le sottoscrizioni di Azure, eseguire il comando seguente:

```azurecli-interactive
az account list
```

Usando l'ID della sottoscrizione di Azure, impostare `AZURE_SUBSCRIPTION_ID` come segue:

```console
export AZURE_SUBSCRIPTION_ID=<your-subscription-id>
```

## <a name="verify-the-configuration"></a>Verificare la configurazione
Per verificare la configurazione, usare Ansible per creare un gruppo di risorse di Azure.

[!INCLUDE [create-resource-group-with-ansible.md](includes/ansible-snippet-create-resource-group.md)]

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"] 
> [Avvio rapido: Configurare una macchina virtuale in Azure usando Ansible](./vm-configure.md)
