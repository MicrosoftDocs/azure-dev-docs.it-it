---
author: yevster
ms.author: yebronsh
ms.date: 1/24/2020
ms.openlocfilehash: 975c6ce1011f76a34f33ec6093f70f281eefa6c3
ms.sourcegitcommit: 226ebca0d0e3b918928f58a3a7127be49e4aca87
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/08/2020
ms.locfileid: "82990213"
---
#### <a name="inventory-certificates"></a>Inventario dei certificati

Documentare tutti i certificati usati per gli endpoint SSL pubblici o per la comunicazione con i database back-end e altri sistemi. È possibile visualizzare tutti i certificati nei server di produzione eseguendo il comando seguente:

```bash
keytool -list -v -keystore <path to keystore>
```
