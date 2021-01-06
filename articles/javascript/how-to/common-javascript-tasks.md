---
title: Principali attività di Azure per sviluppatori JavaScript
description: Un esempio delle attività correnti.
ms.topic: reference
ms.date: 12/08/2020
ms.custom: devx-track-js
ms.openlocfilehash: 396544121a964cf7fd8cbde01012e7466a76eb37
ms.sourcegitcommit: 1c508f5ba73a12e4baeacc88ad9a8359301acb50
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/18/2020
ms.locfileid: "97690798"
---
# <a name="common-top-tasks-for-javascript-developers"></a>Principali attività comuni per sviluppatori JavaScript

Un esempio delle attività correnti.

## <a name="application-settings"></a>Impostazioni applicazione

|Attività|using|
|--|--|
|Impostare variabili di ambiente|[Bash](../tutorial/static-web-app/create-computer-vision-resource-use-in-code.md#add-environment-variables-to-your-local-environment)|
|Creare un token di firma di accesso condiviso del contenitore di archiviazione|[Portale](../tutorial/browser-file-upload-azure-storage-blob.md#5-generate-your-shared-access-signature-sas-token)|
|Impostare il token di firma di accesso condiviso nel codice|[TypeScript](../tutorial/browser-file-upload-azure-storage-blob.md#set-sas-token-in-code-file)|
|Configurare CORS per l'archiviazione|[Portale](../tutorial/browser-file-upload-azure-storage-blob.md#6-configure-cors-for-azure-storage-resource)|

## <a name="azure-resource-groups"></a>Gruppi di risorse di Azure

|Attività|using|
|--|--|
|Creare un gruppo di risorse|[Interfaccia della riga di comando di Azure](../tutorial/static-web-app/create-computer-vision-resource-use-in-code.md#create-azure-resources)|
|Eliminare un gruppo di risorse|[Interfaccia della riga di comando di Azure](../tutorial/static-web-app/clean-up-resources.md#remove-all-the-resources-by-removing-resource-group)|

## <a name="static-web-apps"></a>App Web statiche

|Attività|using|
|--|--|
|Creare un'app Web statica|[Estensione di Visual Studio Code](../tutorial/static-web-app/create-static-web-app-visual-studio-code-extension.md#create-a-static-web-app-resource)|
|Esplorazione del sito|[Estensione di Visual Studio Code](../tutorial/static-web-app/create-static-web-app-visual-studio-code-extension.md#view-azure-static-web-site-in-browser)|

## <a name="storage"></a>Archiviazione

|Attività|using|
|--|--|
|Crea risorsa|[Estensione di Visual Studio Code](../tutorial/browser-file-upload-azure-storage-blob.md#3-create-storage-resource-with-visual-studio-extension)|
|Eliminare una risorsa|[Estensione di Visual Studio Code](../tutorial/browser-file-upload-azure-storage-blob.md#clean-up-resources)|

### <a name="blobs"></a>BLOB

|Attività|using|
|--|--|
|Creare un contenitore nell'archiviazione con [`@azure/storage-blob`](https://www.npmjs.com/package/@azure/storage-blob)|[React/TypeScript](../tutorial/browser-file-upload-azure-storage-blob.md#create-storage-client-and-manage-steps)|
|Caricare un file nell'archiviazione con [`@azure/storage-blob`](https://www.npmjs.com/package/@azure/storage-blob)|[React/TypeScript](../tutorial/browser-file-upload-azure-storage-blob.md#upload-button-functionality)|
|Elencare i file nel contenitore di archiviazione con [`@azure/storage-blob`](https://www.npmjs.com/package/@azure/storage-blob)|[React/TypeScript](../tutorial/browser-file-upload-azure-storage-blob.md#get-list-of-blobs)|

## <a name="cognitive-services"></a>Servizi cognitivi

|Attività|using|
|--|--|
|Creare una risorsa _ComputerVision_ di Servizi cognitivi|[Interfaccia della riga di comando di Azure](../tutorial/static-web-app/create-computer-vision-resource-use-in-code.md#create-azure-resources)|
|Ottenere una risorsa _ComputerVision_ di Servizi cognitivi|[Interfaccia della riga di comando di Azure](../tutorial/static-web-app/create-computer-vision-resource-use-in-code.md#create-azure-resources)|
|Installare Azure SDK|[Bash](../tutorial/static-web-app/add-computer-vision-react-app.md#add-computer-vision-to-local-react-app)|
|Analizzare immagini con [`@azure/cognitiveservices-computervision`](https://www.npmjs.com/package/@azure/cognitiveservices-computervision)|[Visual Studio Code](../tutorial/static-web-app/add-computer-vision-react-app.md#add-computer-vision-code-as-separate-module)|

## <a name="github-actions"></a>Azioni di GitHub 

|Attività|using|
|--|--|
|Aggiungere segreti|[Visual Studio Code](../tutorial/static-web-app/create-static-web-app-visual-studio-code-extension.md#create-a-static-web-app-resource)|
|Visualizzare il processo di compilazione|[Sito Web GitHub](../tutorial/static-web-app/create-static-web-app-visual-studio-code-extension.md#view-the-github-action-build-process)|

## <a name="visual-studio-code"></a>Visual Studio Code

|Attività|using|
|--|--|
|Clonare il repository GitHub|[Visual Studio Code](../tutorial/browser-file-upload-azure-storage-blob.md#2-clone-and-run-the-initial-react-app)|

## <a name="next-steps"></a>Passaggi successivi

* [Distribuire un'app Web](deploy-web-app.md)