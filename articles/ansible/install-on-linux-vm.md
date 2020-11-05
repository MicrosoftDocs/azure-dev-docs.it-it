---
title: Avvio rapido - Configurare Ansible con l'interfaccia della riga di comando di Azure
description: Questo articolo di avvio rapido descrive come installare e configurare Ansible per gestire le risorse di Azure in Ubuntu, CentOS e SLES
keywords: ansible, azure, devops, bash, cloudshell, playbook, interfaccia della riga di comando di azure
ms.topic: quickstart
ms.date: 09/30/2020
ms.custom: devx-track-ansible, devx-track-azurecli
ms.openlocfilehash: b01cf6925f19ae6dc561358546f9ee3b945cad4f
ms.sourcegitcommit: 5c7f5fef798413b1a304cc9ee31c8518b73f27eb
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/30/2020
ms.locfileid: "93066250"
---
# <a name="quickstart-configure-ansible-using-azure-cli"></a>Avvio rapido: Configurare Ansible con l'interfaccia della riga di comando di Azure

Questo argomento di avvio rapido illustra come installare [Ansible ](https://docs.ansible.com/) con l'interfaccia della riga di comando di Azure.

In questo avvio rapido verranno completate le attività seguenti:

> [!div class="checklist"]
> * Creare una coppia di chiavi SSH
> * Creare un gruppo di risorse
> * Creare una macchina virtuale CentOS 
> * Installare Ansible nella macchina virtuale
> * Connettersi alla macchina virtuale tramite SSH
> * Configurare Ansible nella macchina virtuale

## <a name="prerequisites"></a>Prerequisites

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../includes/open-source-devops-prereqs-azure-subscription.md)]
[!INCLUDE [open-source-devops-prereqs-create-sp.md](../includes/open-source-devops-prereqs-create-service-principal.md)]
- **Accesso a Linux o a una macchina virtuale Linux** : in assenza di un computer Linux, creare una [macchina virtuale Linux](/azure/virtual-network/quick-create-cli).

## <a name="create-an-ssh-key-pair"></a>Creare una coppia di chiavi SSH

Per la connessione alle macchine virtuali Linux è possibile usare l'autenticazione della password o l'autenticazione basata su chiave. L'autenticazione basata su chiave è più sicura rispetto all'uso delle password. Questo articolo descrive quindi l'autenticazione basata su chiave.

Con l'autenticazione basata su chiave sono presenti due chiavi:

- **Chiave pubblica** : la chiave pubblica è archiviata nell'host, ad esempio nella macchina virtuale (come in questo articolo)
- **Chiave privata** : la chiave privata consente di connettersi in modo sicuro all'host. La chiave privata è la password vera e propria e deve quindi essere protetta.
        
La procedura seguente illustra in dettaglio la creazione di una coppia di chiavi SSH.

1. Accedere al [portale di Azure](https://portal.azure.com).

1. Aprire [Azure Cloud Shell](/azure/cloud-shell/overview) e, se non è già stato fatto, passare a **Bash**.

1. Creare una chiave SSH usando [ssh-keygen](https://www.ssh.com/ssh/keygen/).

    ```bash
    ssh-keygen -m PEM -t rsa -b 2048 -C "azureuser@azure" -f ~/.ssh/ansible_rsa -N ""
    ```

    **Note** :

    - Il comando `ssh-keygen` visualizza il percorso dei file di chiave generati. Questo nome di directory sarà necessario quando si crea la macchina virtuale.
    - La chiave pubblica è archiviata in `ansible_rsa.pub` e la chiave privata in `ansible_rsa`.

## <a name="create-a-virtual-machine"></a>Creare una macchina virtuale

1. Creare un gruppo di risorse usando [az group create](/cli/azure/group#az-group-create). Potrebbe essere necessario sostituire il parametro `--location` con il valore appropriato per l'ambiente.

    ```azurecli
    az group create --name QuickstartAnsible-rg --location eastus
    ```

1. Creare una macchina virtuale con [az vm create](/cli/azure/vm#az-vm-create). Sostituire il segnaposto con il nome completo del nome file della chiave **pubblica** SSH.

    ```azurecli
    az vm create \
    --resource-group QuickstartAnsible-rg \
    --name QuickstartAnsible-vm \
    --image OpenLogic:CentOS:7.7:latest \
    --admin-username azureuser \
    --ssh-key-values <ssh_public_key_filename>
    ```

1. Verificare la creazione e lo stato della nuova macchina virtuale usando [az vm list](/cli/azure/vm#az-vm-list).

    ```azurecli
    az vm list -d -o table --query "[?name=='QuickstartAnsible-vm']"
    ```

    **Note** :

    - L'output del comando `az vm list` include l'indirizzo IP pubblico usato per connettersi tramite SSH alla macchina virtuale.

## <a name="install-ansible-on-the-virtual-machine"></a>Installare Ansible nella macchina virtuale

Eseguire lo script di installazione di Ansible usando [az vm extension set](/cli/azure/vm/extension?#az-vm-extension-set).

```azurecli
az vm extension set \
 --resource-group QuickstartAnsible-rg \
 --vm-name QuickstartAnsible-vm \
 --name customScript \
 --publisher Microsoft.Azure.Extensions \
 --version 2.1 \
 --settings '{"fileUris":["https://raw.githubusercontent.com/MicrosoftDocs/mslearn-ansible-control-machine/master/configure-ansible-centos.sh"]}' \
 --protected-settings '{"commandToExecute": "./configure-ansible-centos.sh"}'
```

**Note:**

- Al termine, il comando `az vm extension` visualizza i risultati dell'esecuzione dello script di installazione.

## <a name="connect-to-your-virtual-machine-via-ssh"></a>Connettersi alla macchina virtuale tramite SSH

Usando il comando SSH connettersi alla macchina virtuale. Sostituire i segnaposto con i valori appropriati restituiti.

```azurecli
ssh -i <ssh_private_key_filename> azureuser@<vm_ip_address>
```

## <a name="create-azure-credentials"></a>Creare credenziali di Azure

Per configurare le credenziali di Ansible, sono necessarie le informazioni seguenti:

* L'ID sottoscrizione di Azure
* I valori dell'entità servizio

Se si usa Ansible Tower o Jenkins, dichiarare i valori dell'entità servizio come variabili di ambiente.

Configurare le credenziali di Ansible con una delle tecniche seguenti:

- [Creare un file di credenziali di Ansible](#file-credentials)
- [Definire le variabili di ambiente di Ansible](#env-credentials)

#### <a name="span-idfile-credentials-create-ansible-credentials-file"></a><span id="file-credentials"/> Creare un file di credenziali di Ansible

In questa sezione viene creato un file di credenziali locale per fornire credenziali ad Ansible.

Per altre informazioni su come definire le credenziali di Ansible, vedere [Providing Credentials to Azure Modules](https://docs.ansible.com/ansible/guide_azure.html#providing-credentials-to-azure-modules) (Fornire le credenziali ai moduli di Azure).

1. Dopo avere completato correttamente la connessione alla macchina virtuale host, creare e aprire un file denominato `credentials`:

    ```bash
    mkdir ~/.azure
    vi ~/.azure/credentials
    ```

1. Inserire le righe seguenti nel file. Sostituire i segnaposto con i valori dell'entità servizio.

    ```bash
    [default]
    subscription_id=<your-subscription_id>
    client_id=<security-principal-appid>
    secret=<security-principal-password>
    tenant=<security-principal-tenant>
    ```

1. Salvare e chiudere il file.

#### <a name="span-idenv-credentialsdefine-ansible-environment-variables"></a><span id="env-credentials"/>Definire le variabili di ambiente di Ansible

Nella macchina virtuale host esportare i valori dell'entità servizio per configurare le credenziali di Ansible.

```bash
export AZURE_SUBSCRIPTION_ID=<your-subscription_id>
export AZURE_CLIENT_ID=<security-principal-appid>
export AZURE_SECRET=<security-principal-password>
export AZURE_TENANT=<security-principal-tenant>
```

## <a name="test-ansible-installation"></a>Testare l'installazione di Ansible

È ora disponibile una macchina virtuale con Ansible installato e configurato.

[!INCLUDE [ansible-test-configuration.md](includes/ansible-test-configuration.md)]

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"]
> [Ansible in Azure](./index.yml)
