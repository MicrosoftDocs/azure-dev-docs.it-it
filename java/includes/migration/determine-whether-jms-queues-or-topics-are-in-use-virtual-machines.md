---
author: edburns
ms.author: edburns
ms.date: 1/21/2020
ms.openlocfilehash: e36fae7adfda8c05e222151872b137fa600cdd97
ms.sourcegitcommit: 367780fe48d977c82cb84208c128b0bf694b1029
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2020
ms.locfileid: "76830769"
---
### <a name="determine-whether-jms-queues-or-topics-are-in-use"></a>Determinare se sono in uso code o argomenti JMS

Se l'applicazione usa code o argomenti JMS, sarà necessario eseguirne la migrazione a un server JMS ospitato esternamente. Il bus di servizio di Azure e Advanced Message Queueing Protocol possono costituire un'ottima strategia di migrazione per coloro che usano JMS. Per altre informazioni, vedere [Usare JMS (Java Message Service) con il bus di servizio e AMQP 1.0](/azure/service-bus-messaging/service-bus-java-how-to-use-jms-api-amqp).

Se sono stati configurati archivi persistenti JMS, è necessario acquisire la relativa configurazione e applicarla dopo la migrazione.

Se si usa Oracle Message Broker, è possibile eseguire la migrazione di questo software alle macchine virtuali di Azure e usarlo così com'è.
