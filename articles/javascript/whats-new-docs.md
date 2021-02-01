---
title: Novità della documentazione di JavaScript
description: Novità della documentazione di JavaScript nel Developer Center
ms.topic: conceptual
ms.date: 01/29/2021
ms.openlocfilehash: 15e556d741ee94675932942c6235863fab79a084
ms.sourcegitcommit: 3f8aa923e4626b31cc533584fe3b66940d384351
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/01/2021
ms.locfileid: "99224695"
---
# <a name="javascript-docs-whats-new"></a>Documentazione di JavaScript: Novità

Contenuti nuovi e aggiornati disponibili per sviluppatori JavaScript e TypeScript.

## <a name="2021-january"></a>Gennaio 2021

### <a name="whats-new"></a>Novità

|Nome|Note|
|---------------------------------------|--|
|[Novità dei sostenitori degli sviluppatori](whats-new-developer-advocacy.md)|Blog, video, informazioni sui moduli|
|[Esercitazione: convertire il testo in sintesi vocale](./tutorial/convert-text-to-speech-cognitive-services.md)|In questa esercitazione aggiungere il riconoscimento vocale di servizi cognitivi a un'app Express.js esistente per aggiungere la conversione da testo a riconoscimento vocale usando il servizio di riconoscimento vocale di servizi cognitivi. La conversione di testo in sintesi vocale consente di fornire audio senza il costo di generare manualmente l'audio.|
|Guida alle procedure con l'interfaccia della riga di comando di Azure|* [Creare e usare il registro contenitori](./how-to/with-azure-cli/create-container-registry-resource.md)<br>* [Configurazione di un nome di dominio personalizzato](./how-to/with-azure-cli/configure-app-service-custom-domain-name.md)<br>* [Creare e usare MongoDB in Azure con Cosmos DB](./how-to/with-azure-cli/create-mongodb-cosmosdb.md) |
|Guida alle procedure con Visual Studio Code|* [Sviluppare ed eseguire il debug di Node.js](./how-to/with-visual-studio-code/install-run-debug-nodejs.md)<br>* [Clonare e usare un repository GitHub](./how-to/with-visual-studio-code/clone-github-repository.md)<br>* [Creare un'immagine del contenitore dal progetto JavaScript locale](./how-to/with-visual-studio-code/containerize-local-project.md)|

### <a name="whats-updated"></a>Aggiornamenti

|Nome|Note|
|---------------------------------------|--|
|[**Per principianti**](learn-azure-javascript.md#getting-started)|Varie raccolte di materiali online per iniziare a usare JavaScript, Node.js, sviluppo Web e altre aree di interesse per gli sviluppatori JavaScript.|
|[Attività principali per sviluppatori JavaScript](how-to/common-javascript-tasks.md)|Un esempio delle attività correnti.|
|[Configurare il file di avvio Visual Studio Code](./how-to/configure-web-app-settings.md#configure-browser-for-cors-to-connect-with-server)|Se è necessario connettersi al proprio server e occorre ignorare la sicurezza CORS durante l'esecuzione e il debug con il client in locale, la soluzione consigliata consiste nel configurare questa impostazione nel file di debug di Visual Studio Code, `launch.json`, per passare le impostazioni al browser in modo da disabilitare la sicurezza.|

## <a name="2020-december"></a>Dicembre 2020

### <a name="whats-new"></a>Novità

|Nome|Note|
|---------------------------------------|--|
|[Esercitazione: Aggiungere il pulsante di accesso all'app Web statica React per l'autenticazione Microsoft](./tutorial/single-page-application-azure-login-button-sdk-msal.md)|L'autenticazione di Azure presentata in questa esercitazione è un pulsante di accesso e disconnessione e fornisce l'accesso all'account di un utente. Sviluppare l'applicazione con un SDK lato client di Azure, `@azure/msal-browser`, per gestire l'interazione dell'utente nell'applicazione a pagina singola.|
|[Che cos'è Azure per sviluppatori JavaScript?](core/what-is-azure-for-javascript-development.md)|Concetti di Azure che gli sviluppatori JavaScript devono conoscere per avere successo.|
|[Installare Node.js](core/install-nodejs-develop-azure-sdk-project.md)|Installare e gestire Node.js per scenari di sviluppo comuni di Azure|
|[Configurare le app Web in Azure](how-to/configure-web-app-settings.md)|Informazioni su come impostare configurazioni comuni per l'app Web.|
|[Identità, autenticazione e utenti](concepts/identity-authentication-users.md)|Questo articolo è incentrato sui concetti principali che uno sviluppatore JavaScript deve comprendere.|
|[Principali attività comuni per sviluppatori JavaScript](how-to/common-javascript-tasks.md)|Un esempio delle attività correnti.|
|[Automatizzare le attività con l'interfaccia della riga di comando di Azure](core/automate-tasks-with-azure-cli.md)|L'automazione delle attività di Azure è un requisito comune per la distribuzione continua negli ambienti host. L'interfaccia della riga di comando di Azure è la scelta consigliata per gli sviluppatori JavaScript che gestiscono le attività ed eseguono distribuzioni da qualsiasi posizione.|

### <a name="whats-new-in-learn"></a>Novità di Learn


|Nome|
|---------------------------------------|
|App Web statica, JavaScript, CodeTour: Usare le statistiche del basket per ottimizzare la riproduzione dei giochi con Visual Studio Code, su ispirazione di SPACE JAM NEW LEGENDS - [Learn](/learn/paths/optimize-basketball-games-with-machine-learning/)|
|Creare un semplice sito Web con HTML, CSS e JavaScript - [Learn](/learn/modules/build-simple-website/)|
|Usare Visual Studio Code per creare un dashboard JavaScript e Vue.js con un'API serverless basata su Funzioni di Azure e Node.js. - [Learn](/learn/modules/build-api-azure-functions)|

## <a name="2020-november"></a>Novembre 2020

Novità della documentazione di JavaScript di novembre 2020. Questo articolo elenca alcune delle principali modifiche apportate alla documentazione durante questo periodo.

### <a name="whats-new"></a>Novità

|Nome|Note|
|---------------------------------------|--|
|[Esercitazione: Compilare e distribuire un'app Web statica React in Azure](./tutorial/static-web-app/introduction.md)|In questa esercitazione, compilare e distribuire un'applicazione client React in un'app Web statica di Azure con un'azione GitHub.<br>L'app create-react-app consente di analizzare un'immagine con Visione artificiale di Servizi cognitivi. L'azione GitHub viene avviata quando viene eseguito un push a un ramo remoto specifico, creando il client React (create-react-app) e spostando i file risultanti nella risorsa dell'app Web statica di Azure.|
|[Esercitazione: Distribuire un'app in una macchina virtuale Linux](./tutorial/nodejs-virtual-machine-vm/introduction.md)|In questa esercitazione verrà creata una macchina virtuale Linux per un'app Express.js. La macchina virtuale è configurata con un file di configurazione cloud-init e include NGINX e un repository GitHub per un'app Express.js. Quando la macchina virtuale è in esecuzione, è possibile connettersi alla macchina virtuale con SSH, modificare l'app Web in modo da includere la registrazione della traccia e visualizzare l'app server Express.js pubblica in un Web browser.|

### <a name="whats-updated"></a>Aggiornamenti

|Nome|Note|
|---------------------------------------|--|
|[Learn](learn-azure-javascript.md)|Nuovi moduli e certificazioni per JavaScript.|

## <a name="2020-october"></a>Ottobre 2020

Novità della documentazione di JavaScript di ottobre 2020. Questo articolo elenca alcune delle principali modifiche apportate alla documentazione durante questo periodo.

### <a name="whats-new"></a>Novità

|Nome|Note|
|---------------------------------------|--|
|[Esercitazione: Caricare un'immagine in Archiviazione BLOB](./tutorial/browser-file-upload-azure-storage-blob.md)|In questa esercitazione si usa un'**app React** per caricare un file in un BLOB di **Archiviazione di Azure**. L'attività di programmazione viene eseguita automaticamente. Questa esercitazione è incentrata sull'uso corretto degli ambienti di Azure locali e remoti all'interno Visual Studio Code con le estensioni di Azure.|
|[Esercitazione: Distribuire Node.js con l'app di database nel Servizio app da Visual Studio Code](./tutorial/deploy-nodejs-mongodb-app-service-from-visual-studio-code.md)|In questa esercitazione si usa un'app Node.js **Express.js** con un database **MongoDB** tramite l'API nativa MongoDB. Distribuire l'applicazione Node.js nel servizio app di Azure (in Linux), quindi verificare che l'app basata sul cloud funzioni. L'attività di programmazione viene eseguita automaticamente. Questa esercitazione è incentrata sulla creazione delle risorse di Azure e sulla distribuzione in Azure da Visual Studio Code con le estensioni di Azure.|

### <a name="whats-updated"></a>Aggiornamenti

|Nome|Note|
|---------------------------------------|--|
|[Procedura: Funzioni serverless](how-to/develop-serverless-apps.md)|Le funzioni vengono eseguite su un servizio Web, come codice o contenitore Docker, che viene astratto per cui è possibile concentrarsi sul codice per l'endpoint.|
|[Introduzione Eseguire l'autenticazione con i moduli di gestione di Azure per JavaScript](core/node-sdk-azure-authenticate.md)|Esistono più modi per autenticare e creare le credenziali necessarie.|

## <a name="next-steps"></a>Passaggi successivi

* [Configurare l'ambiente di sviluppo](./core/configure-local-development-environment.md)