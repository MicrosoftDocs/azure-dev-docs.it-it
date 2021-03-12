---
title: Usare JavaScript con Redis in Azure
description: Per creare o spostare il database Redis in Azure, è necessaria una cache di Azure per la risorsa Redis.
ms.topic: how-to
ms.date: 03/04/2021
ms.custom: devx-track-js
ms.openlocfilehash: cae27bea4326cb43e2f4c65590f9f2de46f02f7e
ms.sourcegitcommit: 737d95fe31e9db55c2d42a93f194a3f3e4bd3c7d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/10/2021
ms.locfileid: "102622269"
---
# <a name="develop-a-javascript-application-with-azure-cache-for-redis"></a>Sviluppare un'applicazione JavaScript con cache di Azure per Redis


Per creare, spostare o usare un database Redis in Azure, è necessaria una **cache di Azure per la risorsa Redis** . Informazioni su come creare la risorsa e usare il database.

## <a name="create-a-resource-for-a-redis-database"></a>Creare una risorsa per un database Redis

È possibile creare una risorsa con:

* Interfaccia della riga di comando di Azure
* [Azure portal](https://ms.portal.azure.com/#create/Microsoft.Cache)

[!INCLUDE [Azure CLI commands](../../includes/azure-cli-cache-for-redis-db.md)]

## <a name="view-and-use-your-redis-database"></a>Visualizzare e usare il database Redis

Durante lo sviluppo del database Redis con JavaScript, usare la [console Redis](/azure/azure-cache-for-redis/cache-configure#redis-console) dalla portale di Azure per lavorare con il database.

:::image type="content" source="../../media/howto-database/azure-cache-for-redis-console-button.png" alt-text="Durante lo sviluppo del database Redis con JavaScript, usare la console Redis dalla portale di Azure per lavorare con il database.":::

Questa console fornisce la funzionalità dell'interfaccia della riga di comando [Redis](https://redis.io/topics/rediscli) . Tenere presente che [alcuni comandi non sono supportati](/azure/azure-cache-for-redis/cache-configure#redis-commands-not-supported-in-azure-cache-for-redis).

Dopo aver creato la risorsa, [importare i dati](/azure/azure-cache-for-redis/cache-how-to-import-export-data) nella risorsa Redis da archiviazione di Azure usando il portale di Azure. 

## <a name="use-native-sdk-packages-to-connect-to-redis-on-azure"></a>Usare pacchetti SDK nativi per connettersi a Redis in Azure

Il database Redis usa i pacchetti NPM, ad esempio:

* [Redis](https://www.npmjs.com/package/redis)
* [ioredis](https://www.npmjs.com/package/ioredis)

## <a name="install-ioredis-sdk"></a>Installare ioredis SDK 

Utilizzare la procedura seguente per installare il `ioredis` pacchetto e inizializzare il progetto.

1. Verificare che Node.js e NPM siano installati.
1. Creare un progetto Node.js in una nuova cartella:

    ```bash
    mkdir DataDemo && \
        cd DataDemo && \
        npm init -y && \
        npm install ioredis \
        code .
    ```

    Il comando:
    * Crea una cartella di progetto denominata `DataDemo`
    * Modifica il terminale bash in tale cartella
    * Inizializza il progetto, che crea il `package.json` file
    * Aggiunge ioredis NPM SDK al progetto
    * Apre il progetto in Visual Studio Code

## <a name="create-javascript-file-to-bulk-insert-data-into-redis"></a>Creare un file JavaScript per l'inserimento bulk dei dati in Redis

1. In Visual Studio Code creare un `bulk_insert.js` file.

1. Scaricare il file di [MOCK_DATA.csv](https://github.com/Azure-Samples/js-e2e/blob/main/database/redis/MOCK_DATA.csv) e posizionarlo nella stessa directory di `bulk_insert.js` .

1. Copiare il codice JavaScript seguente in `bulk_insert.js` :

    ```nodejs
    const Redis = require('ioredis');
    const fs = require('fs');
    const parse = require('csv-parser')
    const { finished } = require('stream/promises');
    
    const config = {
        "HOST": "YOUR-RESOURCE-NAME.redis.cache.windows.net",
        "KEY": "YOUR-RESOURCE-PASSWORD",
    }
    
    // Create Redis config object
    const configuration = {
        host: config.HOST,
        port: 6380,
        password: config.KEY,
        tls: {
            servername: config.HOST
        },
        database: 0,
        keyPrefix: config.prefix
    }
    var redis = new Redis(configuration);
    
    // insert each row into Redis
    async function insertData(readable) {
        for await (const row of readable) {
            await redis.set(`bar2:${row.id}`, JSON.stringify(row))
        }
    }
    
    // read file, parse CSV, each row is a chunk
    const readable = fs
        .createReadStream('./MOCK_DATA.csv')
        .pipe(parse());
    
    // Pipe rows to insert function
    insertData(readable)
    .then(() => {
        console.log('succeeded');
        redis.disconnect();
    })
    .catch(console.error);
    ```

1. Sostituire gli elementi seguenti nello script con le informazioni sulle risorse di redis:

    * NOME DELLA RISORSA
    * YOUR-AZURE-REDIS-RESOURCE-KEY

1. Eseguire lo script.

    ```bash
    node bulk_insert.js
    ```
    
## <a name="create-javascript-code-to-use-redis"></a>Creare codice JavaScript per l'uso di redis

1. In Visual Studio Code creare un `index.js` file.


1. Copiare il codice JavaScript seguente in `index.js` :

    ```nodejs
    const redis = require('ioredis');
    
    const config = {
        "HOST": "YOUR-RESOURCE-NAME.redis.cache.windows.net",
        "KEY": "YOUR-RESOURCE-PASSWORD",
        "TIMEOUT": 300,
        "KEY_PREFIX": "demoExample:"
    }
    
    // Create Redis config object
    const configuration = {
        host: config.HOST,
        port: 6380,
        password: config.KEY,
        timeout: config.TIMEOUT,
        tls: {
            servername: config.HOST
        },
        database: 0,
        keyPrefix: config.KEY_PREFIX
    }
    
    const connect = () => {
        return redis.createClient(configuration);
    }
    
    const set = async (client, key, expiresInSeconds=configuration.timeout, stringify=true, data) => {
        return await client.setex(key, expiresInSeconds, stringify? JSON.stringify(data): data);
    }
    
    const get = async (client, key, stringParse=true) => {
        const value = await client.get(key);
        return stringParse ? JSON.parse(value) : value;
    }
    
    const remove = async (client, key) => {
          return await client.del(key);
    }
    
    const disconnect = (client) => {
        client.disconnect();
    }
    
    const test = async () => {
        
        // connect
        const dbConnection = await connect();
        
        // set
        const setResult1 = await set(dbConnection, "r1", "1000000", false, "record 1");
        const setResult2 = await set(dbConnection, "r2", "1000000", false, "record 2");
        const setResult3 = await set(dbConnection, "r3", "1000000", false, "record 3");
    
        // get
        const val2 = await get(dbConnection, "r2", false);
        console.log(val2);
        
        // delete
        const remove2 = await remove(dbConnection, "r2");
        
        // get again = won't be there
        const val2Again = await get(dbConnection, "r2", false);
        console.log(val2Again);
        
        // done
        disconnect(dbConnection)
    }
    
    test()
    .then(() => console.log("done"))
    .catch(err => console.log(err))
    ```
 
1. Sostituire gli elementi seguenti nello script con le informazioni sulle risorse di redis:

    * NOME DELLA RISORSA
    * RISORSA-PASSWORD

1. Eseguire lo script.

    ```bash
    node index.js
    ```
    
    Lo script inserisce 3 tasti, quindi Elimina la chiave intermedia. I risultati della console sono:

    ```console
    record 2
    null
    done
    ```

## <a name="use-redis-console-in-azure-portal-to-view-data"></a>Usare la console Redis in portale di Azure per visualizzare i dati

Nella portale di Azure visualizzare la console della risorsa con il comando `SCAN 0 COUNT 1000 MATCH *` . 

:::image type="content" source="../../media/howto-database/azure-cache-for-redis-azure-portal-console-scan.png" alt-text="Nella portale di Azure visualizzare la console della risorsa con il comando ' SCAN 0 COUNT 1000 MATCH *'.":::

## <a name="next-steps"></a>Passaggi successivi

* Come [distribuire un'app Web JavaScript](../deploy-web-app.md)
* [Documentazione di cache di Azure per Redis](/azure/azure-cache-for-redis)
* [Guida introduttiva a cache di Azure per Redis](/azure/azure-cache-for-redis/cache-nodejs-get-started)
* [Centro architetture di Azure procedure consigliate per la memorizzazione nella cache](/azure/architecture/best-practices/caching)
* [Procedure consigliate per la cache di Azure per Redis](/azure/azure-cache-for-redis/cache-best-practices#client-library-specific-guidance)