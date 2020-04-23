---
author: mriem
ms.author: manriem
ms.date: 2/28/2020
ms.openlocfilehash: f5995a3c5efc46bc58b446589ce089bf7d86b2ba
ms.sourcegitcommit: 0af39ee9ff27c37ceeeb28ea9d51e32995989591
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/21/2020
ms.locfileid: "81673077"
---
### <a name="determine-whether-your-application-uses-a-resource-adapter"></a>Determinare se l'applicazione usa un adattatore di risorse

Se l'applicazione richiede un adattatore di risorse, è necessario che sia compatibile con WildFly. Determinare se l'adattatore di risorse funziona in un'istanza autonoma di WildFly distribuendolo nel server e configurandolo correttamente. Se l'adattatore di risorse funziona correttamente, sarà necessario aggiungere i file JAR al classpath del server dell'immagine Docker e inserire i file di configurazione necessari nella posizione corretta nelle directory del server WildFly per renderlo disponibile.
