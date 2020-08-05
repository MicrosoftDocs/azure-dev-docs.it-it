---
title: 'Avvio rapido: Introduzione a Terraform con PowerShell'
description: Questo argomento di avvio rapido illustra come installare e configurare Terraform per la creazione di risorse di Azure.
keywords: azure devops terraform installazione configurazione windows inizializzazione piano applicare esecuzione portale accesso controllo degli accessi in base al ruolo entità servizio script automatizzato powershell
ms.topic: quickstart
ms.date: 07/27/2020
ms.openlocfilehash: 40663f23d79066354cb7a78318eba7a8998676c5
ms.sourcegitcommit: cf23d382eee2431a3958b1c87c897b270587bde0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/29/2020
ms.locfileid: "87400659"
---
# <a name="quickstart-get-started-with-terraform-using-powershell"></a>Avvio rapido: Introduzione a Terraform con PowerShell
 
[!INCLUDE [terraform-intro.md](includes/terraform-intro.md)]

Questo articolo descrive come iniziare a usare [Terraform in Azure](https://www.terraform.io/docs/providers/azurerm/index.html) con PowerShell.

[!INCLUDE [hashicorp-support.md](includes/hashicorp-support.md)]

## <a name="prerequisites"></a>Prerequisiti

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../includes/open-source-devops-prereqs-azure-subscription.md)]

## <a name="configure-your-environment"></a>Configurare l'ambiente

1. Il modulo di PowerShell più recente che consente l'interazione con le risorse di Azure è denominato [Az di Azure PowerShell](https://docs.microsoft.com/powershell/azure/new-azureps-module-az). Quando si usa il modulo Az di Azure PowerShell, la versione consigliata è PowerShell 7 (o successiva) in tutte le piattaforme. Se PowerShell è installato, è possibile verificare la versione immettendo il comando seguente al prompt di PowerShell.

    ```powershell
    $PSVersionTable.PSVersion
    ```

1. [Installare PowerShell](https://docs.microsoft.com/powershell/scripting/install/installing-powershell-core-on-windows?view=powershell-7). Questa demo è stata testata con PowerShell 7.0.2 in Windows 10.

1. Per eseguire l'[autenticazione di Terraform in Azure](https://www.terraform.io/docs/providers/azurerm/guides/azure_cli.html), è necessario [installare l'interfaccia della riga di comando di Azure](/cli/azure/install-azure-cli-windows?view=azure-cli-latest). Questa demo è stata testata con la versione 2.9.1 dell'interfaccia della riga di comando di Azure.

1. [Scaricare Terraform](https://www.terraform.io/downloads.html).

1. Dal download estrarre il file eseguibile in una directory a scelta.

1. [Aggiornare il percorso globale del sistema](https://stackoverflow.com/questions/1618280/where-can-i-set-path-to-make-exe-on-windows) con il file eseguibile.

1. Verificare la configurazione del percorso globale con il comando `terraform`.

    ```powershell
    terraform
    ```

    **Note**:
    - Se viene trovato l'eseguibile di Terraform, viene elencata la sintassi insieme ai comandi disponibili.

## <a name="create-an-azure-service-principal"></a>Creare un'entità servizio di Azure

Se si usano PowerShell e Terraform, è necessario accedere tramite un'entità servizio.

per accedere a una sottoscrizione di Azure usando un'entità servizio, occorre prima di tutto avere accesso a un'entità servizio. Se si ha già un'entità servizio, è possibile ignorare questa sezione.

Sono disponibili diverse opzioni per [creare un'entità servizio con PowerShell](https://docs.microsoft.com/powershell/azure/create-azure-service-principal-azureps). Per questo articolo si creerà un'entità servizio con il ruolo **Collaboratore**. Il ruolo **Collaboratore** (il ruolo predefinito) ha autorizzazioni complete per operazioni di lettura e scrittura in un account Azure. Per altre informazioni sul controllo degli accessi in base al ruolo e i ruoli, vedere [Controllo degli accessi in base al ruolo: ruoli predefiniti](/azure/active-directory/role-based-access-built-in-roles).

Chiamando [New-AzADServicePrincipal](https://docs.microsoft.com/powershell/module/Az.Resources/New-AzADServicePrincipal) viene creata un'entità servizio per la sottoscrizione specificata. Al termine vengono visualizzate le informazioni dell'entità servizio, come i nomi dell'entità servizio e il nome visualizzato. Se si chiama `New-AzADServicePrincipal` senza specificare le credenziali di autenticazione, viene generata automaticamente una password. Questa password non viene però visualizzata poiché viene restituita come tipo `SecureString`. Occorre pertanto chiamare `New-AzADServicePrincipal`, i cui risultati vengono passati a una variabile. È quindi possibile convertire la variabile in testo normale per visualizzarla.

1. Ottenere l'ID della sottoscrizione di Azure da usare. Se non si conosce l'ID sottoscrizione, è possibile ottenere il valore nel [portale di Azure](https://portal.azure.com/).

    1. Accedere al [portale di Azure](https://portal.azure.com/).
    1. In **Servizi di Azure** selezionare **Sottoscrizioni**.
    1. L'elenco delle sottoscrizioni della tabella contiene una colonna con l'ID di ogni sottoscrizione.

1. Avviare PowerShell.

1. Creare una nuova entità servizio usando [New-AzADServicePrincipal](https://docs.microsoft.com/powershell/module/az.resources/new-azadserviceprincipal). Sostituire `<azure_subscription_id>` con l'ID della sottoscrizione di Azure da usare.

    ```powershell
    $sp = New-AzADServicePrincipal -Scope /subscriptions/<azure_subscription_id>
    ```

1. Visualizzare i nomi dell'entità servizio.

    ```powershell
    $sp.ServicePrincipalNames
    ```

1. Visualizzare la password generata automaticamente come testo, [ConvertFrom-SecureString](https://docs.microsoft.com/powershell/module/microsoft.powershell.security/convertfrom-securestring).

    ```powershell
    $UnsecureSecret = ConvertFrom-SecureString -SecureString $sp.Secret -AsPlainText
    ```

**Note**:

- I valori di nome e password dell'entità servizio sono necessari per accedere alla sottoscrizione tramite l'entità servizio.
- Se viene persa, questa password non può essere recuperata. Di conseguenza, è consigliabile archiviarla in un luogo sicuro. Se si dimentica la password, è necessario [reimpostare le credenziali dell'entità servizio](https://docs.microsoft.com/powershell/azure/create-azure-service-principal-azureps#reset-credentials).

## <a name="log-in-to-azure-using-a-service-principal"></a>Accedere ad Azure tramite un'entità servizio

Per accedere a una sottoscrizione di Azure tramite un'entità servizio, chiamare [Connect-AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/Connect-AzAccount) specificando un oggetto di tipo [PsCredential](https://docs.microsoft.com/dotnet/api/system.management.automation.pscredential).

1. Avviare PowerShell.

1. Ottenere un oggetto [PsCredential](https://docs.microsoft.com/dotnet/api/system.management.automation.pscredential) usando una delle tecniche seguenti.

    1. Chiamare [Get-Credential](https://docs.microsoft.com/powershell/module/microsoft.powershell.security/get-credential) e immettere il nome e la password di un'entità servizio quando richiesto:

        ```powershell
        $psCredential = Get-Credential
        ```

    1. Creare un oggetto `PsCredential` in memoria. Sostituire i segnaposto con i valori appropriati per l'entità servizio. Questo modello è il modo in cui si accede da uno script.

        ```powershell
        $spName = "<service_principle_name>"
        $spPassword = ConvertTo-SecureString "<service_principle_password>" -AsPlainText -Force
        $spCredential = New-Object System.Management.Automation.PSCredential($spName , $spPassword)
        ```

1. Chiamare `Connect-AzAccount` passando l'oggetto `PsCredential`. Sostituire il segnaposto `<azure_subscription_tenant_id>` con l'ID tenant della sottoscrizione di Azure.

    ```powershell
    Connect-AzAccount -Credential $spCredential -Tenant "<azure_subscription_tenant_id>" -ServicePrincipal
    ```

## <a name="create-a-terraform-configuration-file"></a>Creare un file di configurazione Terraform

In questa sezione verrà scritto il codice di un file di configurazione di Terraform che crea un gruppo di risorse di Azure.

1. Creare una directory in cui conservare i file di Terraform per questa demo.

    ```powershell
    mkdir QuickstartTerraformTest
    ```

1. Passare alla directory demo.

    ```powershell
    cd QuickstartTerraformTest
    ```

1. Usando l'editor preferito, creare un file di configurazione di Terraform. Questo articolo usa [Visual Studio Code](https://code.visualstudio.com/Download).

    ```powershell
    code QuickstartTerraformTest.tf
    ```

1. Incollare il codice HCL seguente nel nuovo file. Per altre informazioni, vedere le note dopo il codice.

    ```hcl
    provider "azurerm" {
      # The "feature" block is required for AzureRM provider 2.x.
      # If you're using version 1.x, the "features" block isn't allowed.
      version = "~>2.0"
      features {}
    }

    resource "azurerm_resource_group" "rg" {
      name     = "QuickstartTerraformTest-rg"
      location = "eastus"
    }
    ```

    **Note**:
    - Il blocco del provider specifica che viene usato il [provider di Azure (azurerm)](https://www.terraform.io/docs/providers/azurerm/index.html).
    - All'interno del blocco del provider `azurerm` vengono impostati gli attributi `version` e `features`. Come indicato nel commento, l'utilizzo di questi attributo dipende dalla versione. Per altre informazioni su come impostare questi attributi, vedere la [versione 2.0 del provider AzureRM](https://www.terraform.io/docs/providers/azurerm/guides/2.0-upgrade-guide.html).
    - L'unica [dichiarazione di risorse](https://www.terraform.io/docs/configuration/resources.html) è relativa a un tipo di risorsa di [azurerm_resource_group](https://www.terraform.io/docs/providers/azurerm/r/resource_group.html). I due argomenti obbligatori per azure_resource_group sono name e location.

## <a name="set-environment-variables"></a>Impostare le variabili di ambiente

Affinché Terraform usi la sottoscrizione di Azure prevista, impostare le variabili di ambiente. È possibile impostare le variabili di ambiente a livello di sistema Windows o all'interno di una sessione di PowerShell specifica. Per impostare le variabili di ambiente per una sessione specifica, usare il codice seguente. Sostituire i segnaposto con i valori appropriati per l'ambiente.

```powershell
$env:ARM_CLIENT_ID=<service_principle_app_id>
$env:ARM_SUBSCRIPTION_ID=<azure_subscription_id>
$env:ARM_TENANT_ID=<azure_subscription_tenant_id>
```

## <a name="create-and-apply-a-terraform-execution-plan"></a>Creare e applicare un piano di esecuzione di Terraform

In questa sezione viene creato un *piano di esecuzione*, che viene applicato all'infrastruttura cloud.

1. Inizializzare la distribuzione di Terraform con [terraform init](https://www.terraform.io/docs/commands/init.html). Questo passaggio scarica i moduli di Azure necessari per creare un gruppo di risorse di Azure.

    ```powershell
    terraform init
    ```

1. Eseguire [terraform plan](https://www.terraform.io/docs/commands/plan.html) per creare un piano di esecuzione dal file di configurazione di Terraform.

    ```powershell
    terraform plan -out QuickstartTerraformTest.tfplan
    ```

    **Note:**
    - Il comando `terraform plan` consente di creare un piano di esecuzione, ma non di eseguirlo. Determina invece le azioni necessarie per creare la configurazione specificata nei file di configurazione. Questo modello consente di verificare se il piano di esecuzione corrisponde alle aspettative prima di apportare modifiche alle risorse effettive.
    - Il parametro `-out` facoltativo consente di specificare un file di output per il piano. L'uso del parametro `-out` garantisce che il piano esaminato sia esattamente quello che viene applicato.
    - Per altre informazioni su come rendere persistenti i piani di esecuzione e la sicurezza, vedere la [sezione relativa agli avvisi di sicurezza](https://www.terraform.io/docs/commands/plan.html#security-warning).

1. Eseguire [terraform apply](https://www.terraform.io/docs/commands/apply.html) per applicare il piano di esecuzione.

    ```powershell
    terraform apply QuickstartTerraformTest.tfplan
    ```

1. Una volta applicato il piano di esecuzione, è possibile verificare se il gruppo di risorse è stato creato correttamente usando [Get-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/Get-AzResourceGroup).

    ```powershell
    Get-AzResourceGroup -Name QuickstartTerraformTest-rg
    ```

    Se l'operazione è riuscita, il comando visualizza diverse proprietà del gruppo di risorse appena creato.

## <a name="clean-up-resources"></a>Pulire le risorse

Quando non sono più necessarie, eliminare le risorse create in questo articolo.

1. Eseguire [terraform plan](https://www.terraform.io/docs/commands/plan.html) per creare un piano di esecuzione in modo da eliminare definitivamente le risorse indicate nel file di configurazione di Terraform.

    ```powershell
    terraform plan -destroy -out QuickstartTerraformTest.destroy.tfplan
    ```

    **Note:**
    - Il comando `terraform plan` consente di creare un piano di esecuzione, ma non di eseguirlo. Determina invece le azioni necessarie per creare la configurazione specificata nei file di configurazione. Questo modello consente di verificare se il piano di esecuzione corrisponde alle aspettative prima di apportare modifiche alle risorse effettive.
    - Il parametro `-destroy` genera un piano per eliminare definitivamente le risorse.
    - Il parametro `-out` facoltativo consente di specificare un file di output per il piano. L'uso del parametro `-out` garantisce che il piano esaminato sia esattamente quello che viene applicato.
    - Per altre informazioni su come rendere persistenti i piani di esecuzione e la sicurezza, vedere la [sezione relativa agli avvisi di sicurezza](https://www.terraform.io/docs/commands/plan.html#security-warning).

1. Eseguire [terraform apply](https://www.terraform.io/docs/commands/apply.html) per applicare il piano di esecuzione.

    ```powershell
    terraform apply QuickstartTerraformTest.destroy.tfplan
    ```

1. Verificare se il gruppo di risorse è stato eliminato usando [Get-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/Get-AzResourceGroup).

    ```powershell
    Get-AzResourceGroup -Name QuickstartTerraformTest-rg
    ```

    **Note**:
    - Se l'operazione riesce, `Get-AzResourceGroup` visualizza un messaggio per indicare che il gruppo di risorse non esiste.

1. Passare alla directory padre e rimuovere la directory demo. Il parametro `-r` rimuove il contenuto della directory prima di rimuovere la directory. Il contenuto della directory include il file di configurazione creato in precedenza e i file di stato di Terraform.

    ```powershell
    cd .. && rm -r QuickstartTerraformTest
    ```

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"]
> [Creare una macchina virtuale di Azure con Terraform](create-linux-virtual-machine-with-infrastructure.md)