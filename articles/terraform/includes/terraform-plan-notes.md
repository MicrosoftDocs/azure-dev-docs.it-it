---
title: File di inclusione
description: File di inclusione
author: tomarchermsft
ms.service: terraform
ms.topic: include
ms.date: 09/27/2020
ms.author: tarcher
ms.openlocfilehash: 9f3d916bc37cc9d5f86c1f6b2738f419414e5816
ms.sourcegitcommit: e20f6c150bfb0f76cd99c269fcef1dc5ee1ab647
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/28/2020
ms.locfileid: "91401552"
---
  **Note:**

  - Il comando `terraform plan` consente di creare un piano di esecuzione, ma non di eseguirlo. Determina invece le azioni necessarie per creare la configurazione specificata nei file di configurazione. Questo modello consente di verificare se il piano di esecuzione corrisponde alle aspettative prima di apportare modifiche alle risorse effettive.
  - Il parametro `-out` facoltativo consente di specificare un file di output per il piano. L'uso del parametro `-out` garantisce che il piano esaminato sia esattamente quello che viene applicato.
  - Per altre informazioni su come rendere persistenti i piani di esecuzione e la sicurezza, vedere la [sezione relativa agli avvisi di sicurezza](https://www.terraform.io/docs/commands/plan.html#security-warning).