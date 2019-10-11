---
title: Creare l'app Node.js statica in Visual Studio Code
description: Parte 2 dell'esercitazione, creare l'app di esempio
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/24/2019
ms.author: kraigb
ms.openlocfilehash: 566d166a69bbbee59726b8e381bee4a24077d8c6
ms.sourcegitcommit: c04984b6367e922dbc5973af44f8cd0ca81ce157
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/30/2019
ms.locfileid: "71685975"
---
# <a name="create-the-app"></a>Creare l'app

[Passaggio precedente: Introduzione e prerequisiti](tutorial-vscode-static-website-node-01.md)

In questo passaggio si usa l'interfaccia della riga di comando dell'utilità React, [create-react-app](https://github.com/facebook/create-react-app), per creare una semplice app React che è possibile distribuire in Azure. In alternativa, è possibile usare Angular, Vue, un altro framework o qualsiasi cartella che contiene alcuni file HTML. Se è già disponibile un'app da distribuire, è possibile procedere con il passaggio [Creare un account di Archiviazione di Azure](tutorial-vscode-static-website-node-03.md).

1. Usare lo strumento create-react-app per eseguire lo scaffolding di una nuova app React denominata `my-react-app` eseguendo il comando seguente:

    ```bash
    npm create react-app my-react-app
    ```

1. Compilare l'applicazione passando alla nuova cartella ed eseguendo `npm run build`:

    ```bash
    cd my-react-app
    npm run build
    ```

1. Nella cartella *my-react-app* dovrebbe essere presente una cartella *build*. La cartella *build* contiene i file HTML, CSS e JavaScript da distribuire in Archiviazione di Azure.

1. Eseguire l'app usando il comando seguente:

    ```bash
    npm start
    ```

1. Aprire un browser all'indirizzo [http://localhost:3000](http://localhost:3000) per verificare se l'app è in esecuzione:

    ![L'app React di esempio in esecuzione](media/static-website/local-app.png)

1. Arrestare il server premendo **CTRL**+**C** nel terminale o al prompt dei comandi.

> [!div class="nextstepaction"]
> [L'app è stata creata](tutorial-vscode-static-website-node-03.md) [Si è verificato un problema](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-staticwebsite&step=create-app)
