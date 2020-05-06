---
author: mriem
ms.author: manriem
ms.date: 2/28/2020
ms.openlocfilehash: bda7ba230d8287101fc8f3f298b08777bb6e1c61
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/05/2020
ms.locfileid: "81672907"
---
### <a name="determine-whether-ejb-timers-are-in-use"></a>Determinare se i timer EJB sono in uso

Se l'applicazione usa i timer EJB, è necessario verificare che il relativo codice possa essere attivato da ogni istanza di WildFly in modo indipendente. Questa verifica è necessaria perché, nello scenario di distribuzione del servizio Azure Kubernetes, ogni timer EJB verrà attivato nella propria istanza di WildFly.
