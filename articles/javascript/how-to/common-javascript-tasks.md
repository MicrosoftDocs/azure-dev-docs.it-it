---
title: Principali attività di Azure per sviluppatori JavaScript
description: Un esempio delle attività correnti.
ms.topic: reference
ms.date: 01/20/2021
ms.custom: devx-track-js
ms.openlocfilehash: cc5ca751b8d22612c63d26a46934eb5b4c057c69
ms.sourcegitcommit: 71847ee0a1fee3f3320503629d9a8c82319a1f6a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/03/2021
ms.locfileid: "99510999"
---
# <a name="top-tasks-for-javascript-developers"></a>Attività principali per sviluppatori JavaScript

Un esempio delle attività correnti. Se non si riesce a trovare un'attività, lasciare un feedback per richiederla. 

## <a name="app-registration"></a>Registrazione dell'app

|Attività|using|
|--|--|
|Creare la registrazione dell'app|[Portale](../tutorial/single-page-application-azure-login-button-sdk-msal.md#3-create-app-registration-for-authentication)|

## <a name="application-settings"></a>Impostazioni applicazione

|Attività|using|
|--|--|
|Impostare variabili di ambiente|[Bash](../tutorial/static-web-app/create-computer-vision-resource-use-in-code.md#add-environment-variables-to-your-local-environment)|
|Creare un token di firma di accesso condiviso del contenitore di archiviazione|[Portale](../tutorial/browser-file-upload-azure-storage-blob.md#5-generate-your-shared-access-signature-sas-token)|
|Impostare il token di firma di accesso condiviso nel codice|[TypeScript](../tutorial/browser-file-upload-azure-storage-blob.md#set-sas-token-in-code-file)|
|Configurare CORS per l'archiviazione|[Portale](../tutorial/browser-file-upload-azure-storage-blob.md#6-configure-cors-for-azure-storage-resource)|

## <a name="authentication"></a>Authentication

|Attività|using|
|--|--|
|Pulsante Microsoft di accesso/disconnessione tramite `@azure/msal-browser`|[React/TypeScript](../tutorial/single-page-application-azure-login-button-sdk-msal.md#5-add-login-and-logoff-buttons)|
|Revocare un'autorizzazione di AAD|[https://myapplications.microsoft.com/](https://myapplications.microsoft.com/)|
|Revocare un'autorizzazione Consumer|[https://account.live.com/consent/manage](https://account.live.com/consent/manage)|

## <a name="azure-login"></a>Accesso ad Azure

|Attività|using|
|--|--|
|Accedi|[Interfaccia della riga di comando di Azure](../tutorial/deploy-deno-app-azure-app-service-azure-cli.md#2-sign-in-to-azure-cli)<br>[Estensione di Visual Studio Code](../tutorial/tutorial-vscode-docker-node/tutorial-vscode-docker-node-01.md#sign-in-to-azure)|


## <a name="azure-resource-groups"></a>Gruppi di risorse di Azure

|Attività|using|
|--|--|
|Creare un gruppo di risorse|[Interfaccia della riga di comando di Azure](../tutorial/static-web-app/create-computer-vision-resource-use-in-code.md#create-azure-resources)|
|Eliminare un gruppo di risorse|[Interfaccia della riga di comando di Azure](../tutorial/static-web-app/clean-up-resources.md#remove-all-the-resources-by-removing-resource-group)|

## <a name="apps"></a>App

### <a name="static-web-apps"></a>App Web statiche

|Attività|using|
|--|--|
|Creare un'app Angular|[Bash](../tutorial/tutorial-vscode-static-website-node/tutorial-vscode-static-website-node-02.md?tabs=angular)|
|Creare un'app Deno|[Bash](../tutorial/deploy-deno-app-azure-app-service-azure-cli.md#3-create-local-deno-api-app)|
|Creare un'app React destinata al linguaggio JavaScript|[Bash](../tutorial/tutorial-vscode-static-website-node/tutorial-vscode-static-website-node-02.md?tabs=react)|
|Creare un'app React destinata al linguaggio TypeScript|[Bash](../tutorial/single-page-application-azure-login-button-sdk-msal.md#4-create-react-single-page-application-for-typescript)|
|Creare un'app Vue|[Bash](../tutorial/tutorial-vscode-static-website-node/tutorial-vscode-static-website-node-02.md?tabs=vue)|
|Creare un'app Svelte|[Bash](../tutorial/tutorial-vscode-static-website-node/tutorial-vscode-static-website-node-02.md?tabs=svelte)|
|Creare un'app Web statica|[Estensione di Visual Studio Code](../tutorial/static-web-app/create-static-web-app-visual-studio-code-extension.md#create-a-static-web-app-resource)|
|Creare un'app statica ospitata nell'archiviazione|[Estensione di Visual Studio Code](../tutorial/tutorial-vscode-static-website-node/tutorial-vscode-static-website-node-03.md)|
|Distribuire un'app statica ospitata nell'archiviazione|[Estensione di Visual Studio Code](../tutorial/tutorial-vscode-static-website-node/tutorial-vscode-static-website-node-04.md)|
|Esplorazione del sito|[Estensione di Visual Studio Code](../tutorial/static-web-app/create-static-web-app-visual-studio-code-extension.md#view-azure-static-web-site-in-browser)|

### <a name="function-serverless-apps"></a>App per le funzioni (serverless)

|Attività|using|
|--|--|
|Creare app per le funzioni in locale|[Estensione di Visual Studio Code](../tutorial/tutorial-vscode-serverless-node-create-local.md)|
|Codice trigger HTTP|[JavaScript](../tutorial/tutorial-vscode-serverless-node-create-local.md#http-function-javascript-template-code)|
|Eseguire il debug e i test della funzione in locale|[Visual Studio Code](../tutorial/tutorial-vscode-serverless-node-test-local.md)|
|Distribuire la funzione nel cloud di Azure|[Estensione di Visual Studio Code](../tutorial/tutorial-vscode-serverless-node-deploy-hosting.md)|
|Verificare se la funzione è disponibile nell'URL pubblico|[Estensione/browser di Visual Studio Code](../tutorial/tutorial-vscode-serverless-node-deploy-hosting.md#verify-functions-app-is-publicly-available-with-browser)|
|Rimuovere la risorsa app per le funzioni|[Estensione di Visual Studio Code](../tutorial/tutorial-vscode-serverless-node-remove-resource.md)|

### <a name="app-service---full-stack-server-only-or-client-only-apps"></a>Servizio app: app dello stack completo, solo server o solo client

|Attività|using|
|--|--|
|Creare un'app Express.js locale|[Bash](../tutorial/deploy-nodejs-azure-app-service-with-visual-studio-code.md?tabs=bash#3-create-a-local-expressjs-app)|
|Creare una risorsa app: distribuire l'app Express.js, trasmettere i log|[Estensione di Visual Studio Code](../tutorial/deploy-nodejs-mongodb-app-service-from-visual-studio-code.md#create-web-app-resource-and-deploy-expressjs-app)|
|Creare una risorsa app: distribuire l'app Express.js, configurare le impostazioni dell'app, eseguire npm install, passare al sito Web distribuito|[Estensione di Visual Studio Code](../tutorial/deploy-nodejs-azure-app-service-with-visual-studio-code.md?tabs=bash#6-create-app-service-resource-in-visual-studio-code)|
|Creare una risorsa app|[Interfaccia della riga di comando di Azure](../tutorial/tutorial-vscode-azure-cli-node/tutorial-vscode-azure-cli-node-03.md)|
|Crea app, Distribuisci, app browser, Visualizza log|[Interfaccia della riga di comando di Azure](../tutorial/tutorial-vscode-azure-cli-node/tutorial-vscode-azure-cli-node-03.md)|
|Configurare l'app Web per l'uso della stringa di connessione del database|[Interfaccia della riga di comando di Azure](./with-azure-cli/create-mongodb-cosmosdb.md#configure-your-azure-web-app-with-the-connection-string)|
|Configurare l'app Web per l'uso del contenitore|[Interfaccia della riga di comando di Azure](./with-azure-cli/create-container-registry-resource.md#configure-web-app-to-use-container)|
|Configurare il nome di dominio personalizzato dell'app Web|[Interfaccia della riga di comando di Azure](./with-azure-cli/configure-app-service-custom-domain-name.md#register-a-domain-name-with-your-azure-app)|
|Eliminare la risorsa app|[Estensione di Visual Studio Code](../tutorial/deploy-nodejs-mongodb-app-service-from-visual-studio-code.md#clean-up-resources)<br>[Interfaccia della riga di comando di Azure](../tutorial/tutorial-vscode-azure-cli-node/tutorial-vscode-azure-cli-node-07.md)|
|Distribuire o riorganizzare l'app|[Estensione di Visual Studio Code](deploy-web-app.md#deploy-or-redeploy-to-app-service-with-visual-studio-code)|
|Ottenere l'indirizzo IP esterno dell'app Web|[Interfaccia della riga di comando di Azure](./with-azure-cli/configure-app-service-custom-domain-name.md#register-a-domain-name-with-your-azure-app)|
|Acquistare un nome di dominio e configurare un record DNS|[Interfaccia della riga di comando di Azure](./with-azure-cli/configure-app-service-custom-domain-name.md#purchase-a-domain-name-and-configure-dns-record)|
|Trasmettere i log remoti|[Estensione di Visual Studio Code](../tutorial/deploy-nodejs-azure-app-service-with-visual-studio-code.md?tabs=bash#7-stream-remote-service-logs-in-visual-studio-code)<br>[Interfaccia della riga di comando di Azure](../tutorial/tutorial-vscode-azure-cli-node/tutorial-vscode-azure-cli-node-05.md)|

## <a name="cognitive-services"></a>Servizi cognitivi

|Attività|using|
|--|--|
|Creare una risorsa _ComputerVision_ di Servizi cognitivi|[Interfaccia della riga di comando di Azure](../tutorial/static-web-app/create-computer-vision-resource-use-in-code.md#create-azure-resources)|
|Ottenere una risorsa _ComputerVision_ di Servizi cognitivi|[Interfaccia della riga di comando di Azure](../tutorial/static-web-app/create-computer-vision-resource-use-in-code.md#create-azure-resources)|
|Installare Azure SDK|[Bash](../tutorial/static-web-app/add-computer-vision-react-app.md#add-computer-vision-to-local-react-app)|
|Analizzare immagini con [`@azure/cognitiveservices-computervision`](https://www.npmjs.com/package/@azure/cognitiveservices-computervision)|[Visual Studio Code](../tutorial/static-web-app/add-computer-vision-react-app.md#add-computer-vision-code-as-separate-module)|

## <a name="containers-including-docker-tasks"></a>Contenitori, incluse le attività di Docker

|Attività|using|
|--|--|
|Aggiungere i file Docker al progetto locale|[Estensione di Visual Studio Code](../tutorial/tutorial-vscode-docker-node/tutorial-vscode-docker-node-04.md#add-docker-files)|
|Creare un'immagine Docker dal progetto locale|[Estensione di Visual Studio Code](../tutorial/tutorial-vscode-docker-node/tutorial-vscode-docker-node-04.md#build-a-docker-image)|
|Creare un'immagine del contenitore dal progetto JavaScript locale|[Visual Studio Code](./with-visual-studio-code/containerize-local-project.md#create-a-container)|
|Creare una risorsa registro contenitori|[Estensione di Visual Studio Code](../tutorial/tutorial-vscode-docker-node/tutorial-vscode-docker-node-02.md#create-an-azure-container-registry)<br>[Interfaccia della riga di comando di Azure](./with-azure-cli/create-container-registry-resource.md#create-a-container-registry)|
|Crea il Dockerfile|[Estensione di Visual Studio Code](./with-visual-studio-code/containerize-local-project.md#create-a-dockerfile-in-your-project)|
|Distribuire un'immagine nel servizio app|[Estensione di Visual Studio Code](../tutorial/tutorial-vscode-docker-node/tutorial-vscode-docker-node-05.md#deploy-image)|
|Abilitare l'accesso amministratore al registro|[Estensione di Visual Studio Code](../tutorial/tutorial-vscode-docker-node/tutorial-vscode-docker-node-05.md#enable-admin-access-on-the-registry)|
|Ottenere le credenziali del registro contenitori di Azure|[Interfaccia della riga di comando di Azure](./with-azure-cli/create-container-registry-resource.md#get-container-registry-credentials)|
|Accedere al registro contenitori|[INTERFACCIA della riga di comando BASH-Docker](./with-azure-cli/create-container-registry-resource.md#login-to-container-registry-with-docker-cli)|
|Push dell'immagine nella risorsa del registro di sistema Docker|[Estensione di Visual Studio Code](./with-visual-studio-code/containerize-local-project.md#push-local-container-image-to-dockerhub)|
|Push dell'immagine nella risorsa registro contenitori di Azure|[Estensione di Visual Studio Code](../tutorial/tutorial-vscode-docker-node/tutorial-vscode-docker-node-04.md#push-the-image-to-a-registry)<BR>[INTERFACCIA della riga di comando BASH-Docker](./with-azure-cli/create-container-registry-resource.md#push-your-local-image-to-your-container-registry)|
|Esegui contenitore locale|[Estensione di Visual Studio Code](with-visual-studio-code/containerize-local-project.md#build-image-from-your-project)|
|Contrassegnare l'immagine locale|[INTERFACCIA della riga di comando BASH-Docker](./with-azure-cli/create-container-registry-resource.md#tag-your-local-image)|
|Verificare la versione di Docker|[Bash](../tutorial/tutorial-vscode-docker-node/tutorial-vscode-docker-node-01.md#verify-docker-install)|

## <a name="databases"></a>Database

|Attività|using|
|--|--|
|Creare una risorsa Cosmos DB-MongoDB|[Estensione di Visual Studio Code](../tutorial/deploy-nodejs-mongodb-app-service-from-visual-studio-code.md)<br>[Interfaccia della riga di comando di Azure](./with-azure-cli/create-mongodb-cosmosdb.md#create-a-cosmos-db-resource-for-mongodb)|
|Ottenere la stringa di connessione di CosmosDB|[Estensione di Visual Studio Code](../tutorial/deploy-nodejs-mongodb-app-service-from-visual-studio-code.md#get-cosmosdb-connection-string)<br>[Interfaccia della riga di comando di Azure](./with-azure-cli/create-mongodb-cosmosdb.md#get-the-mongodb-connection-string-for-your-resource)|
|Visualizza Cosmos DB|[Esplora Cosmos DB](https://cosmos.azure.com/)|
|Usare l'API mangusta per mongoDB in Cosmos DB|[JavaScript](./with-database/use-mongodb-as-cosmosdb.md#use-mongoose-sdk-to-connect-to-mongodb-on-azure)

## <a name="git"></a>Git

|Attività|using|
|--|--|
|Inizializzare un repository Git locale|[Estensione di Visual Studio Code](../tutorial/deploy-nodejs-azure-app-service-with-visual-studio-code.md?tabs=bash#5-initialize-git-in-visual-studio-code-for-current-app)|
|Creare un branch locale|[Visual Studio Code con il riquadro comandi](./with-visual-studio-code/clone-github-repository.md#create-a-branch-for-changes-with-git-cl)<br>[Visual Studio Code con la barra di stato](./with-visual-studio-code/clone-github-repository.md#create-a-branch-from-status-bar)|
|Clonare il progetto da GitHub al computer locale|[Visual Studio Code](with-visual-studio-code/install-run-debug-nodejs.md#clone-sample-project-to-local-computer)|
|Eseguire il push di un ramo locale nel repository remoto|[Visual Studio Code con la barra di stato](./with-visual-studio-code/clone-github-repository.md#push-a-local-branch-to-remote-from-status-bar)<br>[Visual Studio Code con estensione del corso di origine](./with-visual-studio-code/clone-github-repository.md#push-a-local-branch-to-remote-from-the-source-control-extension)|

## <a name="github"></a>GitHub 

### <a name="actions"></a>Azioni 

|Attività|using|
|--|--|
|Aggiungere segreti|[Visual Studio Code](../tutorial/static-web-app/create-static-web-app-visual-studio-code-extension.md#create-a-static-web-app-resource)|
|Visualizzare il processo di compilazione|[Sito Web GitHub](../tutorial/static-web-app/create-static-web-app-visual-studio-code-extension.md#view-the-github-action-build-process)|


## <a name="monitoring"></a>Monitoraggio

|Attività|using|
|--|--|
|Crea risorsa|[Interfaccia della riga di comando di Azure](../tutorial/nodejs-virtual-machine-vm/create-azure-monitoring-application-insights-web-resource.md#create-azure-monitor-resource-with-azure-cli)|



## <a name="storage"></a>Archiviazione

|Attività|using|
|--|--|
|Crea risorsa|[Estensione di Visual Studio Code](../tutorial/browser-file-upload-azure-storage-blob.md#3-create-storage-resource-with-visual-studio-extension)|
|Eliminare una risorsa|[Estensione di Visual Studio Code](../tutorial/browser-file-upload-azure-storage-blob.md#clean-up-resources)|
|Creare un'app statica ospitata nell'archiviazione|[Estensione di Visual Studio Code](../tutorial/tutorial-vscode-static-website-node/tutorial-vscode-static-website-node-03.md)|
|Distribuire un'app statica ospitata nell'archiviazione|[Estensione di Visual Studio Code](../tutorial/tutorial-vscode-static-website-node/tutorial-vscode-static-website-node-04.md)|

### <a name="blobs"></a>BLOB

|Attività|using|
|--|--|
|Creare un contenitore nell'archiviazione con [`@azure/storage-blob`](https://www.npmjs.com/package/@azure/storage-blob)|[React/TypeScript](../tutorial/browser-file-upload-azure-storage-blob.md#create-storage-client-and-manage-steps)|
|Caricare un file nell'archiviazione con [`@azure/storage-blob`](https://www.npmjs.com/package/@azure/storage-blob)|[React/TypeScript](../tutorial/browser-file-upload-azure-storage-blob.md#upload-button-functionality)|
|Elencare i file nel contenitore di archiviazione con [`@azure/storage-blob`](https://www.npmjs.com/package/@azure/storage-blob)|[React/TypeScript](../tutorial/browser-file-upload-azure-storage-blob.md#get-list-of-blobs)|

## <a name="terminal-usage"></a>Utilizzo terminale

|Attività|using|
|--|--|
|Terminale integrato|[Visual Studio Code](./with-visual-studio-code/install-run-debug-nodejs.md#use-the-integrated-bash-terminal-to-install-dependencies)|

## <a name="virtual-machines"></a>Macchine virtuali

|Attività|using|
|--|--|
|Connettersi alla macchina virtuale con SSH|[Bash](../tutorial/nodejs-virtual-machine-vm/connect-linux-virtual-machine-ssh.md#connect-with-ssh-and-change-web-app)|
|Installare Monitoring SDK|[Bash](../tutorial/nodejs-virtual-machine-vm/connect-linux-virtual-machine-ssh.md#install-monitoring-sdk)|
|Aggiungere codice di monitoraggio a un'app Express.js|[JavaScript](../tutorial/nodejs-virtual-machine-vm/azure-monitor-application-insights-nodejs-expressjs-code.md#edit-indexjs-for-logging-with-azure-monitor-application-insights)|
|Creare un file cloud-init|[YAML](../tutorial/nodejs-virtual-machine-vm/create-linux-virtual-machine-azure-cli.md#create-a-cloud-init-file-to-expedite-linux-virtual-machine-creation)|
|Creare una risorsa VM Linux|[Interfaccia della riga di comando di Azure](../tutorial/nodejs-virtual-machine-vm/create-linux-virtual-machine-azure-cli.md#create-a-virtual-machine-resource)|
|Aprire la porta della VM Linux|[Interfaccia della riga di comando di Azure](../tutorial/nodejs-virtual-machine-vm/create-linux-virtual-machine-azure-cli.md#open-port-for-virtual-machine)|
|Visualizzare i log|[Interfaccia della riga di comando di Azure](../tutorial/nodejs-virtual-machine-vm/azure-monitor-application-insights-nodejs-expressjs-code.md#viewing-the-vm-logs-for-nginx-and-pm2)<br>[Portale](../tutorial/nodejs-virtual-machine-vm/azure-monitor-application-insights-logs.md#view-application-traces-in-azure-portal)|


## <a name="visual-studio-code-develop-and-debug-javascript-apps"></a>Visual Studio Code: sviluppare ed eseguire il debug di app JavaScript 

|Attività|using|
|--|--|
|Completamento del codice|[Visual Studio Code](./with-visual-studio-code/install-run-debug-nodejs.md#use-visual-studio-code-autocompletion-with-mongodb)|
|Debug dell'app Node.js locale|[Visual Studio Code](./with-visual-studio-code/install-run-debug-nodejs.md#debugging-the-local-nodejs-app)|
|Debug dello stack completo locale|[Visual Studio Code](with-visual-studio-code/install-run-debug-nodejs.md#local-full-stack-debugging-in-visual-studio-code)|
|Esplorare il codice e i file di progetto|[Visual Studio Code](./with-visual-studio-code/install-run-debug-nodejs.md#navigate-the-project-files-and-code)|
|Esecuzione dell'app Node.js locale|[Visual Studio Code](./with-visual-studio-code/install-run-debug-nodejs.md#running-the-local-nodejs-app)|

## <a name="samples-supporting-these-tasks"></a>Esempi che supportano queste attività

|Nome | Description|
|--|--|
|App React che usa Servizi cognitivi|Compilare e distribuire localmente un'applicazione client React/TypeScript in un'app Web statica di Azure con un'azione GitHub.<br>[Esercitazione](../tutorial/static-web-app/introduction.md) - [Codice di esempio](https://github.com/Azure-Samples/js-e2e-client-cognitive-services)|
|App React che carica un file in BLOB del servizio di archiviazione di Azure|Questo progetto di esempio è un'app client del framework React (create-react-app) TypeScript, con un modulo HTML per selezionare un file da caricare nei BLOB del servizio di archiviazione di Azure.<br>[Esercitazione](../tutorial/browser-file-upload-azure-storage-blob.md) - [Codice di esempio](https://github.com/Azure-Samples/js-e2e-browser-file-upload-storage-blob)|
|App React con pulsante di accesso|L'applicazione a pagina singola creata in questa esercitazione è un'app React (create-react-app) con le attività seguenti:<br>* Eseguire l'accesso usando un account di accesso supportato da Microsoft, ad esempio Office 365 oppure Outlook.com<br>* Disconnettersi dall'applicazione<br>[Esercitazione](../tutorial/single-page-application-azure-login-button-sdk-msal.md) - [Codice di esempio](https://github.com/Azure-Samples/js-e2e-client-azure-login-button)|
|App Express.js con database MongoDB|L'esercitazione illustra come caricare ed eseguire il progetto in locale con VSCode, usando le estensioni, e come eseguire il codice in remoto in un servizio app. L'esercitazione include la creazione di una risorsa CosmosDB per l'API Mongo, con il recupero delle informazioni di connessione che verranno incluse nell'impostazione di configurazione del servizio app per connettersi a un database cloud.<br>[Esercitazione](../tutorial/deploy-nodejs-mongodb-app-service-from-visual-studio-code.md) - [Codice di esempio](https://github.com/Azure-Samples/js-e2e-express-mongo)|
|App Express.js distribuita in una VM con il file cloud-init|Illustra come creare una macchina virtuale Linux per un'app Express.js. La macchina virtuale è configurata con un file di configurazione cloud-init e include NGINX e un repository GitHub per un'app Express.js. Quando la macchina virtuale è in esecuzione, è possibile connettersi alla macchina virtuale con SSH, modificare l'app Web in modo da includere la registrazione della traccia e visualizzare l'app server Express.js pubblica in un Web browser.<br>[Esercitazione](../tutorial/nodejs-virtual-machine-vm/introduction.md) - [Codice di esempio](https://github.com/Azure-Samples/js-e2e-express-mongo)|

Vedere [Esplora gli esempi di codice](/samples/browse/?languages=javascript%2cnodejs%2ctypescript) per trovare altri esempi che supportano il caso d'uso specifico. 

## <a name="next-steps"></a>Passaggi successivi

* [Distribuire un'app Web](deploy-web-app.md)