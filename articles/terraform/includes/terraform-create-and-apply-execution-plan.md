---
title: File di inclusione
description: File di inclusione
author: tomarchermsft
ms.service: terraform
ms.topic: include
ms.date: 09/27/2020
ms.author: tarcher
ms.openlocfilehash: 23a5bbc2e6a7e364b3c6eab38bfd5e98b97348a6
ms.sourcegitcommit: e20f6c150bfb0f76cd99c269fcef1dc5ee1ab647
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/28/2020
ms.locfileid: "91401555"
---
## <a name="create-and-apply-a-terraform-execution-plan"></a>Creare e applicare un piano di esecuzione di Terraform

In questa sezione viene illustrato come creare un *piano di esecuzione*, che viene applicato all'infrastruttura cloud.

1. Per inizializzare la distribuzione di Terraform, eseguire [terraform init](https://www.terraform.io/docs/commands/init.html). Questo comando scarica i moduli di Azure necessari per creare un gruppo di risorse di Azure.

    ```cmd
    terraform init
    ```

1. Dopo l'inizializzazione, si crea un piano di esecuzione eseguendo [terraform plan](https://www.terraform.io/docs/commands/plan.html).

    ```cmd
    terraform plan -out <terraform_plan>.tfplan
    ```

    [!INCLUDE [terraform-plan-notes.md](terraform-plan-notes.md)]

1. Quando si è pronti per applicare il piano di esecuzione all'infrastruttura cloud, si esegue [terraform apply](https://www.terraform.io/docs/commands/apply.html).

    ```cmd
    terraform apply <terraform_plan>.tfplan
    ```
