---
title: Avvio rapido - Introduzione all'uso di Terraform in Windows
description: Questo argomento di avvio rapido illustra come installare e configurare Terraform per la creazione di risorse di Azure.
keywords: azure devops terraform installazione configurazione windows inizializzazione piano applicare esecuzione portale accesso controllo degli accessi in base al ruolo entità servizio script automatizzato interfaccia della riga di comando powershell
ms.topic: quickstart
ms.date: 06/12/2020
ms.openlocfilehash: d28a79af9a59551153f297cebfb0c51ed2e72838
ms.sourcegitcommit: 2d6c9687b39e33a6b5e980d9a375c9f8f1f2cab7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/15/2020
ms.locfileid: "84783803"
---
# <a name="quickstart-getting-started-with-terraform-using-windows"></a>Avvio rapido: Introduzione all'uso di Terraform in Windows
 
[!INCLUDE [terraform-intro.md](includes/terraform-intro.md)]

## <a name="prerequisites"></a>Prerequisiti

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../includes/open-source-devops-prereqs-azure-subscription.md)]

## <a name="configure-your-environment"></a>Configurare l'ambiente

1. PowerShell 7 (o versione successiva) è la versione consigliata di PowerShell per l'uso con Azure PowerShell in tutte le piattaforme, incluso Windows. Se PowerShell è installato, è possibile verificare la versione immettendo il comando seguente al prompt dei comandi di PowerShell:

    ```powershell
    $PSVersionTable.PSVersion
    ```
    
1. Seguire le istruzioni nell'articolo [Installazione di PowerShell in Windows](https://docs.microsoft.com/powershell/scripting/install/installing-powershell-core-on-windows?view=powershell-7) per installare la versione più recente di PowerShell.

1. Questo articolo usa il [modulo Az di Azure PowerShell](https://docs.microsoft.com/powershell/azure/new-azureps-module-az). Seguire le istruzioni nell'articolo [Installare l'interfaccia della riga di comando di Azure in Windows](/cli/azure/install-azure-cli-windows?view=azure-cli-latest).

1. Aprire un'istanza della versione di PowerShell appena installata.

## <a name="log-into-azure"></a>Accedere ad Azure

È possibile accedere a una sottoscrizione di Azure in diversi modi.

- [Accedere all'account Microsoft](#log-into-your-microsoft-account)
- [Accedere con un'entità servizio di Azure](#log-into-azure-using-an-azure-service-principal)

### <a name="log-into-your-microsoft-account"></a>Accedere all'account Microsoft

Se si chiama [Connect-AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/Connect-AzAccount) senza parametri, vengono visualizzati un URL e un codice. Passare all'URL, immettere il codice e seguire le istruzioni per accedere ad Azure con l'account Microsoft. Dopo aver effettuato l'accesso, tornare al portale.

```powershell
Connect-AzAccount
```

Note:
- Dopo l'accesso, `Connect-AzAccount` visualizza la sottoscrizione di Azure predefinita associata all'account Microsoft connesso. Per informazioni su come passare a un'altra sottoscrizione di Azure, vedere la sezione [Specificare la sottoscrizione di Azure corrente](#specify-the-current-azure-subscription).

### <a name="log-into-azure-using-an-azure-service-principal"></a>Accedere ad Azure con un'entità servizio di Azure

**Creare un'entità servizio di Azure**: per accedere a una sottoscrizione di Azure usando un'entità servizio, occorre prima di tutto avere accesso a un'entità servizio. Se si ha già un'entità servizio, è possibile saltare questa parte della sezione.

Gli strumenti automatici che distribuiscono o usano servizi di Azure, come Terraform, devono sempre avere autorizzazioni limitate. Per evitare che le applicazioni accedano come utenti con privilegi completi, Azure offre le entità servizio. Ma che cosa accade se non si ha un'entità servizio con cui eseguire l'accesso? In un scenario di questo tipo è possibile accedere usando le credenziali utente e quindi creare un'entità servizio. Dopo aver creato l'entità servizio, è possibile utilizzarne le informazioni per i tentativi di accesso successivi.

Sono disponibili diverse opzioni per [creare un'entità servizio con PowerShell](https://docs.microsoft.com/powershell/azure/create-azure-service-principal-azureps). Per questo articolo si creerà un'entità servizio con il ruolo **Collaboratore**. Il ruolo **Collaboratore** (il ruolo predefinito) ha autorizzazioni complete per operazioni di lettura e scrittura in un account Azure. Per altre informazioni sul controllo degli accessi in base al ruolo e i ruoli, vedere [Controllo degli accessi in base al ruolo: ruoli predefiniti](/azure/active-directory/role-based-access-built-in-roles).

Chiamando [New-AzADServicePrincipal](https://docs.microsoft.com/powershell/module/Az.Resources/New-AzADServicePrincipal) viene creata un'entità servizio per la sottoscrizione specificata. Al termine vengono visualizzate le informazioni dell'entità servizio, come i nomi dell'entità servizio e il nome visualizzato. Se si chiama `New-AzADServicePrincipal` senza specificare le credenziali di autenticazione, viene generata automaticamente una password. Questa password non viene però visualizzata poiché viene restituita come tipo `SecureString`. Occorre pertanto chiamare `New-AzADServicePrincipal`, i cui risultati vengono passati a una variabile. Si può quindi eseguire una query sulla variabile per ottenere la password.

1. Immettere il comando seguente, sostituendo `<subscription_id>` con l'ID dell'account della sottoscrizione che si vuole usare.

    ```powershell
    $sp = New-AzADServicePrincipal -Scope /subscriptions/<subscription_id>
    ```

1. Immettere il comando seguente per visualizzare i nomi dell'entità servizio:

    ```powershell
    $sp.ServicePrincipalNames
    ```

1. Chiamare `ConvertFrom-SecureString` per visualizzare la password in formato testo:

    ```powershell
    $UnsecureSecret = ConvertFrom-SecureString -SecureString $sp.Secret -AsPlainText
    ```

Note:
- A questo punto si conoscono i nomi e la password dell'entità servizio. Questi valori sono necessari per accedere alla sottoscrizione usando l'entità servizio.
- Se viene persa, questa password non può essere recuperata. Di conseguenza, è consigliabile archiviarla in un luogo sicuro. Se si dimentica la password, è necessario [reimpostare le credenziali dell'entità servizio](https://docs.microsoft.com/powershell/azure/create-azure-service-principal-azureps#reset-credentials).

**Usare un'entità servizio di Azure per eseguire l'accesso**: per accedere a una sottoscrizione di Azure usando un'entità servizio, chiamare `Connect-AzAccount` e passare un oggetto di tipo [PsCredential](https://docs.microsoft.com/dotnet/api/system.management.automation.pscredential). Sono disponibili due opzioni: il modello interattivo e il modello script.

- **Modello interattivo**: chiamare [Get-Credential](https://docs.microsoft.com/powershell/module/microsoft.powershell.security/get-credential) e immettere le credenziali quando vengono chieste. La chiamata a `Get-Credential` restituisce un oggetto `PsCredential` che viene quindi passato a `Connect-AzAccount`.

    1. Chiamare `Get-Credential` e immettere manualmente il nome e la password di un'entità servizio:

        ```powershell
        $Credential = Get-Credential
        ```

    2. Chiamare `Connect-AzAccount` passando l'oggetto `PsCredential`. Sostituire il segnaposto `<azureSubscriptionTenantId>` con l'ID tenant della sottoscrizione di Azure.

        ```powershell
        Connect-AzAccount -Credential $Credential -Tenant <azureSubscriptionTenantId> -ServicePrincipal
        ```

- **Modello script**: creare un oggetto `PsCredential` e passarlo a `Connect-AzConnect`.

    1. Creare un oggetto `Get-Credential`. Sostituire i segnaposto con i valori appropriati per la sottoscrizione di Azure e l'entità servizio.

        ```powershell
        $spName = "<servicePrincipalName>"
        $spPassword = ConvertTo-SecureString "<servicePrincipalPassword>" -AsPlainText -Force
        $psCredential = New-Object System.Management.Automation.PSCredential($spName , $spPassword)
        ```

    1. Chiamare `Connect-AzAccount`, passando l'oggetto `PsCredential` costruito:

        ```powershell
        Connect-AzAccount -Credential $psCredential -TenantId "<azureSubscriptionTenantId>"  -ServicePrincipal
        ```

## <a name="specify-the-current-azure-subscription"></a>Specificare la sottoscrizione di Azure corrente

Come illustrato nella sezione precedente, per accedere ad Azure è possibile scegliere tra i due scenari seguenti:

- **Accedere con un account Microsoft**: un account Microsoft può essere associato a più sottoscrizioni di Azure, una delle quali è la sottoscrizione predefinita. La sottoscrizione predefinita è quella usata se si esegue l'accesso e non si passa a un'altra sottoscrizione.
- **Accedere con un'entità servizio di Azure**: un'entità servizio è specifica di una sottoscrizione di Azure. Tenere presente che le informazioni relative alla sottoscrizione vengono visualizzate quando si esegue l'accesso.

La procedura seguente si riferisce al primo scenario in cui si eseguono le attività seguenti:

- Visualizzare la sottoscrizione di Azure corrente
- Elencare tutte le sottoscrizioni di Azure disponibili per l'account Microsoft corrente
- Passare a un'altra sottoscrizione di Azure

1. Per visualizzare la sottoscrizione di Azure corrente, usare [Get-AzContext](https://docs.microsoft.com/powershell/module/az.accounts/get-azcontext).

    ```powershell
    Get-AzContext
    ```

1. Se si ha accesso a più sottoscrizioni di Azure attive, usare `Get-AzContext -ListAvailable`:

    ```powershell
    Get-AzContext -ListAvailable | Select Name
    ```

1. I nomi di contesto generati automaticamente possono essere complessi. Per rendere i nomi di contesto più leggibili e più semplici da usare come parametri, è possibile rinominare i contesti. Per rinominare un contesto si usa [Rename-AzContext](https://docs.microsoft.com/powershell/module/az.accounts/rename-azcontext).

    ```powershell
    Rename-AzContext -SourceName <current_context_name> -TargetName <new_context_Name>
    ```

1. Per usare una sottoscrizione di Azure specifica per la sessione di PowerShell corrente, usare [Select-AzContext](https://docs.microsoft.com/powershell/module/az.accounts/select-azcontext).

    ```powershell
    Select-AzContext <context_name>
    ```

## <a name="configure-terraform"></a>Configurare Terraform

1. [Scaricare Terraform](https://www.terraform.io/downloads.html).

1. Dal download estrarre il file eseguibile in una directory a scelta.

1. [Aggiornare il percorso globale del sistema](https://stackoverflow.com/questions/1618280/where-can-i-set-path-to-make-exe-on-windows) con il file eseguibile.

1. Verificare la configurazione del percorso globale con il comando `terraform`. Se viene trovato l'eseguibile di Terraform, viene visualizzato l'elenco delle opzioni disponibili:

    ```powershell
    terraform
    ```

    Se viene trovato l'eseguibile di Terraform, viene elencata la sintassi insieme ai comandi disponibili:

    ```output
    Usage: terraform [-version] [-help] <command> [args]

    The available commands for execution are listed below.
    The most common, useful commands are shown first, followed by
    less common or more advanced commands. If you're just getting
    started with Terraform, stick with the common commands. For the
    other commands, please read the help and docs before usage.
    ...
    ```

## <a name="create-a-terraform-configuration-file"></a>Creare un file di configurazione Terraform

Questa sezione illustra come creare un file di configurazione di Terraform che crea un gruppo di risorse di Azure.

1. Creare una directory in cui conservare i file di Terraform per questa demo.

1. Passare alla directory demo.

1. Usando l'editor preferito, creare un file di configurazione di Terraform denominato `QuickstartTerraformTest.tf`.

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
    - Il blocco del provider specifica che viene usato il [provider di Azure (azurerm)](https://www.terraform.io/docs/providers/azurerm/index.html).
    - All'interno del blocco del provider azurerm sono impostati gli attributi version e features. Come indicato nel commento, l'utilizzo di questi attributo dipende dalla versione. Per altre informazioni su come impostare questi attributi per l'ambiente in uso, vedere la [versione 2.0 del provider AzureRM](https://www.terraform.io/docs/providers/azurerm/guides/2.0-upgrade-guide.html).
    - L'unica [dichiarazione di risorse](https://www.terraform.io/docs/configuration/resources.html) è relativa a un tipo di risorsa di [azurerm_resource_group](https://www.terraform.io/docs/providers/azurerm/r/resource_group.html). I due argomenti obbligatori per azure_resource_group sono name e location.

## <a name="create-and-apply-a-terraform-execution-plan"></a>Creare e applicare un piano di esecuzione di Terraform

Una volta creati i file di configurazione, in questa sezione viene spiegato come creare un *piano di esecuzione* e applicarlo all'infrastruttura cloud.

1. Inizializzare la distribuzione di Terraform con [terraform init](https://www.terraform.io/docs/commands/init.html). Questo passaggio scarica i moduli di Azure necessari per creare un gruppo di risorse di Azure.

    ```powershell
    terraform init
    ```
    
1. Per visualizzare in anteprima le azioni da completare in Terraform, usare il comando [terraform plan](https://www.terraform.io/docs/commands/plan.html).

    ```powershell
    terraform plan
    ```

    **Note:**
    - Il comando `terraform plan` consente di creare un piano di esecuzione, ma non di eseguirlo. Determina invece le azioni necessarie per creare la configurazione specificata nei file di configurazione.
    - Con il comando `terraform plan` è possibile verificare se il piano di esecuzione corrisponde alle aspettative prima di apportare modifiche alle risorse effettive.
    - Il parametro `-out` facoltativo consente di specificare un file di output per il piano. Per altre informazioni sull'utilizzo del parametro `-out`, vedere la sezione [Salvare in modo permanente un piano di esecuzione per una distribuzione successiva](#persist-an-execution-plan-for-later-deployment).

1. Per applicare il piano di esecuzione, usare il comando [terraform apply](https://www.terraform.io/docs/commands/apply.html).

    ```powershell
    terraform apply
    ```
    
1. Terraform mostra gli effetti dell'applicazione del piano di esecuzione e chiede di confermare l'esecuzione. Per confermare il comando, immettere `yes` e premere **INVIO**.

1. Dopo aver confermato l'esecuzione della riproduzione, usare il comando [az group show](/cli/azure/group?view=azure-cli-latest#az-group-show) per verificare che il gruppo di risorse sia stato creato correttamente.

    ```powershell
    Get-AzResourceGroup -Name QuickstartTerraformTest-rg
    ```

    Se l'operazione è riuscita, il comando visualizza diverse proprietà del gruppo di risorse appena creato.

## <a name="persist-an-execution-plan-for-later-deployment"></a>Salvare in modo permanente un piano di esecuzione per una distribuzione successiva

Nella sezione precedente è stato illustrato come eseguire [terraform plan](https://www.terraform.io/docs/commands/plan.html) per creare un piano di esecuzione. È stato quindi illustrato come applicare il piano con il comando [terraform apply](https://www.terraform.io/docs/commands/apply.html). Questo modello funziona in modo ottimale quando i passaggi sono interattivi e sequenziali.

Per scenari più complessi, è possibile salvare in modo permanente il piano di esecuzione in un file, in modo da poterlo applicare in un secondo momento, o anche da un altro computer.

Se si usa questa funzionalità, è consigliabile leggere l'articolo sull'[esecuzione di Terraform nell'automazione](https://learn.hashicorp.com/terraform/development/running-terraform-in-automation).

Nei passaggi seguenti viene illustrato lo schema di base per l'uso di questa funzionalità:

1. Eseguire [terraform init](https://www.terraform.io/docs/commands/init.html).

    ```powershell
    terraform init
    ```

1. Eseguire `terraform plan` con il parametro `-out`.

    ```powershell
    terraform plan -out QuickstartTerraformTest.tfplan
    ```

1. Eseguire `terraform apply` specificando il nome del file del passaggio precedente.

    ```powershell
    terraform apply QuickstartTerraformTest.tfplan
    ```

Note:
- Per consentire l'uso con l'automazione, l'esecuzione di `terraform apply <filename>` non richiede la conferma.
- Se si decide di usare questa funzionalità, leggere la [sezione sugli avvisi di sicurezza](https://www.terraform.io/docs/commands/plan.html#security-warning).

## <a name="clean-up-resources"></a>Pulire le risorse

Quando non sono più necessarie, eliminare le risorse create in questo articolo.

1. Eseguire il comando [terraform destroy](https://www.terraform.io/docs/commands/destroy.html) per eliminare il piano di esecuzione corrente.

    ```powershell
    terraform destroy
    ```

1. Terraform mostra gli effetti dell'eliminazione del piano di esecuzione e chiede di confermare. Per confermare il comando, immettere `yes` e premere **INVIO**.

1. Dopo aver confermato l'esecuzione della riproduzione, l'output è simile all'esempio seguente. Eseguire [az group show](/cli/azure/group?view=azure-cli-latest#az-group-show) per verificare che il gruppo di risorse sia stato eliminato.

    ```powershell
    Get-AzResourceGroup -Name QuickstartTerraformTest-rg
    ```

    Note:
    - Se l'operazione riesce, `Get-AzResourceGroup` visualizza un messaggio per indicare che il gruppo di risorse non esiste.

1. Passare alla directory padre e rimuovere la directory demo. Il parametro `-r` rimuove il contenuto della directory prima di rimuovere la directory. Il contenuto della directory include il file di configurazione creato in precedenza e i file di stato di Terraform.

    ```bash
    cd .. && rm -r QuickstartTerraformTest
    ```

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"]
> [Creare una macchina virtuale di Azure con Terraform](create-linux-virtual-machine-with-infrastructure.md)