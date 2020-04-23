---
author: edburns
ms.author: edburns
ms.date: 1/21/2020
ms.openlocfilehash: 3427cc445333c9851a760a607c402e689c7b6ba4
ms.sourcegitcommit: 0af39ee9ff27c37ceeeb28ea9d51e32995989591
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/21/2020
ms.locfileid: "81673157"
---
### <a name="inventory-all-certificates"></a>Inventario di tutti i certificati

Documentare tutti i certificati usati per gli endpoint SSL pubblici. È possibile visualizzare tutti i certificati nei server di produzione eseguendo il comando seguente:

```bash
keytool -list -v -keystore <path to keystore>
```
