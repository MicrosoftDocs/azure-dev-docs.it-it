---
author: edburns
ms.author: edburns
ms.date: 1/21/2020
ms.openlocfilehash: b2458a80eccfcae24a3364208334cce3aedea861
ms.sourcegitcommit: 21ddeb9bd9abd419d143dc2ca8a7c821a1758cf9
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/11/2020
ms.locfileid: "79129525"
---
### <a name="determine-whether-jms-queues-or-topics-are-in-use"></a>Determinare se sono in uso code o argomenti JMS

Se l'applicazione usa code o argomenti JMS, sarà necessario eseguirne la migrazione a un server JMS ospitato esternamente, ad esempio al bus di servizio di Azure. Vedere [Usare il bus di servizio come broker dei messaggi](/azure/service-bus-messaging/message-transfers-locks-settlement).

Se sono stati configurati archivi persistenti JMS, è necessario acquisire la relativa configurazione e applicarla dopo la migrazione.
