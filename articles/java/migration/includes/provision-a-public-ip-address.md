---
author: mnriem
ms.author: manriem
ms.date: 2/28/2020
ms.openlocfilehash: 92caae7f50ddb0e58f14e8c99a4cd598a72f884f
ms.sourcegitcommit: 850856d3fa2ddd8f96616ee6a1f092d8e0aedab3
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/17/2020
ms.locfileid: "90738142"
---
### <a name="provision-a-public-ip-address"></a>Provisioning di un indirizzo IP pubblico

Se l'applicazione deve essere accessibile dall'esterno delle reti virtuali o interne, sarà necessario un indirizzo IP statico pubblico. È necessario effettuare il provisioning di questo indirizzo IP all'interno del gruppo di risorse del nodo del cluster, come illustrato nell'esempio seguente:

```bash
nodeResourceGroup=$(az aks show -g $resourceGroup -n $aksName --query 'nodeResourceGroup' -o tsv)
publicIp=$(az network public-ip create -g $nodeResourceGroup -n applicationIp --sku Standard --allocation-method Static --query 'publicIp.ipAddress' -o tsv)
echo "Your public IP address is ${publicIp}."
```
