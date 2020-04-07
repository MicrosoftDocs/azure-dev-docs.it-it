---
title: Distribuire un'immagine del contenitore per un'app Node.js da Visual Studio Code
description: Parte 4 dell'esercitazione, distribuire l'immagine nel Servizio app di Azure
ms.topic: conceptual
ms.date: 09/20/2019
ms.openlocfilehash: 8fe8024adca9edda2142dc6582b6456b77ea4b8f
ms.sourcegitcommit: a65fa8dbb168bd39e225a293d9ee73d18ece1864
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/27/2020
ms.locfileid: "80362783"
---
# <a name="deploy-the-image-to-azure-app-service"></a>Distribuire l'immagine nel Servizio app di Azure

[Passaggio precedente: Creare l'immagine dell'app](tutorial-vscode-docker-node-03.md)

In questo passaggio viene distribuita l'immagine di cui è stato eseguito il push in un registro del [Servizio app di Azure](https://azure.microsoft.com/services/app-service/) direttamente da Visual Studio Code.

1. Nell'area **DOCKER** espandere i nodi per l'immagine in **Registries** (Registri), fare clic con il pulsante destro del mouse su `:latest` e quindi scegliere **Deploy Image to Azure App Service** (Distribuisci l'immagine nel Servizio app di Azure).

    ![Eseguire la distribuzione dallo strumento di esplorazione](media/deploy-containers/deploy-image-command.png)

1. Quando richiesto, specificare i valori per il Servizio app:

    - Il nome deve essere univoco in Azure.
    - Selezionare un gruppo di risorse esistente oppure crearne uno nuovo. Un **gruppo di risorse** è essenzialmente una raccolta denominata delle risorse di un'applicazione in Azure.
    - Selezionare un piano di Servizio app esistente oppure crearne uno nuovo. Un **piano di Servizio app** definisce le risorse fisiche che ospitano il sito Web. Per questa esercitazione, è possibile usare il piano di livello gratuito o Basic.
    - Selezionare un piano tariffario per il nuovo piano di servizio app.
    - Selezionare una località (vicina) per le nuove risorse.

1. Al termine della distribuzione, Visual Studio Code visualizza una notifica con l'URL del sito Web:

    ![Messaggio di distribuzione completata](media/deploy-containers/deploy-successful.png)

1. È anche possibile visualizzare i risultati nel pannello **Output** di Visual Studio Code, nella sezione **Docker**:

    ![Output di distribuzione completata](media/deploy-containers/deploy-output.png)

1. Per esplorare il sito Web distribuito, è possibile premere **CTRL**+**clic** sull'URL nel pannello **Output**. Il nuovo Servizio app viene visualizzato anche nell'area **AZURE** di Visual Studio Code, nella sezione **SERVIZIO APP**, dove è possibile fare clic con il pulsante destro del mouse sul sito Web e scegliere **Esplora sito Web**.

> [!div class="nextstepaction"]
> [Il sito si trova in Azure](tutorial-vscode-docker-node-05.md) [Si è verificato un problema](https://www.research.net/r/PWZWZ52?tutorial=docker-extension&step=deploy-app)
