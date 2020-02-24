---
title: Distribuire modifiche in un sito Web Node.js statico da Visual Studio Code
description: Parte 5 dell'esercitazione, apportare modifiche e ripetere la distribuzione
ms.topic: conceptual
ms.date: 09/24/2019
ms.author: buhollan
ms.openlocfilehash: d0f6be1d57c9d378a4428a5a05da0428314f76c0
ms.sourcegitcommit: aceed8548ad4529a81d83eb15a095edc8607cac5
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/18/2020
ms.locfileid: "77440943"
---
# <a name="make-changes-and-redeploy"></a>Apportare modifiche e ripetere la distribuzione

[Passaggio precedente: Eseguire la distribuzione in Archiviazione di Azure](tutorial-vscode-static-website-node-04.md)

In questo passaggio viene apportata una semplice modifica al codice sorgente dell'app, quindi il sito viene ridistribuito per verificare il flusso di lavoro di distribuzione end-to-end.

# <a name="angular"></a>[Angular](#tab/angular)

1. In Visual Studio Code aprire il file _src/app/app.component.html_ e modificare la riga 305 come indicato di seguito:

    ```html
    <span>Welcome To Azure</span>
    ```

1. In un terminale o al prompt dei comandi eseguire `npm run build`.

1. In VS Code fare clic con il pulsante destro del mouse sulla cartella _dist/my-static-site_ aggiornata e scegliere di nuovo **Distribuisci in Sito Web statico**. Scegliere l'account di archiviazione e confermare di voler distribuire le modifiche. L'estensione di Azure elimina automaticamente i vecchi file prima di distribuire le modifiche per evitare problemi di memorizzazione nella cache.

1. Al termine della distribuzione, aggiornare il sito nel browser per osservare le modifiche:

    ![Modifiche nell'app dopo la ridistribuzione](media/static-website/updated-azure-app-angular.png)

# <a name="react"></a>[React](#tab/react)

1. In Visual Studio Code aprire il file _src/app.js_ e cambiare la riga 11 come indicato di seguito:

    ```html
    <h1 className="App-title">Welcome to Azure!</h1>
    ```

1. In un terminale o al prompt dei comandi eseguire `npm run build`.

1. In VS Code fare clic con il pulsante destro del mouse sulla cartella _build_ aggiornata e scegliere di nuovo **Deploy to Static Website** (Distribuisci nel sito Web statico). Scegliere l'account di archiviazione e confermare di voler distribuire le modifiche. L'estensione di Azure elimina automaticamente i vecchi file prima di distribuire le modifiche per evitare problemi di memorizzazione nella cache.

1. Al termine della distribuzione, aggiornare il sito nel browser per osservare le modifiche:

    ![Modifiche nell'app dopo la ridistribuzione](media/static-website/updated-azure-app-react.png)

# <a name="vue"></a>[Vue](#tab/vue)

1. In Visual Studio Code aprire il file _src/App.vue_ e modificare la riga 11 come indicato di seguito:

    ```html
    <HelloWorld msg="Welcome to Azure!" />
    ```

1. In un terminale o al prompt dei comandi eseguire `npm run build`.

1. In VS Code fare clic con il pulsante destro del mouse sulla cartella _dist_ aggiornata e scegliere di nuovo **Distribuisci in Sito Web statico**. Scegliere l'account di archiviazione e confermare di voler distribuire le modifiche. L'estensione di Azure elimina automaticamente i vecchi file prima di distribuire le modifiche per evitare problemi di memorizzazione nella cache.

1. Al termine della distribuzione, aggiornare il sito nel browser per osservare le modifiche:

    ![Modifiche nell'app dopo la ridistribuzione](media/static-website/updated-azure-app-vue.png)

# <a name="svelte"></a>[Svelte](#tab/svelte)

1. In Visual Studio Code aprire il file _src/main.js_ e cambiare la riga 6 come indicato di seguito:

    ```js
    import App from './App.svelte';

    const app = new App({
        target: document.body,
        props: {
            name: 'Welcome to Azure!'
        }
    });

    export default app;
    ```

2. Ora aprire il file _src/App.svelte_ e cambiare la riga 6 come indicato di seguito:

    ```html
    <h1>{name}</h1>
    ```

1. In un terminale o al prompt dei comandi eseguire `npm run build`.

1. In VS Code fare clic con il pulsante destro del mouse sulla cartella _public_ aggiornata e scegliere di nuovo **Distribuisci in Sito Web statico**. Scegliere l'account di archiviazione e confermare di voler distribuire le modifiche. L'estensione di Azure elimina automaticamente i vecchi file prima di distribuire le modifiche per evitare problemi di memorizzazione nella cache.

1. Al termine della distribuzione, aggiornare il sito nel browser per osservare le modifiche:

    ![Modifiche nell'app dopo la ridistribuzione](media/static-website/updated-azure-app-svelte.png)

---

> [!div class="nextstepaction"]
> [Le modifiche sono state distribuite](tutorial-vscode-static-website-node-06.md) [Si è verificato un problema](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-staticwebsite&step=code-change)
