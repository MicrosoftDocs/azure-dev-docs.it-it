---
author: yevster
ms.author: yebronsh
ms.date: 1/20/2020
ms.openlocfilehash: e37d0337361ce742ce7f7994ab634e63bc4b2ead
ms.sourcegitcommit: 367780fe48d977c82cb84208c128b0bf694b1029
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2020
ms.locfileid: "76825823"
---
### <a name="inventory-secrets"></a>Inventario dei segreti

#### <a name="passwords-and-secure-strings"></a>Password e stringhe sicure

Controllare tutte le proprietà e i file di configurazione nei server di produzione per verificare la presenza di stringhe segrete e password. Assicurarsi di controllare i file *server.xml* e *context.xml* in *$CATALINA_BASE/conf*. È anche possibile trovare i file di configurazione contenenti password o credenziali all'interno dell'applicazione. Possono includere il file *META-INF/context.xml* e, per le applicazioni Spring Boot, il file *application.properties* o *application.yml*.
