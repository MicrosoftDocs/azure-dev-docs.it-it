---
ms.custom: devx-track-js
ms.topic: include
ms.date: 03/04/2021
ms.openlocfilehash: 6121b17a6155799c5a6d4d43e5870f7613ab2f0b
ms.sourcegitcommit: 737d95fe31e9db55c2d42a93f194a3f3e4bd3c7d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/10/2021
ms.locfileid: "102622289"
---
## <a name="install-mongoose-sdk"></a>Installare Mangusta SDK 

Per connettersi e usare mongoDB in Azure Cosmos DB con JavaScript e mangusta, seguire questa procedura.

1. Verificare che Node.js e NPM siano installati.
1. Creare un progetto Node.js in una nuova cartella:

    ```bash
    mkdir DataDemo && \
        cd DataDemo && \
        npm init -y && \
        npm install mongoose &&
        code .
    ```

    Il comando:
    * Crea una cartella di progetto denominata `DataDemo`
    * Modifica il terminale bash in tale cartella
    * Inizializza il progetto, che crea il `package.json` file
    * Installa l'SDK
    * Apre il progetto in Visual Studio Code

## <a name="use-mongoose-sdk-to-connect-to-mongodb-on-azure"></a>Usare Mangusta SDK per connettersi a MongoDB in Azure

1. Copiare il codice JavaScript seguente in `index.js` :
1. 
    :::code language="javascript" source="~/../js-e2e//database/mongodb/index_mongoose.js" :::
 
1. Sostituire `YOUR-CONNECTION-STRING` nello script con la stringa di connessione della risorsa. 
1. Eseguire lo script.

    ```bash
    node index.js
    ```

    I risultati sono:

    ```console
    find all
    loop {"_id":"6019a68a6ecddc35d536c92c","name":"Joan Smith","job":"Developer","__v":0}
    loop {"_id":"6019a68e6ecddc35d536c92d","name":"Bob Jones","job":"Quality Assurance","__v":0}
    loop {"_id":"6019a6916ecddc35d536c92e","name":"Michelle Roberts","job":"Program Manager","__v":0}
    succeeded
    ```