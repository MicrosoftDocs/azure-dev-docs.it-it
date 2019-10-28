---
title: Creare l'app Node.js statica in Visual Studio Code
description: Parte 2 dell'esercitazione, creare l'app di esempio
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/24/2019
ms.author: buhollan
ms.openlocfilehash: b57789ca26851c27a3a68c9d095f327ea64164cf
ms.sourcegitcommit: 2757d8bd0cc045b7d02f430d44de859f9de853f4
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/18/2019
ms.locfileid: "72587244"
---
# <a name="create-the-app"></a>Creare l'app

[Passaggio precedente: Introduzione e prerequisiti](tutorial-vscode-static-website-node-01.md)

In questo passaggio si usa l'interfaccia della riga di comando di [Angular](https://cli.angular.io/), [React](https://github.com/facebook/create-react-app) o [Vue](https://cli.vuejs.org/) per creare una semplice app che è possibile distribuire in Azure. In alternativa, è possibile usare qualsiasi altro framework JavaScript che produca un set di file statici o qualsiasi cartella che contenga file HTML, CSS o JavaScript. Se è già disponibile un'app da distribuire, è possibile procedere con il passaggio [Creare un account di Archiviazione di Azure](tutorial-vscode-static-website-node-03.md).

# <a name="angulartabangular"></a>[Angular](#tab/angular)

1. Usare l'interfaccia della riga di comando per eseguire lo scaffolding di una nuova app denominata "my-static-app" eseguendo il comando seguente:

    ```bash
    npx @angular/cli new my-static-app
    ```

    Quando l'interfaccia della riga di comando visualizza domande relative alla configurazione, premere INVIO per selezionare le opzioni predefinite.

1. Compilare l'applicazione passando alla nuova cartella ed eseguendo `npm run build`:

    ```bash
    cd my-static-app
    npm run build
    ```

1. Nella cartella _my-static-app_ dovrebbe essere presente una cartella _dist_. All'interno della cartella _dist_ sarà presente una cartella con lo stesso nome del progetto, ovvero _my-static-app_. La cartella _build/my-static-app_ contiene i file HTML, CSS e JavaScript da distribuire in Archiviazione di Azure.

1. Eseguire l'app usando il comando seguente:

    ```bash
    npm start
    ```

1. Aprire un browser all'indirizzo [http://localhost:3000](http://localhost:3000) per verificare se l'app è in esecuzione:

    ![App Angular di esempio in esecuzione](media/static-website/local-app-angular.png)

1. Arrestare il server premendo **CTRL**+**C** nel terminale o al prompt dei comandi.

# <a name="reacttabreact"></a>[React](#tab/react)

1. Usare l'interfaccia della riga di comando per eseguire lo scaffolding di una nuova app denominata "my-static-app" eseguendo il comando seguente:

    ```bash
    npx create-react-app my-static-app
    ```

1. Compilare l'applicazione passando alla nuova cartella ed eseguendo `npm run build`:

    ```bash
    cd my-static-app
    npm run build
    ```

1. Nella cartella _my-static-app_ dovrebbe essere presente una cartella _build_. La cartella _build_ contiene i file HTML, CSS e JavaScript da distribuire in Archiviazione di Azure.

1. Eseguire l'app usando il comando seguente:

    ```bash
    npm start
    ```

1. Aprire un browser all'indirizzo [http://localhost:3000](http://localhost:3000) per verificare se l'app è in esecuzione:

    ![L'app React di esempio in esecuzione](media/static-website/local-app-react.png)

1. Arrestare il server premendo **CTRL**+**C** nel terminale o al prompt dei comandi.

# <a name="vuetabvue"></a>[Vue](#tab/vue)

1. Usare l'interfaccia della riga di comando per eseguire lo scaffolding di una nuova app denominata "my-static-app" eseguendo il comando seguente:

    ```bash
    npx @vue/cli create my-static-app
    ```

Quando l'interfaccia della riga di comando visualizza domande relative alla configurazione, premere INVIO per selezionare le opzioni predefinite.

1. Compilare l'applicazione passando alla nuova cartella ed eseguendo `npm run build`:

    ```bash
    cd my-static-app
    npm run build
    ```

1. Nella cartella _my-static-app_ dovrebbe essere presente una cartella _dist_. La cartella _dist_ contiene i file HTML, CSS e JavaScript da distribuire in Archiviazione di Azure.

1. Eseguire l'app usando il comando seguente:

     ```bash
     npm run serve
     ```

1. Aprire un browser all'indirizzo [http://localhost:8080](http://localhost:8080) per verificare se l'app è in esecuzione:

    ![App Vue di esempio in esecuzione](media/static-website/local-app-vue.png)

1. Arrestare il server premendo **CTRL**+**C** nel terminale o al prompt dei comandi.

---

> [!div class="nextstepaction"]
> [L'app è stata creata](tutorial-vscode-static-website-node-03.md) [Si è verificato un problema](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-staticwebsite&step=create-app)
