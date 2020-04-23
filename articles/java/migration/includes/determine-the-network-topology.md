---
author: edburns
ms.author: edburns
ms.date: 1/21/2020
ms.openlocfilehash: 9403614c7ae2990a35fcbcce6d2e104f87724da5
ms.sourcegitcommit: 0af39ee9ff27c37ceeeb28ea9d51e32995989591
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/21/2020
ms.locfileid: "81673527"
---
### <a name="determine-the-network-topology"></a>Determinare la topologia di rete

Il set corrente di offerte del Marketplace è un valido punto di partenza per la migrazione. Se l'offerta non copre gli aspetti dell'architettura di cui è necessario eseguire la migrazione, sarà necessario acquisire la topologia di rete della distribuzione esistente e riprodurla in Azure, anche dopo aver approntato l'offerta di base con uno dei modelli di soluzione.

Si tratta di un argomento molto ampio, ma le informazioni di riferimento seguenti possono fornire un orientamento per le attività di migrazione:

* Queste informazioni di riferimento riguardano argomenti generali pertinenti per la migrazione della topologia di rete ad Azure: [Guida alla distribuzione Fast Track](https://docs.oracle.com/en/middleware/fusion-middleware/weblogic-server/12.2.1.4/intro/deploying.html#GUID-E0BE4A3E-44CD-4C95-9540-7A850BF02F6A).
* Questi informazioni di riferimento riguardano aspetti importanti relativi al clustering, che ha un impatto sulla topologia di rete: [Clustering di WebLogic Server](https://docs.oracle.com/en/middleware/fusion-middleware/weblogic-server/12.2.1.4/intro/clustering.html#GUID-E39A18C2-B990-485F-BFB1-0549250FABFE).
* Poiché le origini dati sono server distinti in un sistema WebLogic, è necessario considerarle nell'ambito dell'analisi della topologia di rete. [Origini dati di WebLogic Server](https://docs.oracle.com/en/middleware/fusion-middleware/weblogic-server/12.2.1.4/intro/jdbc.html#GUID-9FD5F552-B2E4-4FEC-8C10-503A08764B52).
* Anche le origini di messaggistica sono server distinti. [Messaggistica di WebLogic Server](https://docs.oracle.com/en/middleware/fusion-middleware/weblogic-server/12.2.1.4/intro/jms.html#GUID-3B5F647D-E001-413B-AC6A-1E103BDBA93F)
* Il bilanciamento del carico è un requisito fondamentale. Queste informazioni di riferimento riguardano il lato WebLogic Server del bilanciamento del carico: [Bilanciamento del carico in un cluster](https://docs.oracle.com/en/middleware/fusion-middleware/weblogic-server/12.2.1.4/clust/load_balancing.html#GUID-B8F6DE4B-1AAC-428B-878B-BFDCE161C054).
