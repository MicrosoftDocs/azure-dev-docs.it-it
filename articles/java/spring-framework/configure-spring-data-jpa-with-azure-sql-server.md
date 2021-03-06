---
title: Usare Spring Data JPA con Database SQL di Azure
description: Informazioni su come usare Spring Data JPA con Database SQL di Azure.
documentationcenter: java
ms.date: 10/14/2020
ms.service: sql-database
ms.tgt_pltfrm: multiple
ms.author: judubois
ms.topic: article
ms.custom: devx-track-java
ms.openlocfilehash: 8483df505bfeda0c04ead0c2d12e941a5f4ba6d8
ms.sourcegitcommit: 709fa38a137b30184a7397e0bfa348822f3ea0a7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/01/2020
ms.locfileid: "96442257"
---
# <a name="use-spring-data-jpa-with-azure-sql-database"></a>Usare Spring Data JPA con Database SQL di Azure

Questo argomento illustra la creazione di un'applicazione di esempio che usa [Spring Data JPA](https://spring.io/projects/spring-data-jpa) per archiviare e recuperare le informazioni in [Database SQL di Azure](/azure/sql-database/).

[JPA (Java Persistence API)](https://en.wikipedia.org/wiki/Java_Persistence_API) è l'API Java standard per il mapping relazionale a oggetti.

[!INCLUDE [spring-data-prerequisites.md](includes/spring-data-prerequisites.md)]

## <a name="sample-application"></a>Applicazione di esempio

In questo articolo si scriverà il codice per un'applicazione di esempio. Per procedere più rapidamente, è possibile usare l'applicazione già pronta, disponibile all'indirizzo [https://github.com/Azure-Samples/quickstart-spring-data-jpa-sql-server](https://github.com/Azure-Samples/quickstart-spring-data-jpa-sql-server).

[!INCLUDE [spring-data-sql-server-setup.md](includes/spring-data-sql-server-setup.md)]

### <a name="generate-the-application-by-using-spring-initializr"></a>Generare l'applicazione con Spring Initializr

Per generare l'applicazione, immettere quanto segue sulla riga di comando:

```bash
curl https://start.spring.io/starter.tgz -d dependencies=web,data-jpa,sqlserver -d baseDir=azure-database-workshop -d bootVersion=2.3.1.RELEASE -d javaVersion=8 | tar -xzvf -
```


### <a name="configure-spring-boot-to-use-azure-sql-database"></a>Configurare Spring Boot per l'uso del database SQL di Azure

Aprire il file *src/main/resources/application.properties* e aggiungere quanto segue. Assicurarsi di sostituire le due variabili `$AZ_DATABASE_NAME` e la variabile `$AZ_SQL_SERVER_PASSWORD` con i valori configurati all'inizio di questo articolo.

```properties
logging.level.org.hibernate.SQL=DEBUG

spring.datasource.url=jdbc:sqlserver://$AZ_DATABASE_NAME.database.windows.net:1433;database=demo;encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;
spring.datasource.username=spring@$AZ_DATABASE_NAME
spring.datasource.password=$AZ_SQL_SERVER_PASSWORD

spring.jpa.show-sql=true
spring.jpa.hibernate.ddl-auto=create-drop
```

> [!WARNING]
> La proprietà di configurazione `spring.jpa.hibernate.ddl-auto=create-drop` indica che Spring Boot creerà automaticamente uno schema del database all'avvio dell'applicazione e proverà a eliminarlo all'arresto. Questa opzione è ideale per i test, ma non deve essere usata nell'ambiente di produzione.

A questo punto dovrebbe essere possibile avviare l'applicazione usando il wrapper Maven fornito:

```bash
./mvnw spring-boot:run
```

Ecco uno screenshot dell'applicazione in esecuzione per la prima volta:

[![Applicazione in esecuzione](media/configure-spring-data-jpa-with-azure-sql-server/create-sql-server-01.png)](media/configure-spring-data-jpa-with-azure-sql-server/create-sql-server-01.png#lightbox)

## <a name="code-the-application"></a>Codice dell'applicazione

Aggiungere quindi il codice Java che userà JPA per archiviare e recuperare i dati da Database SQL di Azure.

[!INCLUDE [spring-data-jpa-create-application.md](includes/spring-data-jpa-create-application.md)]

Ecco uno screenshot di queste richieste cURL:

[![Eseguire il test con cURL](media/configure-spring-data-jpa-with-azure-sql-server/create-sql-server-02.png)](media/configure-spring-data-jpa-with-azure-sql-server/create-sql-server-02.png#lightbox)

Congratulazioni! È stata creata un'applicazione Spring Boot che usa JPA per archiviare e recuperare i dati da Database SQL di Azure.

[!INCLUDE [spring-data-conclusion.md](includes/spring-data-conclusion.md)]

### <a name="additional-resources"></a>Risorse aggiuntive

Per altre informazioni su Spring Data JPA, vedere la [documentazione di riferimento](https://docs.spring.io/spring-data/jpa/docs/current/reference/html/#reference) di Spring.

Per altre informazioni sull'uso di Azure con Java, vedere [Azure per sviluppatori Java](../index.yml) e la documentazione relativa all'[uso di Azure DevOps e Java](/azure/devops/).