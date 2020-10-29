---
title: Creare un'app Node.js da distribuire in Azure tramite l'interfaccia della riga di comando di Azure
description: Parte 2 dell'esercitazione, creare il codice dell'app con l'interfaccia della riga di comando di Azure.
ms.topic: tutorial
ms.date: 09/24/2019
ms.custom: devx-track-js, devx-track-azurecli
ms.openlocfilehash: 89e979dabca91e47083f48e42543453facc57130
ms.sourcegitcommit: 1ddcb0f24d2ae3d1f813ec0f4369865a1c6ef322
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/27/2020
ms.locfileid: "92689147"
---
# <a name="create-the-app-code-using-express"></a>Creare il codice dell'app usando Express

[Passaggio precedente: Introduzione e prerequisiti](tutorial-vscode-azure-cli-node-01.md)

In questo passaggio viene creata una semplice app Node.js con [Express](https://www.expressjs.com) usando il [generatore Express](https://expressjs.com/en/starter/generator.html).

1. Usare il comando seguente per eseguire il generatore Express ed eseguire lo scaffolding di una nuova app Express denominata "myExpressApp". I parametri `--view pug --git` indicano al generatore di usare il motore di modelli [pug](https://pugjs.org/api/getting-started.html), noto in precedenza come Jade, e creare un file con estensione *gitignore* .

    ```bash
    npx express-generator myExpressApp --view pug –git
    ```

1. Passare alla cartella dell'app e installare le relative dipendenze eseguendo i comandi seguenti:

    ```bash
    cd myExpressApp
    npm install
    ```

1. Avviare il server dell'app eseguendo il comando seguente:

    ```bash
    npm start
    ```

1. Aprire un browser all'indirizzo `http://localhost:3000` per vedere l'app in esecuzione:

    ![Esecuzione dell'app Express in locale](media/azure-cli/local-app.png)

1. Dopo aver testato l'app, arrestare il server premendo **CTRL**+**C** nel terminale in cui è stato eseguito `npm start`.

> [!div class="nextstepaction"]
> [L'app è stata creata](tutorial-vscode-azure-cli-node-03.md) [Si è verificato un problema](https://www.research.net/r/PWZWZ52?tutorial=node-deployment&step=express)
