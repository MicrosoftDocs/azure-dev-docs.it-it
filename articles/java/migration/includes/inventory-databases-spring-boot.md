---
author: yevster
ms.author: yebronsh
ms.date: 2/12/2020
ms.openlocfilehash: 6bf4c54c8f428e3ffce79a7ba9ac49aae028e2fd
ms.sourcegitcommit: 226ebca0d0e3b918928f58a3a7127be49e4aca87
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/08/2020
ms.locfileid: "82990233"
---
#### <a name="databases"></a>Database

Per qualsiasi database SQL, identificare la stringa di connessione.

Per un'applicazione Spring Boot, le stringhe di connessione sono in genere incluse nei file di configurazione. 

Ecco un esempio di un file *application.properties*:

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/mysql_db
spring.datasource.username=dbuser
spring.datasource.driver-class-name=com.mysql.jdbc.Driver
```

Ecco un esempio di un file *application.yaml*:

```yaml
spring:
  data:
    mongodb:
      uri: mongodb://mongouser:deepsecret@mongoserver.contoso.com:27017
```

Vedere la documentazione di Spring Data per altri scenari possibili di configurazione:

* [Repository JPA](https://docs.spring.io/spring-data/jpa/docs/current/reference/html/#jpa.repositories)
* [Repository JDBC](https://docs.spring.io/spring-data/jdbc/docs/current/reference/html/#jdbc.repositories)
* [Repository Cassandra](https://docs.spring.io/spring-data/cassandra/docs/current/reference/html/#cassandra.repositories)
* [Repository MongoDB](https://docs.spring.io/spring-data/mongodb/docs/current/reference/html/#mongodb.repositories)
