---
title: Usare Spring Data R2DBC con il database SQL di Azure
description: Informazioni su come usare Spring Data R2DBC con un database SQL di Azure.
documentationcenter: java
ms.date: 04/28/2020
ms.service: sql-database
ms.tgt_pltfrm: multiple
ms.author: judubois
ms.topic: article
ms.openlocfilehash: 64316322d509d588f94185452d5c21fdee6e9ef1
ms.sourcegitcommit: e9accb9d82b5c633dffffd148974911398f2d096
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/06/2020
ms.locfileid: "86018650"
---
# <a name="use-spring-data-r2dbc-with-azure-sql-database"></a>Usare Spring Data R2DBC con il database SQL di Azure

Questo argomento illustra come creare un'applicazione di esempio che usa [Spring Data R2DBC](https://spring.io/projects/spring-data-r2dbc) per archiviare e recuperare informazioni nel [database SQL di Azure](https://docs.microsoft.com/azure/sql-database/) con l'implementazione R2DBC per Microsoft SQL Server del [repository GitHub r2dbc-mssql](https://github.com/r2dbc/r2dbc-mssql).

[R2DBC](https://r2dbc.io/) introduce le API reattive nei database relazionali tradizionali. È possibile usarlo con Spring WebFlux per creare applicazioni Spring Boot completamente reattive che usano API non bloccanti. Offre una migliore scalabilità rispetto all'approccio classico di "un thread per connessione".

[!INCLUDE [spring-data-prerequisites.md](includes/spring-data-prerequisites.md)]

## <a name="sample-application"></a>Applicazione di esempio

In questo articolo si scriverà il codice per un'applicazione di esempio. Per procedere più rapidamente, è possibile usare l'applicazione già pronta, disponibile all'indirizzo [https://github.com/Azure-Samples/quickstart-spring-data-r2dbc-sql-server](https://github.com/Azure-Samples/quickstart-spring-data-r2dbc-sql-server).

[!INCLUDE [spring-data-sql-server-setup.md](includes/spring-data-sql-server-setup.md)]

[!INCLUDE [spring-data-create-reactive.md](includes/spring-data-create-reactive.md)]

### <a name="generate-the-application-by-using-spring-initializr"></a>Generare l'applicazione con Spring Initializr

Per generare l'applicazione, immettere il comando seguente sulla riga di comando:

```bash
curl https://start.spring.io/starter.tgz -d dependencies=webflux,data-r2dbc -d baseDir=azure-database-workshop -d bootVersion=2.3.1.RELEASE -d javaVersion=8 | tar -xzvf -
```

### <a name="add-the-reactive-azure-sql-database-driver-implementation"></a>Aggiungere l'implementazione del driver del database SQL di Azure reattivo

Aprire il file *pom.xml* del progetto generato per aggiungere il driver del database SQL di Azure reattivo dal [repository GitHub r2dbc-mssql](https://github.com/r2dbc/r2dbc-mssql).

Dopo la dipendenza `spring-boot-starter-webflux` aggiungere il testo seguente:

```xml
<dependency>
    <groupId>io.r2dbc</groupId>
    <artifactId>r2dbc-mssql</artifactId>
    <scope>runtime</scope>
</dependency>
```

### <a name="configure-spring-boot-to-use-azure-sql-database"></a>Configurare Spring Boot per l'uso del database SQL di Azure

Aprire il file *src/main/resources/application.properties* e aggiungere il testo seguente:

```properties
logging.level.org.springframework.data.r2dbc=DEBUG

spring.r2dbc.url=r2dbc:pool:mssql://$AZ_DATABASE_NAME.database.windows.net:1433/demo
spring.r2dbc.username=spring@$AZ_DATABASE_NAME
spring.r2dbc.password=$AZ_SQL_SERVER_PASSWORD
```

Sostituire le due variabili `$AZ_DATABASE_NAME` e la variabile `$AZ_SQL_SERVER_PASSWORD` con i valori configurati all'inizio di questo articolo.

> [!NOTE]
> Per prestazioni più elevate, la proprietà `spring.r2dbc.url` viene configurata per l'uso di un pool di connessioni tramite [r2dbc-pool](https://github.com/r2dbc/r2dbc-pool).

A questo punto dovrebbe essere possibile avviare l'applicazione usando il wrapper Maven fornito, come segue:

```bash
./mvnw spring-boot:run
```

Ecco uno screenshot dell'applicazione in esecuzione per la prima volta:

[![Applicazione in esecuzione](media/configure-spring-data-r2dbc-with-azure-azure-sql/create-azure-sql-01.png)](media/configure-spring-data-r2dbc-with-azure-azure-sql/create-azure-sql-01.png#lightbox)

### <a name="create-the-database-schema"></a>Creare lo schema del database

[!INCLUDE [spring-data-r2dbc-create-schema.md](includes/spring-data-r2dbc-create-schema.md)]

```sql
DROP TABLE IF EXISTS todo;
CREATE TABLE todo (id INT IDENTITY PRIMARY KEY, description VARCHAR(255), details VARCHAR(4096), done BIT);
```

Arrestare l'applicazione in esecuzione e quindi riavviarla usando il comando seguente. L'applicazione userà ora il database `demo` creato in precedenza e creerà una `todo` tabella al suo interno.

```bash
./mvnw spring-boot:run
```

Ecco uno screenshot della tabella di database creata:

[![Creazione della tabella di database](media/configure-spring-data-r2dbc-with-azure-azure-sql/create-azure-sql-02.png)](media/configure-spring-data-r2dbc-with-azure-azure-sql/create-azure-sql-02.png#lightbox)

## <a name="code-the-application"></a>Codice dell'applicazione

Aggiungere quindi il codice Java che userà R2DBC per archiviare e recuperare i dati dal server di database SQL di Azure.

[!INCLUDE [spring-data-r2dbc-create-application.md](includes/spring-data-r2dbc-create-application.md)]

Ecco uno screenshot di queste richieste cURL:

[![Eseguire il test con cURL](media/configure-spring-data-r2dbc-with-azure-azure-sql/create-azure-sql-03.png)](media/configure-spring-data-r2dbc-with-azure-azure-sql/create-azure-sql-03.png#lightbox)

Congratulazioni! È stata creata un'applicazione Spring Boot completamente reattiva che usa R2DBC per archiviare e recuperare i dati dal database SQL di Azure.

[!INCLUDE [spring-data-conclusion.md](includes/spring-data-conclusion.md)]

### <a name="additional-resources"></a>Risorse aggiuntive

Per altre informazioni su Spring Data R2DBC, vedere la [documentazione di riferimento](https://docs.spring.io/spring-data/r2dbc/docs/current/reference/html/#reference) di Spring.

Per altre informazioni sull'uso di Azure con Java, vedere [Azure per sviluppatori Java](/azure/developer/java/) e la documentazione relativa all'[uso di Azure DevOps e Java](/azure/devops/).
