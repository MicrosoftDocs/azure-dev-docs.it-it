---
title: Esercitazione - Creare un cluster di macchine virtuali di Azure con Terraform con il registro modulo
description: Informazioni su come usare moduli Terraform per creare un cluster di macchine virtuali Windows in Azure.
keywords: azure devops terraform vm macchina virtuale cluster modulo registro
ms.topic: tutorial
ms.date: 03/09/2020
ms.openlocfilehash: b3b6c99afc47f7cf83d8f712ea0e0444faf08104
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/05/2020
ms.locfileid: "82171277"
---
# <a name="tutorial-create-an-azure-vm-cluster-with-terraform-using-the-module-registry"></a>Esercitazione: Creare un cluster di macchine virtuali di Azure con Terraform con il registro modulo

Questo articolo descrive in modo dettagliato come creare un piccolo cluster di macchine virtuali con il [modulo di calcolo di Azure](https://registry.terraform.io/modules/Azure/compute/azurerm/1.0.2) di Terraform. In questa esercitazione si apprenderà come: 

> [!div class="checklist"]
> * Configurare l'autenticazione con Azure
> * Creare il modello Terraform
> * Visualizzare le modifiche con il piano
> * Applicare la configurazione per creare il cluster di macchine virtuali

[!INCLUDE [hashicorp-support.md](includes/hashicorp-support.md)]

## <a name="prerequisites"></a>Prerequisiti

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../includes/open-source-devops-prereqs-azure-subscription.md)]

## <a name="set-up-authentication-with-azure"></a>Configurare l'autenticazione con Azure

> [!TIP]
> Se si [usano le variabili di ambiente di Terraform](install-configure.md) o si esegue questa esercitazione in [Azure Cloud Shell](/azure/cloud-shell/overview), ignorare questo passaggio.

 Vedere [Installare Terraform e configurare l'accesso ad Azure](install-configure.md) per creare un'entità servizio di Azure. Usare questa entità servizio per compilare un nuovo file `azureProviderAndCreds.tf` in una directory vuota con il codice seguente:

```hcl
variable subscription_id {}
variable tenant_id {}
variable client_id {}
variable client_secret {}

provider "azurerm" {
    version = "~>1.40"

    subscription_id = "xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    tenant_id = "xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    client_id = "xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    client_secret = "xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
}
```

## <a name="create-the-template"></a>Creare il modello

Creare un nuovo modello Terraform denominato `main.tf` con il codice seguente:

```hcl
module mycompute {
    source = "Azure/compute/azurerm"
    resource_group_name = "myResourceGroup"
    location = "East US 2"
    admin_password = "ComplxP@assw0rd!"
    vm_os_simple = "WindowsServer"
    is_windows_image = "true"
    remote_port = "3389"
    nb_instances = 2
    public_ip_dns = ["unique_dns_name"]
    vnet_subnet_id = module.network.vnet_subnets[0]
}

module "network" {
    source = "Azure/network/azurerm"
    location = "East US 2"
    resource_group_name = "myResourceGroup"
}

output "vm_public_name" {
    value = module.mycompute.public_ip_dns_name
}

output "vm_public_ip" {
    value = module.mycompute.public_ip_address
}

output "vm_private_ips" {
    value = module.mycompute.network_interface_private_ip
}
```

Eseguire `terraform init` nella directory di configurazione. Usando almeno la versione 0.10.6 di Terraform, viene restituito l'output seguente:

![Inizializzazione di Terraform](media/create-vm-cluster-module/terraform-init-with-modules.png)

## <a name="visualize-the-changes-with-plan"></a>Visualizzare le modifiche con il piano

Eseguire `terraform plan` per visualizzare in anteprima l'infrastruttura di macchine virtuali creata dal modello.

![Piano Terraform](media/create-vm-cluster-with-infrastructure/terraform-plan.png)


## <a name="create-the-virtual-machines-with-apply"></a>Creare le macchine virtuali con il comando apply

Eseguire `terraform apply` per effettuare il provisioning delle macchine virtuali in Azure.

![Comando apply di Terraform](media/create-vm-cluster-with-infrastructure/terraform-apply.png)

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"] 
> [Visualizzare l'elenco dei moduli Terraform per Azure](https://registry.terraform.io/modules/Azure)