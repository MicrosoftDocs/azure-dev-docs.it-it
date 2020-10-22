---
title: Usare Spring Data JPA con Database di Azure per PostgreSQL
description: Informazioni su come usare Spring Data JPA con un database di Database di Azure per PostgreSQL.
documentationcenter: java
ms.date: 10/12/2020
ms.service: postgresql
ms.tgt_pltfrm: multiple
ms.author: judubois
ms.topic: article
ms.custom: devx-track-java
ms.openlocfilehash: 1099b9568a66c2915b000c31c8e84e8e02b1d3d6
ms.sourcegitcommit: 76f1a47c58810486856e0d128bd154cf7d355e65
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/19/2020
ms.locfileid: "92200589"
---
# <a name="use-spring-data-jpa-with-azure-database-for-postgresql"></a>Usare Spring Data JPA con Database di Azure per PostgreSQL

Questo argomento illustra la creazione di un'applicazione di esempio che usa [Spring Data JPA](https://spring.io/projects/spring-data-jpa) per archiviare e recuperare le informazioni in [Database di Azure per PostgreSQL](/azure/postgresql/).

[JPA (Java Persistence API)](https://en.wikipedia.org/wiki/Java_Persistence_API) è l'API Java standard per il mapping relazionale a oggetti.

[!INCLUDE [spring-data-prerequisites.md](includes/spring-data-prerequisites.md)]

## <a name="sample-application"></a>Applicazione di esempio

In questo articolo si scriverà il codice per un'applicazione di esempio. Per procedere più rapidamente, è possibile usare l'applicazione già pronta, disponibile all'indirizzo [https://github.com/Azure-Samples/quickstart-spring-data-jpa-postgresql](https://github.com/Azure-Samples/quickstart-spring-data-jpa-postgresql).

[!INCLUDE [spring-data-postgresql-setup.md](includes/spring-data-postgresql-setup.md)]

### <a name="generate-the-application-by-using-spring-initializr"></a>Generare l'applicazione con Spring Initializr

Per generare l'applicazione, immettere quanto segue sulla riga di comando:

```bash
curl https://start.spring.io/starter.tgz -d dependencies=web,data-jpa,postgresql -d baseDir=azure-database-workshop -d bootVersion=2.3.4.RELEASE -d javaVersion=8 | tar -xzvf -
```
> [!NOTE]
> Spring Initializr usa Java 11 come versione predefinita. Per usare le utilità di avvio di Spring Boot descritte in questo argomento, è necessario selezionare invece Java 8.

### <a name="configure-spring-boot-to-use-azure-database-for-postgresql"></a>Configurare Spring Boot per l'uso di Database di Azure per PostgreSQL

Aprire il file *src/main/resources/application.properties* e aggiungere quanto segue. Assicurarsi di sostituire le due variabili `$AZ_DATABASE_NAME` e la variabile `$AZ_POSTGRESQL_PASSWORD` con i valori configurati all'inizio di questo articolo.

```properties
logging.level.org.hibernate.SQL=DEBUG

spring.datasource.url=jdbc:postgresql://$AZ_DATABASE_NAME.postgres.database.azure.com:5432/demo
spring.datasource.username=spring@$AZ_DATABASE_NAME
spring.datasource.password=$AZ_POSTGRESQL_PASSWORD

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

[![Applicazione in esecuzione](media/configure-spring-data-jpa-with-azure-postgresql/create-postgresql-01.png)](media/configure-spring-data-jpa-with-azure-postgresql/create-postgresql-01.png#lightbox)

## <a name="code-the-application"></a>Codice dell'applicazione

Aggiungere quindi il codice Java che userà JPA per archiviare e recuperare i dati dal server PostgreSQL.

[!INCLUDE [spring-data-jpa-create-application.md](includes/spring-data-jpa-create-application.md)]
    
Ecco uno screenshot di queste richieste cURL:

[![Eseguire il test con cURL](media/configure-spring-data-jpa-with-azure-postgresql/create-postgresql-02.png)](media/configure-spring-data-jpa-with-azure-postgresql/create-postgresql-02.png#lightbox)
    
Congratulazioni! È stata creata un'applicazione Spring Boot che usa JPA per archiviare e recuperare i dati da Database di Azure per PostgreSQL.

[!INCLUDE [spring-data-conclusion.md](includes/spring-data-conclusion.md)]

### <a name="additional-resources"></a>Risorse aggiuntive

Per altre informazioni su Spring Data JPA, vedere la [documentazione di riferimento](https://docs.spring.io/spring-data/jpa/docs/current/reference/html/#reference) di Spring.

Per altre informazioni sull'uso di Azure con Java, vedere [Azure per sviluppatori Java](../index.yml) e la documentazione relativa all'[uso di Azure DevOps e Java](/azure/devops/).