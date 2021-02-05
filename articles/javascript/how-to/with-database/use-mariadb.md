---
title: Usare JavaScript in Azure MariaDB
description: Per creare o spostare il database MariaDB in Azure, è necessaria una risorsa MariaDB.
ms.topic: how-to
ms.date: 02/04/2021
ms.custom: devx-track-js
ms.openlocfilehash: eab3f4c0e2ce1a3c1b1650ea46981c71f0cddcc1
ms.sourcegitcommit: 7287dff6bf4b30c2033924702c941bf520403e07
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/05/2021
ms.locfileid: "99590860"
---
# <a name="develop-a-javascript-application-with-mariadb-on-azure"></a>Sviluppare un'applicazione JavaScript con MariaDB in Azure

Per creare, spostare o usare un database MariaDB in Azure, è necessaria una risorsa **database di Azure per MariaDB** . Informazioni su come creare la risorsa e usare il database.

## <a name="create-an-azure-database-for-mariadb-resource"></a>Creare una risorsa di database di Azure per MariaDB 

È possibile creare una risorsa con:

* Interfaccia della riga di comando di Azure
* [Azure portal](https://ms.portal.azure.com/#create/Microsoft.MariaDBServer)
* [@azure/arm-mariadb](https://www.npmjs.com/package/@azure/arm-mariadb)

[!INCLUDE [Azure CLI commands](../../includes/azure-cli-mariadb.md)]

## <a name="view-and-use-your-mariadb-on-azure"></a>Visualizza e usa il MariaDB in Azure
Durante lo sviluppo del database MariaDB con JavaScript, usare uno degli strumenti seguenti:

* Interfaccia della riga di comando di _MySQL_ di [Azure cloud Shell](https://shell.azure.com/)
* [MySQL Workbench](https://www.mysql.com/products/workbench/)
* [Estensione](https://marketplace.visualstudio.com/items?itemName=mtxr.sqltools-driver-mysql) Visual Studio Code

## <a name="use-sdk-packages-to-develop-your-mariadb-on-azure"></a>Usare i pacchetti SDK per sviluppare MariaDB in Azure

Azure MariaDB usa i pacchetti NPM già disponibili, ad esempio:

* [MariaDB](https://www.npmjs.com/package/mariadb)

## <a name="use-mariadb-sdk-to-connect-to-mariadb-on-azure"></a>Usare MariaDB SDK per connettersi a MariaDB in Azure

Per connettersi e usare MariaDB in Azure con JavaScript, seguire questa procedura.

1. Verificare che Node.js e NPM siano installati.
1. Creare un progetto Node.js in una nuova cartella:

    ```bash
    mkdir mariaDbDemo && \
        cd mariaDbDemo && \
        npm init -y && \
        npm install mariadb && \
        touch index.js && \
        code .
    ```

    Il comando:
    * Crea una cartella di progetto denominata `mariaDbDemo`
    * modifica il terminale bash in tale cartella
    * Inizializza il progetto, che crea il `package.json` file
    * installa il pacchetto NPM MariaDB
    * Crea il `index.js` file di script
    * apre il progetto in Visual Studio Code

1. Copiare il codice JavaScript seguente in `index.js` :

    ```nodejs
    // To install npm package,
    // run following command at terminal
    // npm install mariadb

    // get mariadb SDK
    const mariadb = require('mariadb');

    // query server and close connection
    const query = async (config) => {
      // creation connection
      const connection = await mariadb.createConnection(config);

      // show databases on server
      const databases = await connection.query('SHOW DATABASES;');
      console.log(databases);

      // show tables in the mysql database
      const tables = await connection.query('SHOW TABLES FROM mysql;');
      console.log(tables);

      // show users configured for the server
      const rows = await connection.query('select User from mysql.user;');
      console.log(rows);

      // close connection
      connection.end();
    };

    const config = {
      host: 'YOUR-RESOURCE_NAME.mariadb.database.azure.com',
      user: 'YOUR-ADMIN-NAME@YOUR-RESOURCE_NAME',
      password: 'YOUR-ADMIN-PASSWORD',
      port: 3306,
    };

    query(config)
      .then(() => console.log('done'))
      .catch((err) => console.log(err));
    ```

1. Sostituire l'host, l'utente e la password con i valori nello script per l'oggetto di configurazione della connessione `config` . 

1. Eseguire lo script.

    ```bash
    [
      { Database: 'information_schema' },
      { Database: 'mysql' },
      { Database: 'performance_schema' },
      { Database: 'quickstartdb' },
      { Database: 'tutorial' },
      meta: [
        ColumnDef {
          _parse: [StringParser],
          collation: [Collation],
          columnLength: 256,
          columnType: 253,
          flags: 1,
          scale: 0,
          type: 'VAR_STRING'
        }
      ]
    ]
    [
      { Tables_in_mysql: '__az_action_history__' },
      { Tables_in_mysql: '__az_changed_static_configs__' },
      { Tables_in_mysql: '__az_replica_information__' },
      { Tables_in_mysql: '__az_replication_current_state__' },
      { Tables_in_mysql: '__az_slave_relay_log_info__' },
      { Tables_in_mysql: '__firewall_rules__' },
      { Tables_in_mysql: '__script_version__' },
      { Tables_in_mysql: 'column_stats' },
      { Tables_in_mysql: 'columns_priv' },
      { Tables_in_mysql: 'db' },
      { Tables_in_mysql: 'event' },
      { Tables_in_mysql: 'func' },
      { Tables_in_mysql: 'general_log' },
      { Tables_in_mysql: 'gtid_slave_pos' },
      { Tables_in_mysql: 'help_category' },
      { Tables_in_mysql: 'help_keyword' },
      { Tables_in_mysql: 'help_relation' },
      { Tables_in_mysql: 'help_topic' },
      { Tables_in_mysql: 'host' },
      { Tables_in_mysql: 'index_stats' },
      { Tables_in_mysql: 'innodb_index_stats' },
      { Tables_in_mysql: 'innodb_table_stats' },
      { Tables_in_mysql: 'plugin' },
      { Tables_in_mysql: 'proc' },
      { Tables_in_mysql: 'procs_priv' },
      { Tables_in_mysql: 'proxies_priv' },
      { Tables_in_mysql: 'roles_mapping' },
      { Tables_in_mysql: 'servers' },
      { Tables_in_mysql: 'slow_log' },
      { Tables_in_mysql: 'table_stats' },
      { Tables_in_mysql: 'tables_priv' },
      { Tables_in_mysql: 'time_zone' },
      { Tables_in_mysql: 'time_zone_leap_second' },
      { Tables_in_mysql: 'time_zone_name' },
      { Tables_in_mysql: 'time_zone_transition' },
      { Tables_in_mysql: 'time_zone_transition_type' },
      { Tables_in_mysql: 'transaction_registry' },
      { Tables_in_mysql: 'user' },
      meta: [
        ColumnDef {
          _parse: [StringParser],
          collation: [Collation],
          columnLength: 292,
          columnType: 253,
          flags: 1,
          scale: 0,
          type: 'VAR_STRING'
        }
      ]
    ]
    [
      { User: 'azurediberry' },
      { User: 'azure_superuser' },
      { User: 'azure_superuser' },
      { User: 'azure_superuser' },
      meta: [
        ColumnDef {
          _parse: [StringParser],
          collation: [Collation],
          columnLength: 320,
          columnType: 254,
          flags: 16515,
          scale: 0,
          type: 'STRING'
        }
      ]
    ]
    done
    ```

## <a name="next-steps"></a>Passaggi successivi

* Come [distribuire un'app Web JavaScript](../deploy-web-app.md)
* [Database di Azure per MariaDB](/azure/mariadb/)
* [Guida alla migrazione per passare al database di Azure per MariaDB](/azure/mariadb/howto-migrate-dump-restore)