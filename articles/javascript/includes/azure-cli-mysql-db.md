---
ms.custom: devx-track-js
ms.topic: include
ms.date: 02/04/2021
ms.openlocfilehash: aa497cf82fb5372cff802d4bb4db2fca0077b437
ms.sourcegitcommit: bccbab4883e6b6b4926fc194c35ad948b11ccc3f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/08/2021
ms.locfileid: "99848596"
---
## <a name="create-an-azure-database-for-mysql-resource-with-azure-cli"></a>Creare una risorsa database di Azure per MySQL con l'interfaccia della riga di comando di Azure

Usare il comando [AZ MySQL server create](/cli/azure/mysql/server#az_mysql_server_create) dell'interfaccia della riga di comando di Azure seguente nel [Azure cloud Shell](https://shell.azure.com) per creare una nuova risorsa MySQL per il database. 

```azurecli
az mysql server create \
    --subscription YOUR-SUBSCRIPTION-ID-OR-NAME \
    --resource-group YOUR-RESOURCE-GROUP \
    --name YOURRESOURCENAME \
    --enable-public-network Enabled \
    --location eastus \
    --admin-user YOUR-ADMIN-NAME \
    --admin-password YOUR-ADMIN-PASSWORD \
    --sku-name B_Gen5_1 \
    --ssl-enforcement Disabled \
    --version 5.7 
```

Il completamento di questo comando può richiedere alcuni minuti e creare una risorsa disponibile pubblicamente nell' `eastus` area. 

## <a name="create-a-database-on-the-server-with-azure-cli"></a>Creare un database nel server con l'interfaccia della riga di comando di Azure

Usare l'interfaccia della riga di comando di Azure [AZ MySQL DB Create](/cli/azure/mysql/db#az_mysql_db_create) nel [Azure cloud Shell](https://shell.azure.com) per creare un nuovo database MySQL nel server. 

```azurecli
az mysql db create \
    --subscription YOUR-SUBSCRIPTION-ID-OR-NAME \
    --resource-group YOUR-RESOURCE-GROUP \
    --server-name YOURRESOURCENAME \
    --name YOURDATABASENAME
```


## <a name="get-the-mysql-connection-string-with-azure-cli"></a>Ottenere la stringa di connessione MySql con l'interfaccia della riga di comando

Recuperare la stringa di connessione MySql per questa istanza con il comando [AZ MySQL server show-Connection-String](/cli/azure/mysql/server#az_mysql_server_show_connection_string) :

```azurecli
az mysql server show-connection-string \
    --subscription YOUR-SUBSCRIPTION-ID-OR-NAME \
    --server-name YOURRESOURCENAME
```

Vengono restituite le stringhe di connessione per le lingue più diffuse come oggetto JSON. Prima di usare la stringa di connessione, è necessario sostituire `{database}` , `{username}` e con i `{password}` propri valori. 

```json
{
  "connectionStrings": {
    "ado.net": "Server=YOURRESOURCENAME.mysql.database.azure.com; Port=3306; Database={database}; Uid={username}@YOURRESOURCENAME; Pwd={password}",
    "jdbc": "jdbc:mysql://YOURRESOURCENAME.mysql.database.azure.com:3306/{database}?user={username}@YOURRESOURCENAME&password={password}",
    "mysql_cmd": "mysql {database} --host YOURRESOURCENAME.mysql.database.azure.com --user {username}@YOURRESOURCENAME --password={password}",
    "node.js": "var conn = mysql.createConnection({host: 'YOURRESOURCENAME.mysql.database.azure.com', user: '{username}@YOURRESOURCENAME',password: {password}, database: {database}, port: 3306});",
    "php": "host=YOURRESOURCENAME.mysql.database.azure.com port=3306 dbname={database} user={username}@YOURRESOURCENAME password={password}",
    "python": "cnx = mysql.connector.connect(user='{username}@YOURRESOURCENAME', password='{password}', host='YOURRESOURCENAME.mysql.database.azure.com', port=3306, database='{database}')",
    "ruby": "client = Mysql2::Client.new(username: '{username}@YOURRESOURCENAME', password: '{password}', database: '{database}', host: 'YOURRESOURCENAME.mysql.database.azure.com', port: 3306)"
  }
}
``` 