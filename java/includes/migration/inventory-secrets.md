---
author: yevster
ms.author: yebronsh
ms.topic: include
ms.date: 1/20/2020
ms.openlocfilehash: 4228d1db44ac16da1c05fe97d20a3491cc9c5f51
ms.sourcegitcommit: 3585b1b5148e0f8eb950037345bafe6a4f6be854
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/21/2020
ms.locfileid: "76288570"
---
### <a name="inventory-secrets"></a>Inventario dei segreti

#### <a name="passwords-and-secure-strings"></a>Password e stringhe sicure

Controllare tutte le proprietà e i file di configurazione nei server di produzione per verificare la presenza di stringhe segrete e password. Assicurarsi di controllare i file *server.xml* e *context.xml* in $CATALINA_BASE/conf. È anche possibile trovare i file di configurazione contenenti password o credenziali all'interno dell'applicazione. Possono includere il file *META-INF/context.xml* e, per le applicazioni Spring Boot, il file *application.properties* o *application.yml*.

#### <a name="certificates"></a>Certificati

Documentare tutti i certificati usati per gli endpoint SSL pubblici. È possibile visualizzare tutti i certificati nei server di produzione eseguendo il comando seguente:

```bash
keytool -list -v -keystore <path to keystore>
```
