---
title: Usare Spring Data JDBC con Database di Azure per PostgreSQL
description: Informazioni su come usare Spring Data JDBC con un database di Database di Azure per PostgreSQL.
documentationcenter: java
ms.date: 10/13/2020
ms.service: postgresql
ms.tgt_pltfrm: multiple
ms.author: judubois
ms.topic: article
ms.custom: devx-track-java
ms.openlocfilehash: a27b8122b3758e997cf5d7595cfd246084acf071
ms.sourcegitcommit: 709fa38a137b30184a7397e0bfa348822f3ea0a7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/01/2020
ms.locfileid: "96441928"
---
# <a name="use-spring-data-jdbc-with-azure-database-for-postgresql"></a>Usare Spring Data JDBC con Database di Azure per PostgreSQL

Questo argomento illustra la creazione di un'applicazione di esempio che usa [Spring Data JDBC](https://spring.io/projects/spring-data-jdbc) per archiviare e recuperare le informazioni in un database di [Database di Azure per PostgreSQL](/azure/postgresql/).

[JDBC](https://en.wikipedia.org/wiki/Java_Database_Connectivity) è l'API Java standard per la connessione ai database relazionali tradizionali.

[!INCLUDE [spring-data-prerequisites.md](includes/spring-data-prerequisites.md)]

## <a name="sample-application"></a>Applicazione di esempio

In questo articolo si scriverà il codice per un'applicazione di esempio. Per procedere più rapidamente, è possibile usare l'applicazione già pronta, disponibile all'indirizzo [https://github.com/Azure-Samples/quickstart-spring-data-jdbc-postgresql](https://github.com/Azure-Samples/quickstart-spring-data-jdbc-postgresql).

[!INCLUDE [spring-data-postgresql-setup.md](includes/spring-data-postgresql-setup.md)]

### <a name="generate-the-application-by-using-spring-initializr"></a>Generare l'applicazione con Spring Initializr

Per generare l'applicazione, immettere il comando seguente sulla riga di comando:

```bash
curl https://start.spring.io/starter.tgz -d dependencies=web,data-jdbc,postgresql -d baseDir=azure-database-workshop -d bootVersion=2.3.4.RELEASE -d javaVersion=8 | tar -xzvf -
``` 
 
### <a name="configure-spring-boot-to-use-azure-database-for-postgresql"></a>Configurare Spring Boot per l'uso di Database di Azure per PostgreSQL

Aprire il file *src/main/resources/application.properties* e aggiungere il testo seguente:

```properties
logging.level.org.springframework.jdbc.core=DEBUG

spring.datasource.url=jdbc:postgresql://$AZ_DATABASE_NAME.postgres.database.azure.com:5432/demo
spring.datasource.username=spring@$AZ_DATABASE_NAME
spring.datasource.password=$AZ_POSTGRESQL_PASSWORD

spring.datasource.initialization-mode=always
```

Sostituire le due variabili `$AZ_DATABASE_NAME` e la variabile `$AZ_POSTGRESQL_PASSWORD` con i valori configurati all'inizio di questo articolo.

> [!WARNING]
> La proprietà di configurazione `spring.datasource.initialization-mode=always` indica che Spring Boot genererà automaticamente a ogni avvio del server uno schema di database, usando il file *schema.sql* che verrà creato in un secondo momento. Questa proprietà è ideale per i test, ma tenere presente che eliminerà i dati a ogni riavvio, quindi non dovrebbe essere usata in produzione.

A questo punto dovrebbe essere possibile avviare l'applicazione usando il wrapper Maven fornito, come segue:

```bash
./mvnw spring-boot:run
```

Ecco uno screenshot dell'applicazione in esecuzione per la prima volta:

[![Applicazione in esecuzione](media/configure-spring-data-jdbc-with-azure-postgresql/create-postgresql-01.png)](media/configure-spring-data-jdbc-with-azure-postgresql/create-postgresql-01.png#lightbox)

### <a name="create-the-database-schema"></a>Creare lo schema del database

Spring Boot eseguirà automaticamente il file *src/main/resources/schema.sql* per creare uno schema di database. Creare tale file con il contenuto seguente:

```sql
DROP TABLE IF EXISTS todo;
CREATE TABLE todo (id SERIAL PRIMARY KEY, description VARCHAR(255), details VARCHAR(4096), done BOOLEAN);
```

Arrestare l'applicazione in esecuzione e quindi riavviarla usando il comando seguente. L'applicazione userà ora il database `demo` creato in precedenza e creerà una `todo` tabella al suo interno.

```bash
./mvnw spring-boot:run
```

## <a name="code-the-application"></a>Codice dell'applicazione

Aggiungere quindi il codice Java che userà JDBC per archiviare e recuperare i dati dal server PostgreSQL.

[!INCLUDE [spring-data-jdbc-create-application.md](includes/spring-data-jdbc-create-application.md)]

Ecco uno screenshot di queste richieste cURL:

[![Eseguire il test con cURL](media/configure-spring-data-jdbc-with-azure-postgresql/create-postgresql-02.png)](media/configure-spring-data-jdbc-with-azure-postgresql/create-postgresql-02.png#lightbox)

Congratulazioni! È stata creata un'applicazione Spring Boot che usa JDBC per archiviare e recuperare i dati da Database di Azure per PostgreSQL.

[!INCLUDE [spring-data-conclusion.md](includes/spring-data-conclusion.md)]

### <a name="additional-resources"></a>Risorse aggiuntive

Per altre informazioni su Spring Data JDBC, vedere la [documentazione di riferimento](https://docs.spring.io/spring-data/jdbc/docs/current/reference/html/#reference) di Spring.

Per altre informazioni sull'uso di Azure con Java, vedere [Azure per sviluppatori Java](../index.yml) e la documentazione relativa all'[uso di Azure DevOps e Java](/azure/devops/).