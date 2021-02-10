---
title: Usare JavaScript in Azure PostgreSQL
description: Per creare o spostare il database PostgreSQL in Azure, è necessaria una risorsa PostgreSQL.
ms.topic: how-to
ms.date: 02/8/2021
ms.custom: devx-track-js
ms.openlocfilehash: 2633a8b9e8f120a23af7840097c2930127e294ec
ms.sourcegitcommit: 98a7e855206ff463c1d95f93c23dd665b26a0aa1
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/09/2021
ms.locfileid: "100006253"
---
# <a name="develop-a-javascript-application-with-postgresql-on-azure"></a>Sviluppare un'applicazione JavaScript con PostgreSQL in Azure

Per creare, spostare o usare un database PostgreSQL in Azure, è necessaria una risorsa **del server database di Azure per PostgreSQL** . Informazioni su come creare la risorsa e usare il database.

## <a name="create-an-azure-database-for-postgresql-resource"></a>Creare una risorsa di database di Azure per PostgreSQL 

Creare una risorsa con:

* [Interfaccia della riga di comando di Azure](../with-azure-cli/create-postgresql-server-resource.md)
* [Visual Studio Code](../with-visual-studio-code/create-azure-database.md#create-a-postgresql-database)
* [Azure portal](https://ms.portal.azure.com/#create/Microsoft.PostgreSQLServer)
* [@azure/arm-postgresql](https://www.npmjs.com/package/@azure/arm-postgresql)

## <a name="view-and-use-your-postgresql-server-on-azure"></a>Visualizzare e usare il server PostgreSQL in Azure
Durante lo sviluppo del database PostgreSQL con JavaScript, usare uno degli strumenti seguenti:

* L'interfaccia della riga di comando [Azure cloud Shell](https://shell.azure.com/) -PSQL è disponibile
* [pgAdmin](https://www.pgadmin.org/)
* [Estensione](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb) Visual Studio Code

## <a name="use-sdk-packages-to-develop-your-postgresql-server-on-azure"></a>Usare i pacchetti SDK per sviluppare il server PostgreSQL in Azure

Azure PostgreSQL usa i pacchetti NPM già disponibili, ad esempio:

* [PG](https://www.npmjs.com/package/pg)

## <a name="use-pg-sdk-to-connect-to-postgresql-on-azure"></a>Usare PG SDK per connettersi a PostgreSQL in Azure

Per connettersi e usare PostgreSQL in Azure con JavaScript, seguire questa procedura.

1. Verificare che Node.js e NPM siano installati.
1. Creare un progetto Node.js in una nuova cartella:

    ```bash
    mkdir DbDemo && \
        cd DbDemo && \
        npm init -y && \
        npm install pg && \
        touch index.js && \
        code .
    ```

    Il comando:
    * Crea una cartella di progetto denominata `DbDemo`
    * Modifica il terminale bash in tale cartella
    * Inizializza il progetto, che crea il `package.json` file
    * Installa il pacchetto NPM PG: per usare async/await
    * Crea il `index.js` file di script
    * Apre il progetto in Visual Studio Code

1. Copiare il codice JavaScript seguente in `index.js` :

    ```nodejs
    const { Client } = require('pg')
    
    const query = async (connectionString) => {
        
        // create connection
        const connection = new Client(connectionString);
        connection.connect();
        
        // show tables in the postgres database
        const tables = await connection.query('SELECT table_name FROM information_schema.tables where table_type=\'BASE TABLE\';');
        console.log(tables.rows);
    
        // show users configured for the server
        const users = await connection.query('select pg_user.usename FROM pg_catalog.pg_user;');
        console.log(users.rows);
        
        // close connection
        connection.end();
    }
    
    const server='YOURRESOURCENAME';
    const user='YOUR-ADMIN-USER';
    const password='YOUR-PASSWORD';
    const database='postgres';

    const connectionString = `postgres://${user}@${server}:${password}@${server}.postgres.database.azure.com:5432/${database}`;
    
    query(connectionString)
    .then(() => console.log('done'))
    .catch((err) => console.log(err));
    ```

1. Sostituire `YOUR-ADMIN-USER` , `YOURRESOURCENAME` e con i `YOUR-PASSWORD` valori nello script per la stringa di connessione. 

1. Eseguire lo script per connettersi al `postgres` Server e visualizzare le tabelle e gli utenti di base.

    ```bash
    node index.js
    ```

1. Visualizzare i risultati. 

    ```bash
    [
      { table_name: 'pg_statistic' },
      { table_name: 'pg_type' },
      { table_name: 'pg_authid' },
      { table_name: 'pg_user_mapping' },
      { table_name: 'pg_attribute' },
      { table_name: 'pg_proc' },
      { table_name: 'pg_class' },
      { table_name: 'pg_attrdef' },
      { table_name: 'pg_constraint' },
      { table_name: 'pg_inherits' },
      { table_name: 'pg_index' },
      { table_name: 'pg_operator' },
      { table_name: 'pg_opfamily' },
      { table_name: 'pg_opclass' },
      { table_name: 'pg_am' },
      { table_name: 'pg_amop' },
      { table_name: 'pg_amproc' },
      { table_name: 'pg_language' },
      { table_name: 'pg_largeobject_metadata' },
      { table_name: 'pg_aggregate' },
      { table_name: 'pg_rewrite' },
      { table_name: 'pg_largeobject' },
      { table_name: 'pg_trigger' },
      { table_name: 'pg_event_trigger' },
      { table_name: 'pg_description' },
      { table_name: 'pg_cast' },
      { table_name: 'pg_enum' },
      { table_name: 'pg_namespace' },
      { table_name: 'pg_conversion' },
      { table_name: 'pg_depend' },
      { table_name: 'pg_database' },
      { table_name: 'pg_db_role_setting' },
      { table_name: 'pg_tablespace' },
      { table_name: 'pg_pltemplate' },
      { table_name: 'pg_auth_members' },
      { table_name: 'pg_shdepend' },
      { table_name: 'pg_shdescription' },
      { table_name: 'pg_ts_config' },
      { table_name: 'pg_ts_config_map' },
      { table_name: 'pg_ts_dict' },
      { table_name: 'pg_ts_parser' },
      { table_name: 'pg_ts_template' },
      { table_name: 'pg_extension' },
      { table_name: 'pg_foreign_data_wrapper' },
      { table_name: 'pg_foreign_server' },
      { table_name: 'pg_foreign_table' },
      { table_name: 'pg_policy' },
      { table_name: 'pg_replication_origin' },
      { table_name: 'pg_default_acl' },
      { table_name: 'pg_init_privs' },
      { table_name: 'pg_seclabel' },
      { table_name: 'pg_shseclabel' },
      { table_name: 'pg_collation' },
      { table_name: 'pg_range' },
      { table_name: 'pg_transform' },
      { table_name: 'sql_features' },
      { table_name: 'sql_implementation_info' },
      { table_name: 'sql_languages' },
      { table_name: 'sql_packages' },
      { table_name: 'sql_parts' },
      { table_name: 'sql_sizing' },
      { table_name: 'sql_sizing_profiles' }
    ]
    [ { usename: 'azure_superuser' }, { usename: 'YOUR-ADMIN-USER' } ]
    done
    ```

## <a name="next-steps"></a>Passaggi successivi

* Come [distribuire un'app Web JavaScript](../deploy-web-app.md)
* [Database di Azure per il server PostgreSQL](/azure/postgresql/)