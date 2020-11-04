---
title: Registrazione, metriche e telemetria in Azure
description: Informazioni sulle opzioni di registrazione in Azure
ms.topic: how-to
ms.date: 10/22/2020
ms.custom: devx-track-js
ms.openlocfilehash: 32abae7005cea4076c000ec92039df4d27a0ec10
ms.sourcegitcommit: 03240a3beece1407a140656d246e11856dc72ac7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/23/2020
ms.locfileid: "92493925"
---
# <a name="logging-metrics-and-telemetry-in-azure"></a>Registrazione, metriche e telemetria in Azure 

Con Azure sono disponibili diverse opzioni per la registrazione, le metriche e la telemetria. Esaminare le opzioni per trovare lo strumento o il servizio adatto alle proprie esigenze:

* [Metriche delle risorse](#resource-metrics-provided-by-azure-services): quando si usano i servizi di Azure, Azure monitora le singole risorse e raccoglie le metriche.  
* [Metriche personalizzate](#custom-logging-to-azure): quando è necessario registrare informazioni dell'applicazione (in locale, nel cloud o in un ambiente ibrido).
* [Librerie client di Azure SDK](#azure-sdk-client-library-logging): quando è necessario visualizzare la registrazione già incorporata nelle librerie client di Azure

## <a name="resource-metrics-provided-by-azure-services"></a>Metriche delle risorse fornite dai servizi di Azure

[Monitoraggio di Azure](/azure/azure-monitor/overview) ottimizza la disponibilità e le prestazioni delle applicazioni e dei servizi in uso offrendo una soluzione completa per raccogliere e analizzare la telemetria e intervenire di conseguenza dal cloud e dagli ambienti locali.

Le metriche sono disponibili all'interno delle risorse nel portale di Azure. 

:::image type="content" source="../media/logging-metrics/azure-resource-metrics-portal.png" alt-text="Semplice app Node.js connessa al database MongoDB.":::

Una volta monitorati i dati, usare il portale di Azure per eseguire query comuni o [query Kusto](/azure/data-explorer/kusto/query/) personalizzate. 

## <a name="custom-logging-to-azure"></a>Registrazione personalizzata in Azure

Usare la funzionalità [Application Insights](/azure/azure-monitor/app/app-insights-overview) di Monitoraggio di Azure, che offre scenari per [server](/azure/azure-monitor/app/nodejs) (Node.js) e [client](/azure/azure-monitor/app/javascript) (browser):

* Server: log di Node.js con [Application Insights](/azure/azure-monitor/app/app-insights-overview) - [pacchetto npm](https://www.npmjs.com/package/applicationinsights)
* Client: log del codice client - [pacchetto npm](https://www.npmjs.com/package/@microsoft/applicationinsights-web)
* Contenitori e VM: log del [cluster Kubernetes](/azure/azure-monitor/insights/container-insights-overview) o di [macchine virtuali di Azure](/azure/azure-monitor/insights/vminsights-overview)
 
## <a name="azure-sdk-client-library-logging"></a>Registrazione della libreria client di Azure SDK

In genere, non è necessario accedere alla registrazione della libreria client interna di Azure SDK. La libreria client principale di Azure per la registrazione è destinata all'uso da parte dei servizi di Azure. 

[Pacchetto NPM](https://www.npmjs.com/package/@azure/logger) | [Codice sorgente della libreria](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/core/logger)

### <a name="enable-logging"></a>Abilitazione della registrazione

È possibile abilitare la registrazione nell'intera applicazione usando una singola variabile di ambiente oppure configurare dinamicamente la registrazione solo per una parte dell'applicazione. Questo articolo descrive i concetti chiave relativi al pacchetto di logger e illustra come abilitare la registrazione con i metodi seguenti:

- Impostare la variabile di ambiente `AZURE_LOG_LEVEL`.
- Chiamare l'oggetto `setLogLevel` importato dalla libreria di logger.
- Chiamare `enable()` su logger specifici.

> [!NOTE]
> Questo articolo si applica alle librerie client che usano le versioni più recenti di Azure SDK. Per verificare se una libreria è supportata, vedere l'elenco delle versioni più recenti di [Azure SDK](https://azure.github.io/azure-sdk/releases/latest/index.html#javascript). Se l'applicazione usa una versione precedente delle librerie client di Azure SDK, fare riferimento a istruzioni specifiche nella documentazione del servizio applicabile.

### <a name="log-levels"></a>Livelli di registrazione

Il pacchetto `@azure/logger` supporta i livelli di log seguenti specificati nell'ordine dal più dettagliato al meno dettagliato:

- verbose
- info
- warning
- error

Quando si imposta un livello di log, a livello di codice o tramite la variabile di ambiente `AZURE_LOG_LEVEL`, verrà emesso qualsiasi log scritto con un livello di log minore o uguale a quello scelto. Se ad esempio si imposta il livello di log su `warning`, verranno generati tutti i log con il livello `warning` o `error`.

### <a name="install-the-logger-package"></a>Installare il pacchetto di logger

La libreria di logger Azure SDK per JavaScript viene fornita come pacchetto [npm](https://www.npmjs.com/). Usare npm per installare il pacchetto `@azure/logger`:

```cmd
npm install @azure/logger
```

### <a name="set-the-logging-environment-variable"></a>Impostare la variabile di ambiente di registrazione

Per abilitare la registrazione nell'intera applicazione, è possibile usare la singola variabile di ambiente `AZURE_LOG_LEVEL`. I log verranno restituiti in stderr. Dopo aver impostato la variabile di ambiente, è necessario riavviare l'applicazione per avviare la generazione dei log.

Questo esempio Bash imposta il livello di log su verbose:

```bash
export AZURE_LOG_LEVEL="verbose"
```

Questo esempio usa PowerShell:

```powershell
$env:AZURE_LOG_LEVEL="verbose"
```

Questo esempio usa CMD:

```cmd
set AZURE_LOG_LEVEL="verbose"
```

### <a name="configure-dynamic-logging"></a>Configurare la registrazione dinamica

Azure SDK per JavaScript consente di abilitare dinamicamente la registrazione in base alle esigenze o per librerie client specifiche. Questa opzione supporta gli sviluppatori che vogliono usare un'implementazione di registrazione personalizzata per alcuni componenti dell'applicazione o che vogliono acquisire log temporanei a scopo di debug.

È possibile usare il modulo `@azure/logger` per impostare il livello di log nel codice:

```js
import { setLogLevel } from "@azure/logger";

setLogLevel("verbose");
```

Per abilitare un canale di log specifico, importare `logger` dal pacchetto per cui emettere i log. L'esempio seguente abilita la registrazione delle informazioni solo per gli hub eventi:

```js
import { logger } from "@azure/event-hubs";

logger.info.enable();
```

### <a name="redirect-log-output"></a>Reindirizzare l'output dei log

Per gestire i messaggi di log manualmente, riassegnare il metodo `log` su `AzureLogger` o su qualsiasi logger importato da una libreria client.

Questo esempio reindirizza i messaggi di log a stderr:

```js
import { AzureLogger } from "@azure/logger";

AzureLogger.log = msg => console.error(msg);
```

Questo esempio reindirizza solo i messaggi di log di informazioni da Hub eventi di Azure:

```js
import { logger } from "@azure/event-hubs";
logger.info.log = msg => console.error(msg);
```

## <a name="next-steps"></a>Passaggi successivi

- [Abilitare la registrazione diagnostica per le app nel Servizio app di Azure](/azure/app-service/troubleshoot-diagnostic-logs)
- [Esaminare le opzioni di registrazione e controllo di sicurezza di Azure](/azure/security/fundamentals/log-audit)
- [Informazioni su come usare i log della piattaforma Azure](/azure/azure-monitor/platform/platform-logs-overview)