---
title: File di inclusione
description: File di inclusione
author: tomarchermsft
ms.service: terraform
ms.topic: include
ms.date: 09/27/2020
ms.author: tarcher
ms.openlocfilehash: daa57dfc18cae3250c42265ebe8cbf7722e4235b
ms.sourcegitcommit: e20f6c150bfb0f76cd99c269fcef1dc5ee1ab647
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/28/2020
ms.locfileid: "91401587"
---
## <a name="configure-your-environment"></a>Configurare l'ambiente

A seconda dell'ambiente, installare e configurare Terraform:

- [Configurare Terraform con Azure Cloud Shell e l'interfaccia della riga di comando di Azure](../get-started-cloud-shell.md)
- [Configurare Terraform con Azure PowerShell](../get-started-powershell.md)

Gli articoli sulla configurazione illustrano anche come eseguire le attività seguenti:

- Creare un file di configurazione di base di Terraform. Il file include il [provider Azure (azurerm)](https://www.terraform.io/docs/providers/azurerm/index.html) nel blocco `provider` e definisce un [gruppo di risorse di Azure](/azure/azure-resource-manager/management/manage-resource-groups-portal#what-is-a-resource-group).
- Creare e applicare un piano di esecuzione di Terraform per "eseguire" il codice.
- Invertire un piano di esecuzione per eliminare le risorse dopo aver terminato di usarle.
