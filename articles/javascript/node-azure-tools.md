---
title: Selezione dello strumento - JavaScript - Azure
description: Installare i singoli strumenti per lo sviluppo con Node.js e JavaScript in Azure
ms.topic: conceptual
ms.date: 03/02/2021
ms.custom: seo-javascript-september2019, seo-javascript-october2019, devx-track-js
ms.openlocfilehash: 8d34bae3e1ac789d24db59a41ab22673e15cc819
ms.sourcegitcommit: 576c878c338d286060010646b96f3ad0fdbcb814
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/04/2021
ms.locfileid: "102117835"
---
# <a name="tools-for-javascript-developers-on-azure"></a>Strumenti per sviluppatori JavaScript in Azure 

JavaScript è un ecosistema di molti strumenti. Questo articolo include una selezione di strumenti creati e gestiti da Microsoft per gli sviluppatori JavaScript. Gli strumenti presentano miglioramenti per i nuovi servizi di Azure e i nuovi scenari di distribuzione/hosting. 

Non è necessario usare questi strumenti per usare Azure, ma consentono di migliorare significativamente l'esperienza, a livello di funzionalità e di supporto. 

## <a name="azure-portal"></a>Portale di Azure

Il [portale di Azure](https://portal.azure.com/) fornisce l'accesso a tutte le sottoscrizioni e a tutte le risorse per l'account. 

## <a name="visual-studio-code"></a>Visual Studio Code

[Visual Studio Code](https://code.visualstudio.com) è l'ambiente di sviluppo integrato preferito per lo sviluppo JavaScript per Azure. L'interfaccia, le funzionalità e le estensioni interagiscono per ridurre il tempo necessario per lo sviluppo e le complessità dello sviluppo. 

Creare un'area di lavoro del progetto alla radice del progetto di sviluppo locale, quindi aggiungere tutte le configurazioni, le impostazioni e le estensioni rilevanti. Archiviare il file dell'area di lavoro con il progetto, in modo che ogni membro del team possa accedere alle impostazioni e agli strumenti necessari per il progetto.

L'uso di Visual Studio Code offre numerosi vantaggi:

* Visual Studio Code mostra la documentazione di riferimento di Azure inline
* Visual Studio Code offre il completamento istruzioni
* Pochi oggetti o tipi ambigui

Visual Studio Code offre un'ampia documentazione per l'[uso dei progetti JavaScript](https://code.visualstudio.com/docs/nodejs/working-with-javascript). 

## <a name="visual-studio-code-extensions"></a>Estensioni di Visual Studio Code

Usare le estensioni gratuite seguenti per usare i servizi di Azure direttamente in Visual Studio Code.

| Strumento | Descrizione  |
|:---------:|---------|
|[Strumenti di Azure](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-node-azure-pack)|Ottieni hosting di siti Web, dati SQL e MongoDB, contenitori Docker, funzioni senza server e altro ancora, tutto in Azure, dal VS Code, in questa unica estensione di Microsoft.|

Se si preferisce installare singole estensioni, questo elenco include i servizi di Azure più diffusi:

| Strumento | Descrizione  |
|:---------:|---------|
| [Account Azure](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account)| Gestione di Sign-In e sottoscrizioni|
| [Funzioni di Azure](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions "Collegamento all'estensione Funzioni di Azure") <br> [![Strumenti: Funzioni di Azure](media/node-azure-tools/icon-azure-functions.png)](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions) | Creare, gestire, visualizzare e distribuire funzioni ed eseguirne il debug|
| [Servizio app di Azure](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice "Collegamento all'estensione Servizio app di Azure") <br> [![Strumenti: Servizio app](media/node-azure-tools/icon-azure-app-service.png)](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice) | Esplorare siti e il portale di Azure, creare nuovi siti ed eseguire la distribuzione in slot |
| [Cosmos DB](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb "Collegamento all'estensione Cosmos DB" )  <br> [![Strumenti: Cosmos DB](media/node-azure-tools/icon-cosmos-db.png)](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb)| Creare, esplorare e aggiornare database multimodello distribuiti a livello globale in Azure |
| [Docker](https://marketplace.visualstudio.com/items?itemName=formulahendry.docker-explorer)   <br> [![Docker](media/node-azure-tools/icon-docker.png)](https://marketplace.visualstudio.com/items?itemName=formulahendry.docker-explorer)| Gestire immagini e contenitori Docker, l'hub Docker e Registro Azure Container |
|[Archiviazione](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurestorage)|Archiviazione di Azure, inclusi contenitori BLOB, condivisioni file, tabelle e code|

> [!div class="nextstepaction"]
> [Altre estensioni di Azure nel marketplace per Visual Studio Code](https://marketplace.visualstudio.com/search?term=azure&target=VSCode&category=All%20categories&sortBy=Relevance)


## <a name="typescript"></a>TypeScript

[TypeScript](https://www.typescriptlang.org/download) offre tutte le funzionalità di JavaScript, oltre a un livello superiore: il sistema di tipi di TypeScript. Il codice JavaScript funzionante esistente è anche un codice TypeScript. Il vantaggio principale di TypeScript è che può evidenziare un comportamento imprevisto nel codice, riducendo la possibilità di bug.

## <a name="typescript-and-the-azure-sdk-client-libraries"></a>TypeScript e le librerie client di Azure SDK

La documentazione di riferimento per le librerie client di Azure SDK è stata scritta per TypeScript perché le librerie client sono scritte con TypeScript. Non è necessario usare TypeScript per usare le librerie client di Azure SDK. 

Altre informazioni sulle [linee guida di TypeScript per Azure SDK](https://azure.github.io/azure-sdk/typescript_introduction.html).


## <a name="azure-cli"></a>Interfaccia della riga di comando di Azure
L'interfaccia della riga di comando di Azure è ottimizzata per la gestione delle risorse di Azure dalla riga di comando. 

L'interfaccia della riga di comando di Azure offre gli scenari d'uso seguenti:

* [Installazione locale dell'interfaccia della riga di comando di Azure](/cli/azure/install-az-cli2)
* [Azure Cloud Shell](https://shell.azure.com/)
* [Contenitore Docker](/cli/azure/run-azure-cli-docker)

Se si usa il portale di Azure, l'interfaccia della riga di comando di Azure sarà disponibile dalla barra di spostamento superiore nel portale.

:::image type="content" source="media/azure-tools/azure-portal-select-azure-cloud-shell.png" alt-text="Se si usa il portale di Azure, l'interfaccia della riga di comando di Azure sarà disponibile dalla barra di spostamento superiore nel portale.":::

