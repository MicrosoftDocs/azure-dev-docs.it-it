---
title: 'Avvio rapido: Introduzione ad Ansible - Azure Cloud Shell'
description: In questo argomento di avvio rapido viene illustrato come eseguire diverse attività di Ansible con Bash in Azure Cloud Shell
keywords: ansible, azure, devops, bash, cloudshell, playbook, bash
ms.topic: quickstart
ms.date: 06/01/2020
ms.openlocfilehash: 2d53c2c5e3eb834e0296be167104e484d144fbd9
ms.sourcegitcommit: 367217792f3b16c769e2c39372358bc6b9c9c044
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/02/2020
ms.locfileid: "84301152"
---
# <a name="quickstart-getting-started-with-ansible---azure-cloud-shell"></a>Avvio rapido: Introduzione ad Ansible - Azure Cloud Shell

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