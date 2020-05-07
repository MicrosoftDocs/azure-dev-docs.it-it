---
author: yevster
ms.author: yebronsh
ms.date: 1/24/2020
ms.openlocfilehash: 3cf4edcf12d7fe72c0fa5a0216d1a8c97a2f2e59
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/05/2020
ms.locfileid: "81672147"
---
### <a name="inventory-certificates"></a>Inventario dei certificati

Documentare tutti i certificati usati per gli endpoint SSL pubblici o per la comunicazione con i database back-end e altri sistemi. È possibile visualizzare tutti i certificati nei server di produzione eseguendo il comando seguente:

```bash
keytool -list -v -keystore <path to keystore>
```
