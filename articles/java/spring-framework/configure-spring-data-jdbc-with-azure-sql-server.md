---
title: Usare Spring Data JDBC con Database SQL di Azure
description: Informazioni su come usare Spring Data JDBC con un database SQL di Azure.
documentationcenter: java
ms.date: 10/13/2020
ms.service: sql-database
ms.tgt_pltfrm: multiple
ms.author: judubois
ms.topic: article
ms.custom: devx-track-java
ms.openlocfilehash: 2bfa70765c8c98b4a590b86cacb979a2ca0d95ce
ms.sourcegitcommit: 8e1d3a384ccb0e083589418d65a70b3a01afebff
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/12/2020
ms.locfileid: "94560244"
---
# <a name="use-spring-data-jdbc-with-azure-sql-database"></a>Usare Spring Data JDBC con Database SQL di Azure

Questo argomento illustra la creazione di un'applicazione di esempio che usa [Spring Data JDBC](https://spring.io/projects/spring-data-jdbc) per archiviare e recuperare le informazioni in [Database SQL di Azure](/azure/sql-database/).

[JDBC](https://en.wikipedia.org/wiki/Java_Database_Connectivity) è l'API Java standard per la connessione ai database relazionali tradizionali.

[!INCLUDE [spring-data-prerequisites.md](includes/spring-data-prerequisites.md)]

## <a name="sample-application"></a>Applicazione di esempio

In questo articolo si scriverà il codice per un'applicazione di esempio. Per procedere più rapidamente, è possibile usare l'applicazione già pronta, disponibile all'indirizzo [https://github.com/Azure-Samples/quickstart-spring-data-jdbc-sql-server](https://github.com/Azure-Samples/quickstart-spring-data-jdbc-sql-server).

[!INCLUDE [spring-data-sql-server-setup.md](includes/spring-data-sql-server-setup.md)]

### <a name="generate-the-application-by-using-spring-initializr"></a>Generare l'applicazione con Spring Initializr

Per generare l'applicazione, immettere il comando seguente sulla riga di comando:

```bash
curl https://start.spring.io/starter.tgz -d dependencies=web,data-jdbc,sqlserver -d baseDir=azure-database-workshop -d bootVersion=2.3.1.RELEASE -d javaVersion=8 | tar -xzvf -
```

> [!NOTE]
> Spring Initializr usa Java 11 come versione predefinita. Per usare le utilità di avvio di Spring Boot descritte in questo argomento, è necessario selezionare invece Java 8.

### <a name="configure-spring-boot-to-use-azure-sql-database"></a>Configurare Spring Boot per l'uso del database SQL di Azure

Aprire il file *src/main/resources/application.properties* e aggiungere il testo seguente:

```properties
logging.level.org.springframework.jdbc.core=DEBUG

spring.datasource.url=jdbc:sqlserver://$AZ_DATABASE_NAME.database.windows.net:1433;database=demo;encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;
spring.datasource.username=spring@$AZ_DATABASE_NAME
spring.datasource.password=$AZ_SQL_SERVER_PASSWORD

spring.datasource.initialization-mode=always
```

Sostituire le due variabili `$AZ_DATABASE_NAME` e la variabile `$AZ_SQL_SERVER_PASSWORD` con i valori configurati all'inizio di questo articolo.

> [!WARNING]
> La proprietà di configurazione `spring.datasource.initialization-mode=always` indica che Spring Boot genererà automaticamente a ogni avvio del server uno schema di database, usando il file `schema.sql` che verrà creato in un secondo momento. Questa proprietà è ideale per i test, ma tenere presente che eliminerà i dati a ogni riavvio, quindi non dovrebbe essere usata in produzione.

A questo punto dovrebbe essere possibile avviare l'applicazione usando il wrapper Maven fornito, come segue:

```bash
./mvnw spring-boot:run
```

Ecco uno screenshot dell'applicazione in esecuzione per la prima volta:

[![Applicazione in esecuzione](media/configure-spring-data-jdbc-with-azure-sql-server/create-sql-server-01.png)](media/configure-spring-data-jdbc-with-azure-sql-server/create-sql-server-01.png#lightbox)

### <a name="create-the-database-schema"></a>Creare lo schema del database

Spring Boot eseguirà automaticamente *src/main/resources/schema.sql* per creare uno schema di database. Creare tale file con il contenuto seguente:

```sql
DROP TABLE IF EXISTS todo;
CREATE TABLE todo (id INT IDENTITY PRIMARY KEY, description VARCHAR(255), details VARCHAR(4096), done BIT);
```

Arrestare l'applicazione in esecuzione e quindi riavviarla usando il comando seguente. L'applicazione userà ora il database `demo` creato in precedenza e creerà una `todo` tabella al suo interno.

```bash
./mvnw spring-boot:run
```

## <a name="code-the-application"></a>Codice dell'applicazione

Aggiungere quindi il codice Java che userà JDBC per archiviare e recuperare i dati dal server di Database SQL di Azure.

[!INCLUDE [spring-data-jdbc-create-application.md](includes/spring-data-jdbc-create-application.md)]

Ecco uno screenshot di queste richieste cURL:

[![Eseguire il test con cURL](media/configure-spring-data-jdbc-with-azure-sql-server/create-sql-server-02.png)](media/configure-spring-data-jdbc-with-azure-sql-server/create-sql-server-02.png#lightbox)

Congratulazioni! È stata creata un'applicazione Spring Boot che usa JDBC per archiviare e recuperare i dati da Database SQL di Azure.

[!INCLUDE [spring-data-conclusion.md](includes/spring-data-conclusion.md)]

### <a name="additional-resources"></a>Risorse aggiuntive

Per altre informazioni su Spring Data JDBC, vedere la [documentazione di riferimento](https://docs.spring.io/spring-data/jdbc/docs/current/reference/html/#reference) di Spring.

Per altre informazioni sull'uso di Azure con Java, vedere [Azure per sviluppatori Java](../index.yml) e la documentazione relativa all'[uso di Azure DevOps e Java](/azure/devops/).

### <a name="clean-up-resources"></a>Pulire le risorse

Quando le risorse create in questo articolo non sono più necessarie, usare il [portale di Azure](https://portal.azure.com/) per eliminarle in modo da evitare addebiti imprevisti.
