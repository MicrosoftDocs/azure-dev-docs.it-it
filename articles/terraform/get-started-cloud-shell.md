---
title: 'Avvio rapido: Introduzione a Terraform con Azure Cloud Shell'
description: Questo argomento di avvio rapido illustra come installare e configurare Terraform per la creazione di risorse di Azure.
keywords: azure devops terraform installazione configurazione cloud shell init piano applicare esecuzione portale accesso controllo degli accessi in base al ruolo entità servizio script automatizzato
ms.topic: quickstart
ms.date: 08/08/2020
ms.openlocfilehash: 736c805b8dd8c95d1950537b754059cca9fc5712
ms.sourcegitcommit: 6a8485d659d6239569c4e3ecee12f924c437b235
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/09/2020
ms.locfileid: "88026143"
---
# <a name="quickstart-get-started-with-terraform-using-azure-cloud-shell"></a>Avvio rapido: Introduzione all'uso di Terraform in Azure Cloud Shell
 
[!INCLUDE [terraform-intro.md](includes/terraform-intro.md)]

Questo articolo descrive come iniziare a usare [Terraform in Azure](https://www.terraform.io/docs/providers/azurerm/index.html).

In questo articolo vengono illustrate le operazioni seguenti:
> [!div class="checklist"]
> * Eseguire l'autenticazione in Azure con `az login`
> * Creare un'entità servizio di Azure usando l'interfaccia della riga di comando di Azure
> * Eseguire l'autenticazione in Azure con un'entità servizio
> * Configurare la sottoscrizione corrente di Azure. Questa operazione deve essere eseguita se sono presenti più sottoscrizioni
> * Scrivere uno script di Terraform per creare un gruppo di risorse di Azure
> * Creare e applicare un piano di esecuzione di Terraform
> * Usare il flag `terraform plan -destroy` per ripristinare un piano di esecuzione

[!INCLUDE [hashicorp-support.md](includes/hashicorp-support.md)]

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

## <a name="create-a-terraform-configuration-file"></a>Creare un file di configurazione Terraform

Questa sezione illustra come creare un file di configurazione di Terraform che crea un gruppo di risorse di Azure.

1. Modificare le directory impostandole sulla condivisione file montata in cui è stato salvato in modo permanente il lavoro in Cloud Shell. Per altre informazioni sul modo in cui Cloud Shell salva i file in modo permanente, vedere [Connettersi all'archiviazione di File di Microsoft Azure](/azure/cloud-shell/overview#connect-your-microsoft-azure-files-storage)

    ```bash
    cd clouddrive
    ```

1. Creare una directory in cui conservare i file di Terraform per questa demo.

    ```bash
    mkdir QuickstartTerraformTest
    ```

1. Passare alla directory demo.

    ```bash
    cd QuickstartTerraformTest
    ```

1. Usando l'editor preferito, creare un file di configurazione di Terraform. In questo articolo si usa l'[editor di Cloud Shell](/azure/cloud-shell/using-cloud-shell-editor) incorporato.

    ```bash
    code QuickstartTerraformTest.tf
    ```
 
1. Incollare il codice HCL seguente nel nuovo file.

    ```hcl
    provider "azurerm" {
      # The "feature" block is required for AzureRM provider 2.x.
      # If you are using version 1.x, the "features" block is not allowed.
      version = "~>2.0"
      features {}
    }
    resource "azurerm_resource_group" "rg" {
            name = "QuickstartTerraformTest-rg"
            location = "eastus"
    }
    ```

    **Note**:

    - Il blocco `provider` specifica che viene usato il [provider di Azure (`azurerm`)](https://www.terraform.io/docs/providers/azurerm/index.html).
    - All'interno del blocco del provider `azurerm` vengono impostati gli attributi `version` e `features`. Come indicato nel commento, l'utilizzo di questi attributo dipende dalla versione. Per altre informazioni su come impostare questi attributi per l'ambiente in uso, vedere la [versione 2.0 del provider AzureRM](https://www.terraform.io/docs/providers/azurerm/guides/2.0-upgrade-guide.html).
    - L'unica [dichiarazione di risorse](https://www.terraform.io/docs/configuration/resources.html) è relativa a un tipo di risorsa di [azurerm_resource_group](https://www.terraform.io/docs/providers/azurerm/r/resource_group.html). I due argomenti obbligatori per `azure_resource_group` sono `name` e `location`.

1. Salvare il file ( **&lt;CTRL>+S**).

1. Uscire dall'editor ( **&lt;CTRL>+Q**).

## <a name="create-and-apply-a-terraform-execution-plan"></a>Creare e applicare un piano di esecuzione di Terraform

In questa sezione viene creato un *piano di esecuzione*, che viene applicato all'infrastruttura cloud.

1. Inizializzare la distribuzione di Terraform con [terraform init](https://www.terraform.io/docs/commands/init.html). Questo passaggio scarica i moduli di Azure necessari per creare un gruppo di risorse di Azure.

    ```bash
    terraform init
    ```

1. Eseguire [terraform plan](https://www.terraform.io/docs/commands/plan.html) per creare un piano di esecuzione dal file di configurazione di Terraform.

    ```bash
    terraform plan -out QuickstartTerraformTest.tfplan
    ```

    **Note:**
    - Il comando `terraform plan` consente di creare un piano di esecuzione, ma non di eseguirlo. Determina invece le azioni necessarie per creare la configurazione specificata nei file di configurazione. Questo modello consente di verificare se il piano di esecuzione corrisponde alle aspettative prima di apportare modifiche alle risorse effettive.
    - Il parametro `-out` facoltativo consente di specificare un file di output per il piano. L'uso del parametro `-out` garantisce che il piano esaminato sia esattamente quello che viene applicato.
    - Per altre informazioni su come rendere persistenti i piani di esecuzione e la sicurezza, vedere la [sezione relativa agli avvisi di sicurezza](https://www.terraform.io/docs/commands/plan.html#security-warning).

1. Eseguire [terraform apply](https://www.terraform.io/docs/commands/apply.html) per applicare il piano di esecuzione.

    ```bash
    terraform apply QuickstartTerraformTest.tfplan
    ```

1. Dopo l'applicazione del piano di esecuzione, è possibile verificare se il gruppo di risorse è stato creato correttamente usando [az group show](/cli/azure/group?#az-group-show).

    ```azurecli
    az group show -n "QuickstartTerraformTest-rg"
    ```

    **Note**:

    - Se l'operazione è riuscita, `az group show` visualizza diverse proprietà del gruppo di risorse appena creato.

## <a name="clean-up-resources"></a>Pulire le risorse

Quando non sono più necessarie, eliminare le risorse create in questo articolo.

1. Eseguire [terraform plan](https://www.terraform.io/docs/commands/plan.html) per creare un piano di esecuzione in modo da eliminare definitivamente le risorse indicate nel file di configurazione di Terraform.

    ```bash
    terraform plan -destroy -out QuickstartTerraformTest.destroy.tfplan
    ```

    **Note**:
    - Il comando `terraform plan` consente di creare un piano di esecuzione, ma non di eseguirlo. Determina invece le azioni necessarie per creare la configurazione specificata nei file di configurazione. Questo modello consente di verificare se il piano di esecuzione corrisponde alle aspettative prima di apportare modifiche alle risorse effettive.
    - Il parametro `-destroy` genera un piano per eliminare definitivamente le risorse.
    - Il parametro `-out` facoltativo consente di specificare un file di output per il piano. L'uso del parametro `-out` garantisce che il piano esaminato sia esattamente quello che viene applicato.
    - Per altre informazioni su come rendere persistenti i piani di esecuzione e la sicurezza, vedere la [sezione relativa agli avvisi di sicurezza](https://www.terraform.io/docs/commands/plan.html#security-warning).

1. Eseguire [terraform apply](https://www.terraform.io/docs/commands/apply.html) per applicare il piano di esecuzione.

    ```bash
    terraform apply QuickstartTerraformTest.destroy.tfplan
    ```

1. Verificare che il gruppo di risorse sia stato eliminato usando [az group show](/cli/azure/group?#az-group-show).

    ```azurecli
    az group show -n "QuickstartTerraformTest-rg"
    ```

    **Note**:
    - Se l'operazione riesce, `az group show` visualizza un messaggio per indicare che il gruppo di risorse non esiste.

1. Passare alla directory padre e rimuovere la directory demo. Il parametro `-r` rimuove il contenuto della directory prima di rimuovere la directory. Il contenuto della directory include il file di configurazione creato in precedenza e i file di stato di Terraform.

    ```bash
    cd .. && rm -r QuickstartTerraformTest
    ```

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"]
> [Creare una macchina virtuale di Azure con Terraform](create-linux-virtual-machine-with-infrastructure.md)