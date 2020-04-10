---
author: burkeholland
ms.service: app-service
ms.topic: include
ms.date: 03/31/2020
ms.author: buhollan
ms.openlocfilehash: 59eea52aab3f2b1941a3accb5848bc4ad92d2992
ms.sourcegitcommit: f89c59f772364ec717e751fb59105039e6fab60c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2020
ms.locfileid: "80740507"
---
1. In un terminale o un prompt dei comandi passare alla posizione in cui creare la cartella dell'app.

1. Eseguire il comando seguente per creare una nuova app Express denominata *myexpressapp* usando il generatore Express. I parametri `--git --view pug` indicano al generatore di creare un file con estensione *gitignore* e di usare il motore modello [pug](https://pugjs.org/api/getting-started.html), noto in precedenza come Jade.

    ```bash
    npx express-generator myexpressapp --git --view pug
    ```

1. Passare alla cartella dell'app:

    ```bash
    cd myexpressapp
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

    ![Esecuzione dell'applicazione Express](../media/deploy-azure/express.png)

1. Premere **CTRL**+**C** nel terminale per arrestare il server.
