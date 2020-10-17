---
title: Usare Spring Data R2DBC con Database di Azure per PostgreSQL
description: Informazioni su come usare Spring Data R2DBC con un'istanza di Database di Azure per PostgreSQL.
documentationcenter: java
ms.date: 10/10/2020
ms.service: postgresql
ms.tgt_pltfrm: multiple
ms.author: judubois
ms.topic: article
ms.custom: devx-track-java
ms.openlocfilehash: b22b008c54536a2cf708b3574d77d95dba995027
ms.sourcegitcommit: f460914ac5843eb7392869a08e3a80af68ab227b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/13/2020
ms.locfileid: "92010077"
---
# <a name="use-spring-data-r2dbc-with-azure-database-for-postgresql"></a>Usare Spring Data R2DBC con Database di Azure per PostgreSQL

Questo argomento illustra la creazione di un'applicazione di esempio che usa [Spring Data R2DBC](https://spring.io/projects/spring-data-r2dbc) per archiviare e recuperare le informazioni in un database di [Database di Azure per PostgreSQL](/azure/postgresql/). L'esempio usa l'implementazione di R2DBC per PostgreSQL dal repository [r2dbc-postgresql](https://github.com/pgjdbc/r2dbc-postgresql) in GitHub.

[R2DBC](https://r2dbc.io/) introduce le API reattive nei database relazionali tradizionali. È possibile usarlo con Spring WebFlux per creare applicazioni Spring Boot completamente reattive che usano API non bloccanti. Offre una migliore scalabilità rispetto all'approccio classico di "un thread per connessione".

[!INCLUDE [spring-data-prerequisites.md](includes/spring-data-prerequisites.md)]

## <a name="sample-application"></a>Applicazione di esempio

In questo articolo si scriverà il codice per un'applicazione di esempio. Per procedere più rapidamente, è possibile usare l'applicazione già pronta, disponibile all'indirizzo [https://github.com/Azure-Samples/quickstart-spring-data-r2dbc-postgresql](https://github.com/Azure-Samples/quickstart-spring-data-r2dbc-postgresql).

[!INCLUDE [spring-data-postgresql-setup.md](includes/spring-data-postgresql-setup.md)]

[!INCLUDE [spring-data-create-reactive.md](includes/spring-data-create-reactive.md)]

### <a name="generate-the-application-by-using-spring-initializr"></a>Generare l'applicazione con Spring Initializr

Per generare l'applicazione, immettere il comando seguente sulla riga di comando:

```bash
curl https://start.spring.io/starter.tgz -d dependencies=webflux,data-r2dbc -d baseDir=azure-database-workshop -d bootVersion=2.3.4.RELEASE -d javaVersion=8 | tar -xzvf -
```

> [!NOTE]
> Spring Initializr usa Java 11 come versione predefinita. Per usare le utilità di avvio di Spring Boot descritte in questo argomento, è necessario selezionare invece Java 8.

### <a name="add-the-reactive-postgresql-driver-implementation"></a>Aggiungere l'implementazione del driver PostgreSQL reattivo

Aprire il file *pom.xml* del progetto generato, quindi aggiungere il driver PostgreSQL reattivo dal [repository r2dbc-postgresql in GitHub](https://github.com/pgjdbc/r2dbc-postgresql). Dopo la dipendenza `spring-boot-starter-webflux` aggiungere il testo seguente:

```xml
<dependency>
    <groupId>io.r2dbc</groupId>
    <artifactId>r2dbc-postgresql</artifactId>
    <scope>runtime</scope>
</dependency>
```

### <a name="configure-spring-boot-to-use-azure-database-for-postgresql"></a>Configurare Spring Boot per l'uso di Database di Azure per PostgreSQL

Aprire il file *src/main/resources/application.properties* e aggiungere il testo seguente:

```properties
logging.level.org.springframework.data.r2dbc=DEBUG

spring.r2dbc.url=r2dbc:pool:postgres://$AZ_DATABASE_NAME.postgres.database.azure.com:5432/demo
spring.r2dbc.username=spring@$AZ_DATABASE_NAME
spring.r2dbc.password=$AZ_POSTGRESQL_PASSWORD
spring.r2dbc.properties.sslMode=REQUIRE
```

> [!WARNING]
> Per motivi di sicurezza, con Database di Azure per PostgreSQL è necessario usare connessioni SSL. Questo è il motivo per cui è necessario aggiungere la proprietà di configurazione `spring.r2dbc.properties.sslMode=REQUIRE`. In caso contrario, il driver PostgreSQL R2DBC proverà, senza riuscirci, a connettersi usando una connessione non sicura.

Sostituire le due variabili `$AZ_DATABASE_NAME` e la variabile `$AZ_POSTGRESQL_PASSWORD` con i valori configurati all'inizio di questo articolo.

> [!NOTE]
> Per prestazioni più elevate, la proprietà `spring.r2dbc.url` viene configurata per l'uso di un pool di connessioni tramite [r2dbc-pool](https://github.com/r2dbc/r2dbc-pool).

A questo punto dovrebbe essere possibile avviare l'applicazione usando il wrapper Maven fornito, come segue:

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

Arrestare l'applicazione in esecuzione e quindi riavviarla usando il comando seguente. L'applicazione userà ora il database `demo` creato in precedenza e creerà una `todo` tabella al suo interno.

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

Per altre informazioni sull'uso di Azure con Java, vedere [Azure per sviluppatori Java](../index.yml) e la documentazione relativa all'[uso di Azure DevOps e Java](/azure/devops/).