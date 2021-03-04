---
ms.custom: devx-track-js
ms.topic: include
ms.date: 02/18/2021
ms.openlocfilehash: 079ffe7f07b6038a1f6e320ca37eed3aafb41ec2
ms.sourcegitcommit: 576c878c338d286060010646b96f3ad0fdbcb814
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/04/2021
ms.locfileid: "102118632"
---
## <a name="create-an-azure-cache-for-redis-resource-with-azure-cli"></a>Creare una cache di Azure per la risorsa Redis con l'interfaccia della riga di comando

Usare il comando [AZ Redis create](/cli/azure/redis#az_redis_create) dell'interfaccia della riga di comando di Azure seguente nel [Azure cloud Shell](https://shell.azure.com) per creare una nuova risorsa MariaDB per il database. 

```azurecli
az redis create \
    --subscription YOUR-SUBSCRIPTION-ID-OR-NAME \
    --resource-group YOUR-RESOURCE-GROUP \
    --name YOUR-RESOURCE-NAME \
    --location eastus \
    --sku Basic \
    --vm-size c0 \
    --enable-non-ssl-port
```

Il completamento di questo comando può richiedere alcuni minuti e creare una risorsa disponibile pubblicamente nell' `eastus` area. 

La risposta include i dettagli di configurazione del server, tra cui: 
* la versione di redis: `redisVersion`
* porte: `sslPort` e `port`

```text
{
  "accessKeys": null,
  "enableNonSslPort": true,
  "hostName": "YOUR-RESOURCE-NAME.redis.cache.windows.net",
  "id": "/subscriptions/YOUR-SUBSCRIPTION-ID-OR-NAME/resourceGroups/YOUR-RESOURCE-GROUP/providers/Microsoft.Cache/Redis/YOUR-RESOURCE-NAME",
  "instances": [
    {
      "isMaster": false,
      "nonSslPort": 13000,
      "shardId": null,
      "sslPort": 15000,
      "zone": null
    }
  ],
  "linkedServers": [],
  "location": "East US",
  "minimumTlsVersion": null,
  "name": "YOUR-RESOURCE-NAME",
  "port": 6379,
  "provisioningState": "Creating",
  "redisConfiguration": {
    "maxclients": "256",
    "maxfragmentationmemory-reserved": "12",
    "maxmemory-delta": "2",
    "maxmemory-reserved": "2"
  },
  "redisVersion": "4.0.14",
  "replicasPerMaster": null,
  "resourceGroup": "YOUR-RESOURCE-GROUP",
  "shardCount": null,
  "sku": {
    "capacity": 0,
    "family": "C",
    "name": "Basic"
  },
  "sslPort": 6380,
  "staticIp": null,
  "subnetId": null,
  "tags": {},
  "tenantSettings": {},
  "type": "Microsoft.Cache/Redis",
  "zones": null
}
```

## <a name="add-firewall-rule-for-your-client-ip-address-to-redis-resource"></a>Aggiungere una regola del firewall per l'indirizzo IP del client alla risorsa Redis

Aggiungere regole del firewall con il comando [AZ Redis firewall-rules create](/cli/azure/redis/firewall-rules#az_redis_firewall_rules_create) per definire l'accesso alla cache dall'IP client o dall'indirizzo IP dell'app.

```azurecli
az redis firewall-rules create \
    --subscription YOUR-SUBSCRIPTION-ID-OR-NAME \
    --resource-group YOUR-RESOURCE-GROUP \
    --name YOUR-RESOURCE-NAME \
    --rule-name AllowMyIP \
    --start-ip 123.123.123.123 \
    --end-ip 123.123.123.123
```

Se non si conosce l'indirizzo IP del client, usare uno dei metodi seguenti:
* Usare il portale di Azure per visualizzare e modificare le regole del firewall, ad esempio l'aggiunta dell'IP client rilevato.
* Quando si esegue il codice di Node.js, viene visualizzato un errore relativo alla violazione delle regole del firewall che include l'indirizzo IP del client. Copiare l'indirizzo IP e usarlo per impostare la regola del firewall.

## <a name="get-the-redis-keys-with-azure-cli"></a>Ottenere le chiavi Redis con l'interfaccia della riga di comando di Azure

Recuperare la stringa di connessione Redis per questa istanza con il comando [AZ Redis list-keys](/cli/azure/redis#az_redis_list_keys) :

```azurecli
az redis list-keys \
    --subscription YOUR-SUBSCRIPTION-ID-OR-NAME \
    --resource-group YOUR-RESOURCE-GROUP \
    --name YOUR-RESOURCE-NAME
```

Vengono restituite le due chiavi:

```json
{
    "primaryKey": "5Uey0GHWtCs9yr1FMUMcu1Sv17AJA2QMqPm9CyNKjAA=",
    "secondaryKey": "DEPr+3zWbL6d5XwxPajAriXKgoSeCqraN8SLSoiMWhM="
  }
```

## <a name="connect-azure-cache-for-redis-to-your-app-service-web-app"></a>Connettere cache di Azure per Redis all'app Web del servizio app

Aggiungere le informazioni di connessione all'app Web del servizio app con il comando [AZ webapp config appSettings set](/cli/azure/webapp/config/appsettings#az_webapp_config_appsettings_set) .

```azurecli
az webapp config appsettings set \
--subscription YOUR-SUBSCRIPTION-ID-OR-NAME \
--resource-group YOUR-RESOURCE-GROUP \
--name YOUR-APP-SERVICE-RESOURCE-NAME \
--settings "REDIS_URL=YOUR-REDIS-HOST-NAME" "REDIS_PORT=YOUR-REDIS-PORT" "REDIS_KEY=YOUR-REDIS-KEY"
```

Sostituire le impostazioni seguenti nel codice precedente:

* -SUBSCRIPTION-ID-O-NAME
* GRUPPO DI RISORSE
* YOUR-APP-SERVICE-RESOURCE-NAME
* -REDIS-NOME-HOST
* -REDIS-PORT
* CHIAVE-REDIS
