---
title: Creare siti statici in Azure con Node.js, API e markup
description: Come usare Azure per creare un'app JAMstack (JavaScript, API e markup)
author: kraigb
manager: barbkess
ms.devlang: nodejs
ms.topic: article
ms.service: azure-nodejs
ms.date: 08/20/2019
ms.author: kraigb
ms.custom: seo-javascript-september2019
ms.openlocfilehash: dc1d376be0f57d7d79a7a67d43dca49c30163c90
ms.sourcegitcommit: 52fa18873a6a8dc7f28c063cca0175bae2720b2a
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/09/2019
ms.locfileid: "70808469"
---
# <a name="how-to-build-jamstack-static-site-web-apps-with-azure"></a>Come creare app Web JAMstack (sito statico) con Azure

È possibile creare e gestire vantaggiosamente straordinarie app Web combinando un front-end *JavaScript*, le *API* (API personalizzate o di terze parti compilate come codice serverless) e il *markup* (HTML e CSS) basato su modelli fornito come pagine statiche. Con questa combinazione, nota anche come JAMstack, non è necessario scrivere complicato codice back-end per fornire pagine Web. Il sistema invece fornisce solo pagine statiche (HTML, CSS e JavaScript) che chiamano le API di cui si dispone per il lavoro sul lato server. Poiché è possibile scrivere tali API con tecnologie serverless a scalabilità automatica, si evitano completamente i costi e le problematiche di sicurezza correlati all'uso di tipici host Web o server sempre attivi. Per altre informazioni, vedere [jamstack.org](https://jamstack.org/).

Per implementare un sito statico/JAMstack in Azure, è possibile usare diversi strumenti e servizi:

- Configurare un database in base alle esigenze.
- Implementare codice API serverless in Funzioni di Azure. Queste API in genere usano il database.
- Scegliere le librerie desiderate per lo sviluppo front-end, ad esempio Angular. Caricare quindi questi file HTML, CSS e JavaScript statici in Archiviazione BLOB di Azure, che fornisce un server Web predefinito.
- Creare un proxy inverso in modo che tutto il traffico passi per un dominio URL.

È possibile guardare una demo del processo con la sessione //build 2019 [Sviluppo front-end produttivo con JavaScript, Visual Studio Code e Azure](https://mybuild.techcommunity.microsoft.com/sessions/77038?source=sessions#top-anchor).

> [!VIDEO https://medius.studios.ms/Embed/Video-nc/B19-BRK3021?latestplayer=true]

È disponibile un'esercitazione dettagliata in [ Distribuire un sito Web statico in Azure](https://code.visualstudio.com/tutorials/static-website/getting-started) nella documentazione di Visual Studio Code.

Anche gli articoli seguenti forniscono altri dettagli:

- **Database**: è possibile usare qualsiasi database si voglia, ad esempio i diversi servizi di database in Azure descritti in [Come integrare database di Azure nelle app Node.js](node-howto-integrate-databases.md).
  
- **API serverless**:

  - Iniziare da [Creare la prima funzione con Visual Studio Code](/azure/azure-functions/functions-create-first-function-vs-code), che presenta Funzioni di Azure nel contesto di Visual Studio Code, che semplifica molti dei dettagli.
  - Al termine dell'articolo, si avrà un progetto di Funzioni di Azure (una cartella) che contiene una sottocartella denominata per la funzione, che corrisponde al relativo endpoint HTTP. Tale cartella della funzione contiene un file *index.js* con il codice.
  - È possibile modificare la funzione in base alle esigenze, nonché aggiungere altre funzioni al progetto e quindi distribuirle nuovamente in Azure dove sono disponibili pubblicamente.
  - Per altre risorse sullo sviluppo serverless, vedere [Come scrivere codice Node.js serverless in Azure](node-howto-write-serverless-code.md).

- **Distribuire il front-end in Archiviazione di Azure**: con le API a disposizione, è ora possibile scrivere il codice front-end per usare tali API, sfruttando qualsiasi framework si voglia. Quando si è pronti, seguire l'articolo [Esercitazione: Ospitare un sito Web statico nell'archiviazione BLOB](/azure/storage/blobs/storage-blob-static-website-host) per caricare questi file in Azure e attivare l'hosting di siti Web statici.

- **Creare un proxy inverso**: un proxy inverso, come illustrato in [Usare i proxy di Funzioni di Azure](/azure/azure-functions/functions-proxies), consente di indirizzare facilmente determinate richieste a URL diversi. In questo caso, si vogliono indirizzare le richieste per i file front-end all'URL di Archiviazione di Azure, dove sono stati distribuiti tali file, e le richieste API all'URL di Funzioni di Azure.

  - Per creare questi proxy, modificare il file *proxies.json* nel progetto di Funzioni in modo che appaia come illustrato di seguito, sostituendo `<storage_url>` e `<api_url>` con i propri URL:
  
    ```json
    {
      "$schema": "http://json.schemastore.org/proxies",
      "proxies": {
        "Static frontend on Azure Storage": {
          "matchCondition": {
            "route": "/{*restOfPath}"
          },
          "backendUri": "<storage_url>/{restOfPath}"
        },
        "Azure Functions API": {
          "matchCondition": {
            "route": "/api/{*restOfPath}"
          },
          "backendUri": "<api_url>/api/{restOfPath}"
        }
      }
    }
    ```
