---
author: mnriem
ms.author: manriem
ms.date: 2/28/2020
ms.openlocfilehash: c8cd0672bfedf640077e9ae3b9272b12d8ce0b61
ms.sourcegitcommit: 850856d3fa2ddd8f96616ee6a1f092d8e0aedab3
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/17/2020
ms.locfileid: "90738143"
---
### <a name="provision-azure-container-registry-and-azure-kubernetes-service"></a>Effettuare il provisioning di Registro Azure Container e del servizio Azure Kubernetes

Usare i comandi seguenti per creare un registro contenitori e un cluster di Azure Kubernetes con un'entità servizio che ha il ruolo Lettore nel registro. Assicurarsi di [scegliere il modello di rete appropriato](/azure/aks/operator-best-practices-network#choose-the-appropriate-network-model) per i requisiti di rete del cluster.

```bash
az group create -g $resourceGroup -l eastus
az acr create -g $resourceGroup -n $acrName --sku Standard
az aks create -g $resourceGroup -n $aksName --attach-acr $acrName --network-plugin azure
```
