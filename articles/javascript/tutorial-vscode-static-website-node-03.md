---
title: Creare un account di Archiviazione di Azure per un sito Web Node.js statico da Visual Studio Code
description: Parte 3 dell'esercitazione, creare un account di Archiviazione di Azure
ms.topic: conceptual
ms.date: 09/24/2019
ms.openlocfilehash: 4adca67f850497777abce7d550e39532e59257d9
ms.sourcegitcommit: 553da4e9aa988e5bb823364244ea81961cee5bc7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/01/2020
ms.locfileid: "85792721"
---
# <a name="create-an-azure-storage-account"></a>Creare un account di Archiviazione di Azure

[Passaggio precedente: Creare l'app](tutorial-vscode-static-website-node-02.md)

In questo passaggio viene creato un account di Archiviazione di Azure, che funge da semplice archivio file (o rete per la distribuzione di contenuti) con un server Web incorporato. Grazie a questo server incorporato, Archiviazione di Azure rappresenta la scelta ideale per ospitare rapidamente siti statici.

1. Nella cartella `my-static-app` creata nel passaggio precedente avviare Visual Studio Code in modo che apra tale cartella automaticamente:

    ```bash
    code .
    ```

1. In VS Code selezionare il logo di Azure per aprire l'area **Azure**. In **Archiviazione di Azure** fare clic con il pulsante destro del mouse sulla propria sottoscrizione di Azure e scegliere **Crea account di archiviazione**:

    ![Creare l'account di archiviazione in VS Code](media/static-website/create-storage-account.png)

1. Quando viene chiesto di immettere il nome del nuovo account di archiviazione, immettere un nome univoco globale e premere INVIO. I caratteri validi per il nome dell'app sono 'a-z' e '0-9'.

    > [!NOTE]
    > Vengono creati un account di archiviazione e un gruppo di risorse con lo stesso nome. L'account di archiviazione viene inserito automaticamente nell'area Stati Uniti occidentali. Se si vogliono specificare il gruppo di risorse e la località, scegliere l'opzione "Crea account di archiviazione (avanzato)" dal menu di scelta rapida.

1. Durante la creazione dell'account di archiviazione, nel pannello **Output** di VS Code viene visualizzato lo stato di avanzamento:

    ![Finestra di output di VS Code ](media/static-website/output-storage.png)

1. Una volta completato l'account di archiviazione, viene visualizzato un messaggio indicante che l'hosting del sito Web statico è stato abilitato.

    ![Creare un account di archiviazione](media/static-website/static-website-enabled-notification.png)

    > [!IMPORTANT]
    > Il file *index.html* viene usato per il documento di errore perché le moderne app a pagina singola, come React, Angular e Vue, gestiscono gli errori di route nel client. Per i classici siti Web statici, usare una pagina di errore 404 personalizzata.

> [!div class="nextstepaction"]
> [Il contenitore di archiviazione è stato creato](tutorial-vscode-static-website-node-04.md) [Si è verificato un errore](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-staticwebsite&step=create-storage)
