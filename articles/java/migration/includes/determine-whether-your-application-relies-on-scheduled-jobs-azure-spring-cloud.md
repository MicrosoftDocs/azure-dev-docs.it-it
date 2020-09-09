---
author: yevster
ms.author: yebronsh
ms.date: 7/21/2020
ms.openlocfilehash: b1acff280bddea700b0382609c1003129b05dad2
ms.sourcegitcommit: 4036ac08edd7fc6edf8d11527444061b0e4531ef
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/28/2020
ms.locfileid: "89062649"
---
#### <a name="determine-whether-your-application-relies-on-scheduled-jobs"></a>Determinare se l'applicazione si basa su processi pianificati

I processi pianificati, ad esempio le attività dell'utilità Quartz Scheduler o i processi Cron Unix, NON possono essere usati con Azure Spring Cloud. Azure Spring Cloud non impedisce la distribuzione interna di un'applicazione contenente attività pianificate. Tuttavia, se l'applicazione viene ampliata, lo stesso processo pianificato può essere eseguito più di una volta per ogni periodo pianificato. Questa situazione può provocare conseguenze indesiderate.

Eseguire l'inventario di tutte le attività pianificate in esecuzione nei server di produzione, all'interno o all'esterno del codice dell'applicazione.
