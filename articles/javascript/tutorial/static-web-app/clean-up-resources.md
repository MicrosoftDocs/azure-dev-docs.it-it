---
title: Rimuovere la risorsa di Azure
description: Pulire le risorse fatturabili rimuovendo il gruppo di risorse con un comando dell'interfaccia della riga di comando di Azure.
ms.topic: tutorial
ms.date: 11/13/2020
ms.custom: devx-track-js
ms.openlocfilehash: bc68f550d0a2c1bc1550eb755e6ef33bae6ae9b7
ms.sourcegitcommit: 593d177cfb5f56f236ea59389e43a984da30f104
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/18/2021
ms.locfileid: "98561637"
---
# <a name="7-clean-up-resources-for-static-web-app"></a>7. Pulire le risorse per l'app Web statica

Al termine di questa esercitazione, è necessario rimuovere il gruppo di risorse, incluse la risorsa Visione artificiale e l'app Web statica, per evitare di ricevere addebiti per ulteriori utilizzi. 

## <a name="remove-all-the-resources-by-removing-resource-group"></a>Rimuovere tutte le risorse eliminando il gruppo di risorse

Nello stesso terminale, usare il [comando dell'interfaccia della riga di comando di Azure](/cli/azure/group#az_group_delete) per eliminare il gruppo di risorse:

```azurecli
az group delete --name rg-demo  -y
```

L'esecuzione del comando può richiedere alcuni minuti. 
