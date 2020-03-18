---
title: Creare il Servizio app di Azure da Visual Studio Code
description: Parte 2 dell'esercitazione, creare l'app Node.js ed eseguirla in locale
ms.topic: conceptual
ms.date: 03/04/2020
ms.openlocfilehash: b2935554fb337d5f5db1a71e016638869f9d1dc4
ms.sourcegitcommit: 56e5f51daf6f671f7b6e84d4c6512473b35d31d2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/07/2020
ms.locfileid: "78893632"
---
# <a name="create-and-run-a-local-nodejs-app"></a>Creare ed eseguire un'app Node.js in locale

[Passaggio precedente: Introduzione e prerequisiti](tutorial-vscode-azure-app-service-node-01.md)

In questo passaggio viene creata una semplice app Node.js con il generatore di applicazioni Express. L'app verrà quindi eseguita in locale.

1. In un terminale o un prompt dei comandi passare alla posizione in cui creare la cartella dell'app.

1. Eseguire il comando seguente per creare una nuova app Express denominata *expressApp1* usando il generatore Express. I parametri `--view pug --git` indicano al generatore di usare il motore di modelli [pug](https://pugjs.org/api/getting-started.html), noto in precedenza come Jade, e creare un file con estensione *gitignore*.

    ```bash
    npx express-generator expressApp1 --view pug -–git
    ```

1. Passare alla cartella dell'app:

    ```bash
    cd expressApp1
    ```

1. Installare le dipendenze dell'applicazione:

    ```bash
    npm install
    ```

1. Avviare il server:

    ```bash
    npm start
    ```

1. Testare l'app aprendo un browser all'indirizzo [http://localhost:3000](http://localhost:3000). Il sito dovrebbe essere simile al seguente:

    ![Esecuzione dell'applicazione Express](media/deploy-azure/express.png)

1. Premere **CTRL**+**C** nel terminale per arrestare il server.

> [!div class="nextstepaction"]
> [L'app Node.js è stata creata](tutorial-vscode-azure-app-service-node-03.md) [Si è verificato un problema](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azureappservice&step=create-app)
