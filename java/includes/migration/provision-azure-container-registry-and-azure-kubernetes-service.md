---
author: mriem
ms.author: manriem
ms.date: 2/28/2020
ms.openlocfilehash: e72cefaab3ccdbbaae01992cc11285944fb09a36
ms.sourcegitcommit: 56e5f51daf6f671f7b6e84d4c6512473b35d31d2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/07/2020
ms.locfileid: "78897420"
---
### <a name="provision-azure-container-registry-and-azure-kubernetes-service"></a>Effettuare il provisioning di Registro Azure Container e del servizio Azure Kubernetes

Usare i comandi seguenti per creare un registro contenitori e un cluster di Azure Kubernetes con un'entità servizio che ha il ruolo Lettore nel registro. Assicurarsi di [scegliere il modello di rete appropriato](/azure/aks/operator-best-practices-network#choose-the-appropriate-network-model) per i requisiti di rete del cluster.

```bash
az group create -g $resourceGroup -l eastus
az acr create -g $resourceGroup -n $acrName --sku Standard
az aks create -g $resourceGroup -n $aksName --attach-acr $acrName --network-plugin azure
```
