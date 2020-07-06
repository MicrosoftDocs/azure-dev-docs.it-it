---
author: yevster
ms.author: yebronsh
ms.date: 4/15/2020
ms.openlocfilehash: 3ff76d977c231b4b238f9dbec7f3bfaddfe5e484
ms.sourcegitcommit: 81577378a4c570ced1e9c6765f4a9eee8453c889
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/08/2020
ms.locfileid: "84507603"
---
### <a name="ensure-console-logging-and-configure-diagnostic-settings"></a>Verificare la registrazione della console e configurare le impostazioni di diagnostica

Configurare la registrazione in modo che tutto l'output venga instradato alla console e non ai file.

Dopo la distribuzione di un'applicazione in Azure Spring Cloud, [aggiungere un'impostazione di diagnostica](/azure/spring-cloud/diagnostic-services) per rendere gli eventi registrati disponibili per l'utilizzo, ad esempio tramite Log Analytics in Monitoraggio di Azure.

#### <a name="logstashelk-stack"></a>Stack LogStash/ELK

Se si usa lo stack LogStash/ELK per l'aggregazione dei log, configurare l'impostazione di diagnostica per trasmettere l'output della console a un'istanza di [Hub eventi di Azure](/azure/event-hubs/). Usare quindi il plug-in [LogStash EventHub](https://github.com/logstash-plugins/logstash-input-azure_event_hubs) per inserire gli eventi registrati in LogStash.

#### <a name="splunk"></a>Splunk

Se si usa Splunk per l'aggregazione dei log, configurare l'impostazione di diagnostica per trasmettere l'output della console ad [Archiviazione BLOB di Azure](/azure/storage/blobs/). Usare quindi il [componente aggiuntivo Splunk per servizi cloud Microsoft](https://splunkbase.splunk.com/app/3757/) per inserire gli eventi registrati in Splunk.
