---
title: Principali attività di Azure per sviluppatori JavaScript
description: Un esempio delle attività correnti.
ms.topic: reference
ms.date: 01/05/2021
ms.custom: devx-track-js
ms.openlocfilehash: 2f19a660a60e91601dc31b829bb8fbc82f04b37d
ms.sourcegitcommit: 075f39972e390e79ed09a3fcfdbfc776727e08fc
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/06/2021
ms.locfileid: "97952483"
---
# <a name="common-top-tasks-for-javascript-developers"></a>Principali attività comuni per sviluppatori JavaScript

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
|Creare un piano per la risorsa app|[Interfaccia della riga di comando di Azure](../tutorial/tutorial-vscode-azure-cli-node/tutorial-vscode-azure-cli-node-03.md#create-app-service-plan)|
|Creare una risorsa app|[Interfaccia della riga di comando di Azure](../tutorial/tutorial-vscode-azure-cli-node/tutorial-vscode-azure-cli-node-03.md)|
|Configurazione distribuzione|[Interfaccia della riga di comando di Azure](../tutorial/deploy-deno-app-azure-app-service-azure-cli.md#5-configure-the-azure-app-service-webapp)
|Distribuire l'app|[Interfaccia della riga di comando di Azure](../tutorial/tutorial-vscode-azure-cli-node/tutorial-vscode-azure-cli-node-04.md)|
|Visualizzare l'app nel browser|[Interfaccia della riga di comando di Azure](../tutorial/tutorial-vscode-azure-cli-node/tutorial-vscode-azure-cli-node-03.md#browse-web-app)|
|Eliminare la risorsa app|[Estensione di Visual Studio Code](../tutorial/deploy-nodejs-mongodb-app-service-from-visual-studio-code.md#clean-up-resources)<br>[Interfaccia della riga di comando di Azure](../tutorial/tutorial-vscode-azure-cli-node/tutorial-vscode-azure-cli-node-07.md)|
|Trasmettere i log remoti|[Estensione di Visual Studio Code](../tutorial/deploy-nodejs-azure-app-service-with-visual-studio-code.md?tabs=bash#7-stream-remote-service-logs-in-visual-studio-code)<br>[Interfaccia della riga di comando di Azure](../tutorial/tutorial-vscode-azure-cli-node/tutorial-vscode-azure-cli-node-05.md)|

## <a name="cognitive-services"></a>Servizi cognitivi

|Attività|using|
|--|--|
|Creare una risorsa _ComputerVision_ di Servizi cognitivi|[Interfaccia della riga di comando di Azure](../tutorial/static-web-app/create-computer-vision-resource-use-in-code.md#create-azure-resources)|
|Ottenere una risorsa _ComputerVision_ di Servizi cognitivi|[Interfaccia della riga di comando di Azure](../tutorial/static-web-app/create-computer-vision-resource-use-in-code.md#create-azure-resources)|
|Installare Azure SDK|[Bash](../tutorial/static-web-app/add-computer-vision-react-app.md#add-computer-vision-to-local-react-app)|
|Analizzare immagini con [`@azure/cognitiveservices-computervision`](https://www.npmjs.com/package/@azure/cognitiveservices-computervision)|[Visual Studio Code](../tutorial/static-web-app/add-computer-vision-react-app.md#add-computer-vision-code-as-separate-module)|

## <a name="containers"></a>Contenitori

Se non si trova l'opzione desiderata, vedere le [attività di Docker](#docker). 

|Attività|using|
|--|--|
|Creare una risorsa registro contenitori|[Estensione di Visual Studio Code](../tutorial/tutorial-vscode-docker-node/tutorial-vscode-docker-node-02.md#create-an-azure-container-registry)|
|Eseguire il push di un'immagine nella risorsa registro|[Estensione di Visual Studio Code](../tutorial/tutorial-vscode-docker-node/tutorial-vscode-docker-node-04.md#push-the-image-to-a-registry)|
|Abilitare l'accesso amministratore al registro|[Estensione di Visual Studio Code](../tutorial/tutorial-vscode-docker-node/tutorial-vscode-docker-node-05.md#enable-admin-access-on-the-registry)|
|Distribuire un'immagine nel servizio app|[Estensione di Visual Studio Code](../tutorial/tutorial-vscode-docker-node/tutorial-vscode-docker-node-05.md?branch=main#deploy-image)|


## <a name="databases"></a>Database

|Attività|using|
|--|--|
|Creare una risorsa CosmosDB - MongoDB|[Estensione di Visual Studio Code](../tutorial/deploy-nodejs-mongodb-app-service-from-visual-studio-code.md)|
|Ottenere la stringa di connessione di CosmosDB|[Estensione di Visual Studio Code](../tutorial/deploy-nodejs-mongodb-app-service-from-visual-studio-code.md#get-cosmosdb-connection-string)|

## <a name="docker"></a>Docker

Se non si trova l'opzione desiderata, vedere le [attività di Contenitori](#containers). 

|Attività|using|
|--|--|
|Verificare la versione di Docker|[Bash](../tutorial/tutorial-vscode-docker-node/tutorial-vscode-docker-node-01.md#verify-docker-install)|
|Aggiungere i file Docker al progetto locale|[Estensione di Visual Studio Code](../tutorial/tutorial-vscode-docker-node/tutorial-vscode-docker-node-04.md#add-docker-files)|
|Creare un'immagine Docker dal progetto locale|[Bash](../tutorial/tutorial-vscode-docker-node/tutorial-vscode-docker-node-04.md#build-a-docker-image)|

## <a name="git"></a>Git

|Attività|using|
|--|--|
|Inizializzare un repository Git locale|[Estensione di Visual Studio Code](../tutorial/deploy-nodejs-azure-app-service-with-visual-studio-code.md?tabs=bash#5-initialize-git-in-visual-studio-code-for-current-app)|

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


## <a name="visual-studio-code"></a>Visual Studio Code

|Attività|using|
|--|--|
|Clonare il repository GitHub|[Visual Studio Code](../tutorial/browser-file-upload-azure-storage-blob.md#2-clone-and-run-the-initial-react-app)|

## <a name="samples-supporting-these-tasks"></a>Esempi che supportano queste attività

|Nome | Description|
|--|--|
|Compilare e distribuire un'app Web statica in Azure|Compilare e distribuire localmente un'applicazione client React/TypeScript in un'app Web statica di Azure con un'azione GitHub.<br>[Esercitazione](../tutorial/static-web-app/introduction.md) - [Codice di esempio](https://github.com/Azure-Samples/js-e2e-client-cognitive-services)|
|Caricare file nei BLOB del servizio di archiviazione di Azure|Questo progetto di esempio è un'app client del framework React (create-react-app) TypeScript, con un modulo HTML per selezionare un file da caricare nei BLOB del servizio di archiviazione di Azure.<br>[Esercitazione](../tutorial/browser-file-upload-azure-storage-blob.md) - [Codice di esempio](https://github.com/Azure-Samples/js-e2e-browser-file-upload-storage-blob)|
|Aggiungere un pulsante di accesso all'applicazione client|L'applicazione a pagina singola creata in questa esercitazione è un'app React (create-react-app) con le attività seguenti:<br>* Eseguire l'accesso usando un account di accesso supportato da Microsoft, ad esempio Office 365 oppure Outlook.com<br>* Disconnettersi dall'applicazione<br>[Esercitazione](../tutorial/single-page-application-azure-login-button-sdk-msal.md) - [Codice di esempio](https://github.com/Azure-Samples/js-e2e-client-azure-login-button)|
|App CosmosDB (MongoDB)/Express.js distribuita nel servizio app|L'esercitazione illustra come caricare ed eseguire il progetto in locale con VSCode, usando le estensioni, e come eseguire il codice in remoto in un servizio app. L'esercitazione include la creazione di una risorsa CosmosDB per l'API Mongo, con il recupero delle informazioni di connessione che verranno incluse nell'impostazione di configurazione del servizio app per connettersi a un database cloud.<br>[Esercitazione](../tutorial/deploy-nodejs-mongodb-app-service-from-visual-studio-code.md#get-cosmosdb-connection-string) - [Codice di esempio](https://github.com/Azure-Samples/js-e2e-express-mongo)|

## <a name="next-steps"></a>Passaggi successivi

* [Distribuire un'app Web](deploy-web-app.md)