---
author: yevster
ms.author: yebronsh
ms.date: 4/15/2020
ms.openlocfilehash: b53308d4a9db52a25665d0daa74be678e726c499
ms.sourcegitcommit: 226ebca0d0e3b918928f58a3a7127be49e4aca87
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/08/2020
ms.locfileid: "82990183"
---
### <a name="ensure-console-logging-and-configure-diagnostic-settings"></a>Verificare la registrazione della console e configurare le impostazioni di diagnostica

Configurare la registrazione in modo che tutte le applicazioni in Azure Spring Cloud registrino gli eventi nella console e non su file.

Dopo la distribuzione di un'applicazione in Azure Spring Cloud, [aggiungere un'impostazione di diagnostica](/azure/spring-cloud/diagnostic-services) per rendere gli eventi registrati disponibili per l'utilizzo, ad esempio tramite Log Analytics in Monitoraggio di Azure.

#### <a name="logstashelk-stack"></a>Stack LogStash/ELK

Se si usa lo stack LogStash/ELK per l'aggregazione dei log, configurare l'impostazione di diagnostica per trasmettere l'output della console a un [Hub eventi di Azure](/azure/event-hubs/). Usare quindi il plug-in [LogStash EventHub](https://github.com/logstash-plugins/logstash-input-azure_event_hubs) per inserire gli eventi registrati in LogStash.

#### <a name="splunk"></a>Splunk

Se si usa Splunk per l'aggregazione dei log, configurare l'impostazione di diagnostica per trasmettere l'output della console all'[archiviazione BLOB di Azure](/azure/storage/blobs/). Usare quindi il [componente aggiuntivo Splunk per servizi cloud Microsoft](https://splunkbase.splunk.com/app/3757/) per inserire gli eventi registrati in Splunk.
