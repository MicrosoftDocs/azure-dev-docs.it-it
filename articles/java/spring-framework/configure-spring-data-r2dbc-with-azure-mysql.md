---
title: Usare Spring Data R2DBC con Database di Azure per MySQL
description: Informazioni su come usare Spring Data R2DBC con un database di Azure per MySQL.
documentationcenter: java
ms.date: 10/10/2020
ms.service: mysql
ms.tgt_pltfrm: multiple
ms.author: judubois
ms.topic: article
ms.custom: devx-track-java
ms.openlocfilehash: 7cdcae8f20101702e19757c270e81cafb712f8bf
ms.sourcegitcommit: 709fa38a137b30184a7397e0bfa348822f3ea0a7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/01/2020
ms.locfileid: "96441957"
---
# <a name="use-spring-data-r2dbc-with-azure-database-for-mysql"></a>Usare Spring Data R2DBC con Database di Azure per MySQL

Questo argomento illustra come creare un'applicazione di esempio che usa [Spring Data R2DBC](https://spring.io/projects/spring-data-r2dbc) per archiviare e recuperare informazioni in [Database di Azure per MySQL](/azure/mysql/) con l'implementazione R2DBC per MySQL del [repository GitHub r2dbc-mysql](https://github.com/mirromutth/r2dbc-mysql).

[R2DBC](https://r2dbc.io/) introduce le API reattive nei database relazionali tradizionali. È possibile usarlo con Spring WebFlux per creare applicazioni Spring Boot completamente reattive che usano API non bloccanti. Offre una migliore scalabilità rispetto all'approccio classico di "un thread per connessione".

[!INCLUDE [spring-data-prerequisites.md](includes/spring-data-prerequisites.md)]

## <a name="sample-application"></a>Applicazione di esempio

In questo articolo si scriverà il codice per un'applicazione di esempio. Per procedere più rapidamente, è possibile usare l'applicazione già pronta, disponibile all'indirizzo [https://github.com/Azure-Samples/quickstart-spring-data-r2dbc-mysql](https://github.com/Azure-Samples/quickstart-spring-data-r2dbc-mysql).

[!INCLUDE [spring-data-mysql-setup.md](includes/spring-data-mysql-setup.md)]

[!INCLUDE [spring-data-create-reactive.md](includes/spring-data-create-reactive.md)]

### <a name="generate-the-application-by-using-spring-initializr"></a>Generare l'applicazione con Spring Initializr

Per generare l'applicazione, immettere quanto segue sulla riga di comando:

```bash
curl https://start.spring.io/starter.tgz -d dependencies=webflux,data-r2dbc -d baseDir=azure-database-workshop -d bootVersion=2.3.4.RELEASE -d javaVersion=8 | tar -xzvf -
```


### <a name="add-the-reactive-mysql-driver-implementation"></a>Aggiungere l'implementazione del driver MySQL reattivo

Aprire il file *pom.xml* del progetto generato per aggiungere il driver MySQL reattivo dal [repository GitHub r2dbc-mysql](https://github.com/mirromutth/r2dbc-mysql).

Dopo la dipendenza `spring-boot-starter-webflux` aggiungere il frammento di codice seguente:

```xml
<dependency>
   <groupId>dev.miku</groupId>
   <artifactId>r2dbc-mysql</artifactId>
   <version>0.8.1.RELEASE</version>
   <scope>runtime</scope>
</dependency>
```

### <a name="configure-spring-boot-to-use-azure-database-for-mysql"></a>Configurare Spring Boot per l'uso di Database di Azure per MySQL

Aprire il file *src/main/resources/application.properties* e aggiungere:

```properties
logging.level.org.springframework.data.r2dbc=DEBUG

spring.r2dbc.url=r2dbc:pool:mysql://$AZ_DATABASE_NAME.mysql.database.azure.com:3306/demo
spring.r2dbc.username=spring@$AZ_DATABASE_NAME
spring.r2dbc.password=$AZ_MYSQL_PASSWORD
```

- Sostituire le due variabili `$AZ_DATABASE_NAME` con il valore configurato all'inizio di questo articolo.
- Sostituire la variabile `$AZ_MYSQL_PASSWORD` con il valore configurato all'inizio di questo articolo.

> [!NOTE]
> Per prestazioni più elevate, la proprietà `spring.r2dbc.url` viene configurata per l'uso di un pool di connessioni tramite [r2dbc-pool](https://github.com/r2dbc/r2dbc-pool).

A questo punto dovrebbe essere possibile avviare l'applicazione usando il wrapper Maven fornito:

```bash
./mvnw spring-boot:run
```

Ecco uno screenshot dell'applicazione in esecuzione per la prima volta:

[![Applicazione in esecuzione](media/configure-spring-data-r2dbc-with-azure-mysql/create-mysql-01.png)](media/configure-spring-data-r2dbc-with-azure-mysql/create-mysql-01.png#lightbox)

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

[![Creazione della tabella di database](media/configure-spring-data-r2dbc-with-azure-mysql/create-mysql-02.png)](media/configure-spring-data-r2dbc-with-azure-mysql/create-mysql-02.png#lightbox)

## <a name="code-the-application"></a>Codice dell'applicazione

Aggiungere quindi il codice Java che userà R2DBC per archiviare e recuperare i dati dal server MySQL.

[!INCLUDE [spring-data-r2dbc-create-application.md](includes/spring-data-r2dbc-create-application.md)]

Ecco uno screenshot di queste richieste cURL:

[![Eseguire il test con cURL](media/configure-spring-data-r2dbc-with-azure-mysql/create-mysql-03.png)](media/configure-spring-data-r2dbc-with-azure-mysql/create-mysql-03.png#lightbox)

Congratulazioni! È stata creata un'applicazione Spring Boot completamente reattiva che usa R2DBC per archiviare e recuperare i dati da Database di Azure per MySQL.

[!INCLUDE [spring-data-conclusion.md](includes/spring-data-conclusion.md)]

### <a name="additional-resources"></a>Risorse aggiuntive

Per altre informazioni su Spring Data R2DBC, vedere la [documentazione di riferimento](https://docs.spring.io/spring-data/r2dbc/docs/current/reference/html/#reference) di Spring.

Per altre informazioni sull'uso di Azure con Java, vedere [Azure per sviluppatori Java](../index.yml) e la documentazione relativa all'[uso di Azure DevOps e Java](/azure/devops/).