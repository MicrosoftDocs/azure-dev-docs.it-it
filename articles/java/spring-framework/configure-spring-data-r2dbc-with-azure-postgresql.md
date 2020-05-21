---
title: Usare Spring Data R2DBC con Database di Azure per PostgreSQL
description: Informazioni su come usare Spring Data R2DBC con un'istanza di Database di Azure per PostgreSQL.
documentationcenter: java
ms.date: 03/18/2020
ms.service: postgresql
ms.tgt_pltfrm: multiple
ms.author: judubois
ms.topic: article
ms.openlocfilehash: 154997506c15b84c7e0110fa2e45645bfd1585a2
ms.sourcegitcommit: fbbc341a0b9e17da305bd877027b779f5b0694cc
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/19/2020
ms.locfileid: "83631660"
---
# <a name="use-spring-data-r2dbc-with-azure-database-for-postgresql"></a>Usare Spring Data R2DBC con Database di Azure per PostgreSQL

Questo argomento illustra come creare un'applicazione di esempio che usa [Spring Data R2DBC](https://spring.io/projects/spring-data-r2dbc) per archiviare e recuperare informazioni in [Database di Azure per PostgreSQL](https://docs.microsoft.com/azure/postgresql/) con l'implementazione R2DBC per PostgreSQL del [repository GitHub r2dbc-postgresql](https://github.com/r2dbc/r2dbc-postgresql).

[R2DBC](https://r2dbc.io/) introduce le API reattive nei database relazionali tradizionali. È possibile usarlo con Spring WebFlux per creare applicazioni Spring Boot completamente reattive che usano API non bloccanti. Offre una migliore scalabilità rispetto all'approccio classico di "un thread per connessione".

[!INCLUDE [spring-data-prerequisites.md](includes/spring-data-prerequisites.md)]

## <a name="prepare-the-working-environment"></a>Preparare l'ambiente di lavoro

Prima di tutto, configurare alcune variabili di ambiente usando i comandi seguenti:

```bash
AZ_RESOURCE_GROUP=r2dbc-workshop
AZ_DATABASE_NAME=<YOUR_DATABASE_NAME>
AZ_LOCATION=<YOUR_AZURE_REGION>
AZ_POSTGRESQL_USERNAME=spring
AZ_POSTGRESQL_PASSWORD=<YOUR_POSTGRESQL_PASSWORD>
AZ_LOCAL_IP_ADDRESS=<YOUR_LOCAL_IP_ADDRESS>
```

Sostituire i segnaposto con i valori seguenti, che vengono usati nell'intero articolo:

- `<YOUR_DATABASE_NAME>`: il nome del server PostgreSQL. Deve essere univoco in Azure.
- `<YOUR_AZURE_REGION>`: l'area di Azure da usare. È possibile usare `eastus` per impostazione predefinita, ma è consigliabile configurare un'area più vicina a dove si risiede. Per l'elenco completo di aree disponibili, immettere `az account list-locations`.
- `<YOUR_POSTGRESQL_PASSWORD>`: la password del server di database PostgreSQL. La password deve essere composta da un minimo di otto caratteri di tre categorie seguenti: lettere maiuscole, lettere minuscole, numeri (0-9) e caratteri non alfanumerici (!, $, #, % e così via).
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

## <a name="create-an-azure-database-for-postgresql-instance"></a>Creare un'istanza di Database di Azure per PostgreSQL

Il primo componente che verrà creato è un server PostgreSQL gestito.

> [!NOTE]
> Per informazioni più dettagliate sulla creazione di server PostgreSQL, vedere [Creare un server di Database di Azure per PostgreSQL nel portale di Azure](/azure/postgresql/quickstart-create-server-database-portal).

In [Azure Cloud Shell](https://shell.azure.com/) eseguire lo script seguente:

```azurecli
az postgres server create \
    --resource-group $AZ_RESOURCE_GROUP \
    --name $AZ_DATABASE_NAME \
    --location $AZ_LOCATION \
    --sku-name B_Gen5_1 \
    --storage-size 5120 \
    --admin-user $AZ_POSTGRESQL_USERNAME \
    --admin-password $AZ_POSTGRESQL_PASSWORD \
    | jq
```

Questo comando crea un piccolo server PostgreSQL.

### <a name="configure-a-firewall-rule-for-your-postgresql-server"></a>Configurare una regola del firewall per il server PostgreSQL

Le istanze di Database di Azure per PostgreSQL sono protette per impostazione predefinita. Includono un firewall che non consente alcuna connessione in ingresso. Per poter usare il database, è necessario aggiungere una regola del firewall che consenta all'indirizzo IP locale di accedere al server di database.

Poiché l'indirizzo IP locale è stato configurato all'inizio di questo articolo, è possibile aprire il firewall del server eseguendo:

```azurecli
az postgres server firewall-rule create \
    --resource-group $AZ_RESOURCE_GROUP \
    --name $AZ_DATABASE_NAME-database-allow-local-ip \
    --server $AZ_DATABASE_NAME \
    --start-ip-address $AZ_LOCAL_IP_ADDRESS \
    --end-ip-address $AZ_LOCAL_IP_ADDRESS \
    | jq
```

### <a name="configure-a-postgresql-database"></a>Configurare un database PostgreSQL

Il server PostgreSQL creato in precedenza è vuoto. Non include nessun database che è possibile usare con l'applicazione Spring Boot. Creare un nuovo database denominato `demo`:

```azurecli
az postgres db create \
    --resource-group $AZ_RESOURCE_GROUP \
    --name demo \
    --server-name $AZ_DATABASE_NAME \
    | jq
```

[!INCLUDE [spring-data-create-reactive.md](includes/spring-data-create-reactive.md)]

### <a name="generate-the-application-by-using-spring-initializr"></a>Generare l'applicazione con Spring Initializr

Per generare l'applicazione, immettere quanto segue sulla riga di comando:

```bash
curl https://start.spring.io/starter.tgz -d dependencies=webflux,data-r2dbc -d baseDir=azure-database-workshop -d bootVersion=2.3.0.RELEASE -d javaVersion=8 | tar -xzvf -
```

### <a name="add-the-reactive-postgresql-driver-implementation"></a>Aggiungere l'implementazione del driver PostgreSQL reattivo

Aprire il file *pom.xml* del progetto generato per aggiungere il driver PostgreSQL reattivo dal [repository GitHub r2dbc-postgresql](https://github.com/r2dbc/r2dbc-postgresql).

Dopo la dipendenza `spring-boot-starter-webflux` aggiungere il frammento di codice seguente:

```xml
<dependency>
    <groupId>io.r2dbc</groupId>
    <artifactId>r2dbc-postgresql</artifactId>
    <scope>runtime</scope>
</dependency>
```

### <a name="configure-spring-boot-to-use-azure-database-for-postgresql"></a>Configurare Spring Boot per l'uso di Database di Azure per PostgreSQL

Aprire il file *src/main/resources/application.properties* e aggiungere:

```properties
logging.level.org.springframework.data.r2dbc=DEBUG

spring.r2dbc.url=r2dbc:pool:postgres://$AZ_DATABASE_NAME.postgres.database.azure.com:5432/demo
spring.r2dbc.username=spring@$AZ_DATABASE_NAME
spring.r2dbc.password=$AZ_POSTGRESQL_PASSWORD
spring.r2dbc.properties.sslMode=REQUIRE
```

> [!WARNING]
> Per motivi di sicurezza, con Database di Azure per PostgreSQL è necessario usare connessioni SSL. Questo è il motivo per cui è necessario aggiungere la proprietà di configurazione `spring.r2dbc.properties.sslMode=REQUIRE`. In caso contrario, il driver PostgreSQL R2DBC proverà, senza riuscirci, a connettersi usando una connessione non sicura.

- Sostituire le due variabili `$AZ_DATABASE_NAME` con il valore configurato all'inizio di questo articolo.
- Sostituire la variabile `$AZ_POSTGRESQL_PASSWORD` con il valore configurato all'inizio di questo articolo.

> [!NOTE]
> Per prestazioni più elevate, la proprietà `spring.r2dbc.url` viene configurata per l'uso di un pool di connessioni tramite [r2dbc-pool](https://github.com/r2dbc/r2dbc-pool).

A questo punto dovrebbe essere possibile avviare l'applicazione usando il wrapper Maven fornito:

```bash
./mvnw spring-boot:run
```

Ecco uno screenshot dell'applicazione in esecuzione per la prima volta:

[![Applicazione in esecuzione](media/configure-spring-data-r2dbc-with-azure-postgresql/create-postgresql-01.png)](media/configure-spring-data-r2dbc-with-azure-postgresql/create-postgresql-01.png#lightbox)

### <a name="create-the-database-schema"></a>Creare lo schema del database

[!INCLUDE [spring-data-r2dbc-create-schema.md](includes/spring-data-r2dbc-create-schema.md)]

```sql
DROP TABLE IF EXISTS todo;
CREATE TABLE todo (id SERIAL PRIMARY KEY, description VARCHAR(255), details VARCHAR(4096), done BOOLEAN);
```

Arrestare l'applicazione in esecuzione e quindi riavviarla. L'applicazione userà ora il database `demo` creato in precedenza e creerà una `todo` tabella al suo interno.

```bash
./mvnw spring-boot:run
```

Ecco uno screenshot della tabella di database creata:

[![Creazione della tabella di database](media/configure-spring-data-r2dbc-with-azure-postgresql/create-postgresql-02.png)](media/configure-spring-data-r2dbc-with-azure-postgresql/create-postgresql-02.png#lightbox)

## <a name="code-the-application"></a>Codice dell'applicazione

Aggiungere quindi il codice Java che userà R2DBC per archiviare e recuperare i dati dal server PostgreSQL.

[!INCLUDE [spring-data-r2dbc-create-application.md](includes/spring-data-r2dbc-create-application.md)]

Ecco uno screenshot di queste richieste cURL:

[![Eseguire il test con cURL](media/configure-spring-data-r2dbc-with-azure-postgresql/create-postgresql-03.png)](media/configure-spring-data-r2dbc-with-azure-postgresql/create-postgresql-03.png#lightbox)

Congratulazioni! È stata creata un'applicazione Spring Boot completamente reattiva che usa R2DBC per archiviare e recuperare i dati da Database di Azure per PostgreSQL.

[!INCLUDE [spring-data-conclusion.md](includes/spring-data-conclusion.md)]

### <a name="additional-resources"></a>Risorse aggiuntive

Per altre informazioni su Spring Data R2DBC, vedere la [documentazione di riferimento](https://docs.spring.io/spring-data/r2dbc/docs/current/reference/html/#reference) di Spring.

Per altre informazioni sull'uso di Azure con Java, vedere [Azure per sviluppatori Java](/azure/developer/java/) e la documentazione relativa all'[uso di Azure DevOps e Java](/azure/devops/).
