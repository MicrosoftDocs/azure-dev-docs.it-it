---
author: burkeholland
ms.service: app-service
ms.topic: include
ms.date: 03/31/2020
ms.author: buhollan
ms.openlocfilehash: c95177d6b4cb101b764acd8f8dad54f937a495eb
ms.sourcegitcommit: 553da4e9aa988e5bb823364244ea81961cee5bc7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/01/2020
ms.locfileid: "85793000"
---
1. In un prompt dei comandi del terminale passare alla posizione in cui creare la cartella dell'app.

1. Eseguire il comando seguente per creare una nuova app Express denominata `myexpressapp` usando il generatore Express. I parametri `--git --view pug` indicano al generatore di creare un file con estensione gitignore e di usare il motore di modelli [Pug](https://pugjs.org/api/getting-started.html), noto in precedenza come Jade.

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

1. Testare l'app aprendo un browser all'indirizzo `http://localhost:3000`. Ecco come dovrà essere il sito:

    ![Esecuzione dell'applicazione Express](../media/deploy-azure/express.png)

1. Premere **CTRL**+**C** nel terminale per arrestare il server.
 