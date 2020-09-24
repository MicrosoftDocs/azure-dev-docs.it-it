---
author: mnriem
ms.author: manriem
ms.date: 2/28/2020
ms.openlocfilehash: c77a5ae60807cf25415ab86dab9d9891abab13f9
ms.sourcegitcommit: 850856d3fa2ddd8f96616ee6a1f092d8e0aedab3
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/17/2020
ms.locfileid: "90738386"
---
### <a name="determine-whether-ejb-timers-are-in-use"></a>Determinare se i timer EJB sono in uso

Se l'applicazione usa i timer EJB, è necessario verificare che il relativo codice possa essere attivato da ogni istanza di WildFly in modo indipendente. Questa verifica è necessaria perché, nello scenario di distribuzione del servizio Azure Kubernetes, ogni timer EJB verrà attivato nella propria istanza di WildFly.
