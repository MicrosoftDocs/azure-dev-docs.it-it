---
title: Avvio rapido - Introduzione all'uso di Terraform in Azure Cloud Shell
description: Questo argomento di avvio rapido illustra come installare e configurare Terraform per la creazione di risorse di Azure.
keywords: azure devops terraform installazione configurazione cloud shell init piano applicare esecuzione portale accesso controllo degli accessi in base al ruolo entità servizio script automatizzato
ms.topic: quickstart
ms.date: 06/11/2020
ms.openlocfilehash: 4b0f6802673d886cecdc9523d99886c19fbad94a
ms.sourcegitcommit: 2d6c9687b39e33a6b5e980d9a375c9f8f1f2cab7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/15/2020
ms.locfileid: "84779663"
---
# <a name="quickstart-getting-started-with-terraform-using-azure-cloud-shell"></a>Avvio rapido: Introduzione all'uso di Terraform in Azure Cloud Shell
 
[!INCLUDE [terraform-intro.md](includes/terraform-intro.md)]

Questo articolo descrive come iniziare a usare [Terraform in Azure](https://www.terraform.io/docs/providers/azurerm/index.html) dall'ambiente [Azure Cloud Shell](/azure/cloud-shell/overview).

[!INCLUDE [hashicorp-support.md](includes/hashicorp-support.md)]

## <a name="prerequisites"></a>Prerequisiti

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../includes/open-source-devops-prereqs-azure-subscription.md)]

## <a name="configure-azure-cloud-shell"></a>Configurare Azure Cloud Shell

1. Accedere al [portale di Azure](https://portal.azure.com).

1. Se non è già stato effettuato l'accesso, il portale di Azure visualizza un elenco degli account Microsoft disponibili. Per continuare, selezionare un account Microsoft associato a una o più sottoscrizioni di Azure attive e immettere le credenziali.

1. Aprire Cloud Shell.

    ![Accesso a Cloud Shell](media/install-configure/portal-cloud-shell.png)

1. Se Cloud Shell non è stato usato in precedenza, configurare le impostazioni dell'ambiente e di archiviazione. In questa articolo si usa l'ambiente Bash.

## <a name="log-into-azure"></a>Accedere ad Azure

Cloud Shell viene autenticato automaticamente nell'account Microsoft con cui è stato effettuato l'accesso al portale di Azure.

Inoltre, dopo aver eseguito l'accesso all'account Microsoft, si accede automaticamente alla sottoscrizione di Azure predefinita per tale account. Se l'account Microsoft corrente è corretto e si vuole cambiare sottoscrizione, vedere la sezione [Specificare la sottoscrizione di Azure corrente](#specify-the-current-azure-subscription).

Se si hanno più account Microsoft con sottoscrizioni di Azure, è possibile accedere a uno di questi account usando una delle opzioni seguenti:

- [Accedere all'account Microsoft](#log-into-your-microsoft-account)
- [Accedere con un'entità servizio di Azure](#log-into-azure-using-an-azure-service-principal)

### <a name="log-into-your-microsoft-account"></a>Accedere all'account Microsoft

Se si chiama `az login` senza parametri, vengono visualizzati un URL e un codice. Passare all'URL, immettere il codice e seguire le istruzioni per accedere ad Azure con l'account Microsoft. Dopo aver effettuato l'accesso, tornare al portale.

```azurecli
az login
```

Note:
- Dopo l'accesso, `az login` visualizza un elenco delle sottoscrizioni di Azure associate all'account Microsoft connesso.
- Viene visualizzato un elenco di proprietà per ogni sottoscrizione di Azure disponibile. La proprietà `isDefault` identifica la sottoscrizione di Azure in uso. Per informazioni su come passare a un'altra sottoscrizione di Azure, vedere la sezione [Specificare la sottoscrizione di Azure corrente](#specify-the-current-azure-subscription).

### <a name="log-into-azure-using-an-azure-service-principal"></a>Accedere ad Azure con un'entità servizio di Azure

**Creare un'entità servizio di Azure**: per accedere a una sottoscrizione di Azure usando un'entità servizio, occorre prima di tutto avere accesso a un'entità servizio. Se si ha già un'entità servizio, è possibile saltare questa parte della sezione.

Gli strumenti automatici che distribuiscono o usano servizi di Azure, come Terraform, devono sempre avere autorizzazioni limitate. Per evitare che le applicazioni accedano come utenti con privilegi completi, Azure offre le entità servizio. Ma che cosa accade se non si ha un'entità servizio con cui eseguire l'accesso? In un scenario di questo tipo è possibile accedere usando le credenziali utente e quindi creare un'entità servizio. Dopo aver creato l'entità servizio, è possibile utilizzarne le informazioni per i tentativi di accesso successivi.

Sono disponibili diverse opzioni per [creare un'entità servizio con l'interfaccia della riga di comando di Azure](/cli/azure/create-an-azure-service-principal-azure-cli?). Per questo articolo si userà [az ad sp create-for-rbac](/cli/azure/ad/sp?#az-ad-sp-create-for-rbac) per creare un'entità servizio con il ruolo **Collaboratore**. Il ruolo **Collaboratore** (l'impostazione predefinita) ha autorizzazioni complete per operazioni di lettura e scrittura in un account Azure. Per altre informazioni sul controllo degli accessi in base al ruolo e i ruoli, vedere [Controllo degli accessi in base al ruolo: ruoli predefiniti](/azure/active-directory/role-based-access-built-in-roles).

Immettere il comando seguente, sostituendo `<subscription_id>` con l'ID dell'account della sottoscrizione che si vuole usare.
    
```azurecli
az ad sp create-for-rbac --role="Contributor" --scopes="/subscriptions/<subscription_id>"
```

Note:
- Al termine, `az ad sp create-for-rbac` visualizza diversi valori, inclusa la password generata automaticamente. Se viene persa, questa password non può essere recuperata. Di conseguenza, è consigliabile archiviarla in un luogo sicuro. Se si dimentica la password, è necessario [reimpostare le credenziali dell'entità servizio](/cli/azure/create-an-azure-service-principal-azure-cli#reset-credentials).
    
**Accedere con un'entità servizio di Azure**: Nella chiamata seguente a `az login` sostituire i segnaposto con le informazioni dell'entità servizio.

```azurecli
az login --service-principal -u <service_principal_name> -p "<service_principal_password>" --tenant "<service_principal_tenant>"
```

Note:
- Dopo l'accesso, `az login` visualizza diverse proprietà della sottoscrizione di Azure, ad esempio `id` e `name`.

## <a name="specify-the-current-azure-subscription"></a>Specificare la sottoscrizione di Azure corrente

Come illustrato nella sezione precedente, per accedere ad Azure è possibile scegliere tra i due scenari seguenti:

- **Accedere con un account Microsoft**: un account Microsoft può essere associato a più sottoscrizioni di Azure, una delle quali è la sottoscrizione predefinita. La sottoscrizione predefinita è quella usata se si esegue l'accesso e non si passa a un'altra sottoscrizione.
- **Accedere con un'entità servizio di Azure**: un'entità servizio è specifica di una sottoscrizione di Azure. Tenere presente che le informazioni relative alla sottoscrizione vengono visualizzate quando si esegue l'accesso.

La procedura seguente si riferisce al primo scenario in cui si eseguono le attività seguenti:

- Visualizzare la sottoscrizione di Azure corrente
- Elencare tutte le sottoscrizioni di Azure disponibili per l'account Microsoft corrente
- Passare a un'altra sottoscrizione di Azure

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

    Note:
    - La chiamata a `az account set` non visualizza i risultati del passaggio alla sottoscrizione di Azure specificata. È però possibile usare `az account show` per confermare che la sottoscrizione di Azure corrente è cambiata.

## <a name="configure-terraform"></a>Configurare Terraform

In Cloud Shell è installata automaticamente la versione più recente di Terraform. Terraform usa inoltre automaticamente le informazioni della sottoscrizione di Azure corrente. Di conseguenza, non è necessaria alcuna operazione di installazione o configurazione.

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

    Note:
    - Il blocco `provider` specifica che viene usato il [provider di Azure (`azurerm`)](https://www.terraform.io/docs/providers/azurerm/index.html).
    - All'interno del blocco del provider `azurerm` vengono impostati gli attributi `version` e `features`. Come indicato nel commento, l'utilizzo di questi attributo dipende dalla versione. Per altre informazioni su come impostare questi attributi per l'ambiente in uso, vedere la [versione 2.0 del provider AzureRM](https://www.terraform.io/docs/providers/azurerm/guides/2.0-upgrade-guide.html).
    - L'unica [dichiarazione di risorse](https://www.terraform.io/docs/configuration/resources.html) è relativa a un tipo di risorsa di [azurerm_resource_group](https://www.terraform.io/docs/providers/azurerm/r/resource_group.html). I due argomenti obbligatori per `azure_resource_group` sono `name` e `location`.

1. Salvare il file ( **&lt;CTRL>+S**).

1. Uscire dall'editor ( **&lt;CTRL>+Q**).

## <a name="create-and-apply-a-terraform-execution-plan"></a>Creare e applicare un piano di esecuzione di Terraform

Una volta creati i file di configurazione, in questa sezione viene spiegato come creare un *piano di esecuzione* e applicarlo all'infrastruttura cloud.

1. Inizializzare la distribuzione di Terraform con [terraform init](https://www.terraform.io/docs/commands/init.html). Questo passaggio scarica i moduli di Azure necessari per creare un gruppo di risorse di Azure.

    ```bash
    terraform init
    ```

1. Per visualizzare in anteprima le azioni da completare in Terraform, usare il comando [terraform plan](https://www.terraform.io/docs/commands/plan.html).

    ```bash
    terraform plan
    ```

    **Note:**
    - Il comando `terraform plan` consente di creare un piano di esecuzione, ma non di eseguirlo. Determina invece le azioni necessarie per creare la configurazione specificata nei file di configurazione.
    - Con il comando `terraform plan` è possibile verificare se il piano di esecuzione corrisponde alle aspettative prima di apportare modifiche alle risorse effettive.
    - Il parametro `-out` facoltativo consente di specificare un file di output per il piano. Per altre informazioni sull'utilizzo del parametro `-out`, vedere la sezione [Salvare in modo permanente un piano di esecuzione per una distribuzione successiva](#persist-an-execution-plan-for-later-deployment).

1. Per applicare il piano di esecuzione, usare il comando [terraform apply](https://www.terraform.io/docs/commands/apply.html).

    ```bash
    terraform apply
    ```

1. Terraform mostra gli effetti dell'applicazione del piano di esecuzione e chiede di confermare l'esecuzione. Per confermare il comando, immettere `yes` e premere **INVIO**.

1. Dopo aver confermato l'esecuzione della riproduzione, usare il comando [az group show](/cli/azure/group?#az-group-show) per verificare che il gruppo di risorse sia stato creato correttamente.

    ```azurecli
    az group show -n "QuickstartTerraformTest-rg"
    ```

    Se l'operazione è riuscita, `az group show` visualizza diverse proprietà del gruppo di risorse appena creato.

## <a name="persist-an-execution-plan-for-later-deployment"></a>Salvare in modo permanente un piano di esecuzione per una distribuzione successiva

Nella sezione precedente è stato illustrato come eseguire [terraform plan](https://www.terraform.io/docs/commands/plan.html) per creare un piano di esecuzione. È stato quindi illustrato come applicare il piano con il comando [terraform apply](https://www.terraform.io/docs/commands/apply.html). Questo modello funziona in modo ottimale quando i passaggi sono interattivi e sequenziali.

Per scenari più complessi, è possibile salvare in modo permanente il piano di esecuzione in un file, in modo da poterlo applicare in un secondo momento, o anche da un altro computer.

Se si usa questa funzionalità, è consigliabile leggere l'articolo sull'[esecuzione di Terraform nell'automazione](https://learn.hashicorp.com/terraform/development/running-terraform-in-automation).

Nei passaggi seguenti viene illustrato lo schema di base per l'uso di questa funzionalità:

1. Eseguire [terraform init](https://www.terraform.io/docs/commands/init.html).

    ```bash
    terraform init
    ```

1. Eseguire `terraform plan` con il parametro `-out`.

    ```bash
    terraform plan -out QuickstartTerraformTest.tfplan
    ```

1. Eseguire `terraform apply` specificando il nome del file del passaggio precedente.

    ```bash
    terraform apply QuickstartTerraformTest.tfplan
    ```

Note:
- Per consentire l'uso con l'automazione, l'esecuzione di `terraform apply <filename>` non richiede la conferma.
- Se si decide di usare questa funzionalità, leggere la [sezione sugli avvisi di sicurezza](https://www.terraform.io/docs/commands/plan.html#security-warning).

## <a name="clean-up-resources"></a>Pulire le risorse

Quando non sono più necessarie, eliminare le risorse create in questo articolo.

1. Eseguire il comando [terraform destroy](https://www.terraform.io/docs/commands/destroy.html) per eliminare il piano di esecuzione corrente.

    ```bash
    terraform destroy
    ```

1. Terraform mostra gli effetti dell'eliminazione del piano di esecuzione e chiede di confermare. Per confermare, immettere `yes` e premere **INVIO**.

1. Dopo aver confermato l'esecuzione della riproduzione, l'output è simile all'esempio seguente. Eseguire [az group show](/cli/azure/group?#az-group-show) per verificare che il gruppo di risorse sia stato eliminato.

    ```azurecli
    az group show -n "QuickstartTerraformTest-rg"
    ```

    Note:
    - Se l'operazione riesce, `az group show` visualizza un messaggio per indicare che il gruppo di risorse non esiste.

1. Passare alla directory padre e rimuovere la directory demo. Il parametro `-r` rimuove il contenuto della directory prima di rimuovere la directory. Il contenuto della directory include il file di configurazione creato in precedenza e i file di stato di Terraform.

    ```bash
    cd .. && rm -r QuickstartTerraformTest
    ```

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"]
> [Creare una macchina virtuale di Azure con Terraform](create-linux-virtual-machine-with-infrastructure.md)