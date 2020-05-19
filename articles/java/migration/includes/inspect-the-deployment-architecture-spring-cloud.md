---
author: yevster
ms.author: yebronsh
ms.date: 5/4/2020
ms.openlocfilehash: 7b2ad87dfe2e2c7358737ff02ec52b97b1332ae7
ms.sourcegitcommit: 226ebca0d0e3b918928f58a3a7127be49e4aca87
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/08/2020
ms.locfileid: "82990133"
---
### <a name="inspect-the-deployment-architecture"></a>Esaminare l'architettura di distribuzione

#### <a name="document-hardware-requirements-for-each-service"></a>Documentare i requisiti hardware per ogni servizio

Per ogni servizio Spring Cloud (escluso il server di configurazione, il registro o il gateway), documentare le informazioni seguenti:

* Numero di istanze in esecuzione.
* Numero di CPU allocate a ogni istanza.
* Quantità di RAM allocata a ogni istanza.

#### <a name="document-geo-replicationdistribution"></a>Documentare la replica geografica/distribuzione

Determinare se le applicazioni Spring Cloud sono attualmente distribuite tra più aree o data center. Documentare i requisiti di tempo di attività/contratto di servizio per le applicazioni di cui si intende eseguire la migrazione.

#### <a name="identify-clients-that-bypass-the-service-registry"></a>Identificare i client che ignorano il registro dei servizi

Identificare tutte le applicazioni client che richiamano i servizi di cui eseguire la migrazione senza usare Spring Cloud Service Registry. Dopo la migrazione, tali chiamate non saranno più possibili. Aggiornare i client affinché usino [Spring Cloud OpenFeign](https://spring.io/projects/spring-cloud-openfeign) prima della migrazione.
