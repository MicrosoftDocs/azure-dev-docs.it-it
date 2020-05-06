---
author: mriem
ms.author: manriem
ms.date: 2/28/2020
ms.openlocfilehash: 4d2df28d5c752d20454c28a6d4a53698aa7ff5b6
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/05/2020
ms.locfileid: "81673017"
---
### <a name="determine-whether-your-application-relies-on-scheduled-jobs"></a>Determinare se l'applicazione si basa su processi pianificati

I processi pianificati, ad esempio le attività dell'utilità Quartz Scheduler o i processi Cron Unix, NON possono essere usati con il servizio Azure Kubernetes. Il servizio Azure Kubernetes non impedisce la distribuzione interna di un'applicazione contenente attività pianificate. Tuttavia, se l'applicazione viene ampliata, lo stesso processo pianificato può essere eseguito più di una volta per ogni periodo pianificato. Questa situazione può provocare conseguenze indesiderate.

Per eseguire i processi pianificati nel cluster del servizio Azure Kubernetes, definire i processi Cron Kubernetes come necessario. Per altre informazioni, vedere [Esecuzione di attività automatiche con un processo Cron](https://kubernetes.io/docs/tasks/job/automated-tasks-with-cron-jobs/).
