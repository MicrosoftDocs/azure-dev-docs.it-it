---
title: Effettuare il provisioning di un database MySQL di Azure usando le librerie di Azure SDK
description: Usare le librerie di gestione incluse nelle librerie di Azure SDK per Python per effettuare il provisioning di un database MySQL di Azure, PostgreSQL o MariaDB.
ms.date: 10/05/2020
ms.topic: conceptual
ms.custom: devx-track-python, devx-track-azurecli
ms.openlocfilehash: 873b854ac2702ac62484a8ed37a5367084eb4b00
ms.sourcegitcommit: 1ddcb0f24d2ae3d1f813ec0f4369865a1c6ef322
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/27/2020
ms.locfileid: "92689017"
---
# <a name="example-use-the-azure-libraries-to-provision-a-database"></a>Esempio: Usare le librerie di Azure per effettuare il provisioning di un database

Questo esempio illustra come usare le librerie di gestione di Azure SDK in uno script Python per effettuare il provisioning di un database MySQL di Azure. Fornisce anche uno script semplice per eseguire query sul database usando la libreria mysql-connector (non inclusa in Azure SDK). I [comandi equivalenti dell'interfaccia della riga di comando di Azure](#for-reference-equivalent-azure-cli-commands) vengono descritti più avanti in questo articolo. Se si preferisce usare il portale di Azure, vedere [Creare un server PostgreSQL](/azure/postgresql/quickstart-create-server-database-portal) o [Creare un server MariaDB](/azure/mariadb/quickstart-create-mariadb-server-database-using-azure-portal).

È possibile usare codice simile per effettuare il provisioning di un database PostgreSQL o MariaDB.

Se non diversamente specificato, tutti i comandi di questo articolo funzionano allo stesso modo nella shell Bash Linux/macOS e nella shell dei comandi di Windows.

## <a name="1-set-up-your-local-development-environment"></a>1: Configurare un ambiente di sviluppo locale

Se non è già stato fatto, seguire tutte le istruzioni riportate in [Configurare l'ambiente di sviluppo Python locale per Azure](configure-local-development-environment.md).

Assicurarsi di creare un'entità servizio per lo sviluppo locale e di creare e attivare un ambiente virtuale per questo progetto.

## <a name="2-install-the-needed-azure-library-packages"></a>2: Installare i pacchetti delle librerie di Azure necessari

Creare un file denominato *requirements.txt* con il contenuto seguente:

```text
azure-mgmt-resource==10.2.0
azure-mgmt-rdbms
azure-cli-core
mysql
mysql-connector
```

Il requisito relativo alla versione specifica per azure-mgmt-resource assicura che verrà usata una versione compatibile con la versione corrente di azure-mgmt-web. Queste versioni non sono basate su azure.core e quindi usano metodi meno recenti per l'autenticazione.

In un terminale o da un prompt dei comandi con l'ambiente virtuale attivato, installare i requisiti:

```cmd
pip install -r requirements.txt
```

> [!NOTE]
> In Windows il tentativo di installare la libreria mysql in una libreria Python a 32 bit genera un errore relativo al file *mysql.h* . In questo caso, installare una versione a 64 bit di Python e riprovare.

## <a name="3-write-code-to-provision-the-database"></a>3: Scrivere codice per effettuare il provisioning del database

Creare un file Python denominato *provision_db.py* con il codice seguente. I commenti spiegano i dettagli.

```python
import random, os
from azure.common.client_factory import get_client_from_cli_profile
from azure.mgmt.resource import ResourceManagementClient

# For PostgreSQL, use the PostgreSQLManagement class.
# For MariaDB, use the MariaDBManagement class.
from azure.mgmt.rdbms.mysql import MySQLManagementClient

from azure.mgmt.rdbms.mysql.models import ServerForCreate, ServerPropertiesForDefaultCreate, ServerVersion

# Constants we need in multiple places: the resource group name and the region
# in which we provision resources. You can change these values however you want.
RESOURCE_GROUP_NAME = 'PythonAzureExample-DB-rg'
LOCATION = "westus"

# Step 1: Provision the resource group.
resource_client = get_client_from_cli_profile(ResourceManagementClient)

rg_result = resource_client.resource_groups.create_or_update(RESOURCE_GROUP_NAME,
    { "location": LOCATION })

print(f"Provisioned resource group {rg_result.name}")

# For details on the previous code, see Example: Provision a resource group
# at https://docs.microsoft.com/azure/developer/python/azure-sdk-example-resource-group


# Step 2: Provision the database server

# We use a random number to create a reasonably unique database server name.
# If you've already provisioned a database and need to re-run the script, set
# the DB_SERVER_NAME environment variable to that name instead.
#
# Also set DB_USER_NAME and DB_USER_PASSWORD variables to avoid using the defaults.

db_server_name = os.environ.get("DB_SERVER_NAME", f"PythonAzureExample-MySQL-{random.randint(1,100000):05}")
db_admin_name = os.environ.get("DB_ADMIN_NAME", "azureuser")
db_admin_password = os.environ.get("DB_ADMIN_PASSWORD", "ChangePa$$w0rd24")

# Obtain the management client object
mysql_client = get_client_from_cli_profile(MySQLManagementClient)

# Provision the server and wait for the result
poller = mysql_client.servers.create(RESOURCE_GROUP_NAME,
    db_server_name,
    ServerForCreate(
        location=LOCATION,
        properties=ServerPropertiesForDefaultCreate(
            administrator_login=db_admin_name,
            administrator_login_password=db_admin_password,
            version=ServerVersion.five_full_stop_seven
        )
    )
)

server = poller.result()

print(f"Provisioned MySQL server {server.name}")

# Step 3: Provision a firewall rule to allow the local workstation to connect

RULE_NAME = "allow_ip"
ip_address = os.environ["PUBLIC_IP_ADDRESS"]

# For the above code, create an environment variable named PUBLIC_IP_ADDRESS that
# contains your workstation's public IP address as reported by a site like
# https://whatismyipaddress.com/.

# Provision the rule and wait for completion
poller = mysql_client.firewall_rules.create_or_update(RESOURCE_GROUP_NAME,
    db_server_name, RULE_NAME,
    ip_address,  # Start ip range
    ip_address   # End ip range
)

firewall_rule = poller.result()

print(f"Provisioned firewall rule {firewall_rule.name}")

# Step 4: Provision a database on the server

DB_NAME = "example-db1"

poller = mysql_client.databases.create_or_update(RESOURCE_GROUP_NAME,
    db_server_name, DB_NAME)

db_result = poller.result()

print(f"Provisioned MySQL database {db_result.name} with ID {db_result.id}")
```

Per eseguire questo esempio, è necessario creare una variabile di ambiente denominata `PUBLIC_IP_ADDRESS` con l'indirizzo IP della workstation.

Questo codice usa i metodi di autenticazione basati sull'interfaccia della riga di comando (`get_client_from_cli_profile`) perché illustra azioni che altrimenti si potrebbero eseguire direttamente con l'interfaccia della riga di comando di Azure. In entrambi i casi si usa la stessa identità per l'autenticazione.

Per usare tale codice in uno script di produzione, è preferibile usare invece `DefaultAzureCredential` (scelta consigliata) o un metodo basato su entità servizio, come descritto in [Come autenticare le app Python con i servizi di Azure](azure-sdk-authenticate.md).

### <a name="reference-links-for-classes-used-in-the-code"></a>Collegamenti di riferimento per le classi usate nel codice

- [ResourceManagementClient (azure.mgmt.resource)](/python/api/azure-mgmt-resource/azure.mgmt.resource.resourcemanagementclient)
- [MySQLManagementClient (azure.mgmt.rdbms.mysql)](/python/api/azure-mgmt-rdbms/azure.mgmt.rdbms.mysql.mysqlmanagementclient)
- [ServerForCreate (azure.mgmt.rdbms.mysql.models)](/python/api/azure-mgmt-rdbms/azure.mgmt.rdbms.mysql.models.serverforcreate)
- [ServerPropertiesForDefaultCreate (azure.mgmt.rdbms.mysql.models)](/python/api/azure-mgmt-rdbms/azure.mgmt.rdbms.mysql.models.serverpropertiesfordefaultcreate)
- [ServerVersion (azure.mgmt.rdbms.mysql.models)](/python/api/azure-mgmt-rdbms/azure.mgmt.rdbms.mysql.models.serverversion)

Vedere anche:
    - [PostgreSQLManagementClient (azure.mgmt.rdbms.postgresql)](/python/api/azure-mgmt-rdbms/azure.mgmt.rdbms.postgresql.postgresqlmanagementclient)
    - [MariaDBManagementClient (azure.mgmt.rdbms.mariadb)](/python/api/azure-mgmt-rdbms/azure.mgmt.rdbms.mariadb.mariadbmanagementclient)

## <a name="4-run-the-script"></a>4: Eseguire lo script

```cmd
python provision_db.py
```

## <a name="5-insert-a-record-and-query-the-database"></a>5: Inserire un record ed eseguire query sul database

Creare un file denominato *use_db.py* con il codice seguente. Prendere nota delle dipendenze dalle variabili di ambiente `DB_SERVER_NAME`, `DB_ADMIN_NAME` e `DB_ADMIN_PASSWORD`, che devono essere popolate con i valori del codice di provisioning.

Questo codice funziona solo per MySQL; per PostgreSQL e MariaDB si usano librerie diverse.

```python
import os
import mysql.connector

db_server_name = os.environ["DB_SERVER_NAME"]
db_admin_name = os.environ.get("DB_ADMIN_NAME", "azureuser")
db_admin_password = os.environ.get("DB_ADMIN_PASSWORD", "ChangePa$$w0rd24")

DB_NAME = "example-db"

connection = mysql.connector.connect(user=f"{db_admin_name}@{db_server_name}",
    password=db_admin_password, host=f"{db_server_name}.mysql.database.azure.com", port=3306,
    database=DB_NAME)

cursor = connection.cursor()

table_name = "ExampleTable1"

sql_create = f"CREATE TABLE {table_name} (name varchar(255), code int)"

cursor.execute(sql_create)
print(f"Successfully created table {table_name}")

sql_insert = f"INSERT INTO {table_name} (name, code) VALUES ('Azure', 1)"
insert_data = "('Azure', 1)"

cursor.execute(sql_insert)
print("Successfully inserted data into table")

sql_select_values= f"SELECT * FROM {table_name}"

cursor.execute(sql_select_values)
row = cursor.fetchone()

while row:
    print(str(row[0]) + " " + str(row[1]))
    row = cursor.fetchone()

connection.commit()
```

Tutto questo codice usa l'API mysql.connector. L'unica parte specifica di Azure è il dominio host completo per il server MySQL (mysql.database.azure.com).

Quindi eseguire il codice:

```cmd
python use_db.py
```

## <a name="6-clean-up-resources"></a>6: Pulire le risorse

```azurecli
az group delete -n PythonAzureExample-DB-rg  --no-wait
```

Se non è necessario mantenere le risorse di cui è stato effettuato il provisioning in questo esempio, eseguire questo comando per evitare addebiti ricorrenti nella sottoscrizione.

[!INCLUDE [resource_group_begin_delete](includes/resource-group-begin-delete.md)]

### <a name="for-reference-equivalent-azure-cli-commands"></a>Per riferimento: comandi equivalenti dell'interfaccia della riga di comando di Azure

I seguenti comandi dell'interfaccia della riga di comando di Azure completano gli stessi passaggi di provisioning dello script Python. Per un database PostgreSQL, usare i comandi [`az postgres`](/cli/azure/postgres); per MariaDB, usare i comandi [`az mariadb`](/cli/azure/mariadb).

# <a name="cmd"></a>[cmd](#tab/cmd)

```azurecli
az group create -l centralus -n PythonAzureExample-DB-rg

az mysql server create -l westus -g PythonAzureExample-DB-rg -n PythonAzureExample-MySQL-12345 ^
    -u azureuser -p ChangePa$$w0rd24 --sku-name B_Gen5_1

# Change the IP address to the public IP address of your workstation, that is, the address shown
# by a site like https://whatismyipaddress.com/. 

az mysql server firewall-rule create -g PythonAzureExample-DB-rg --server PythonAzureExample-MySQL-12345 ^
    -n allow_ip --start-ip-address 10.11.12.13 --end-ip-address 10.11.12.13

az mysql db create -g PythonAzureExample-DB-rg --server PythonAzureExample-MySQL-12345 -n example-db
```

# <a name="bash"></a>[Bash](#tab/bash)

```azurecli
az group create -l centralus -n PythonAzureExample-DB-rg

az mysql server create -l westus -g PythonAzureExample-DB-rg -n PythonAzureExample-MySQL-12345 \
    -u azureuser -p ChangePa$$w0rd24 --sku-name B_Gen5_1

# Change the IP address to the public IP address of your workstation, that is, the address shown
# by a site like https://whatismyipaddress.com/. 

az mysql server firewall-rule create -g PythonAzureExample-DB-rg --server PythonAzureExample-MySQL-12345 \
    -n allow_ip --start-ip-address 10.11.12.13 --end-ip-address 10.11.12.13

az mysql db create -g PythonAzureExample-DB-rg --server PythonAzureExample-MySQL-12345 -n example-db
```

---

## <a name="see-also"></a>Vedere anche

- [Esempio: Effettuare il provisioning di un gruppo di risorse](azure-sdk-example-resource-group.md)
- [Esempio: Elencare i gruppi di risorse in una sottoscrizione](azure-sdk-example-list-resource-groups.md)
- [Esempio: Effettuare il provisioning di Archiviazione di Azure](azure-sdk-example-storage.md)
- [Esempio: Usare Archiviazione di Azure](azure-sdk-example-storage-use.md)
- [Esempio: Effettuare il provisioning di una macchina virtuale](azure-sdk-example-virtual-machines.md)
- [Esempio: Effettuare il provisioning e la distribuzione di un'app Web](azure-sdk-example-web-app.md)
