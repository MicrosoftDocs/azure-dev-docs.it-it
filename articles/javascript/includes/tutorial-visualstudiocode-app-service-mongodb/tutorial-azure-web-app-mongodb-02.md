---
title: file di inclusione tutorial-azure-web-app-mongodb-02.md
description: file di inclusione tutorial-azure-web-app-mongodb-02.md
ms.date: 10/13/2020
ms.topic: include
ms.custom: devx-track-javascript
ms.openlocfilehash: fafb2b452e9e1bfab34d9c2d9d46adaabf4fd022
ms.sourcegitcommit: 8a2a7df568c69fff2080ffab248409040efda1ac
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/19/2020
ms.locfileid: "92183852"
---
In questa sezione dell'esercitazione scaricare l'applicazione di esempio nel computer locale ed eseguirla dal terminale di Visual Studio Code. Visualizzare quindi l'app in esecuzione in locale nel browser. 

## <a name="download-and-run-the-initial-expressjs-app"></a>Scaricare ed eseguire l'app Express.js iniziale

L'app Web Express.js iniziale viene fornita come punto di partenza. In questa procedura scaricare l'app, installare le dipendenze ed eseguire l'app. L'app iniziale prova a connettersi a un database, se è disponibile. Se non è disponibile, il sito Web risponde comunque correttamente a una richiesta. 

1. [Scaricare il repository GitHub compresso](https://github.com/Azure-Samples/js-e2e-express-mongo.git) nel computer locale e quindi decomprimerlo in una cartella. 
1. Aprire la cartella in Visual Studio Code. È possibile fare clic con il pulsante destro del mouse sulla cartella e scegliere **Apri con Code** oppure usare il comando equivalente dell'interfaccia della riga di comando all'interno della cartella:

    ```console
    code .
    ```

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
    http://localhost:8080/
    ```

    Se nel browser viene visualizzata la semplice app Web con il messaggio che indica che il database non è stato trovato, questa sezione dell'esercitazione è stata completata correttamente.

    :::image type="content" source="../../media/tutorial-end-to-end-app-cosmos/nodejs-app-connected-mongodb-form.png" alt-text="Semplice app Node.js connessa al database MongoDB.":::