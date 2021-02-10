---
ms.custom: devx-track-js
ms.topic: include
ms.date: 02/04/2021
ms.openlocfilehash: 0edd926e2acf016eb6389fe58c7f1d5e78628dbf
ms.sourcegitcommit: 98a7e855206ff463c1d95f93c23dd665b26a0aa1
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/09/2021
ms.locfileid: "100004722"
---
## <a name="create-an-azure-database-for-mysql-resource-with-azure-cli"></a>Creare una risorsa database di Azure per MySQL con l'interfaccia della riga di comando di Azure

Usare il comando [AZ MySQL server create](/cli/azure/mysql/server#az_mysql_server_create) dell'interfaccia della riga di comando di Azure seguente nel [Azure cloud Shell](https://shell.azure.com) per creare una nuova risorsa MySQL per il database. 

```azurecli
az mysql server create \
    --subscription YOUR-SUBSCRIPTION-ID-OR-NAME \
    --resource-group YOUR-RESOURCE-GROUP \
    --name YOURRESOURCENAME \
    --location eastus \
    --admin-user YOUR-ADMIN-NAME \
    --ssl-enforcement Disabled \
    --enable-public-network Enabled 
```

Il completamento di questo comando può richiedere alcuni minuti e creare una risorsa disponibile pubblicamente nell' `eastus` area. 

La risposta include i dettagli di configurazione del server, tra cui: 
* password generata automaticamente per l'account amministratore
* comando per la connessione al server con l'interfaccia della riga di comando di MySQL

```text
{
  "additionalProperties": {},
  "administratorLogin": "mySqlAdmin",
  "byokEnforcement": "Disabled",
  "connectionString": "mysql defaultdb --host mysqldb-2.mysql.database.azure.com --user mySqlAdmin@mysqldb-2 --password=123456789",
  "databaseName": "defaultdb",
  "earliestRestoreDate": "2021-02-08T16:48:01.673000+00:00",
  "fullyQualifiedDomainName": "mysqldb-2.mysql.database.azure.com",
  "id": "/subscriptions/.../resourceGroups/my-resource-group/providers/Microsoft.DBforMySQL/servers/mysqldb-2",
  "identity": null,
  "infrastructureEncryption": "Disabled",
  "location": "westus",
  "masterServerId": "",
  "minimalTlsVersion": "TLSEnforcementDisabled",
  "name": "mysqldb-2",
  "password": "123456789",
  "privateEndpointConnections": [],
  "publicNetworkAccess": "Enabled",
  "replicaCapacity": 5,
  "replicationRole": "None",
  "resourceGroup": "my-resource-group",
  "sku": {
    "additionalProperties": {},
    "capacity": 2,
    "family": "Gen5",
    "name": "GP_Gen5_2",
    "size": null,
    "tier": "GeneralPurpose"
  },
  "sslEnforcement": "Disabled",
  "storageProfile": {
    "additionalProperties": {},
    "backupRetentionDays": 7,
    "geoRedundantBackup": "Disabled",
    "storageAutogrow": "Enabled",
    "storageMb": 51200
  },
  "tags": null,
  "type": "Microsoft.DBforMySQL/servers",
  "userVisibleState": "Ready",
  "version": "5.7"
}
```

Prima di potersi connettere al server a livello di codice, è necessario configurare le regole del firewall per consentire l'indirizzo IP del client tramite. 

## <a name="add-firewall-rule-for-your-client-ip-address-to-mysql-resource"></a>Aggiungere una regola del firewall per l'indirizzo IP del client alla risorsa MySQL

Per impostazione predefinita, le regole del firewall non sono configurate. È necessario aggiungere l'indirizzo IP del client in modo che la connessione client al server con JavaScript abbia esito positivo.

```azurecli
az mysql server firewall-rule create \
--resource-group YOUR-RESOURCE-GROUP \
--server YOURRESOURCENAME \
--name AllowMyIP --start-ip-address 123.123.123.123 \
--end-ip-address 123.123.123.123
```

Se non si conosce l'indirizzo IP del client, usare uno dei metodi seguenti:
* Usare il portale di Azure per visualizzare e modificare le regole del firewall, inclusa l'aggiunta dell'IP client rilevato
* Eseguire Node.js codice, l'errore relativo alla violazione delle regole del firewall include l'indirizzo IP del client

Sebbene sia possibile aggiungere l'intera gamma di indirizzi Internet come una regola del firewall, 0.0.0.0-255.255.255.255, in questo modo il server rimane aperto agli attacchi. 

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