---
title: includere file
description: includere file
author: tomarchermsft
ms.service: terraform
ms.topic: include
ms.date: 05/31/2020
ms.author: tarcher
ms.openlocfilehash: 43e684a44c657ab44f3f8f13719e08f0e339f498
ms.sourcegitcommit: db56786f046a3bde1bd9b0169b4f62f0c1970899
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/03/2020
ms.locfileid: "84329899"
---
Iniziare a usare [Terraform](https://www.terraform.io) configurando [Terraform in Azure](https://www.terraform.io/docs/providers/azurerm/index.html) e creando un gruppo di risorse di Azure di base.

Terraform consente di definire, visualizzare in anteprima e distribuire l'infrastruttura cloud. Con Terraform è possibile creare file di configurazione usando la [sintassi HCL](https://www.terraform.io/docs/configuration/syntax.html). La sintassi HCL consente di specificare il provider di servizi cloud, ad esempio Azure, e gli elementi che costituiscono l'infrastruttura cloud. Dopo aver creato i file di configurazione, è necessario creare un *piano di esecuzione* che consenta di visualizzare in anteprima le modifiche apportate all'infrastruttura prima che vengano distribuite. Dopo aver verificato le modifiche, è possibile applicare il piano di esecuzione per distribuire l'infrastruttura.
