---
author: yevster
ms.author: yebronsh
ms.topic: include
ms.date: 1/20/2020
ms.openlocfilehash: 8c2ada1a8a11e1f0ae108e4356434825a2cf0b76
ms.sourcegitcommit: 3585b1b5148e0f8eb950037345bafe6a4f6be854
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/21/2020
ms.locfileid: "76288560"
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

#### <a name="all-other-external-resources"></a>Tutte le altre risorse esterne

Non è possibile documentare tutte le possibili dipendenze esterne in questa guida. È responsabilità del team verificare che sia possibile soddisfare tutte le dipendenze esterne dell'applicazione dopo la migrazione.
