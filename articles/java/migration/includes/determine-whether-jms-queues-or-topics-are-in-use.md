---
author: edburns
ms.author: edburns
ms.date: 1/21/2020
ms.openlocfilehash: a521396292214fd4e68b4625a0e5f5bcfca8be92
ms.sourcegitcommit: 0af39ee9ff27c37ceeeb28ea9d51e32995989591
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/21/2020
ms.locfileid: "81672917"
---
### <a name="determine-whether-java-message-service-jms-queues-or-topics-are-in-use"></a>Determinare se sono in uso code o argomenti di JMS (Java Message Service)

Se l'applicazione usa code o argomenti di JMS, sarà necessario eseguirne la migrazione a un server JMS ospitato esternamente. Il bus di servizio di Azure e il protocollo AMQP (Advanced Message Queueing Protocol) possono risultare un'ottima strategia di migrazione se si usa JMS. Per altre informazioni, vedere [Usare JMS con il bus di servizio e AMQP 1.0](/azure/service-bus-messaging/service-bus-java-how-to-use-jms-api-amqp).

Se sono stati configurati archivi persistenti JMS, è necessario acquisire la relativa configurazione e applicarla dopo la migrazione.
