---
title: Configurare un provider di attestazioni di Azure con Terraform
description: Informazioni su come usare Terraform per creare un provider di attestazioni in Azure.
keywords: azure devops terraform attestazione
ms.topic: how-to
ms.date: 11/08/2020
ms.custom: devx-track-terraform
ms.openlocfilehash: c79c472da4604458475bd230a844e2d308c2bda1
ms.sourcegitcommit: 593d177cfb5f56f236ea59389e43a984da30f104
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/18/2021
ms.locfileid: "98561547"
---
# <a name="configure-an-azure-attestation-policy-using-terraform"></a>Configurare un criterio di attestazione di Azure con Terraform

Questo articolo mostra un esempio di codice Terraform per la creazione di un [provider di attestazioni](/azure/attestation/overview) in Azure.

## <a name="prerequisites"></a>Prerequisiti

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../includes/open-source-devops-prereqs-azure-subscription.md)]
- *Certificato di firma dei criteri*: un file che specifica un set di chiavi di firma attendibili sotto forma di file pem.

[!INCLUDE [terraform-configure-environment.md](includes/terraform-configure-environment.md)]

## <a name="configure-an-azure-attestation-provider"></a>Configurare un provider di attestazioni di Azure

```hcl
resource "azurerm_resource_group" "corpAttestation" {
  name                        = "corp-attestation"
  location                    = "westus"
}

resource "azurerm_attestation_provider" "corpAttestation" {
  name                              = "attestationprovider007"
  resource_group_name               = azurerm_resource_group.corpAttestation.name
  location                          = azurerm_resource_group.corpAttestation.location

  policy_signing_certificate_data   = file("./certs/cert.pem")
}
```

[!INCLUDE [terraform-troubleshooting.md](includes/terraform-troubleshooting.md)]

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"] 
> [Vedere altre informazioni sull'uso di Terraform in Azure](/azure/terraform)