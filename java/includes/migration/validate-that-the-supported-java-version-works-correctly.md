---
author: edburns
ms.author: edburns
ms.date: 1/21/2020
ms.openlocfilehash: cc634f89170f4db6f961da91e3eb3abe7393539d
ms.sourcegitcommit: 367780fe48d977c82cb84208c128b0bf694b1029
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2020
ms.locfileid: "76830989"
---
### <a name="validate-that-the-supported-java-version-works-correctly"></a>Verificare che la versione di Java supportata funzioni correttamente

Tutti i percorsi di migrazione per WebLogic in Azure richiedono una versione specifica di Java, che varia per ogni percorso. Sarà necessario verificare che l'applicazione sia in grado di funzionare correttamente usando tale versione supportata.

Per ottenere la versione corrente, accedere al server di produzione ed eseguire questo comando:

```bash
java -version
```

> [!NOTE]
> Quando si esegue la migrazione a WebLogic in macchine virtuali di Azure, i requisiti per le versioni specifiche di Java sono determinati dall'istanza di Java preinstallata nelle macchine virtuali.