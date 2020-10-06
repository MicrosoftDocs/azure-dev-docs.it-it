---
title: Configurare un cluster di macchine virtuali di Azure usando Terraform
description: Informazioni su come usare moduli Terraform per creare un cluster di macchine virtuali Windows in Azure.
keywords: azure devops terraform vm macchina virtuale cluster modulo registro
ms.topic: how-to
ms.date: 09/27/2020
ms.custom: devx-track-terraform
ms.openlocfilehash: 73f375090a2178b38b0fc7e0afd4eb8c6b514672
ms.sourcegitcommit: e20f6c150bfb0f76cd99c269fcef1dc5ee1ab647
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/28/2020
ms.locfileid: "91401586"
---
# <a name="configure-an-azure-vm-cluster-using-terraform"></a>Configurare un cluster di macchine virtuali di Azure usando Terraform

Questo articolo mostra un esempio di codice Terraform per la creazione di un cluster di macchine virtuali in Azure.

## <a name="prerequisites"></a>Prerequisiti

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../includes/open-source-devops-prereqs-azure-subscription.md)]

[!INCLUDE [terraform-configure-environment.md](includes/terraform-configure-environment.md)]

## <a name="configure-an-azure-vm-cluster"></a>Configurare un cluster di macchine virtuali di Azure

```hcl
module "windowsservers" {
  source              = "Azure/compute/azurerm"
  resource_group_name = azurerm_resource_group.rg.name
  is_windows_image    = true
  vm_hostname         = "mywinvm"                         // Line can be removed if only one VM module per resource group
  admin_password      = "ComplxP@ssw0rd!"                 // See note following code about storing passwords in config files
  vm_os_simple        = "WindowsServer"
  public_ip_dns       = ["winsimplevmips"]                // Change to a unique name per data center region
  vnet_subnet_id      = module.network.vnet_subnets[0]
    
  depends_on = [azurerm_resource_group.rg]
}

module "network" {
  source              = "Azure/network/azurerm"
  resource_group_name = azurerm_resource_group.rg.name
  subnet_prefixes     = ["10.0.1.0/24"]
  subnet_names        = ["subnet1"]

  depends_on = [azurerm_resource_group.rg]
}

output "windows_vm_public_name" {
  value = module.windowsservers.public_ip_dns_name
}

output "vm_public_ip" {
  value = module.windowsservers.public_ip_address
}

output "vm_private_ips" {
  value = module.windowsservers.network_interface_private_ip
}
```

**Note**:

- Nell'esempio di codice precedente alla variabile `admin_password` viene assegnato un valore letterale per motivi di semplicità. Esistono diversi modi per archiviare i dati sensibili, ad esempio le password. La decisione relativa a come proteggere i dati dipende da scelte individuali che tengono in considerazione il proprio ambiente specifico e la propria esperienza nell'esposizione di questi dati. Per avere un'idea del rischio, si pensi che l'archiviazione di un file come questo nel controllo del codice sorgente potrebbe consentire ad altri utenti di visualizzare la password. Per altre informazioni su questo argomento, vedere la documentazione di HashiCorp su come [dichiarare le variabili di input](https://www.terraform.io/docs/configuration/variables.html) e sulle tecniche per [gestire i dati sensibili (ad esempio, le password)](https://www.terraform.io/docs/state/sensitive-data.html).

[!INCLUDE [terraform-troubleshooting.md](includes/terraform-troubleshooting.md)]

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"] 
> [Vedere altre informazioni sull'uso di Terraform in Azure](/azure/terraform)
