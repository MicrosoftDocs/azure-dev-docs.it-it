---
title: File di inclusione
description: File di inclusione
author: tomarchermsft
ms.service: terraform
ms.topic: include
ms.date: 09/27/2020
ms.author: tarcher
ms.openlocfilehash: 9fcde450a19515c38eb6653d75e644ca23d08964
ms.sourcegitcommit: e20f6c150bfb0f76cd99c269fcef1dc5ee1ab647
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/28/2020
ms.locfileid: "91401554"
---
## <a name="create-a-base-terraform-configuration-file"></a>Creare un file di configurazione di base di Terraform

Un file di configurazione di Terraform inizia con la specifica del provider. Quando si usa Azure, si specifica il [provider Azure (azurerm)](https://www.terraform.io/docs/providers/azurerm/index.html) nel blocco `provider`.

```terraform
provider "azurerm" {
  version = "~>2.0"
  features {}
}

resource "azurerm_resource_group" "rg" {
  name = "<your_resource_group_name>"
  location = "<your_resource_group_location>"
}

# Your Terraform code goes here...

```

**Note**:

- Anche se l'attributo `version` è facoltativo, HashiCorp consiglia di aggiungerlo a una versione specifica del provider. 
- Se si usa il provider Azure 1.x, il blocco `features` non è consentito.
- Se si usa il provider Azure 2.x, il blocco `features` è obbligatorio.
- La [dichiarazione della risorsa](https://www.terraform.io/docs/configuration/resources.html) di [azurerm_resource_group](https://www.terraform.io/docs/providers/azurerm/r/resource_group.html) ha due argomenti: `name` e `location`. Impostare i segnaposto sui valori appropriati per l'ambiente.
- Il [valore denominato locale](https://www.terraform.io/docs/configuration/expressions.html#references-to-named-values) di `rg` per il gruppo di risorse viene usato in tutti gli articoli dedicati a procedure ed esercitazioni quando si fa riferimento al gruppo di risorse. Tale valore è indipendente dal nome del gruppo di risorse e fa riferimento solo al nome della variabile nel codice. Se si modifica questo valore nella definizione del gruppo di risorse, sarà necessario modificarlo anche nel codice che vi fa riferimento.
