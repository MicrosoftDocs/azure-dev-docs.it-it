---
title: Scrivere codice Node.js serverless con Funzioni di Azure
description: Indicazioni su come usare Funzioni di Azure per creare e distribuire codice serverless.
author: kraigb
manager: barbkess
ms.devlang: nodejs
ms.topic: article
ms.service: azure-nodejs
ms.date: 08/19/2019
ms.author: kraigb
ms.openlocfilehash: a5ed7d5d99009593845966969217f8a5081da4c4
ms.sourcegitcommit: 9cd460ee16b637e701aa30078932878c0d0a7945
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/30/2019
ms.locfileid: "70181950"
---
# <a name="how-to-write-serverless-nodejs-code-on-azure"></a>Come scrivere codice Node.js serverless in Azure

Il codice serverless consente di creare endpoint su richiesta reattivi su Internet senza doversi preoccupare del provisioning o della gestione dell'infrastruttura. Il codice serverless è composto da script o altre parti di codice eseguite in risposta a vari eventi. In Azure l'offerta serverless è denominata Funzioni di Azure.

Per iniziare, vedere:

- [Creare la prima funzione con Visual Studio Code](/azure/azure-functions/functions-create-first-function-vs-code). Questo articolo presenta Funzioni di Azure nel contesto di Visual Studio Code, che semplifica molti dei dettagli.

A questo punto, approfondire la conoscenza delle operazioni supportate in Funzioni di Azure prendendo visione degli articoli seguenti:

- [Introduzione a Funzioni di Azure](/azure/azure-functions/functions-overview), che descrive i vantaggi dello sviluppo serverless, i costi e i diversi trigger che è possibile usare per eseguire codice serverless.

- [Concetti relativi a trigger e associazioni in Funzioni di Azure](/azure/azure-functions/functions-triggers-bindings)

- [Guida per sviluppatori di Funzioni di Azure](/azure/azure-functions/functions-reference) e quindi [Guida per gli sviluppatori JavaScript di Funzioni di Azure](/azure/azure-functions/functions-reference-node)

- Se si è interessati a scrivere funzioni *con stato* in un ambiente serverless, vedere [Informazioni su Durable Functions](/azure/azure-functions/durable/durable-functions-overview) e [Creare la prima funzione durevole in JavaScript](/azure/azure-functions/durable/quickstart-js-vscode).

Da qui è possibile accedere a molte altre risorse che consentono di esplorare ulteriormente il codice serverless:

- Modulo Microsoft Learn: [Abilitare gli aggiornamenti automatici in un'app Web con Funzioni di Azure e il Servizio SignalR](https://docs.microsoft.com/learn/modules/automatic-update-of-a-webapp-using-azure-functions-and-signalr/)

- Informazioni sull'uso di diversi trigger per eseguire codice serverless:

  - [Eseguire codice su un timer](/azure/azure-functions/functions-create-scheduled-function)
  - [Eseguire codice quando vengono caricati o aggiornati file in Archiviazione BLOB di Azure](/azure/storage/blobs/storage-upload-process-images?tabs=nodejsv10)
  - [Eseguire codice quando viene scritto un messaggio in Archiviazione code di Azure](/azure/azure-functions/functions-create-storage-queue-triggered-function)

- [Archiviare dati non strutturati usando Funzioni di Azure e Azure Cosmos DB](/azure/azure-functions/functions-integrate-store-unstructured-data-cosmosdb.md?tabs=javascript). Per informazioni su altri database, vedere [Come integrare i database di Azure in codice Node.js](node-howto-integrate-databases.md)

- [Scrivere codici per Funzioni di Azure e testarle in locale](/azure/azure-functions/functions-develop-local)

- [Strategie per il test del codice in Funzioni di Azure](/azure/azure-functions/functions-test-a-function) e [Gestione degli errori](/azure/azure-functions/functions-bindings-error-pages)

- [Configurare l'autenticazione con Azure Active Directory](/azure/app-service/configure-authentication-provider-aad.md?toc=%2fazure%2fazure-functions%2ftoc.json)

- [Configurare la distribuzione continua con Azure Pipelines](/azure/azure-functions/functions-how-to-azure-devops)

- [Esempi di Node.js + Funzioni di Azure](/samples/browse/?languages=javascript%2Cnodejs&products=azure-functions)