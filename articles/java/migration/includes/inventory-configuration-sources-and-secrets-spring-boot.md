---
author: yevster
ms.author: yebronsh
ms.date: 5/4/2020
ms.openlocfilehash: 430f4fef9c512f91da4fd95f04320ddd127ea784
ms.sourcegitcommit: 226ebca0d0e3b918928f58a3a7127be49e4aca87
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/08/2020
ms.locfileid: "82990253"
---
### <a name="inventory-configuration-sources-and-secrets"></a>Segreti e origini della configurazione dell'inventario

#### <a name="inventory-passwords-and-secure-strings"></a>Password e stringhe sicure dell'inventario

Controllare tutte le proprietà e i file di configurazione e tutte le variabili di ambiente nei server di produzione per verificare la presenza di stringhe segrete e password. In un'applicazione Spring Boot tali stringhe si trovano in genere nel file *application.properties* o *application.yml*.

[!INCLUDE [inventory-certificates-h4](inventory-certificates-h4.md)]
