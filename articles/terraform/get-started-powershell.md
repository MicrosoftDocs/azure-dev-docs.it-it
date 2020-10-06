---
title: Avvio rapido - Configurare Terraform con Azure PowerShell
description: Questo argomento di avvio rapido illustra come installare e configurare Terraform usando Azure PowerShell.
keywords: azure devops terraform installazione configurazione windows inizializzazione piano applicare esecuzione portale accesso controllo degli accessi in base al ruolo entità servizio script automatizzato powershell
ms.topic: quickstart
ms.date: 09/27/2020
ms.custom: devx-track-terraform
ms.openlocfilehash: 8f95d0bb09d7e9e7ea789b90a27178cdf5426d74
ms.sourcegitcommit: e20f6c150bfb0f76cd99c269fcef1dc5ee1ab647
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/28/2020
ms.locfileid: "91401551"
---
# <a name="quickstart-configure-terraform-using-azure-powershell"></a>Avvio rapido: Configurare Terraform con Azure PowerShell
 
[!INCLUDE [terraform-intro.md](includes/terraform-intro.md)]

Questo articolo descrive come iniziare a usare [Terraform in Azure](https://www.terraform.io/docs/providers/azurerm/index.html) con PowerShell.

In questo articolo vengono illustrate le operazioni seguenti:
> [!div class="checklist"]
> * Installare la versione più recente di PowerShell
> * Installare il nuovo modulo Az di PowerShell
> * Installare l'interfaccia della riga di comando di Azure
> * Installazione di Terraform
> * Creare un'entità servizio di Azure per finalità di autenticazione
> * Accedere ad Azure con un'entità servizio 
> * Configurare le variabili di ambiente in modo che Terraform esegua correttamente l'autenticazione nella sottoscrizione di Azure
> * Creare un file di configurazione di base di Terraform
> * Creare e applicare un piano di esecuzione di Terraform
> * Invertire un piano di esecuzione

## <a name="prerequisites"></a>Prerequisiti

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../includes/open-source-devops-prereqs-azure-subscription.md)]

## <a name="configure-your-environment"></a>Configurare l'ambiente

1. Il modulo di PowerShell più recente che consente l'interazione con le risorse di Azure è denominato [Az di Azure PowerShell](/powershell/azure/new-azureps-module-az). Quando si usa il modulo Az di Azure PowerShell, la versione consigliata è PowerShell 7 (o successiva) in tutte le piattaforme. Se PowerShell è installato, è possibile verificare la versione immettendo il comando seguente al prompt di PowerShell.

    ```powershell
    $PSVersionTable.PSVersion
    ```

1. [Installare PowerShell](/powershell/scripting/install/installing-powershell-core-on-windows). Questa demo è stata testata con PowerShell 7.0.2 in Windows 10.

1. Per eseguire l'[autenticazione di Terraform in Azure](https://www.terraform.io/docs/providers/azurerm/guides/azure_cli.html), è necessario [installare l'interfaccia della riga di comando di Azure](/cli/azure/install-azure-cli-windows). Questa demo è stata testata con la versione 2.9.1 dell'interfaccia della riga di comando di Azure.

1. [Scaricare Terraform](https://www.terraform.io/downloads.html).

1. Dal download estrarre il file eseguibile in una directory a scelta.

1. [Aggiornare il percorso globale del sistema](https://stackoverflow.com/questions/1618280/where-can-i-set-path-to-make-exe-on-windows) con il file eseguibile.

1. Verificare la configurazione del percorso globale con il comando `terraform`.

    ```powershell
    terraform
    ```

    **Note**:
    - Se viene trovato l'eseguibile di Terraform, viene elencata la sintassi insieme ai comandi disponibili.

## <a name="authenticate-to-azure"></a>Eseguire l'autenticazione ad Azure

Se si usano PowerShell e Terraform, è necessario accedere tramite un'entità servizio. Le due sezioni successive illustrano le attività seguenti:

- [Creare un'entità servizio di Azure](#create-an-azure-service-principal)
- [Accedere ad Azure tramite un'entità servizio](#log-in-to-azure-using-a-service-principal)


### <a name="span-idcreate-an-azure-service-principalcreate-an-azure-service-principal"></a><span id="create-an-azure-service-principal"/>Creare un'entità servizio di Azure

per accedere a una sottoscrizione di Azure usando un'entità servizio, occorre prima di tutto avere accesso a un'entità servizio. Se si ha già un'entità servizio, è possibile ignorare questa sezione.

Sono disponibili diverse opzioni per [creare un'entità servizio con PowerShell](/powershell/azure/create-azure-service-principal-azureps). Per questo articolo si creerà un'entità servizio con il ruolo **Collaboratore**. Il ruolo **Collaboratore** (il ruolo predefinito) ha autorizzazioni complete per operazioni di lettura e scrittura in un account Azure. Per altre informazioni sul controllo degli accessi in base al ruolo e i ruoli, vedere [Controllo degli accessi in base al ruolo: ruoli predefiniti](/azure/active-directory/role-based-access-built-in-roles).

Chiamando [New-AzADServicePrincipal](/powershell/module/Az.Resources/New-AzADServicePrincipal) viene creata un'entità servizio per la sottoscrizione specificata. Al termine vengono visualizzate le informazioni dell'entità servizio, come i nomi dell'entità servizio e il nome visualizzato. Se si chiama `New-AzADServicePrincipal` senza specificare le credenziali di autenticazione, viene generata automaticamente una password. Questa password non viene però visualizzata poiché viene restituita come tipo `SecureString`. Occorre pertanto chiamare `New-AzADServicePrincipal`, i cui risultati vengono passati a una variabile. È quindi possibile convertire la variabile in testo normale per visualizzarla.

1. Ottenere l'ID della sottoscrizione di Azure da usare. Se non si conosce l'ID sottoscrizione, è possibile ottenere il valore nel [portale di Azure](https://portal.azure.com/).

    1. Accedere al [portale di Azure](https://portal.azure.com/).
    1. In **Servizi di Azure** selezionare **Sottoscrizioni**.
    1. L'elenco delle sottoscrizioni della tabella contiene una colonna con l'ID di ogni sottoscrizione.

1. Avviare PowerShell.

1. Creare una nuova entità servizio usando [New-AzADServicePrincipal](/powershell/module/az.resources/new-azadserviceprincipal). Sostituire `<azure_subscription_id>` con l'ID della sottoscrizione di Azure da usare.

    ```powershell
    $sp = New-AzADServicePrincipal -Scope /subscriptions/<azure_subscription_id>
    ```

1. Visualizzare i nomi dell'entità servizio.

    ```powershell
    $sp.ServicePrincipalNames
    ```

1. Visualizzare la password generata automaticamente come testo, [ConvertFrom-SecureString](/powershell/module/microsoft.powershell.security/convertfrom-securestring).

    ```powershell
    $UnsecureSecret = ConvertFrom-SecureString -SecureString $sp.Secret -AsPlainText
    ```

**Note**:

- I valori di nome e password dell'entità servizio sono necessari per accedere alla sottoscrizione tramite l'entità servizio.
- Se viene persa, questa password non può essere recuperata. Di conseguenza, è consigliabile archiviarla in un luogo sicuro. Se si dimentica la password, è necessario [reimpostare le credenziali dell'entità servizio](/powershell/azure/create-azure-service-principal-azureps#reset-credentials).

### <a name="span-idlog-in-to-azure-using-a-service-principallog-in-to-azure-using-a-service-principal"></a><span id="log-in-to-azure-using-a-service-principal"/>Accedere ad Azure tramite un'entità servizio

Per accedere a una sottoscrizione di Azure tramite un'entità servizio, chiamare [Connect-AzAccount](/powershell/module/az.accounts/Connect-AzAccount) specificando un oggetto di tipo [PsCredential](/dotnet/api/system.management.automation.pscredential).

1. Avviare PowerShell.

1. Ottenere un oggetto [PsCredential](/dotnet/api/system.management.automation.pscredential) usando una delle tecniche seguenti.

    1. Chiamare [Get-Credential](/powershell/module/microsoft.powershell.security/get-credential) e immettere il nome e la password di un'entità servizio quando richiesto:

        ```powershell
        $spCredential = Get-Credential
        ```

    1. Creare un oggetto `PsCredential` in memoria. Sostituire i segnaposto con i valori appropriati per l'entità servizio. Questo modello è il modo in cui si accede da uno script.

        ```powershell
        $spName = "<service_principal_name>"
        $spPassword = ConvertTo-SecureString "<service_principal_password>" -AsPlainText -Force
        $spCredential = New-Object System.Management.Automation.PSCredential($spName , $spPassword)
        ```

1. Chiamare `Connect-AzAccount` passando l'oggetto `PsCredential`. Sostituire il segnaposto `<azure_subscription_tenant_id>` con l'ID tenant della sottoscrizione di Azure.

    ```powershell
    Connect-AzAccount -Credential $spCredential -Tenant "<azure_subscription_tenant_id>" -ServicePrincipal
    ```

## <a name="set-environment-variables"></a>Impostare le variabili di ambiente

Affinché Terraform usi la sottoscrizione di Azure prevista, impostare le variabili di ambiente. È possibile impostare le variabili di ambiente a livello di sistema Windows o all'interno di una sessione di PowerShell specifica. Per impostare le variabili di ambiente per una sessione specifica, usare il codice seguente. Sostituire i segnaposto con i valori appropriati per l'ambiente.

```powershell
$env:ARM_CLIENT_ID="<service_principal_app_id>"
$env:ARM_SUBSCRIPTION_ID="<azure_subscription_id>"
$env:ARM_TENANT_ID="<azure_subscription_tenant_id>"
```

[!INCLUDE [terraform-create-base-config-file.md](includes/terraform-create-base-config-file.md)]

[!INCLUDE [terraform-create-and-apply-execution-plan.md](includes/terraform-create-and-apply-execution-plan.md)]

[!INCLUDE [terraform-reverse-execution-plan.md](includes/terraform-reverse-execution-plan.md)]

[!INCLUDE [terraform-troubleshooting.md](includes/terraform-troubleshooting.md)]

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"]
> [Creare una macchina virtuale Linux usando Terraform](create-linux-virtual-machine-with-infrastructure.md)
