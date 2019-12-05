---
title: Creare il Servizio app di Azure da Visual Studio Code
description: Parte 2 dell'esercitazione, creare l'app Node.js
ms.topic: conceptual
ms.date: 09/20/2019
ms.openlocfilehash: 96c786b7cc8112c36c0aff06761417a97e30bf44
ms.sourcegitcommit: e77f8f652128b798dbf972078a7b460ed21fb5f8
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/25/2019
ms.locfileid: "74466806"
---
# <a name="create-your-nodejs-application"></a>Creare l'applicazione Node.js

[Passaggio precedente: Introduzione e prerequisiti](tutorial-vscode-azure-app-service-node-01.md)

In questo passaggio viene creata una semplice app Node.js, usando il generatore di applicazioni Express, che è poi possibile distribuire in Azure.

È anche possibile usare l'app dell'[esercitazione su Node.js in Visual Studio Code](https://code.visualstudio.com/docs/nodejs/nodejs-tutorial), nel qual caso si può procedere con il passaggio [Distribuire l'app](tutorial-vscode-azure-app-service-node-03.md).

1. In un terminale o al prompt dei comandi usare il comando seguente per eseguire il generatore Express ed eseguire lo scaffolding di una nuova app Express denominata "myExpressApp". I parametri `--view pug --git` indicano al generatore di usare il motore di modelli [pug](https://pugjs.org/api/getting-started.html), noto in precedenza come Jade, e creare un file con estensione *gitignore*.

    ```bash
    npx express-generator myExpressApp --view pug -–git
    ```

1. Installare le dipendenze dell'applicazione eseguendo `npm install` nella cartella dell'app:

    ```bash
    cd myExpressApp
    npm install
    ```

1. Avviare il server eseguendo `npm start`:

    ```bash
    npm start
    ```

1. Testare l'app aprendo un browser all'indirizzo [http://localhost:3000](http://localhost:3000). Il sito dovrebbe essere simile al seguente:

    ![Esecuzione dell'applicazione Express](media/deploy-azure/express.png)

> [!div class="nextstepaction"]
> [L'app Node.js è stata creata](tutorial-vscode-azure-app-service-node-03.md) [Si è verificato un problema](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azureappservice&step=create-app)
