---
title: Avvio rapido - Configurare Terraform con Azure Cloud Shell
description: Questo argomento di avvio rapido illustra come installare e configurare Terraform in Azure Cloud Shell.
keywords: azure devops terraform installazione configurazione cloud shell init piano applicare esecuzione portale accesso controllo degli accessi in base al ruolo entità servizio script automatizzato
ms.topic: quickstart
ms.date: 09/27/2020
ms.custom: devx-track-terraform, devx-track-azurecli
adobe-target: true
ms.openlocfilehash: 6befee6004b23410693f82c83f62e52318fd1c08
ms.sourcegitcommit: b380f6e637b47e6e3822b364136853e1d342d5cd
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/14/2021
ms.locfileid: "100395406"
---
# <a name="quickstart-configure-terraform-using-azure-cloud-shell"></a>Avvio rapido: Configurare Terraform con Azure Cloud Shell
 
[!INCLUDE [terraform-intro.md](includes/terraform-intro.md)]

Questo articolo descrive come iniziare a usare [Terraform in Azure](https://www.terraform.io/docs/providers/azurerm/index.html).

In questo articolo vengono illustrate le operazioni seguenti:
> [!div class="checklist"]
> * Eseguire l'autenticazione ad Azure
> * Creare un'entità servizio di Azure usando l'interfaccia della riga di comando di Azure
> * Eseguire l'autenticazione in Azure con un'entità servizio
> * Configurare la sottoscrizione corrente di Azure. Questa operazione deve essere eseguita se sono presenti più sottoscrizioni
> * Creare un file di configurazione di base di Terraform
> * Creare e applicare un piano di esecuzione di Terraform
> * Invertire un piano di esecuzione

## <a name="prerequisites"></a>Prerequisiti

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../includes/open-source-devops-prereqs-azure-subscription.md)]

## <a name="configure-your-environment"></a>Configurare l'ambiente

1. Accedere al [portale di Azure](https://portal.azure.com).

1. Se non è già stato effettuato l'accesso, il portale di Azure visualizza un elenco degli account Microsoft disponibili. Per continuare, selezionare un account Microsoft associato a una o più sottoscrizioni di Azure attive e immettere le credenziali.

1. Aprire Cloud Shell.

    ![Accesso a Cloud Shell](media/install-configure/portal-cloud-shell.png)

1. Se Cloud Shell non è stato usato in precedenza, configurare le impostazioni dell'ambiente e di archiviazione. In questa articolo si usa l'ambiente Bash.

**Note**:
- In Cloud Shell è installata automaticamente la versione più recente di Terraform. Terraform usa inoltre automaticamente le informazioni della sottoscrizione di Azure corrente. Di conseguenza, non è necessaria alcuna operazione di installazione o configurazione.

## <a name="authenticate-to-azure"></a>Eseguire l'autenticazione ad Azure

L'autenticazione di Cloud Shell viene eseguita automaticamente con l'account Microsoft usato per accedere al portale di Azure. Se l'account include più sottoscrizioni di Azure, è possibile [passare a una delle altre sottoscrizioni](#set-the-current-azure-subscription).

Terraform supporta diverse opzioni per l'autenticazione in Azure. Questo articolo descrive le tecniche seguenti:

- Se si usa Terraform in modo interattivo, è consigliabile eseguire l'[autenticazione tramite l'account Microsoft](#authenticate-via-microsoft-account).
- Se si usa Terraform dal codice, uno dei metodi consigliati consiste nell'eseguire l'[autenticazione tramite l'entità servizio di Azure](#authenticate-via-azure-service-principal).

### <a name="authenticate-via-microsoft-account"></a>Eseguire l'autenticazione tramite l'account Microsoft

Se si chiama `az login` senza parametri, vengono visualizzati un URL e un codice. Passare all'URL, immettere il codice e seguire le istruzioni per accedere ad Azure con l'account Microsoft. Dopo aver effettuato l'accesso, tornare al portale.

```azurecli
az login
```

**Note**:

- Dopo l'accesso, `az login` visualizza un elenco delle sottoscrizioni di Azure associate all'account Microsoft connesso.
- Viene visualizzato un elenco di proprietà per ogni sottoscrizione di Azure disponibile. La proprietà `isDefault` identifica la sottoscrizione di Azure in uso. Per informazioni su come passare a un'altra sottoscrizione di Azure, vedere la sezione [Impostare la sottoscrizione corrente di Azure](#set-the-current-azure-subscription).

### <a name="authenticate-via-azure-service-principal"></a>Eseguire l'autenticazione tramite l'entità servizio di Azure

**Creare un'entità servizio di Azure**: per accedere a una sottoscrizione di Azure usando un'entità servizio, occorre prima di tutto avere accesso a un'entità servizio. Se si ha già un'entità servizio, è possibile saltare questa parte della sezione.

Gli strumenti automatici che distribuiscono o usano servizi di Azure, come Terraform, devono sempre avere autorizzazioni limitate. Per evitare che le applicazioni accedano come utenti con privilegi completi, Azure offre le entità servizio. Ma che cosa accade se non si ha un'entità servizio con cui eseguire l'accesso? In un scenario di questo tipo è possibile accedere usando le credenziali utente e quindi creare un'entità servizio. Dopo aver creato l'entità servizio, è possibile utilizzarne le informazioni per i tentativi di accesso successivi.

Sono disponibili diverse opzioni per [creare un'entità servizio con l'interfaccia della riga di comando di Azure](/cli/azure/create-an-azure-service-principal-azure-cli?). Per questo articolo si userà [az ad sp create-for-rbac](/cli/azure/ad/sp?#az-ad-sp-create-for-rbac) per creare un'entità servizio con il ruolo **Collaboratore**. Il ruolo **Collaboratore** (l'impostazione predefinita) ha autorizzazioni complete per operazioni di lettura e scrittura in un account Azure. Per altre informazioni sul controllo degli accessi in base al ruolo e i ruoli, vedere [Controllo degli accessi in base al ruolo: ruoli predefiniti](/azure/active-directory/role-based-access-built-in-roles).

Immettere il comando seguente, sostituendo `<subscription_id>` con l'ID dell'account della sottoscrizione che si vuole usare.

```azurecli
az ad sp create-for-rbac --role="Contributor" --scopes="/subscriptions/<subscription_id>"
```

**Note**:

- Al termine, `az ad sp create-for-rbac` visualizza diversi valori. I valori `name`, `password` e `tenant` vengono usati nel passaggio successivo.
- Se viene persa, questa password non può essere recuperata. Di conseguenza, è consigliabile archiviarla in un luogo sicuro. Se si dimentica la password, è necessario [reimpostare le credenziali dell'entità servizio](/cli/azure/create-an-azure-service-principal-azure-cli#reset-credentials).

**Accedere con un'entità servizio di Azure**: Nella chiamata seguente a `az login` sostituire i segnaposto con le informazioni dell'entità servizio.

```azurecli
az login --service-principal -u <service_principal_name> -p "<service_principal_password>" --tenant "<service_principal_tenant>"
```

## <a name="set-the-current-azure-subscription"></a>Impostare la sottoscrizione corrente di Azure

Un account Microsoft può essere associato a più sottoscrizioni di Azure. I passaggi seguenti illustrano come passare da una sottoscrizione all'altra:

1. Per visualizzare la sottoscrizione di Azure corrente, usare [az account show](/cli/azure/account#az-account-show).

    ```azurecli
    az account show
    ```

1. Se si ha accesso a più sottoscrizioni di Azure disponibili, usare [az account list](/cli/azure/account#az-account-list) per visualizzare un elenco di valori di ID nome delle sottoscrizioni:

    ```azurecli
    az account list --query "[].{name:name, subscriptionId:id}"
    ```

1. Per usare una sottoscrizione di Azure specifica per la sessione di Cloud Shell corrente, usare [az account set](/cli/azure/account#az-account-set). Sostituire il segnaposto `<subscription_id>` con l'ID o il nome della sottoscrizione che si vuole usare:

    ```azurecli
    az account set --subscription="<subscription_id>"
    ```

    **Note**:

    - La chiamata a `az account set` non visualizza i risultati del passaggio alla sottoscrizione di Azure specificata. È però possibile usare `az account show` per confermare che la sottoscrizione di Azure corrente è cambiata.

[!INCLUDE [terraform-create-base-config-file.md](includes/terraform-create-base-config-file.md)]

[!INCLUDE [terraform-create-and-apply-execution-plan.md](includes/terraform-create-and-apply-execution-plan.md)]

[!INCLUDE [terraform-reverse-execution-plan.md](includes/terraform-reverse-execution-plan.md)]

[!INCLUDE [terraform-troubleshooting.md](includes/terraform-troubleshooting.md)]

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"]
> [Creare una macchina virtuale Linux usando Terraform](create-linux-virtual-machine-with-infrastructure.md)
