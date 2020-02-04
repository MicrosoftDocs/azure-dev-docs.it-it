---
author: edburns
ms.author: edburns
ms.date: 1/21/2020
ms.openlocfilehash: 33146c309d8fdaf21dc218c11054460cfc3dc8f4
ms.sourcegitcommit: 367780fe48d977c82cb84208c128b0bf694b1029
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2020
ms.locfileid: "76830979"
---
### <a name="domain-configuration"></a>Configurazione del dominio

L'unità di configurazione principale in WebLogic Server è il dominio. Di conseguenza, il file *config.xml* contiene una grande quantità di configurazioni che è necessario considerare attentamente per la migrazione. Il file include riferimenti a file XML aggiuntivi archiviati in sottodirectory. Oracle consiglia di usare normalmente la **console di amministrazione** per configurare gli oggetti e i servizi gestibili di WebLogic Server e di consentire a WebLogic Server di gestire il file *config.xml*. Per altre informazioni, vedere [File di configurazione del dominio](https://docs.oracle.com/en/middleware/fusion-middleware/weblogic-server/12.2.1.4/domcf/config_files.html).

#### <a name="inside-your-application"></a>All'interno dell'applicazione

Esaminare il file *WEB-INF/weblogic.xml* e/o il file *WEB-INF/web.xml*.
