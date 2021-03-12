---
ms.custom: devx-track-js
ms.topic: include
ms.date: 03/04/2021
ms.openlocfilehash: a16cdd0846485622d6f61a049c3dfc2d848812f5
ms.sourcegitcommit: 737d95fe31e9db55c2d42a93f194a3f3e4bd3c7d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/10/2021
ms.locfileid: "102622291"
---
## <a name="install-mongodb-sdk"></a>Installare MongoDB SDK 

Per connettersi e usare mongoDB in Azure Cosmos DB con JavaScript e MongoDB, seguire questa procedura.

1. Verificare che Node.js e NPM siano installati.
1. Creare un progetto Node.js in una nuova cartella:

    ```bash
    mkdir DataDemo && \
        cd DataDemo && \
        npm init -y && \
        npm install mongodb &&
        code .
    ```

    Il comando:
    * Crea una cartella di progetto denominata `DataDemo`
    * Modifica il terminale bash in tale cartella
    * Inizializza il progetto, che crea il `package.json` file
    * Installa l'SDK
    * Apre il progetto in Visual Studio Code

## <a name="create-javascript-file-to-bulk-insert-data-into-mongodb-database"></a>Creare un file JavaScript per l'inserimento bulk dei dati nel database MongoDB

1. In Visual Studio Code creare un `bulk_insert.js` file.

1. Scaricare il file di [MOCK_DATA.csv](https://github.com/Azure-Samples/js-e2e/blob/main/database/redis/MOCK_DATA.csv) e posizionarlo nella stessa directory di `bulk_insert.js` .

1. Copiare il codice JavaScript seguente in `bulk_insert.js` :

    :::code language="javascript" source="~/../js-e2e//database/mongodb/bulk_insert_mongodb.js" :::

1. Sostituire gli elementi seguenti nello script con le informazioni sulle risorse:

    * YOUR_RESOURCE_PRIMARY_CONNECTION_STRING

1. Eseguire lo script.

    ```bash
    node bulk_insert.js
    ```

## <a name="create-javascript-code-to-use-mongodb"></a>Creare codice JavaScript per usare MongoDB

1. In Visual Studio Code creare un `index.js` file.

1. Copiare il codice JavaScript seguente in `index.js` :

    :::code language="javascript" source="~/../js-e2e//database/mongodb/index_mongodb.js" :::

1. Sostituire gli elementi seguenti nello script con le informazioni sulle risorse:

    * YOUR_RESOURCE_PRIMARY_CONNECTION_STRING

1. Eseguire lo script.

    ```bash
    node index.js
    ```