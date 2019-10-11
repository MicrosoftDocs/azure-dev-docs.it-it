---
title: Creare un account di Archiviazione di Azure per un sito Web Node.js statico da Visual Studio Code
description: Parte 3 dell'esercitazione, creare un account di Archiviazione di Azure
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/24/2019
ms.author: kraigb
ms.openlocfilehash: 44e6479379fff3ddf1012cdb61cf73440cad346e
ms.sourcegitcommit: c04984b6367e922dbc5973af44f8cd0ca81ce157
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/30/2019
ms.locfileid: "71685971"
---
# <a name="create-an-azure-storage-account"></a>Creare un account di Archiviazione di Azure

[Passaggio precedente: Creare l'app](tutorial-vscode-static-website-node-02.md)

In questo passaggio viene creato un account di Archiviazione di Azure, che funge da semplice archivio file (o rete per la distribuzione di contenuti) con un server Web incorporato. Grazie a questo server incorporato, Archiviazione di Azure rappresenta la scelta ideale per ospitare rapidamente siti statici.

1. Nella cartella `my-react-app` creata nel passaggio precedente avviare Visual Studio Code in modo che apra tale cartella automaticamente:

    ```bash
    code .
    ```

1. In VS Code selezionare il logo di Azure per aprire l'area **Azure**. In **Archiviazione di Azure** fare clic con il pulsante destro del mouse sulla propria sottoscrizione di Azure e scegliere **Crea account di archiviazione**:

    ![Creare l'account di archiviazione in VS Code](media/static-website/create-storage-account.png)

1. Quando viene chiesto di immettere il nome del nuovo account di archiviazione, immettere un nome univoco globale e premere INVIO. I caratteri validi per il nome dell'app sono 'a-z' e '0-9'.

1. Quando viene chiesto di selezionare un gruppo di risorse, scegliere **Crea un nuovo gruppo di risorse** e accettare il nome predefinito.

1. Quando viene chiesto di selezionare una località, scegliere un'[area](https://azure.microsoft.com/regions/) vicina.

1. Durante la creazione dell'account di archiviazione, nel pannello **Output** di VS Code viene visualizzato lo stato di avanzamento:

1. Una volta completato l'account di archiviazione, fare clic su di esso con il pulsante destro del mouse e scegliere **Configure Static Website** (Configura sito Web statico). Abilitando l'hosting del sito Web statico, Archiviazione di Azure fornisce automaticamente il documento di indice ed eventuali altri asset statici.

    ![Crea account di archiviazione](media/static-website/configure-static-website.png)

1. Quando richiesto, immettere *index.html* sia per il nome del documento sia per il nome del documento di errore 404. Il file *index.html* viene usato per il documento di errore perché le moderne app a pagina singola, come React, Angular e Vue, gestiscono gli errori nel client. Per i classici siti Web statici, usare una pagina di errore 404 personalizzata.

> [!div class="nextstepaction"]
> [Il contenitore di archiviazione è stato creato](tutorial-vscode-static-website-node-04.md) [Si è verificato un errore](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-staticwebsite&step=create-storage)
