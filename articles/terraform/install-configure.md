---
title: 'Avvio rapido: Installare e configurare Terraform per il provisioning delle risorse di Azure'
description: Informazioni su come installare e configurare Terraform per la creazione di risorse di Azure.
keywords: azure devops terraform installare configurare
ms.topic: quickstart
ms.date: 04/26/2020
ms.openlocfilehash: e395227171417ccf73ef073a6067732fb11a418d
ms.sourcegitcommit: 756e4873f904db954a56c20ebb2f1f5116ee4596
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/05/2020
ms.locfileid: "82169737"
---
# <a name="quickstart-install-and-configure-terraform-to-provision-azure-resources"></a>Guida introduttiva: Installare e configurare Terraform per il provisioning delle risorse di Azure
 
Terraform offre un modo semplice per definire, visualizzare in anteprima e distribuire l'infrastruttura cloud usando un [linguaggio di creazione modelli semplice](https://www.terraform.io/docs/configuration/syntax.html). Questo articolo descrive la procedura da seguire per usare Terraform per effettuare il provisioning di risorse in Azure.

[!INCLUDE [hashicorp-support.md](includes/hashicorp-support.md)]

## <a name="prerequisites"></a>Prerequisiti

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../includes/open-source-devops-prereqs-azure-subscription.md)]

## <a name="install-terraform"></a>Installazione di Terraform

Per impostazione predefinita, la versione più recente di Terraform viene installata per l'uso in [Azure Cloud Shell](/azure/cloud-shell/overview). Se si sceglie di installare Terraform in locale, completare questo passaggio; in caso contrario, passare a [Configurare l'accesso di Terraform in Azure](#configure-terraform-access-to-azure).

1. [Installare Terraform](https://www.terraform.io/downloads.html) specificando il pacchetto appropriato per il sistema operativo in uso.
1. Il download contiene un singolo file eseguibile. Definire un percorso globale del file eseguibile in base al sistema operativo in uso:
    - [Linux o macOS](https://stackoverflow.com/questions/14637979/how-to-permanently-set-path-on-linux)
    - [Windows](https://stackoverflow.com/questions/1618280/where-can-i-set-path-to-make-exe-on-windows).
1. Verificare la configurazione del percorso globale con il comando `terraform`. Se Terraform viene rilevato ed eseguito, viene visualizzato l'elenco di opzioni disponibili:

    ```console
    azureuser@Azure:~$ terraform
    Usage: terraform [--version] [--help] <command> [args]
    ```
    
## <a name="configure-terraform-access-to-azure"></a>Configurare l'accesso di Terraform ad Azure

Per consentire a Terraform di eseguire il provisioning di risorse in Azure, creare un'[entità servizio di Azure AD](/cli/azure/create-an-azure-service-principal-azure-cli). L'entità servizio concede agli script Terraform la possibilità di effettuare il provisioning di risorse nella sottoscrizione di Azure.

Se si hanno più sottoscrizioni di Azure, eseguire prima una query sull'account con [az account list](/cli/azure/account#az-account-list) per ottenere un elenco di valori di ID sottoscrizione e ID tenant:

```azurecli-interactive
az account list --query "[].{name:name, subscriptionId:id, tenantId:tenantId}"
```

Per usare una sottoscrizione selezionata, impostare la sottoscrizione per questa sessione con [az account set](/cli/azure/account#az-account-set). Impostare la`SUBSCRIPTION_ID` variabile di ambiente che conterrà il valore del campo restituito`id` dalla sottoscrizione che si intende usare:

```azurecli-interactive
az account set --subscription="${SUBSCRIPTION_ID}"
```

A questo punto è possibile creare un'entità servizio per l'uso con Terraform. Usare [az ad sp create-for-rbac](/cli/azure/ad/sp#az-ad-sp-create-for-rbac) e impostare l'*ambito* alla sottoscrizione come indicato di seguito:

```azurecli-interactive
az ad sp create-for-rbac --role="Contributor" --scopes="/subscriptions/${SUBSCRIPTION_ID}"
```

Vengono restituiti i valori `appId`, `password`, `sp_name` e `tenant`. Prendere nota di `appId` e `password`.

## <a name="configure-terraform-environment-variables"></a>Configurare le variabili di ambiente di Terraform

Per configurare Terraform per l'uso dell'entità servizio di Azure AD, impostare le variabili di ambiente seguenti che saranno quindi usate dai [moduli Terraform per Azure](https://registry.terraform.io/modules/Azure). È anche possibile impostare l'ambiente se si usa un cloud di Azure diverso da Azure pubblico.

- `ARM_SUBSCRIPTION_ID`
- `ARM_CLIENT_ID`
- `ARM_CLIENT_SECRET`
- `ARM_TENANT_ID`
- `ARM_ENVIRONMENT`

Di seguito è riportato uno script della shell di esempio che è possibile usare per impostare tali variabili:

```bash
#!/bin/sh
echo "Setting environment variables for Terraform"
export ARM_SUBSCRIPTION_ID=your_subscription_id
export ARM_CLIENT_ID=your_appId
export ARM_CLIENT_SECRET=your_password
export ARM_TENANT_ID=your_tenant_id

# Not needed for public, required for usgovernment, german, china
export ARM_ENVIRONMENT=public
```

## <a name="run-a-sample-script"></a>Eseguire uno script di esempio

Creare un file `test.tf` in una directory vuota e incollare al suo interno lo script seguente.

```hcl
provider "azurerm" {
  # The "feature" block is required for AzureRM provider 2.x. 
  # If you are using version 1.x, the "features" block is not allowed.
  version = "~>2.0"
  features {}
}
resource "azurerm_resource_group" "rg" {
        name = "testResourceGroup"
        location = "westus"
}
```

Salvare il file e quindi inizializzare la distribuzione di Terraform. Questo passaggio scarica i moduli di Azure necessari per creare un gruppo di risorse di Azure.

```bash
terraform init
```

L'output è simile all'esempio seguente:

```console
* provider.azurerm: version = "~> 0.3"

Terraform has been successfully initialized!
```

È possibile visualizzare in anteprima le azioni da completare dallo script di Terraform con `terraform plan`. Quando si è pronti per creare il gruppo di risorse, applicare il piano Terraform come indicato di seguito:

```bash
terraform apply
```

L'output è simile all'esempio seguente:

```console
An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  + azurerm_resource_group.rg
      id:       <computed>
      location: "westus"
      name:     "testResourceGroup"
      tags.%:   <computed>

azurerm_resource_group.rg: Creating...
  location: "" => "westus"
  name:     "" => "testResourceGroup"
  tags.%:   "" => "<computed>"
azurerm_resource_group.rg: Creation complete after 1s
```

## <a name="next-steps"></a>Passaggi successivi

In questo articolo, viene installato Terraform o usato Cloud Shell per configurare le credenziali di Azure e iniziare la creazione di risorse nella sottoscrizione di Azure. Per creare una distribuzione più completa di Terraform in Azure, vedere l'articolo seguente:

> [!div class="nextstepaction"]
> [Creare una macchina virtuale di Azure con Terraform](create-linux-virtual-machine-with-infrastructure.md)