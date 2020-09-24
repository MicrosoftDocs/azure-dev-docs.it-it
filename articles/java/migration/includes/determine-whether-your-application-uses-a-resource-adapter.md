---
author: mnriem
ms.author: manriem
ms.date: 2/28/2020
ms.openlocfilehash: 9fa87bf3d0de64271a9ffb0e7e8fbff3cd1afd12
ms.sourcegitcommit: 850856d3fa2ddd8f96616ee6a1f092d8e0aedab3
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/17/2020
ms.locfileid: "90738501"
---
### <a name="determine-whether-your-application-uses-a-resource-adapter"></a>Determinare se l'applicazione usa un adattatore di risorse

Se l'applicazione richiede un adattatore di risorse, è necessario che sia compatibile con WildFly. Determinare se l'adattatore di risorse funziona in un'istanza autonoma di WildFly distribuendolo nel server e configurandolo correttamente. Se l'adattatore di risorse funziona correttamente, sarà necessario aggiungere i file JAR al classpath del server dell'immagine Docker e inserire i file di configurazione necessari nella posizione corretta nelle directory del server WildFly per renderlo disponibile.
