---
author: edburns
ms.author: edburns
ms.date: 1/21/2020
ms.openlocfilehash: 69bddfce67388d3392e6908f3ddf1af378b116cf
ms.sourcegitcommit: 0af39ee9ff27c37ceeeb28ea9d51e32995989591
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/21/2020
ms.locfileid: "81673487"
---
### <a name="determine-whether-java-message-service-jms-queues-or-topics-are-in-use"></a>Determinare se sono in uso code o argomenti di JMS (Java Message Service)

Se l'applicazione usa code o argomenti JMS, sarà necessario eseguirne la migrazione a un server JMS ospitato esternamente. Il bus di servizio di Azure e Advanced Message Queueing Protocol possono costituire un'ottima strategia di migrazione per coloro che usano JMS. Per altre informazioni, vedere [Usare JMS con il bus di servizio e AMQP 1.0](/azure/service-bus-messaging/service-bus-java-how-to-use-jms-api-amqp).

Se sono stati configurati archivi persistenti JMS, è necessario acquisire la relativa configurazione e applicarla dopo la migrazione.

Se si usa Oracle Message Broker, è possibile eseguire la migrazione di questo software alle macchine virtuali di Azure e usarlo così com'è.
