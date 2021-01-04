---
title: Distribuire un sito Web Node.js statico in Azure da Visual Studio Code
description: Parte 1 dell'esercitazione sull'app Web statica, introduzione e prerequisiti.
ms.topic: tutorial
ms.date: 09/23/2019
ms.custom: devx-track-js
ms.openlocfilehash: a4280e3c28ee99e38eb4430c991450667277b29f
ms.sourcegitcommit: f723980ade4cbc13548a5d8ac3f3fa681b8a2dbd
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/16/2020
ms.locfileid: "97609315"
---
# <a name="deploy-a-static-website-to-azure-from-visual-studio-code"></a>Distribuire un sito Web statico in Azure da Visual Studio Code

In questa esercitazione viene creato un sito Web statico e distribuito in Azure usando [Archiviazione di Azure](/azure/storage). Un sito Web statico è composto da file HTML, CSS, JavaScript e altri file statici come immagini o tipi di carattere. Un sito statico è solitamente un'[applicazione a pagina singola](https://en.wikipedia.org/wiki/Single-page_application) scritta in Angular, React o Vue. Tuttavia, l'app viene progettata e i file vengono ospitati e gestiti direttamente dall'_archiviazione_ invece che tramite un server Web. L'hosting nell'archiviazione è più semplice e meno costoso rispetto alla gestione di un server Web.

## <a name="walkthrough-video"></a>Video della procedura dettagliata

Guardare questo video per una procedura dettagliata completa del contenuto di questo articolo.

> [!VIDEO https://channel9.msdn.com/Shows/Docs-Azure/Deploy-static-website-to-Azure-from-Visual-Studio-Code/player]

## <a name="prerequisites"></a>Prerequisiti

- Una [sottoscrizione di Azure](#azure-subscription).
- [Visual Studio Code](https://code.visualstudio.com/).
- [Estensione Archiviazione di Azure](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurestorage).
- [Node.js e npm](https://nodejs.org/en/download), la gestione pacchetti Node.js. Questo requisito serve solo per generare un progetto di esempio. Non è necessario installare Node.js se si ha già il codice di un'app.

### <a name="azure-subscription"></a>Sottoscrizione di Azure

Se non si ha una sottoscrizione di Azure, [iscriversi](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-static-website&mktingSource=vscode-tutorial-static-website) subito per ottenere un account gratuito con 200 USD in crediti di Azure per provare qualsiasi combinazione di servizi.

## <a name="sign-in-to-azure"></a>Accedere ad Azure

[!INCLUDE [azure-sign-in](../../includes/azure-sign-in.md)]

> [!div class="nextstepaction"] 
> [I prerequisiti sono stati installati](tutorial-vscode-static-website-node-02.md) [Si è verificato un problema](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-staticwebsite&step=getting-started)
