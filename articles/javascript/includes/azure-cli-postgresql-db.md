---
ms.custom: devx-track-js
ms.topic: include
ms.date: 02/04/2021
ms.openlocfilehash: 1fe0e6ef7147040d1a5496f6492cc1e63e2e425e
ms.sourcegitcommit: 98a7e855206ff463c1d95f93c23dd665b26a0aa1
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/09/2021
ms.locfileid: "100006261"
---
## <a name="create-an-azure-database-for-postgresql-server-resource-with-azure-cli"></a>Creare una risorsa del server di database di Azure per PostgreSQL con l'interfaccia della riga di comando

Usare il comando [AZ Postgres server create](/cli/azure/postgres/server#az_postgres_server_create) dell'interfaccia della riga di comando di Azure seguente nel [Azure cloud Shell](https://shell.azure.com) per creare una nuova risorsa server PostgreSQL per il database. 

```azurecli
az postgres server create \
    --subscription YOUR-SUBSCRIPTION-ID-OR-NAME \
    --resource-group YOUR-RESOURCE-GROUP \
    --location eastus \
    --name YOURRESOURCENAME \
    --admin-user YOUR-ADMIN-USER \
    --ssl-enforcement Disabled \
    --enable-public-network Enabled 
```

Il completamento di questo comando può richiedere alcuni minuti e creare una risorsa disponibile pubblicamente nell' `eastus` area. 

La risposta include i dettagli di configurazione del server, tra cui: 
* Password generata automaticamente per l'account amministratore
* Stringa di connessione

```text
{
  "additionalProperties": {},
  "administratorLogin": "YOUR-ADMIN-USER",
  "byokEnforcement": "Disabled",
  "connectionString": "postgres://YOUR-ADMIN-USER%40YOURRESOURCENAME:123456789@YOURRESOURCENAME.postgres.database.azure.com/postgres?sslmode=require",
  "earliestRestoreDate": "2021-02-08T21:56:20.130000+00:00",
  "fullyQualifiedDomainName": "YOURRESOURCENAME.postgres.database.azure.com",
  "id": "/subscriptions/.../resourceGroups/YOUR-RESOURCE-GROUP/providers/Microsoft.DBforPostgreSQL/servers/YOURRESOURCENAME",
  "identity": null,
  "infrastructureEncryption": "Disabled",
  "location": "eastus",
  "masterServerId": "",
  "minimalTlsVersion": "TLSEnforcementDisabled",
  "name": "YOURRESOURCENAME",
  "password": "123456789",
  "privateEndpointConnections": [],
  "publicNetworkAccess": "Enabled",
  "replicaCapacity": 5,
  "replicationRole": "None",
  "resourceGroup": "YOUR-RESOURCE-GROUP",
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
  "type": "Microsoft.DBforPostgreSQL/servers",
  "userVisibleState": "Ready",
  "version": "9.6"
}
```

Prima di potersi connettere al server, è necessario configurare le regole del firewall per consentire l'indirizzo IP del client tramite. 

## <a name="add-a-firewall-rule-for-your-client-ip-address-to-postgresql-resource"></a>Aggiungere una regola del firewall per l'indirizzo IP del client alla risorsa PostgreSQL

Per impostazione predefinita, le regole del firewall non sono configurate. È necessario aggiungere l'indirizzo IP del client in modo che la connessione client al server con JavaScript abbia esito positivo.

Usare l'interfaccia della riga di comando di Azure [AZ Postgres server firewall-rule create](/cli/azure/postgres/server#az_postgres_server_firewall_rule_create) nel [Azure cloud Shell](https://shell.azure.com) per creare una nuova regola del firewall per il database. 


```azurecli
az postgres server firewall-rule create \
    --subscription YOUR-SUBSCRIPTION-ID-OR-NAME \
    --resource-group YOUR-RESOURCE-GROUP \
    --server YOURRESOURCENAME \
    --name AllowMyIP \
    --start-ip-address 123.123.123.123 \
    --end-ip-address 123.123.123.123
```

Se non si conosce l'indirizzo IP del client, usare uno dei metodi seguenti:
* Usare il portale di Azure per visualizzare e modificare le regole del firewall, inclusa l'aggiunta dell'IP client rilevato
* Eseguire il codice di Node.js, l'errore relativo alla violazione delle regole del firewall include l'indirizzo IP del client

Sebbene sia possibile aggiungere l'intera gamma di indirizzi Internet come una regola del firewall, 0.0.0.0-255.255.255.255, in questo modo il server rimane aperto agli attacchi. 

## <a name="create-a-postgresql-database-on-the-server-with-azure-cli"></a>Creare un database PostgreSQL nel server con l'interfaccia della riga di comando di Azure

Usare l'interfaccia della riga di comando di Azure [AZ Postgres DB Create](/cli/azure/postgres/db#az_postgres_db_create) nel [Azure cloud Shell](https://shell.azure.com) per creare un nuovo database PostgreSQL nel server. 

```azurecli
az postgres db create \
    --subscription YOUR-SUBSCRIPTION-ID-OR-NAME \
    --resource-group YOUR-RESOURCE-GROUP \
    --server-name YOURRESOURCENAME \
    --name YOURDATABASENAME
```

## <a name="get-the-postgresql-connection-string-with-azure-cli"></a>Ottenere la stringa di connessione PostgreSQL con l'interfaccia della riga di comando Azure

Recuperare la stringa di connessione PostgreSQL per questa istanza con il comando [AZ Postgres server show-Connection-String](/cli/azure/postgres/server#az_postgres_server_show_connection_string) :

```azurecli
az postgres server show-connection-string \
    --subscription YOUR-SUBSCRIPTION-ID-OR-NAME \
    --server-name YOURRESOURCENAME
```

Vengono restituite le stringhe di connessione per le lingue più diffuse come oggetto JSON. Prima di usare la stringa di connessione, è necessario sostituire `{database}` , `{username}` e con i `{password}` propri valori. Sostituire `YOURRESOURCENAME` con il nome della risorsa.

```json
{
  "connectionStrings": {
    "C++ (libpq)": "host=YOURRESOURCENAME.postgres.database.azure.com port=5432 dbname={database} user={username}YOURRESOURCENAME password={password} sslmode=require",
    "ado.net": "Server=YOURRESOURCENAME.postgres.database.azure.com;Database={database};Port=5432;User Id={username}@YOURRESOURCENAME;Password={password};",
    "jdbc": "jdbc:postgresql://YOURRESOURCENAME.postgres.database.azure.com:5432/{database}?user={username}@YOURRESOURCENAME&password={password}",
    "node.js": "var client = new pg.Client('postgres://{username}@YOURRESOURCENAME:{password}@YOURRESOURCENAME.postgres.database.azure.com:5432/{database}');",
    "php": "host=YOURRESOURCENAME.postgres.database.azure.com port=5432 dbname={database} user={username}@YOURRESOURCENAME password={password}",
    "psql_cmd": "postgresql://{username}@YOURRESOURCENAME:{password}@YOURRESOURCENAME.postgres.database.azure.com/{database}?sslmode=require",
    "python": "cnx = psycopg2.connect(database='{database}', user='{username}@YOURRESOURCENAME', host='YOURRESOURCENAME.postgres.database.azure.com', password='{password}', port='5432')",
    "ruby": "cnx = PG::Connection.new(:host => 'YOURRESOURCENAME.postgres.database.azure.com', :user => '{username}@YOURRESOURCENAME', :dbname => '{database}', :port => '5432', :password => '{password}')"
  }
}
``` 