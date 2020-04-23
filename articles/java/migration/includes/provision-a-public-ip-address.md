---
author: mriem
ms.author: manriem
ms.date: 2/28/2020
ms.openlocfilehash: 5527a38151afd531cbd256428bd729b7aba8fe45
ms.sourcegitcommit: 0af39ee9ff27c37ceeeb28ea9d51e32995989591
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/21/2020
ms.locfileid: "81673167"
---
### <a name="provision-a-public-ip-address"></a>Provisioning di un indirizzo IP pubblico

Se l'applicazione deve essere accessibile dall'esterno delle reti virtuali o interne, sarà necessario un indirizzo IP statico pubblico. È necessario effettuare il provisioning di questo indirizzo IP all'interno del gruppo di risorse del nodo del cluster, come illustrato nell'esempio seguente:

```bash
nodeResourceGroup=$(az aks show -g $resourceGroup -n $aksName --query 'nodeResourceGroup' -o tsv)
publicIp=$(az network public-ip create -g $nodeResourceGroup -n applicationIp --sku Standard --allocation-method Static --query 'publicIp.ipAddress' -o tsv)
echo "Your public IP address is ${publicIp}."
```
