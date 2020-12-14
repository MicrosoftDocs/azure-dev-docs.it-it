---
title: Selezione dello strumento - JavaScript - Azure
description: Installare i singoli strumenti per lo sviluppo con Node.js e JavaScript in Azure
ms.topic: reference
ms.date: 12/07/2020
ms.custom: seo-javascript-september2019, seo-javascript-october2019, devx-track-js
ms.openlocfilehash: 714d096e4afde345bffa2582026f28fa2126a38c
ms.sourcegitcommit: ae2fa266a36958c04625bb0ab212e6f2db98e026
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/08/2020
ms.locfileid: "96857794"
---
# <a name="tools-for-javascript-developers-on-azure"></a>Strumenti per sviluppatori JavaScript in Azure 

JavaScript è un ecosistema di molti strumenti. Questo articolo include una selezione di strumenti creati e gestiti da Microsoft per gli sviluppatori JavaScript. Gli strumenti presentano miglioramenti per i nuovi servizi di Azure e i nuovi scenari di distribuzione/hosting. 

Non è necessario usare questi strumenti per usare Azure, ma consentono di migliorare significativamente l'esperienza, a livello di funzionalità e di supporto. 

## <a name="azure-portal"></a>Portale di Azure

Il [portale di Azure](https://portal.azure.com/) fornisce l'accesso a tutte le sottoscrizioni e a tutte le risorse per l'account. 

## <a name="azure-cli"></a>Interfaccia della riga di comando di Azure
L'interfaccia della riga di comando di Azure è ottimizzata per la gestione delle risorse di Azure dalla riga di comando. 

L'interfaccia della riga di comando di Azure offre gli scenari d'uso seguenti:

* [Installazione locale](/cli/azure/install-az-cli2)
* [Shell Web](https://shell.azure.com/)
* [Contenitore](/cli/azure/run-azure-cli-docker)

Se si usa il portale di Azure, l'interfaccia della riga di comando di Azure sarà disponibile dalla barra di spostamento superiore nel portale.

:::image type="content" source="media/azure-tools/azure-portal-select-azure-cloud-shell.png" alt-text="Se si usa il portale di Azure, l'interfaccia della riga di comando di Azure sarà disponibile dalla barra di spostamento superiore nel portale.":::

## <a name="typescript"></a>TypeScript

[TypeScript](https://www.typescriptlang.org/download) offre tutte le funzionalità di JavaScript, oltre a un livello superiore: il sistema di tipi di TypeScript. Il codice JavaScript funzionante esistente è anche un codice TypeScript. Il vantaggio principale di TypeScript è che può evidenziare un comportamento imprevisto nel codice, riducendo la possibilità di bug.

## <a name="typescript-and-the-azure-sdk-client-libraries"></a>TypeScript e le librerie client di Azure SDK

La documentazione di riferimento per le librerie client di Azure SDK è stata scritta per TypeScript perché le librerie client sono scritte con TypeScript. Non è necessario usare TypeScript per usare le librerie client di Azure SDK. 

Altre informazioni sulle [linee guida di TypeScript per Azure SDK](https://azure.github.io/azure-sdk/typescript_introduction.html).

## <a name="visual-studio-code"></a>Visual Studio Code

[Visual Studio Code](https://code.visualstudio.com) è l'ambiente di sviluppo integrato preferito per lo sviluppo JavaScript per Azure. L'interfaccia, le funzionalità e le estensioni interagiscono per ridurre il tempo necessario per lo sviluppo e le complessità dello sviluppo. 

Creare un'area di lavoro del progetto alla radice del progetto di sviluppo locale, quindi aggiungere tutte le configurazioni, le impostazioni e le estensioni rilevanti. Archiviare il file dell'area di lavoro con il progetto, in modo che ogni membro del team possa accedere alle impostazioni e agli strumenti necessari per il progetto.

L'uso di Visual Studio Code offre numerosi vantaggi:

* Visual Studio Code mostra la documentazione di riferimento di Azure inline
* Visual Studio Code offre il completamento istruzioni
* Pochi oggetti o tipi ambigui

Visual Studio Code offre un'ampia documentazione per l'[uso dei progetti JavaScript](https://code.visualstudio.com/docs/nodejs/working-with-javascript). 

## <a name="visual-studio-code-extensions"></a>Estensioni di Visual Studio Code
Per interagire con i servizi di Azure direttamente in Visual Studio Code, usare le estensioni gratuite seguenti.

| Strumento | Descrizione  |
|:---------:|---------|
| [Funzioni di Azure](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions "Collegamento all'estensione Funzioni di Azure") <br> [![Strumenti: Funzioni di Azure](media/node-azure-tools/icon-azure-functions.png)](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions) | Creare, gestire, visualizzare e distribuire funzioni ed eseguirne il debug|
| [Servizio app di Azure](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice "Collegamento all'estensione Servizio app di Azure") <br> [![Strumenti: Servizio app](media/node-azure-tools/icon-azure-app-service.png)](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice) | Esplorare siti e il portale di Azure, creare nuovi siti ed eseguire la distribuzione in slot |
| [Cosmos DB](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb "Collegamento all'estensione Cosmos DB" )  <br> [![Strumenti: Cosmos DB](media/node-azure-tools/icon-cosmos-db.png)](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb)| Creare, esplorare e aggiornare database multimodello distribuiti a livello globale in Azure |
| [Docker](https://marketplace.visualstudio.com/items?itemName=formulahendry.docker-explorer)   <br> [![Docker](media/node-azure-tools/icon-docker.png)](https://marketplace.visualstudio.com/items?itemName=formulahendry.docker-explorer)| Gestire immagini e contenitori Docker, l'hub Docker e Registro Azure Container |

> [!div class="nextstepaction"]
> [Altre estensioni di Azure nel marketplace per Visual Studio Code](https://marketplace.visualstudio.com/search?term=azure&target=VSCode&category=All%20categories&sortBy=Relevance)
