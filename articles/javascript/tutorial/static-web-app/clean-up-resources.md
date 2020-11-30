---
title: Rimuovere la risorsa di Azure
description: Pulire le risorse fatturabili rimuovendo il gruppo di risorse con un comando dell'interfaccia della riga di comando di Azure.
ms.topic: tutorial
ms.date: 11/13/2020
ms.custom: devx-track-js
ms.openlocfilehash: 0b425ce873c53ca95628cfe8c3f68ea4b010aba3
ms.sourcegitcommit: 4dac39849ba2e48034ecc91ef578d11aab796e58
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/20/2020
ms.locfileid: "94993464"
---
# <a name="6-clean-up-resources"></a>6. Pulire le risorse

Al termine di questa esercitazione, è necessario rimuovere il gruppo di risorse, incluse la risorsa Visione artificiale e l'app Web statica, per evitare di ricevere addebiti per ulteriori utilizzi. 

## <a name="remove-all-the-resources-by-removing-resource-group"></a>Rimuovere tutte le risorse eliminando il gruppo di risorse

Nello stesso terminale, usare il [comando dell'interfaccia della riga di comando di Azure](/cli/azure/group?view=azure-cli-latest#az_group_delete) per eliminare il gruppo di risorse:

```azurecli
az group delete --name rg-demo  -y
```

L'esecuzione del comando può richiedere alcuni minuti. 
