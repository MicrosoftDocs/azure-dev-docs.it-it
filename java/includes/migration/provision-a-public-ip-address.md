---
author: mriem
ms.author: manriem
ms.date: 2/28/2020
ms.openlocfilehash: 5527a38151afd531cbd256428bd729b7aba8fe45
ms.sourcegitcommit: 56e5f51daf6f671f7b6e84d4c6512473b35d31d2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/07/2020
ms.locfileid: "78897590"
---
### <a name="provision-a-public-ip-address"></a>Provisioning di un indirizzo IP pubblico

Se l'applicazione deve essere accessibile dall'esterno delle reti virtuali o interne, sarà necessario un indirizzo IP statico pubblico. È necessario effettuare il provisioning di questo indirizzo IP all'interno del gruppo di risorse del nodo del cluster, come illustrato nell'esempio seguente:

```bash
nodeResourceGroup=$(az aks show -g $resourceGroup -n $aksName --query 'nodeResourceGroup' -o tsv)
publicIp=$(az network public-ip create -g $nodeResourceGroup -n applicationIp --sku Standard --allocation-method Static --query 'publicIp.ipAddress' -o tsv)
echo "Your public IP address is ${publicIp}."
```
