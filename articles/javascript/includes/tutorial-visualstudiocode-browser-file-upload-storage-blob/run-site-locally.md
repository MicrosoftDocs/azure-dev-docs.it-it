---
ms.topic: include
ms.date: 10/13/2020
ms.custom: devx-track-javascript
title: file di inclusione run-site-locally.md
description: file di inclusione run-site-locally.md
ms.openlocfilehash: 129546c7f6a845c335405c6fc615f907848138f6
ms.sourcegitcommit: ced8331ba36b28e6e2eacd23a64b39ddc7ffe6ab
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/21/2020
ms.locfileid: "92344146"
---
In questa sezione dell'esercitazione scaricare l'applicazione di esempio nel computer locale ed eseguirla dal terminale di Visual Studio Code. Visualizzare quindi l'app in esecuzione in locale nel browser.

## <a name="clone-and-run-the-initial-react-app"></a>Clonare ed eseguire l'app React iniziale

Viene fornita l'app React completa. In questa procedura clonare l'app, installare le dipendenze ed eseguire l'app. L'app iniziale prova a connettersi ad Archiviazione di Azure, se il servizio è configurato nel codice; se non è disponibile, viene visualizzato il messaggio `Storage is not configured`. 

1. Aprire Visual Studio Code.
1. Clonare il repository GitHub selezionando l'icona Git, quindi **Clona repository** . Per eseguire questa procedura, è necessario accedere a GitHub. Usare l'URL del repository `https://github.com/Azure-Samples/js-e2e-browser-file-upload-storage-blob`, quindi selezionare una cartella nel computer locale in cui clonare l'esempio. Quando richiesto, aprire il repository clonato. 

    :::image type="content" source="../../media/tutorial-browser-file-upload/vscode-git-clone-repository.png" alt-text="Clonare il repository GitHub selezionando l'icona Git e quindi 'Clona repository'.":::

1. In Visual Studio Code aprire una finestra del terminale ed eseguire il comando seguente per installare le dipendenze dell'esempio.

    ```javascript
    npm install
    ```

1. Nella stessa finestra del terminale eseguire il comando per eseguire l'app Web.

    ```javascript
    npm start
    ```

1. Aprire un Web browser e usare l'URL seguente per visualizzare l'app Web nel computer locale.

    ```url
    http://localhost:3000/
    ```

    Se nel browser viene visualizzata la semplice app Web con il messaggio che indica che l'archiviazione non è configurata, questa sezione dell'esercitazione è stata completata correttamente.

    :::image type="content" source="../../media/tutorial-browser-file-upload/browser-react-app-no-azure-storage-resource-configured.png" alt-text="Clonare il repository GitHub selezionando l'icona Git e quindi 'Clona repository'.":::

1. Arrestare il codice premendo CTRL+C nel terminale Visual Studio Code.
