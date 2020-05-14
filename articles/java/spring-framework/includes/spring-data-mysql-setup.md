---
author: judubois
ms.date: 05/06/2020
ms.author: judubois
ms.openlocfilehash: bdd2d2f1eacbe3dd5f12a236f691d32e7a29fb68
ms.sourcegitcommit: a631b36ec1277ee9397a860c597ffdd5495d88e7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/13/2020
ms.locfileid: "83369915"
---
## <a name="prepare-the-working-environment"></a>Preparare l'ambiente di lavoro

Prima di tutto, configurare alcune variabili di ambiente usando i comandi seguenti:

```bash
AZ_RESOURCE_GROUP=r2dbc-workshop
AZ_DATABASE_NAME=<YOUR_DATABASE_NAME>
AZ_LOCATION=<YOUR_AZURE_REGION>
AZ_MYSQL_USERNAME=spring
AZ_MYSQL_PASSWORD=<YOUR_MYSQL_PASSWORD>
AZ_LOCAL_IP_ADDRESS=<YOUR_LOCAL_IP_ADDRESS>
```

Sostituire i segnaposto con i valori seguenti, che vengono usati nell'intero articolo:

- `<YOUR_DATABASE_NAME>`: il nome del server MySQL. Deve essere univoco in Azure.
- `<YOUR_AZURE_REGION>`: l'area di Azure da usare. È possibile usare `eastus` per impostazione predefinita, ma è consigliabile configurare un'area più vicina a dove si risiede. Per l'elenco completo di aree disponibili, immettere `az account list-locations`.
- `<YOUR_MYSQL_PASSWORD>`: la password del server di database MySQL. La password deve essere composta da un minimo di otto caratteri di tre categorie seguenti: lettere maiuscole, lettere minuscole, numeri (0-9) e caratteri non alfanumerici (!, $, #, % e così via).
- `<YOUR_LOCAL_IP_ADDRESS>`: l'indirizzo IP del computer locale, da cui verrà eseguita l'applicazione Spring Boot. Un modo pratico per trovarlo è puntare il browser all'indirizzo [whatismyip.akamai.com](http://whatismyip.akamai.com/).

Successivamente, creare un gruppo di risorse:

```azurecli
az group create \
    --name $AZ_RESOURCE_GROUP \
    --location $AZ_LOCATION \
    | jq
```

> [!NOTE]
> Per visualizzare i dati JSON e renderli più leggibili, viene usata l'utilità `jq`, installata per impostazione predefinita in [Azure Cloud Shell](https://shell.azure.com/).
> Se non si preferisce usare questa utilità, è possibile rimuovere tranquillamente la parte `| jq` di tutti i comandi che verranno usati.

## <a name="create-an-azure-database-for-mysql-instance"></a>Creare un'istanza di Database di Azure per MySQL

Il primo componente che verrà creato è un server MySQL gestito.

> [!NOTE]
> Per informazioni più dettagliate sulla creazione di server MySQL, vedere [Creare un server di Database di Azure per MySQL nel portale di Azure](/azure/mysql/quickstart-create-mysql-server-database-using-azure-portal).

In [Azure Cloud Shell](https://shell.azure.com/) eseguire lo script seguente:

```azurecli
az mysql server create \
    --resource-group $AZ_RESOURCE_GROUP \
    --name $AZ_DATABASE_NAME \
    --location $AZ_LOCATION \
    --sku-name B_Gen5_1 \
    --storage-size 5120 \
    --admin-user $AZ_MYSQL_USERNAME \
    --admin-password $AZ_MYSQL_PASSWORD \
    | jq
```

Questo comando crea un piccolo server MySQL.

### <a name="configure-a-firewall-rule-for-your-mysql-server"></a>Configurare una regola del firewall per il server MySQL

Le istanze di Database di Azure per MySQL sono protette per impostazione predefinita. Includono un firewall che non consente alcuna connessione in ingresso. Per poter usare il database, è necessario aggiungere una regola del firewall che consenta all'indirizzo IP locale di accedere al server di database.

Poiché l'indirizzo IP locale è stato configurato all'inizio di questo articolo, è possibile aprire il firewall del server eseguendo:

```azurecli
az mysql server firewall-rule create \
    --resource-group $AZ_RESOURCE_GROUP \
    --name $AZ_DATABASE_NAME-database-allow-local-ip \
    --server $AZ_DATABASE_NAME \
    --start-ip-address $AZ_LOCAL_IP_ADDRESS \
    --end-ip-address $AZ_LOCAL_IP_ADDRESS \
    | jq
```

### <a name="configure-a-mysql-database"></a>Configurare un database MySQL

Il server MySQL creato in precedenza è vuoto. Non include nessun database che è possibile usare con l'applicazione Spring Boot. Creare un nuovo database denominato `demo`:

```azurecli
az mysql db create \
    --resource-group $AZ_RESOURCE_GROUP \
    --name demo \
    --server-name $AZ_DATABASE_NAME \
    | jq
```
