---
title: Usare JavaScript in MySQL di Azure
description: Per creare o spostare il database MySQL in Azure, è necessaria una risorsa MySQL.
ms.topic: how-to
ms.date: 02/8/2021
ms.custom: devx-track-js
ms.openlocfilehash: caab884c0a943f00ccc1701a7d364bb0825bce78
ms.sourcegitcommit: 98a7e855206ff463c1d95f93c23dd665b26a0aa1
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/09/2021
ms.locfileid: "100006262"
---
# <a name="develop-a-javascript-application-with-mysql-on-azure"></a>Sviluppare un'applicazione JavaScript con MySQL in Azure

Per creare, spostare o usare un database MySQL in Azure, è necessaria una risorsa **database di Azure per MySQL** . Informazioni su come creare la risorsa e usare il database.

## <a name="create-an-azure-database-for-mysql-resource"></a>Creare una risorsa di database di Azure per MySQL 

È possibile creare una risorsa con:

* Interfaccia della riga di comando di Azure
* [Azure portal](https://ms.portal.azure.com/#create/Microsoft.MySQLServer)
* [@azure/arm-mysql](https://www.npmjs.com/package/@azure/arm-mysql)

[!INCLUDE [Azure CLI commands](../../includes/azure-cli-mysql-db.md)]

## <a name="view-and-use-your-mysql-on-azure"></a>Visualizza e usa MySQL in Azure
Durante lo sviluppo del database MySQL con JavaScript, usare uno degli strumenti seguenti:

* Interfaccia della riga di comando di _MySQL_ di [Azure cloud Shell](https://shell.azure.com/)
* [MySQL Workbench](https://www.mysql.com/products/workbench/)
* [Estensione](https://marketplace.visualstudio.com/items?itemName=mtxr.sqltools-driver-mysql) Visual Studio Code

## <a name="use-sdk-packages-to-develop-your-mysql-on-azure"></a>Usare i pacchetti SDK per sviluppare MySQL in Azure

Azure MySQL usa i pacchetti NPM già disponibili, ad esempio:

* [MySQL](https://www.npmjs.com/package/MySQL)
* [Promessa-MySQL](https://www.npmjs.com/package/promise-mysql)

## <a name="use-promise-mysql-sdk-to-connect-to-mysql-on-azure"></a>Usare Promise-MySQL SDK per connettersi a MySQL in Azure

Per connettersi e usare MySQL in Azure con JavaScript, seguire questa procedura.

1. Verificare che Node.js e NPM siano installati.
1. Creare un progetto Node.js in una nuova cartella:

    ```bash
    mkdir MySQLDemo && \
        cd MySQLDemo && \
        npm init -y && \
        npm install promise-mysql && \
        touch index.js && \
        code .
    ```

    Il comando:
    * Crea una cartella di progetto denominata `MySQLDemo`
    * modifica il terminale bash in tale cartella
    * Inizializza il progetto, che crea il `package.json` file
    * installa il pacchetto NPM Promise-MySQL per usare async/await
    * Crea il `index.js` file di script
    * apre il progetto in Visual Studio Code

1. Copiare il codice JavaScript seguente in `index.js` :

    ```nodejs
    // To install npm package,
    // run following command at terminal
    // npm install promise-mysql

    // get MySQL SDK
    const MySQL = require('promise-mysql');

    // query server and close connection
    const query = async (config) => {
      // creation connection
      const connection = await MySQL.createConnection(config);

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
      host: 'YOUR-RESOURCE_NAME.mysql.database.azure.com',
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
      RowDataPacket { Database: 'information_schema' },
      RowDataPacket { Database: 'defaultdb' },
      RowDataPacket { Database: 'dbproducts' },
      RowDataPacket { Database: 'mysql' },
      RowDataPacket { Database: 'performance_schema' },
      RowDataPacket { Database: 'sys' }
    ]
    [
      RowDataPacket { Tables_in_mysql: '__az_action_history__' },
      RowDataPacket { Tables_in_mysql: '__az_changed_static_configs__' },
      RowDataPacket { Tables_in_mysql: '__az_replica_information__' },
      RowDataPacket { Tables_in_mysql: '__az_replication_current_state__' },
      RowDataPacket { Tables_in_mysql: '__firewall_rules__' },
      RowDataPacket { Tables_in_mysql: '__querystore_event_wait__' },
      RowDataPacket { Tables_in_mysql: '__querystore_query_metrics__' },
      RowDataPacket { Tables_in_mysql: '__querystore_query_text__' },
      RowDataPacket {
        Tables_in_mysql: '__querystore_wait_stats_procedure_errors__'
      },
      RowDataPacket {
        Tables_in_mysql: '__querystore_wait_stats_procedure_status__'
      },
      RowDataPacket { Tables_in_mysql: '__recommendation__' },
      RowDataPacket { Tables_in_mysql: '__recommendation_session__' },
      RowDataPacket { Tables_in_mysql: '__script_version__' },
      RowDataPacket { Tables_in_mysql: 'columns_priv' },
      RowDataPacket { Tables_in_mysql: 'db' },
      RowDataPacket { Tables_in_mysql: 'engine_cost' },
      RowDataPacket { Tables_in_mysql: 'event' },
      RowDataPacket { Tables_in_mysql: 'func' },
      RowDataPacket { Tables_in_mysql: 'general_log' },
      RowDataPacket { Tables_in_mysql: 'gtid_executed' },
      RowDataPacket { Tables_in_mysql: 'help_category' },
      RowDataPacket { Tables_in_mysql: 'help_keyword' },
      RowDataPacket { Tables_in_mysql: 'help_relation' },
      RowDataPacket { Tables_in_mysql: 'help_topic' },
      RowDataPacket { Tables_in_mysql: 'innodb_index_stats' },
      RowDataPacket { Tables_in_mysql: 'innodb_table_stats' },
      RowDataPacket { Tables_in_mysql: 'ndb_binlog_index' },
      RowDataPacket { Tables_in_mysql: 'plugin' },
      RowDataPacket { Tables_in_mysql: 'proc' },
      RowDataPacket { Tables_in_mysql: 'procs_priv' },
      RowDataPacket { Tables_in_mysql: 'proxies_priv' },
      RowDataPacket { Tables_in_mysql: 'query_store' },
      RowDataPacket { Tables_in_mysql: 'query_store_wait_stats' },
      RowDataPacket { Tables_in_mysql: 'recommendation' },
      RowDataPacket { Tables_in_mysql: 'server_cost' },
      RowDataPacket { Tables_in_mysql: 'servers' },
      RowDataPacket { Tables_in_mysql: 'slave_master_info' },
      RowDataPacket { Tables_in_mysql: 'slave_relay_log_info' },
      RowDataPacket { Tables_in_mysql: 'slave_worker_info' },
      RowDataPacket { Tables_in_mysql: 'slow_log' },
      RowDataPacket { Tables_in_mysql: 'tables_priv' },
      RowDataPacket { Tables_in_mysql: 'time_zone' },
      RowDataPacket { Tables_in_mysql: 'time_zone_leap_second' },
      RowDataPacket { Tables_in_mysql: 'time_zone_name' },
      RowDataPacket { Tables_in_mysql: 'time_zone_transition' },
      RowDataPacket { Tables_in_mysql: 'time_zone_transition_type' },
      RowDataPacket { Tables_in_mysql: 'user' }
    ]
    [
      RowDataPacket { User: 'mySqlAdmin' },
      RowDataPacket { User: 'azure_superuser' },
      RowDataPacket { User: 'azure_superuser' },
      RowDataPacket { User: 'mysql.session' },
      RowDataPacket { User: 'mysql.sys' }
    ]
    done
    ```

## <a name="next-steps"></a>Passaggi successivi

* Come [distribuire un'app Web JavaScript](../deploy-web-app.md)
* [Database di Azure per MySQL](/azure/mysql/)
* [Migrazione con dump e ripristino](/azure/mysql/concepts-migrate-dump-restore)
* [Migrazione con MySQL Workbench](/azure/mysql/concepts-migrate-import-export)