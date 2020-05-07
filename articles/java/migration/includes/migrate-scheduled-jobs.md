---
author: yevster
ms.author: yebronsh
ms.date: 1/22/2020
ms.openlocfilehash: 1015c179c0f93485decd77bd89a3ceec8833652e
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/05/2020
ms.locfileid: "81672157"
---
### <a name="migrate-scheduled-jobs"></a>Eseguire la migrazione dei processi pianificati

Per eseguire processi pianificati in Azure, è consigliabile usare un [trigger Timer per Funzioni di Azure](/azure/azure-functions/functions-bindings-timer). Non è necessario eseguire la migrazione del codice del processo stesso in una funzione. La funzione può semplicemente richiamare un URL nell'applicazione per attivare il processo. Se le esecuzioni di processi devono essere richiamate dinamicamente e/o monitorate a livello centrale, provare a usare [Spring Batch](https://spring.io/projects/spring-batch).

In alternativa, è possibile creare un'app per la logica con un trigger Ricorrenza per richiamare l'URL senza scrivere codice all'esterno dell'applicazione. Per altre informazioni, vedere [Panoramica di App per la logica di Azure](/azure/logic-apps/logic-apps-overview) e [Creare, pianificare ed eseguire attività e flussi di lavoro ricorrenti con il trigger Ricorrenza in App per la logica di Azure](/azure/connectors/connectors-native-recurrence).

> [!NOTE]
> Per evitare un uso improprio, potrebbe essere necessario assicurarsi che l'endpoint di chiamata del processo richieda le credenziali. In questo caso, la funzione di trigger dovrà fornire le credenziali.
