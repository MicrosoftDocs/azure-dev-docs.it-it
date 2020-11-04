---
title: Codice Node.js serverless con Funzioni di Azure
description: Funzioni di Azure offre un'infrastruttura di codice serverless che consente di creare endpoint HTTP reattivi su richiesta.
ms.topic: how-to
ms.date: 10/27/2020
ms.custom: seo-javascript-september2019, seo-javascript-october2019, devx-track-js
ms.openlocfilehash: bcf8528bbb5011f10fdbb57b31b08a426fd8d8d8
ms.sourcegitcommit: 3d3ee59f73c966da7df65bada49e059d02e74b91
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/28/2020
ms.locfileid: "92898748"
---
# <a name="use-azure-functions-to-develop-nodejs-serverless-code"></a>Usare Funzioni di Azure per sviluppare codice Node.js serverless

Funzioni di Azure offre un'infrastruttura di codice serverless che consente di creare endpoint HTTP reattivi su richiesta. Il codice serverless è composto da codice JavaScript o TypeScript che viene eseguito in risposta a vari eventi. 

Le funzioni vengono eseguite su un servizio Web, come codice o contenitore Docker, che viene astratto per cui è possibile concentrarsi sul codice per l'endpoint. Le funzioni consentono anche di attivare un'altra funzione, per cui un flusso di lavoro di una funzione può sostituire la funzionalità server back-end ospitata esistente e rimuovere la necessità di gestire tale server. 

## <a name="what-is-a-function-resource"></a>Che cos'è una risorsa di Funzioni?

Una risorsa di Funzioni di Azure è un'unità logica per tutte le funzioni correlate in una singola località geografica di Azure. La risorsa può contenere una o più funzioni, che possono essere indipendenti l'una dall'altra o correlate con trigger di input o output. È possibile scegliere tra molte funzioni comuni o crearne una personalizzata.

:::image type="content" source="../media/howto-serverless/portal-screenshot-new-azure-function-type.png" alt-text="È possibile scegliere tra molte funzioni comuni o crearne una personalizzata.":::

Le impostazioni delle risorse di Funzioni includono le tipiche configurazioni serverless, come variabili di ambiente, autenticazione, registrazione e CORS.  

## <a name="durable-stateful-functions"></a>Funzioni durevoli, con stato 

L'estensione [Durable Functions](/azure/azure-functions/durable/durable-functions-overview) consente di scrivere funzioni che mantengono lo *stato* o di gestire funzioni a esecuzione prolungata in Azure. [Creare la prima funzione durevole in JavaScript](/azure/azure-functions/durable/quickstart-js-vscode).

## <a name="static-web-apps-include-functions"></a>Le app Web statiche includono funzioni 

Quando si sviluppa un'applicazione client front-end statica, ad esempio Angular, React o Vue, che necessita anche di API serverless, usare [app Web statiche](/azure/static-web-apps/getting-started?tabs=react) con funzioni per rispettare entrambi i requisiti. 

## <a name="a-simple-javascript-function-for-http-requests"></a>Una semplice funzione JavaScript per le richieste HTTP

Per funzione si intende una funzione asincrona esportata con informazioni su richiesta e contesto. Lo screenshot parziale seguente del portale di Azure mostra il codice della funzione. 

:::image type="content" source="../media/howto-serverless/portal-screenshot-azure-function-http.png" alt-text="Screenshot di una funzione di Azure nel Portale.":::

## <a name="develop-functions-locally-with-visual-studio-code-and-extensions"></a>Sviluppare le funzioni in locale con Visual Studio Code ed estensioni

Creare la [prima funzione](/azure/azure-functions/functions-create-first-function-vs-code) con Visual Studio Code. Visual Studio Code semplifica molti dettagli con l'[estensione Funzioni di Azure](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions).

Questa estensione consente di creare funzioni JavaScript e TypeScript con modelli comuni. 

Ecco un esempio in JavaScript di una funzione HTTP per Azure: 

```nodejs
module.exports = async function (context, req) {
    context.log('JavaScript HTTP trigger function processed a request.');

    const name = (req.query.name || (req.body && req.body.name));
    const responseMessage = name
        ? "Hello, " + name + ". This HTTP triggered function executed successfully."
        : "This HTTP triggered function executed successfully. Pass a name in the query string or in the request body for a personalized response.";

    context.res = {
        // status: 200, /* Defaults to 200 */
        body: responseMessage
    };
}
```

Ecco un esempio in TypeScript di una funzione HTTP per Azure: 

```typescript
import { AzureFunction, Context, HttpRequest } from "@azure/functions"

const httpTrigger: AzureFunction = async function (context: Context, req: HttpRequest): Promise<void> {
    context.log('HTTP trigger function processed a request.');
    const name = (req.query.name || (req.body && req.body.name));
    const responseMessage = name
        ? "Hello, " + name + ". This HTTP triggered function executed successfully."
        : "This HTTP triggered function executed successfully. Pass a name in the query string or in the request body for a personalized response.";

    context.res = {
        // status: 200, /* Defaults to 200 */
        body: responseMessage
    };

};

export default httpTrigger;
```

## <a name="configuring-the-function"></a>Configurazione della funzione

Le funzioni vengono configurate con il file **function.json**. Questa configurazione consente di configurare il modo in cui la funzione viene attivata ("direzione": in ingresso) e il risultato restituito ("direzione": in uscita). Consente inoltre di impostare le variabili di ambiente e altre informazioni necessarie per il funzionamento della funzione. Vedere altre informazioni su [trigger e binding](/azure/azure-functions/functions-triggers-bindings?tabs=javascript.md). 

```json
{
  "bindings": [
    {
      "authLevel": "function",
      "type": "httpTrigger",
      "direction": "in",
      "name": "req",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "type": "http",
      "direction": "out",
      "name": "res"
    }
  ]
}
```

## <a name="develop-functions-remotely-using-the-azure-portal"></a>Sviluppare funzioni in remoto con il portale di Azure

Quando si [crea una funzione di Azure con il portale di Azure](https://ms.portal.azure.com/#create/Microsoft.FunctionApp), è possibile configurare la funzione, scrivere il codice all'interno di un modello già popolato e testare la funzione. 

Il portale crea solo funzioni JavaScript, non TypeScript. Per sviluppare con TypeScript, scaricare la funzione o crearla in locale in Visual Studio Code con l'estensione Funzioni. 

## <a name="next-steps"></a>Passaggi successivi

Il [guida per sviluppatori di Funzioni di Azure per JavaScript](/azure/azure-functions/functions-reference-node) è un buon punto di partenza. 

Usare il modulo di Microsoft Learn per informazioni su come [abilitare gli aggiornamenti automatici in un'app Web con Funzioni di Azure e il Servizio SignalR](/learn/modules/automatic-update-of-a-webapp-using-azure-functions-and-signalr/).

* [Eseguire codice su un timer](/azure/azure-functions/functions-create-scheduled-function)
* [Eseguire codice quando vengono caricati o aggiornati file in Archiviazione BLOB di Azure](/azure/storage/blobs/storage-upload-process-images?tabs=nodejsv10)
* [Eseguire codice quando viene scritto un messaggio in Archiviazione code di Azure](/azure/azure-functions/functions-create-storage-queue-triggered-function)
* [Archiviare dati non strutturati usando Funzioni di Azure e Azure Cosmos DB](/azure/azure-functions/functions-integrate-store-unstructured-data-cosmosdb?tabs=javascript)
* [Esempi di Node.js + Funzioni di Azure](/samples/browse/?languages=javascript%2Cnodejs&products=azure-functions)