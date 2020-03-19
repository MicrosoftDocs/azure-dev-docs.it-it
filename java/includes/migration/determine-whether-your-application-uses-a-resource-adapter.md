---
author: mriem
ms.author: manriem
ms.date: 2/28/2020
ms.openlocfilehash: f5995a3c5efc46bc58b446589ce089bf7d86b2ba
ms.sourcegitcommit: 56e5f51daf6f671f7b6e84d4c6512473b35d31d2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/07/2020
ms.locfileid: "78897505"
---
### <a name="determine-whether-your-application-uses-a-resource-adapter"></a>Determinare se l'applicazione usa un adattatore di risorse

Se l'applicazione richiede un adattatore di risorse, è necessario che sia compatibile con WildFly. Determinare se l'adattatore di risorse funziona in un'istanza autonoma di WildFly distribuendolo nel server e configurandolo correttamente. Se l'adattatore di risorse funziona correttamente, sarà necessario aggiungere i file JAR al classpath del server dell'immagine Docker e inserire i file di configurazione necessari nella posizione corretta nelle directory del server WildFly per renderlo disponibile.
