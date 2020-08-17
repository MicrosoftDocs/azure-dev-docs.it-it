---
author: edburns
ms.author: edburns
ms.date: 1/21/2020
ms.openlocfilehash: f50115be65be8e746527b3d4ee833a738109ddbc
ms.sourcegitcommit: b923aee828cd4b309ef92fe1f8d8b3092b2ffc5a
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/10/2020
ms.locfileid: "88052258"
---
### <a name="determine-whether-weblogic-clustering-is-used"></a>Determinare se viene usato il clustering WebLogic

Molto probabilmente, l'applicazione viene distribuita in più server WebLogic per ottenere disponibilità elevata. È possibile eseguire la migrazione di questi cluster direttamente dall'installazione locale a WebLogic in esecuzione nelle macchine virtuali di Azure. Per altre informazioni, vedere [File di configurazione del dominio](https://docs.oracle.com/middleware/12213/wls/DOMCF/config_files.htm#DOMCF127) nella documentazione di Oracle.

### <a name="account-for-load-balancing-requirements"></a>Includere i requisiti per il bilanciamento del carico

Il bilanciamento del carico è una parte essenziale della migrazione del cluster Oracle WebLogic Server in Azure.  La soluzione più semplice consiste nell'usare il supporto predefinito per [Gateway applicazione di Azure](/azure/application-gateway/overview) fornito nell'offerta di Azure Marketplace per il cluster Oracle WebLogic Server.  Per un'esercitazione su questo argomento, vedere [Esercitazione: Eseguire la migrazione di un cluster di WebLogic Server ad Azure con Gateway applicazione di Azure come servizio di bilanciamento del carico](../migrate-weblogic-with-app-gateway.md).

Per un riepilogo delle funzionalità di Gateway applicazione di Azure rispetto ad altre soluzioni di bilanciamento del carico di Azure, vedere [Panoramica delle opzioni di bilanciamento del carico in Azure](/azure/architecture/guide/technology-choices/load-balancing-overview).
