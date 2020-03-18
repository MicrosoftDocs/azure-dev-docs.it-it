---
author: edburns
ms.author: edburns
ms.date: 1/21/2020
ms.openlocfilehash: 26d0422b9ea569b5657a5b7841319e77ce0f1684
ms.sourcegitcommit: 56e5f51daf6f671f7b6e84d4c6512473b35d31d2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/07/2020
ms.locfileid: "78894136"
---
### <a name="determine-whether-session-replication-is-used"></a>Determinare se viene usata la replica delle sessioni

Se l'applicazione si basa sulla replica delle sessioni, con o senza Oracle Coherence*Web, sono disponibili tre opzioni:

* Coherence*Web può essere eseguito insieme a WebLogic Server nelle macchine virtuali di Azure, ma è necessario configurare manualmente questa opzione dopo aver effettuato il provisioning dell'offerta. Se si usa la versione autonoma di Coherence, è possibile eseguire anche questa nelle macchine virtuali di Azure, ma è necessario configurare manualmente questa opzione dopo aver effettuato il provisioning dell'offerta.
* Effettuare il refactoring dell'applicazione per l'uso di un database per la gestione delle sessioni.
* Effettuare il refactoring dell'applicazione per esternalizzare la sessione nel servizio Azure Redis. Per altre informazioni, vedere [Azure Cache for Redis](/azure/azure-cache-for-redis/cache-overview).

Per tutte queste opzioni, è consigliabile verificare come viene eseguita la replica dello stato delle sessioni HTTP da WebLogic. Per altre informazioni, vedere [Replica dello stato delle sessioni HTTP](https://docs.oracle.com/en/middleware/fusion-middleware/weblogic-server/12.2.1.4/clust/failover.html#GUID-E13D8142-66BA-46A1-854F-4FC6F82992DD) nella documentazione di Oracle.
