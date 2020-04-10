---
author: yevster
ms.author: yebronsh
ms.date: 1/20/2020
ms.openlocfilehash: 4b5b73eee66c4a5c9eb28b79804e0dc610f639d6
ms.sourcegitcommit: 951fc116a9519577b5d35b6fb584abee6ae72b0f
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/02/2020
ms.locfileid: "80612064"
---
### <a name="inventory-external-resources"></a>Inventario delle risorse esterne

Le risorse esterne, ad esempio le origini dati, i broker di messaggi JMS e altre, vengono inserite tramite JNDI (Java Naming and Directory Interface). Alcune di queste risorse possono richiedere la migrazione o la riconfigurazione.

#### <a name="inside-your-application"></a>All'interno dell'applicazione

Esaminare il file *META-INF/context.xml*. Cercare gli elementi `<Resource>` all'interno dell'elemento `<Context>`.

#### <a name="on-the-application-servers"></a>Nei server applicazioni

Esaminare i file *$CATALINA_BASE/conf/context.xml* e *$CATALINA_BASE/conf/server.xml*, oltre ai file con estensione *xml* disponibili nelle directory *$CATALINA_BASE/conf/[nome-motore]/[nome-host]* .

Nei file *context.xml* le risorse JNDI verranno descritte dagli elementi `<Resource>` all'interno dell'elemento `<Context>` di primo livello.

Nei file *server.xml* le risorse JNDI verranno descritte dagli elementi `<Resource>` all'interno dell'elemento `<GlobalNamingResources>`.

#### <a name="datasources"></a>Datasources

Le origini dati sono risorse JNDI con l'attributo `type` impostato su `javax.sql.DataSource`. Per ogni origine dati, documentare le informazioni seguenti:

* Qual è il nome dell'origine dati?
* Qual è la configurazione del pool di connessioni?
* Dove è possibile trovare il file JAR del driver JDBC?

Per altre informazioni, vedere la sezione di [procedure per le origini dati JNDI](https://tomcat.apache.org/tomcat-9.0-doc/jndi-datasource-examples-howto.html) nella documentazione di Tomcat.

#### <a name="all-other-external-resources"></a>Tutte le altre risorse esterne

Non è possibile documentare tutte le possibili dipendenze esterne in questa guida. È responsabilità del team verificare che sia possibile soddisfare tutte le dipendenze esterne dell'applicazione dopo la migrazione.
