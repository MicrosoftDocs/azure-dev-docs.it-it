---
title: Rimuovere la risorsa di Azure
description: Pulire le risorse fatturabili rimuovendo il gruppo di risorse con un comando dell'interfaccia della riga di comando di Azure.
ms.topic: tutorial
ms.date: 11/13/2020
ms.custom: devx-track-js
ms.openlocfilehash: 132ccc26a4ddf17eb38be1573f462492fe1f7f0c
ms.sourcegitcommit: 1c508f5ba73a12e4baeacc88ad9a8359301acb50
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/18/2020
ms.locfileid: "97687494"
---
# <a name="7-clean-up-resources-for-static-web-app"></a>7. Pulire le risorse per l'app Web statica

Al termine di questa esercitazione, è necessario rimuovere il gruppo di risorse, incluse la risorsa Visione artificiale e l'app Web statica, per evitare di ricevere addebiti per ulteriori utilizzi. 

## <a name="remove-all-the-resources-by-removing-resource-group"></a>Rimuovere tutte le risorse eliminando il gruppo di risorse

Nello stesso terminale, usare il [comando dell'interfaccia della riga di comando di Azure](/cli/azure/group?view=azure-cli-latest#az_group_delete) per eliminare il gruppo di risorse:

```azurecli
az group delete --name rg-demo  -y
```

L'esecuzione del comando può richiedere alcuni minuti. 
