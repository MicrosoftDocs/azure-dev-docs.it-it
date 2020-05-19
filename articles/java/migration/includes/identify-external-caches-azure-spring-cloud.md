---
author: yevster
ms.author: yebronsh
ms.date: 4/15/2020
ms.openlocfilehash: f2bc0442aafcfffc963d4e5a6da18085d9205a22
ms.sourcegitcommit: 226ebca0d0e3b918928f58a3a7127be49e4aca87
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/08/2020
ms.locfileid: "82990163"
---
#### <a name="identify-external-caches"></a>Identificare le cache esterne

Identificare le cache esterne in uso. Spesso, Redis viene usato tramite Spring Data Redis. Per informazioni sulla configurazione, vedere la documentazione di [Spring Data Redis](https://spring.io/projects/spring-data-redis).

Determinare se i dati della sessione vengono memorizzati nella cache tramite una [sessione Spring](https://spring.io/projects/spring-session) cercando la rispettiva configurazione (in [Java](https://docs.spring.io/spring-session/docs/current/reference/html5/#httpsession-redis-jc) o [XML](https://docs.spring.io/spring-session/docs/current/reference/html5/#httpsession-redis-xml)).
