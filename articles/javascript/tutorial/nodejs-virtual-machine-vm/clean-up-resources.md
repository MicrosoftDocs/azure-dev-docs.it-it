---
title: Rimuovere la risorsa della macchina virtuale Linux
description: Pulire le risorse di Azure rimuovendo il gruppo di risorse con un comando dell'interfaccia della riga di comando di Azure.
ms.topic: tutorial
ms.date: 11/13/2020
ms.custom: devx-track-js
ms.openlocfilehash: ac61f1b73e873ee1c1c6cd343792a79a88e76123
ms.sourcegitcommit: 593d177cfb5f56f236ea59389e43a984da30f104
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/18/2021
ms.locfileid: "98560987"
---
# <a name="7-clean-up-resources"></a>7. Pulire le risorse

Al termine di questa esercitazione, è necessario rimuovere l'intero gruppo di risorse per evitare di ricevere addebiti per ulteriori utilizzi. 

## <a name="remove-all-the-resources-by-removing-resource-group"></a>Rimuovere tutte le risorse eliminando il gruppo di risorse

Nello stesso terminale, usare il [comando dell'interfaccia della riga di comando di Azure](/cli/azure/group#az_group_delete) per eliminare il gruppo di risorse:

```azurecli
az group delete --name rg-demo-vm-eastus -y
```

L'esecuzione di questo comando richiede alcuni minuti. 

## <a name="next-step"></a>Passaggio successivo

* Altre informazioni sulle [VM Linux di Azure](/azure/virtual-machines)
