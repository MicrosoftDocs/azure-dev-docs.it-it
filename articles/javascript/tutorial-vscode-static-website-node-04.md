---
title: Distribuire i file dell'app Node.js statica in Archiviazione di Azure da Visual Studio Code
description: Parte 4 dell'esercitazione sull'app Web statica, distribuire i file in Archiviazione di Azure
ms.topic: tutorial
ms.date: 09/24/2019
ms.author: buhollan
ms.custom: devx-track-js
ms.openlocfilehash: e7c1ca3390f8161c7323103868c591e8c02f539e
ms.sourcegitcommit: 4dd392ea864be52421d0239e59198bc44b0a5a16
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/25/2020
ms.locfileid: "91365294"
---
# <a name="deploy-the-website-to-azure-storage"></a>Distribuire il sito Web in Archiviazione di Azure

[Passaggio precedente: Creare un account di archiviazione](tutorial-vscode-static-website-node-03.md)

In questo passaggio si usa Visual Studio Code per distribuire i file del sito Web statico creati nei passaggi precedenti in Archiviazione di Azure, dove verranno ospitati e gestiti.

# <a name="angular"></a>[Angular](#tab/angular)

1. In Visual Studio Code passare all'area **Archiviazione di Azure**, espandere la propria sottoscrizione, espandere il nodo relativo all'account di Archiviazione di Azure creato nel passaggio precedente, quindi espandere il nodo **Contenitori BLOB**. Il codice dell'app verrà distribuito nel contenitore `$web`.

   ![I nodi di Archiviazione di Azure nell'area Archiviazione di Azure](media/static-website/storage-nodes.png)

1. Selezionare l'area **File**, fare clic con il pulsante destro del mouse sulla cartella _dist/my-static-app_ e scegliere **Distribuisci in Sito Web statico**:

    ![Angular - Comando Deploy to Static Website](media/static-website/deploy-build-angular.png)

1. Quando richiesto, scegliere l'account di archiviazione creato in precedenza.

1. Al termine della distribuzione, viene visualizzato un messaggio con il pulsante **Browse to website** (Passa a sito Web). Selezionare il pulsante per aprire l'endpoint primario del codice dell'app distribuito.

    ![Angular - Messaggio di distribuzione completata](media/static-website/deployment-complete.png)

    ![Angular - Sito Web statico in esecuzione in Azure](media/static-website/azure-app-angular.png)

# <a name="react"></a>[React](#tab/react)

1. In Visual Studio Code passare all'area **Archiviazione di Azure**, espandere la propria sottoscrizione, espandere il nodo relativo all'account di Archiviazione di Azure creato nel passaggio precedente, quindi espandere il nodo **Contenitori BLOB**. Il codice dell'app verrà distribuito nel contenitore `$web`.

   ![React - I nodi di Archiviazione di Azure nell'area Archiviazione di Azure](media/static-website/storage-nodes.png)

1. Selezionare l'area **File**, fare clic con il pulsante destro del mouse sulla cartella _build_ e scegliere **Deploy to Static Website** (Distribuisci in sito Web statico):

    ![React - Comando Deploy to Static Website](media/static-website/deploy-build-react.png)

1. Quando richiesto, scegliere l'account di archiviazione creato in precedenza.

1. Al termine della distribuzione, viene visualizzato un messaggio con il pulsante **Browse to website** (Passa a sito Web). Selezionare il pulsante per aprire l'endpoint primario del codice dell'app distribuito.

    ![React - Messaggio di distribuzione completata](media/static-website/deployment-complete.png)

    ![React - Sito Web statico in esecuzione in Azure](media/static-website/azure-app-react.png)

# <a name="vue"></a>[Vue](#tab/vue)

1. In Visual Studio Code passare all'area **Archiviazione di Azure**, espandere la propria sottoscrizione, espandere il nodo relativo all'account di Archiviazione di Azure creato nel passaggio precedente, quindi espandere il nodo **Contenitori BLOB**. Il codice dell'app verrà distribuito nel contenitore `$web`.

   ![Vue - I nodi di Archiviazione di Azure nell'area Archiviazione di Azure](media/static-website/storage-nodes.png)

1. Selezionare l'area **File**, fare clic con il pulsante destro del mouse sulla cartella _dist_ e scegliere **Distribuisci in Sito Web statico**:

    ![Vue - Comando Deploy to Static Website](media/static-website/deploy-build-vue.png)

1. Quando richiesto, scegliere l'account di archiviazione creato in precedenza.

1. Al termine della distribuzione, viene visualizzato un messaggio con il pulsante **Browse to website** (Passa a sito Web). Selezionare il pulsante per aprire l'endpoint primario del codice dell'app distribuito.

    ![Vue - Messaggio di distribuzione completata](media/static-website/deployment-complete.png)

    ![Vue - Sito Web statico in esecuzione in Azure](media/static-website/azure-app-vue.png)

# <a name="svelte"></a>[Svelte](#tab/svelte)

1. In Visual Studio Code passare all'area **Archiviazione di Azure**, espandere la propria sottoscrizione, espandere il nodo relativo all'account di Archiviazione di Azure creato nel passaggio precedente, quindi espandere il nodo **Contenitori BLOB**. Il codice dell'app verrà distribuito nel contenitore `$web`.

   ![Svelte - I nodi di Archiviazione di Azure nell'area Archiviazione di Azure](media/static-website/storage-nodes.png)

1. Selezionare l'area **File**, fare clic con il pulsante destro del mouse sulla cartella _public_ e scegliere **Distribuisci in Sito Web statico**:

    ![Svelte - Comando Deploy to Static Website](media/static-website/deploy-build-svelte.png)

1. Quando richiesto, scegliere l'account di archiviazione creato in precedenza.

1. Al termine della distribuzione, viene visualizzato un messaggio con il pulsante **Browse to website** (Passa a sito Web). Selezionare il pulsante per aprire l'endpoint primario del codice dell'app distribuito.

    ![Svelte - Messaggio di distribuzione completata](media/static-website/deployment-complete-svelte.png)

    ![Svelte - Sito Web statico in esecuzione in Azure](media/static-website/azure-app-svelte.png)

---

> [!div class="nextstepaction"]
> [Il sito si trova in Azure](tutorial-vscode-static-website-node-05.md) [Si è verificato un problema](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-staticwebsite&step=create-storage)
