---
title: Rimuovere la risorsa della macchina virtuale Linux
description: Pulire le risorse di Azure rimuovendo il gruppo di risorse con un comando dell'interfaccia della riga di comando di Azure.
ms.topic: tutorial
ms.date: 11/13/2020
ms.custom: devx-track-js
ms.openlocfilehash: 5c8c0bb8a1413da72cb2c32d9ce541bee1c36cac
ms.sourcegitcommit: dc74b60217abce66fe6cc93923e869e63ac86a8f
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/18/2020
ms.locfileid: "94872822"
---
# <a name="7-clean-up-resources"></a>7. Pulire le risorse

Al termine di questa esercitazione, è necessario rimuovere l'intero gruppo di risorse per evitare di ricevere addebiti per ulteriori utilizzi. 

## <a name="remove-all-the-resources-by-removing-resource-group"></a>Rimuovere tutte le risorse eliminando il gruppo di risorse

Nello stesso terminale, usare il [comando dell'interfaccia della riga di comando di Azure](/cli/azure/group?view=azure-cli-latest#az_group_delete) per eliminare il gruppo di risorse:

```azurecli
az group delete --name rg-demo-vm-eastus -y
```

L'esecuzione di questo comando richiede alcuni minuti. 

## <a name="next-step"></a>Passaggio successivo

* Altre informazioni sulle [VM Linux di Azure](/azure/virtual-machines)
