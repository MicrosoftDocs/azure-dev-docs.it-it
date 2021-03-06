---
title: Architettura dell'app Visione artificiale
description: Questa sezione dell'esercitazione descrive l'app client e il processo di distribuzione.
ms.topic: tutorial
ms.date: 12/17/2020
ms.custom: devx-track-js
ms.openlocfilehash: b8949b355ec30775dfacf5ea77db53edb595578c
ms.sourcegitcommit: 593d177cfb5f56f236ea59389e43a984da30f104
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/18/2021
ms.locfileid: "98561647"
---
# <a name="2-application-architecture-for-static-web-app-with-computer-vision"></a>2. Architettura dell'app Web statica con Visione artificiale

Informazioni sull'applicazione client e sul processo di distribuzione.

## <a name="what-is-an-azure-static-web-app"></a>Che cos'è un'app Web statica di Azure

Quando si compilano app Web statiche, sono disponibili diverse opzioni in Azure, in base al grado di funzionalità e al controllo a cui si è interessati. Questa esercitazione è incentrata sul servizio più semplice con molte delle scelte effettuate per l'utente, per potersi concentrare sul codice front-end e non sull'ambiente host.

## <a name="client-application-architecture"></a>Architettura dell'applicazione client

Il client React (create-react-app) fornisce le funzionalità seguenti: 
* Visualizza un messaggio se la chiave e l'endpoint di Azure per [**Visione artificiale**](/azure/cognitive-services/computer-vision/) di Servizi cognitivi non sono stati trovati
* Consente di analizzare un'immagine con Visione artificiale di Servizi cognitivi
    * Immettere un URL di immagine pubblico o analizzare l'immagine dalla raccolta
    * Al termine dell'analisi
        * Immagine visualizzata
        * Visualizzare i risultati JSON di Visione artificiale 

:::image type="content" source="../../media/static-web-app/browser-screenshot-react-computervision-app-image-analysis-result.png" alt-text="Screenshot parziale del browser dei risultati dell'esempio di Visione artificiale di Servizi cognitivi dell'app React.":::

## <a name="deploy-to-azure-with-github-action"></a>Eseguire la distribuzione in Azure con GitHub Actions

L'azione GitHub inizia quando viene eseguito un push in un ramo specifico:
* Inserisce i segreti GitHub per la chiave e l'endpoint di Visione artificiale nella build
* Compila il client React (create-react-app)
* Sposta i file risultanti nella risorsa [**app Web statica**](/azure/static-web-apps) di Azure

> [!div class="nextstepaction"]
> [Scaricare ed eseguire l'app React Image Analyzer di Servizi cognitivi in locale](run-the-react-cognitive-services-image-analyzer-app-locally.md)