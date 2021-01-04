---
title: Configurare un provider di attestazioni di Azure con Terraform
description: Informazioni su come usare Terraform per creare un provider di attestazioni in Azure.
keywords: azure devops terraform attestazione
ms.topic: how-to
ms.date: 11/08/2020
ms.custom: devx-track-terraform
ms.openlocfilehash: c42f2a00886bedf3e9f26566cb619c69bb5073b7
ms.sourcegitcommit: f723980ade4cbc13548a5d8ac3f3fa681b8a2dbd
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/16/2020
ms.locfileid: "97609396"
---
# <a name="configure-an-azure-attestation-policy-using-terraform"></a>Configurare un criterio di attestazione di Azure con Terraform

Questo articolo mostra un esempio di codice Terraform per la creazione di un [provider di attestazioni](https://docs.microsoft.com/azure/attestation/overview) in Azure.

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
