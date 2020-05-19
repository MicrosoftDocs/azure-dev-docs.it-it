---
author: edburns
ms.author: edburns
ms.date: 1/21/2020
ms.openlocfilehash: 750a8cdd34ead8390a0c4c2cb028f4624bd256a9
ms.sourcegitcommit: 226ebca0d0e3b918928f58a3a7127be49e4aca87
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/08/2020
ms.locfileid: "82988898"
---
### <a name="inspect-your-domain-configuration"></a>Esaminare la configurazione del dominio

L'unità di configurazione principale in WebLogic Server è il dominio. Di conseguenza, il file *config.xml* contiene una grande quantità di configurazioni che è necessario considerare attentamente per la migrazione. Il file include riferimenti a file XML aggiuntivi archiviati in sottodirectory. Oracle consiglia di usare normalmente la **console di amministrazione** per configurare gli oggetti e i servizi gestibili di WebLogic Server e di consentire a WebLogic Server di gestire il file *config.xml*. Per altre informazioni, vedere [File di configurazione del dominio](https://docs.oracle.com/en/middleware/fusion-middleware/weblogic-server/12.2.1.4/domcf/config_files.html).

#### <a name="inside-your-application"></a>All'interno dell'applicazione

Esaminare il file *WEB-INF/weblogic.xml* e/o il file *WEB-INF/web.xml*.
