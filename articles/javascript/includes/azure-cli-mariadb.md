---
ms.custom: devx-track-js
ms.topic: include
ms.date: 02/08/2021
ms.openlocfilehash: 0c441254e2c053198cc8e41e0d2457adbf3079af
ms.sourcegitcommit: 98a7e855206ff463c1d95f93c23dd665b26a0aa1
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/09/2021
ms.locfileid: "100004749"
---
## <a name="create-a-mariadb-resource-with-azure-cli"></a>Creare una risorsa MariaDB con l'interfaccia della riga di comando Azure

Usare l'interfaccia della riga di comando di Azure [AZ mariadb server create](/cli/azure/mariadb/server#az_mariadb_server_create) nel [Azure cloud Shell](https://shell.azure.com) per creare una nuova risorsa MariaDB per il database. 

```azurecli
az mariadb server create \
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
  "administratorLogin": "mariaAdmin",
  "connectionString": "mysql defaultdb --host mariadb-2.mariadb.database.azure.com --user mariaAdmin@mariadb-2 --password=123456789",
  "databaseName": "defaultdb",
  "earliestRestoreDate": "2021-02-08T19:14:17.317000+00:00",
  "fullyQualifiedDomainName": "mariadb-2.mariadb.database.azure.com",
  "id": "/subscriptions/bb881e62-cf77-4d5d-89fb-29d71e930b66/resourceGroups/my-resource-group/providers/Microsoft.DBforMariaDB/servers/mariadb-2",
  "location": "westus",
  "masterServerId": "",
  "name": "mariadb-2",
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
  "type": "Microsoft.DBforMariaDB/servers",
  "userVisibleState": "Ready",
  "version": "10.2"
}
```

Prima di potersi connettere al server a livello di codice, è necessario configurare le regole del firewall per consentire l'indirizzo IP del client tramite. 

## <a name="add-firewall-rule-for-your-client-ip-address-to-mariadb-resource"></a>Aggiungere una regola del firewall per l'indirizzo IP del client alla risorsa MariaDB

Per impostazione predefinita, le regole del firewall non sono configurate. È necessario aggiungere l'indirizzo IP del client in modo che la connessione client al server con JavaScript abbia esito positivo.

```azurecli
az mariadb server firewall-rule create \
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

Usare il comando [AZ MariaDB DB Create](/cli/azure/mariadb/db#az_mariadb_db_create) dell'interfaccia della riga di comando di Azure seguente nel [Azure cloud Shell](https://shell.azure.com) per creare un nuovo database di MariaDB nel server. 

```azurecli
az mariadb db create \
    --subscription YOUR-SUBSCRIPTION-ID-OR-NAME \
    --resource-group YOUR-RESOURCE-GROUP \
    --server-name YOURRESOURCENAME \
    --name YOURDATABASENAME
```

## <a name="get-the-mariadb-connection-string-with-azure-cli"></a>Ottenere la stringa di connessione MariaDB con l'interfaccia della riga di comando Azure

Recuperare la stringa di connessione MariaDB per questa istanza con il comando [AZ mariadb server show-Connection-String](/cli/azure/mariadb/server#az_mariadb_server_show_connection_string) :

```azurecli
az mariadb server show-connection-string \
    --subscription YOUR-SUBSCRIPTION-ID-OR-NAME \
    --server-name YOURRESOURCENAME \
    --database-name YOURDATABASENAME \
    --admin-user YOUR-ADMIN-NAME \
    --admin-password YOUR-ADMIN-PASSWORD 
```

Vengono restituite le stringhe di connessione per le lingue più diffuse come oggetto JSON:

```json
{
  "connectionStrings": {
    "ado.net": "Server=YOURRESOURCENAME.mariadb.database.azure.com; Port=3306; Database=YOURDATABASENAME; Uid=YOUR-ADMIN-NAME@YOURRESOURCENAME; Pwd=YOUR-ADMIN-PASSWORD",
    "jdbc": "jdbc:mariadb://YOURRESOURCENAME.mariadb.database.azure.com:3306/YOURDATABASENAME?user=YOUR-ADMIN-NAME@YOURRESOURCENAME&password=YOUR-ADMIN-PASSWORD",
    "node.js": "var conn = mysql.createConnection({host: 'YOURRESOURCENAME.mariadb.database.azure.com', user: 'YOUR-ADMIN-NAME@YOURRESOURCENAME',password: YOUR-ADMIN-PASSWORD, database: YOURDATABASENAME, port: 3306});",
    "php": "host=YOURRESOURCENAME.mariadb.database.azure.com port=3306 dbname=YOURDATABASENAME user=YOUR-ADMIN-NAME@YOURRESOURCENAME password=YOUR-ADMIN-PASSWORD",
    "python": "cnx = mysql.connector.connect(user='YOUR-ADMIN-NAME@YOURRESOURCENAME', password='YOUR-ADMIN-PASSWORD', host='YOURRESOURCENAME.mariadb.database.azure.com', port=3306, database='YOURDATABASENAME')",
    "ruby": "client = Mysql2::Client.new(username: 'YOUR-ADMIN-NAME@YOURRESOURCENAME', password: 'YOUR-ADMIN-PASSWORD', database: 'YOURDATABASENAME', host: 'YOURRESOURCENAME.mariadb.database.azure.com', port: 3306)"
  }
}
``` 




