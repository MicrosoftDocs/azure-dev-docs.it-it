---
author: yevster
ms.author: yebronsh
ms.date: 1/20/2020
ms.openlocfilehash: e37d0337361ce742ce7f7994ab634e63bc4b2ead
ms.sourcegitcommit: 0af39ee9ff27c37ceeeb28ea9d51e32995989591
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/21/2020
ms.locfileid: "81671647"
---
### <a name="inventory-secrets"></a>Inventario dei segreti

#### <a name="passwords-and-secure-strings"></a>Password e stringhe sicure

Controllare tutte le proprietà e i file di configurazione nei server di produzione per verificare la presenza di stringhe segrete e password. Assicurarsi di controllare i file *server.xml* e *context.xml* in *$CATALINA_BASE/conf*. È anche possibile trovare i file di configurazione contenenti password o credenziali all'interno dell'applicazione. Possono includere il file *META-INF/context.xml* e, per le applicazioni Spring Boot, il file *application.properties* o *application.yml*.
