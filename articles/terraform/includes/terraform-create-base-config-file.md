---
title: includere file
description: File di inclusione
author: tomarchermsft
ms.service: terraform
ms.topic: include
ms.date: 02/18/2021
ms.author: tarcher
ms.openlocfilehash: 2cecafe9581835adf23ffffcc98c63fb6529cb7c
ms.sourcegitcommit: b0a119a624e9cb6b76d968951543a414bd08eaa0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/08/2021
ms.locfileid: "102473783"
---
## <a name="create-a-base-terraform-configuration-file"></a>Creare un file di configurazione di base di Terraform

Un file di configurazione di Terraform inizia con la specifica del provider. Quando si usa Azure, si specifica il [provider Azure (azurerm)](https://www.terraform.io/docs/providers/azurerm/index.html) nel blocco `provider`.

```terraform
terraform {
  required_providers {
    azurerm = {
      source = "hashicorp/azurerm"
      version = "~>2.0"
    }
  }
}
provider "azurerm" {
  features {}
}
resource "azurerm_resource_group" "rg" {
  name = "<resource_group_name>"
  location = "<location>"
}

# Your Terraform code goes here...

```

**Note**:

- Anche se l'attributo `version` è facoltativo, HashiCorp consiglia di aggiungerlo a una versione specifica del provider. 
- Se si usa il provider di Azure 1. x, il `features` blocco non è consentito.
- Se si usa Azure Provider 2. x, il `features` blocco è obbligatorio.
- La [dichiarazione della risorsa](https://www.terraform.io/docs/configuration/resources.html) di [azurerm_resource_group](https://www.terraform.io/docs/providers/azurerm/r/resource_group.html) ha due argomenti: `name` e `location`. Impostare i segnaposto sui valori appropriati per l'ambiente.
- Il [valore denominato locale](https://www.terraform.io/docs/configuration/expressions.html#references-to-named-values) di `rg` per il gruppo di risorse viene usato in tutti gli articoli dedicati a procedure ed esercitazioni quando si fa riferimento al gruppo di risorse. Questo valore è indipendente dal nome del gruppo di risorse e fa riferimento solo al nome della variabile nel codice. Se si modifica questo valore nella definizione del gruppo di risorse, sarà necessario modificarlo anche nel codice che vi fa riferimento.
