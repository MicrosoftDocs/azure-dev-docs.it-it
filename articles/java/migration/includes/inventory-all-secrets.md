---
author: edburns
ms.author: edburns
ms.date: 1/21/2020
ms.openlocfilehash: a771d0689e83e0b61bf9b8c31e0cbf36949fdc20
ms.sourcegitcommit: 0af39ee9ff27c37ceeeb28ea9d51e32995989591
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/21/2020
ms.locfileid: "81673397"
---
### <a name="inventory-all-secrets"></a>Inventario di tutti i segreti

Prima della comparsa di tecnologie di tipo "configurazione come servizio", ad esempio Azure Key Vault, non esisteva un concetto ben definito di "segreti". Era invece disponibile un set disparato di impostazioni di configurazione assimilabili a quello che ora chiamiamo "segreti". Con i server app come WebLogic Server, questi segreti si trovano in molti file e archivi di configurazione diversi. Controllare tutte le proprietà e i file di configurazione nei server di produzione per verificare la presenza di segreti e password. Assicurarsi di controllare *weblogic.xml* nei WAR. I file di configurazione contenenti password o credenziali possono trovarsi anche all'interno dell'applicazione. Per altre informazioni, vedere [Concetti di base di Azure Key Vault](/azure/key-vault/basic-concepts).
