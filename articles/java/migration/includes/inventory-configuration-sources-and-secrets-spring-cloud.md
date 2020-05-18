---
author: yevster
ms.author: yebronsh
ms.date: 5/4/2020
ms.openlocfilehash: b15ebf1491dd494701dd5c18e0248e73bdd86f2c
ms.sourcegitcommit: 226ebca0d0e3b918928f58a3a7127be49e4aca87
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/08/2020
ms.locfileid: "82990203"
---
### <a name="inventory-configuration-sources-and-secrets"></a>Segreti e origini della configurazione dell'inventario

#### <a name="inventory-passwords-and-secure-strings"></a>Password e stringhe sicure dell'inventario

Controllare tutte le proprietà e i file di configurazione e tutte le variabili di ambiente nei server di produzione per verificare la presenza di stringhe segrete e password. In un'applicazione Spring Cloud tali stringhe sono in genere disponibili nel file *application.properties* o *application.yml* nei singoli servizi o nel repository Spring Cloud Config.

[!INCLUDE [inventory-certificates-h4](inventory-certificates-h4.md)]

#### <a name="determine-whether-spring-cloud-vault-is-used"></a>Determinare se viene usato Spring Cloud Vault

Se si usa Spring Cloud Vault per archiviare i segreti e accedervi, identificare l'archivio segreti sottostante, ad esempio HashiCorp Vault o CredHub. Identificare quindi tutti i segreti usati dal codice dell'applicazione.

#### <a name="locate-the-configuration-server-source"></a>Individuare l'origine del server di configurazione

Se l'applicazione usa un [server Spring Cloud Config](https://cloud.spring.io/spring-cloud-config/reference/html/#_spring_cloud_config_server), identificare il percorso in cui è archiviata la configurazione. Questa impostazione si trova in genere nel file *bootstrap.yml* o *bootstrap.properties* oppure talvolta nel file *application.yml* o *application.properties*. L'impostazione sarà simile all'esempio seguente:

```properties
spring.cloud.config.server.git.uri: file://${user.home}/spring-cloud-config-repo
```

Sebbene come archivio dati di backup della configurazione Spring Cloud si usi in genere GIT, come illustrato in precedenza, è possibile che sia in uso uno degli altri back-end possibili. Consultare la [documentazione di Spring Cloud Config](https://cloud.spring.io/spring-cloud-config/reference/html/#_environment_repository) per informazioni su altri back-end, ad esempio [database relazionale (JDBC)](https://cloud.spring.io/spring-cloud-config/reference/html/#_jdbc_backend), [SVN](https://cloud.spring.io/spring-cloud-config/reference/html/#_version_control_backend_filesystem_use) e [file system locale](https://cloud.spring.io/spring-cloud-config/reference/html/#_file_system_backend).

> [!NOTE]
> Se i dati del server di configurazione sono archiviati in locale, ad esempio in GitHub Enterprise, sarà necessario usare un repository GIT per renderli disponibili per Azure Spring Cloud.
