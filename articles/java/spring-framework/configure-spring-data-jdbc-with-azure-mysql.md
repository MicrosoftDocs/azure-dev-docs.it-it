---
title: Usare Spring Data JDBC con Database di Azure per MySQL
description: Informazioni su come usare Spring Data JDBC con un database di Azure per MySQL.
documentationcenter: java
ms.date: 04/07/2020
ms.service: mysql
ms.tgt_pltfrm: multiple
ms.author: judubois
ms.topic: article
ms.openlocfilehash: de4a913e7cd4e96c8223ddd005a9d3014780d0fe
ms.sourcegitcommit: fbbc341a0b9e17da305bd877027b779f5b0694cc
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/19/2020
ms.locfileid: "83631632"
---
# <a name="use-spring-data-jdbc-with-azure-database-for-mysql"></a>Usare Spring Data JDBC con Database di Azure per MySQL

Questo argomento illustra la creazione di un'applicazione di esempio che usa [Spring Data JDBC](https://spring.io/projects/spring-data-jdbc) per archiviare e recuperare le informazioni in [Database di Azure per MySQL](https://docs.microsoft.com/azure/mysql/).

[JDBC](https://en.wikipedia.org/wiki/Java_Database_Connectivity) è l'API Java standard per la connessione ai database relazionali tradizionali.

[!INCLUDE [spring-data-prerequisites.md](includes/spring-data-prerequisites.md)]

[!INCLUDE [spring-data-mysql-setup.md](includes/spring-data-mysql-setup.md)]

### <a name="generate-the-application-by-using-spring-initializr"></a>Generare l'applicazione con Spring Initializr

Per generare l'applicazione, immettere quanto segue sulla riga di comando:

```bash
curl https://start.spring.io/starter.tgz -d dependencies=web,data-jdbc,mysql -d baseDir=azure-database-workshop -d bootVersion=2.3.0.RELEASE -d javaVersion=8 | tar -xzvf -
```

### <a name="configure-spring-boot-to-use-azure-database-for-mysql"></a>Configurare Spring Boot per l'uso di Database di Azure per MySQL

Aprire il file *src/main/resources/application.properties* e aggiungere:

```properties
logging.level.org.springframework.jdbc.core=DEBUG

spring.datasource.url=jdbc:mysql://$AZ_DATABASE_NAME.mysql.database.azure.com:3306/demo?serverTimezone=UTC
spring.datasource.username=spring@$AZ_DATABASE_NAME
spring.datasource.password=$AZ_MYSQL_PASSWORD

spring.datasource.initialization-mode=always
```

- Sostituire le due variabili `$AZ_DATABASE_NAME` con il valore configurato all'inizio di questo articolo.
- Sostituire la variabile `$AZ_MYSQL_PASSWORD` con il valore configurato all'inizio di questo articolo.

> [!WARNING]
> La proprietà di configurazione `spring.datasource.initialization-mode=always` indica che Spring Boot genererà automaticamente a ogni avvio del server uno schema di database, usando il file `schema.sql` che verrà creato in un secondo momento. Questa proprietà è particolarmente adatta in un ambiente di test, ma è necessario ricordare che comporta l'eliminazione dei dati a ogni riavvio e quindi non dovrebbe essere usata nell'ambiente di produzione.

> [!NOTE]
> Alla proprietà di configurazione `spring.datasource.url` verrà aggiunto `?serverTimezone=UTC`, per indicare al driver JDBC di usare il formato di data UTC (o Coordinated Universal Time) per la connessione al database. In caso contrario, il server Java non userà lo stesso formato di data del database, causando un errore.

A questo punto dovrebbe essere possibile avviare l'applicazione usando il wrapper Maven fornito:

```bash
./mvnw spring-boot:run
```

Ecco uno screenshot dell'applicazione in esecuzione per la prima volta:

[![Applicazione in esecuzione](media/configure-spring-data-jdbc-with-azure-mysql/create-mysql-01.png)](media/configure-spring-data-jdbc-with-azure-mysql/create-mysql-01.png#lightbox)

### <a name="create-the-database-schema"></a>Creare lo schema del database

Spring Boot eseguirà automaticamente *src/main/resources/`schema.sql`* per creare uno schema di database. Creare tale file con il contenuto seguente:

```sql
DROP TABLE IF EXISTS todo;
CREATE TABLE todo (id SERIAL PRIMARY KEY, description VARCHAR(255), details VARCHAR(4096), done BOOLEAN);
```

Arrestare l'applicazione in esecuzione e quindi riavviarla. L'applicazione userà ora il database `demo` creato in precedenza e creerà una `todo` tabella al suo interno.

```bash
./mvnw spring-boot:run
```

## <a name="code-the-application"></a>Codice dell'applicazione

Aggiungere quindi il codice Java che userà JDBC per archiviare e recuperare i dati dal server MySQL.

[!INCLUDE [spring-data-jdbc-create-application.md](includes/spring-data-jdbc-create-application.md)]

Ecco uno screenshot di queste richieste cURL:

[![Eseguire il test con cURL](media/configure-spring-data-jdbc-with-azure-mysql/create-mysql-02.png)](media/configure-spring-data-jdbc-with-azure-mysql/create-mysql-02.png#lightbox)

Congratulazioni! È stata creata un'applicazione Spring Boot che usa JDBC per archiviare e recuperare i dati da Database di Azure per MySQL.

[!INCLUDE [spring-data-conclusion.md](includes/spring-data-conclusion.md)]

### <a name="additional-resources"></a>Risorse aggiuntive

Per altre informazioni su Spring Data JDBC, vedere la [documentazione di riferimento](https://docs.spring.io/spring-data/jdbc/docs/current/reference/html/#reference) di Spring.

Per altre informazioni sull'uso di Azure con Java, vedere [Azure per sviluppatori Java](/azure/developer/java/) e la documentazione relativa all'[uso di Azure DevOps e Java](/azure/devops/).
