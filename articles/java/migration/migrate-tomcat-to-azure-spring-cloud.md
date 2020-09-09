---
title: Eseguire la migrazione di applicazioni Tomcat ad Azure Spring Cloud
description: Questa guida descrive gli aspetti da considerare per la migrazione di un'applicazione Tomcat esistente ad Azure Spring Cloud
author: yevster
ms.author: yebronsh
ms.topic: conceptual
ms.date: 6/16/2020
ms.openlocfilehash: 7b8a29c2769b3c4b04a40053d0470bfc6b1a0cca
ms.sourcegitcommit: 2f98cf2a394d4fd82ddc917ac1041c1dc08473b6
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/01/2020
ms.locfileid: "89275185"
---
# <a name="migrate-a-tomcat-application-to-azure-spring-cloud"></a>Eseguire la migrazione di un'applicazione Tomcat ad Azure Spring Cloud

Questa guida descrive gli aspetti da considerare per la migrazione di un'applicazione Tomcat esistente da eseguire in Azure Spring Cloud.

## <a name="pre-migration"></a>Pre-migrazione

Per garantire una corretta migrazione, prima di iniziare completare i passaggi di valutazione e inventario descritti nelle sezioni seguenti.

### <a name="switch-to-a-supported-platform"></a>Passare a una piattaforma supportata

Azure Spring Cloud offre versioni specifiche di Java SE. Per garantire la compatibilità, eseguire la migrazione dell'applicazione a una delle versioni supportate dell'ambiente corrente prima di procedere con i passaggi rimanenti. Assicurarsi di testare completamente la configurazione risultante. Usare la versione stabile più recente della distribuzione Linux in questi test.

[!INCLUDE [note-obtain-your-current-java-version](includes/note-obtain-your-current-java-version.md)]

[!INCLUDE [determine-whether-and-how-the-file-system-is-used](includes/determine-whether-and-how-the-file-system-is-used.md)]

### <a name="identify-the-application-builddependency-system"></a>Identificare il sistema di compilazione e dipendenze dell'applicazione

Identificare gli strumenti usati per compilare/assemblare l'applicazione, scaricando anche tutte le dipendenze.

[!INCLUDE [inventory-external-resources](includes/inventory-external-resources.md)]

### <a name="inventory-secrets"></a>Inventario dei segreti

#### <a name="passwords-and-secure-strings"></a>Password e stringhe sicure

Controllare tutte le proprietà e i file di configurazione nei server di produzione per verificare la presenza di stringhe segrete e password. Assicurarsi di controllare i file *server.xml* e *context.xml* in *$CATALINA_BASE/conf*. È anche possibile trovare i file di configurazione contenenti password o credenziali all'interno dell'applicazione in *META-INF/context.xml*.

[!INCLUDE [inventory-certificates](includes/inventory-certificates.md)]

### <a name="determine-whether-your-application-contains-os-specific-code"></a>Determinare se l'applicazione contiene codice specifico del sistema operativo

[!INCLUDE [determine-whether-your-application-contains-os-specific-code-no-title](includes/determine-whether-your-application-contains-os-specific-code-no-title.md)]

[!INCLUDE [identify-all-outside-processes-and-daemons-running-on-the-production-servers](includes/identify-all-outside-processes-and-daemons-running-on-the-production-servers.md)]

### <a name="determine-whether-tomcat-is-connected-to-a-web-server"></a>Determinare se Tomcat è connesso a un server Web

Tomcat può essere connesso a un server Web statico, ad esempio Apache, tramite un connettore Tomcat come `mod_jk`. Esaminare il file `workers.properties` nella directory `conf` per determinare se esiste una connessione di questo tipo.

### <a name="special-cases"></a>Casi speciali

Alcuni scenari di produzione possono richiedere ulteriori modifiche o imporre limitazioni aggiuntive. Sebbene tali scenari siano poco frequenti, è importante assicurarsi che siano inapplicabili all'applicazione o risolti correttamente.

#### <a name="determine-if-the-application-uses-filters"></a>Determinare se l'applicazione usa i filtri

Esaminare il file *web.xml* dell'applicazione per verificare se sono presenti [filtri configurati](https://tomcat.apache.org/tomcat-9.0-doc/config/filter.html#Expires_Filter/Basic_configuration_sample).

[!INCLUDE [determine-whether-your-application-relies-on-scheduled-jobs-azure-spring-cloud](includes/determine-whether-your-application-relies-on-scheduled-jobs-azure-spring-cloud.md)]

#### <a name="determine-whether-non-http-connectors-are-used"></a>Determinare se vengono usati i connettori non HTTP

Azure Spring Cloud supporta solo connessioni HTTP in una singola porta HTTP non personalizzabile. Se l'applicazione richiede porte o protocolli aggiuntivi, non usare Azure Spring Cloud.

Per identificare i connettori HTTP usati dall'applicazione, cercare gli elementi `<Connector>` all'interno del file *server.xml* nella configurazione di Tomcat.

#### <a name="determine-whether-ssl-session-tracking-is-used"></a>Determinare se viene usato il monitoraggio delle sessioni SSL

In Azure Spring Cloud la sessione SSL viene terminata prima di raggiungere il codice dell'applicazione, quindi non è possibile usare il [rilevamento della sessione SSL](https://tomcat.apache.org/tomcat-9.0-doc/servletapi/javax/servlet/SessionTrackingMode.html#SSL). Sarà invece necessario usare [Spring Session](https://docs.spring.io/spring-session/docs/current/reference/html5/index.html).

#### <a name="determine-whether-tomcat-realms-are-used"></a>Determinare se vengono usate aree di autenticazione Tomcat

In Azure Spring Cloud è necessario usare Spring Security al posto delle aree di autenticazione Tomcat. Esaminare il file *server.xml* per eseguire l'inventario di eventuali [aree di autenticazione configurate](https://tomcat.apache.org/tomcat-9.0-doc/realm-howto.html#Configuring_a_Realm).

#### <a name="determine-whether-servlet-filters-are-used"></a>Determinare se vengono usati filtri servlet

Esaminare il file *web.xml* nell'applicazione per verificare la presenza di elementi `<filter>`. Per un elenco dei filtri disponibili, vedere la [documentazione sui filtri di Tomcat](https://tomcat.apache.org/tomcat-9.0-doc/config/filter.html).

## <a name="migration"></a>Migrazione

### <a name="remove-connection-to-web-server-if-present"></a>Rimuovere la connessione al server Web, se presente

Se Tomcat è connesso a un server Web statico, in genere ad Apache tramite `mod_jk`, disabilitare tale connessione in modo che Tomcat venga eseguito come server autonomo, creando reindirizzamenti Web dal server standard se necessario. Valutare se eseguire la migrazione del contenuto Web statico ad Archiviazione di Azure con la rete di distribuzione dei contenuti di Azure. Per altre informazioni, vedere [Hosting di siti Web statici in Archiviazione di Azure](/azure/storage/blobs/storage-blob-static-website) e [Avvio rapido: Integrare un account di archiviazione di Azure con la rete CDN di Azure](/azure/cdn/cdn-create-a-storage-account-with-cdn).

### <a name="update-to-tomcat-9"></a>Eseguire l'aggiornamento a Tomcat 9

Se l'applicazione corrente è in esecuzione in una versione di Tomcat precedente alla 9, eseguire la migrazione a Tomcat 9 e verificare che l'applicazione sia completamente funzionante. Per altre informazioni, vedere la [guida alla migrazione di Tomcat 9](http://tomcat.apache.org/migration-9.html).

### <a name="switch-to-maven-or-gradle"></a>Passare a Maven o Gradle

Spring Boot e Spring Cloud richiedono Maven o Gradle per la gestione delle compilazioni e delle dipendenze. Se l'applicazione usa un altro sistema di gestione delle compilazioni o delle dipendenze, passare alla versione corrente di [Maven](https://maven.apache.org/download.cgi) prima di procedere. Anche se Gradle è supportato, verrà usato Maven per tutti i passaggi di questa guida. Se si decide di usare Gradle, adattare le istruzioni di conseguenza.

Creare un [file POM](https://maven.apache.org/pom.html) per l'applicazione e assicurarsi che l'applicazione venga compilata ed eseguita con funzionalità complete prima di procedere.

### <a name="migrate-to-spring-boot"></a>Eseguire la migrazione a Spring Boot

La tabella seguente contiene un riepilogo delle migrazioni e delle modifiche del codice necessarie per eseguire la migrazione di un'applicazione Tomcat a Spring Boot e, successivamente, ad Azure Spring Cloud. Se nell'applicazione viene usato un elemento indicato nella colonna Legacy, è necessario sostituirlo con l'elemento corrispondente della colonna Migrazione minima o, idealmente, Migrazione consigliata.

|Legacy | Dove controllare |Migrazione minima |Migrazione consigliata|
|---|---|---|---|
|[JDBC tramite DataSource](https://docs.oracle.com/javase/tutorial/jdbc/basics/connecting.html)|*server.xml*|[Spring Data Datasource](https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-sql) con [modello JDBC](https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-using-jdbc-template)|Considerare Spring Data e JPA, se appropriato.|
|Servlet |*web.xml*|Abilitare [Servlet Context Initialization](https://docs.spring.io/spring-boot/docs/current-SNAPSHOT/reference/html/spring-boot-features.html#boot-features-embedded-container-context-initializer) e aggiungere l'annotazione `@WebServlet`|Riscrivere come [controller Spring-MVC (con `@RestController`](https://spring.io/guides/gs/rest-service/#_create_a_resource_controller))
|Filtri |*web.xml*| Abilitare [Servlet Context Initialization](https://docs.spring.io/spring-boot/docs/current-SNAPSHOT/reference/html/spring-boot-features.html#boot-features-embedded-container-context-initializer) e [aggiungere l'annotazione `@WebFilter`](https://docs.oracle.com/javaee/7/api/javax/servlet/annotation/WebFilter.html) |Come per Migrazione minima|
|JSP (Java Server Page)|Contenuto dei file *web.xml* WAR|[JSP Views per Spring MVC](https://docs.spring.io/spring/docs/current/spring-framework-reference/web.html#mvc-view-jsp)|Ospitare il livello di visualizzazione separatamente|
|JMS (Java Message Service)|*server.xml*|Creare un'istanza della factory di connessione [Spring Bean](https://docs.spring.io/spring-boot/docs/current/reference/html/using-spring-boot.html#using-boot-spring-beans-and-dependency-injection)|Usare [Spring JMS](https://docs.spring.io/spring-framework/docs/current/spring-framework-reference/integration.html#jms-using)
|Contenuto statico (immagini, file JavaScript e così via) all'interno del file WAR|Directory di contenuto statico (in genere */static*, */public* o */resources*)|Spostare il contenuto in */src/main/resources*|Vedere i [consigli sul contenuto statico nella pre-migrazione](#read-only-static-content).
|Contenuto statico (immagini, file JavaScript e così via) all'esterno del file WAR|Percorso nel file system locale|Spostare il contenuto in *src/main/resources*. Cercare nel codice sorgente i percorsi hardcoded e sostituirli con [ClassPathResource](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/core/io/ClassPathResource.html) |Vedere i [consigli sul contenuto statico nella pre-migrazione](#read-only-static-content).

1. Se l'applicazione si basa sulle librerie inserite tramite risorse JNDI, ad esempio i driver JDBC, aggiungere queste librerie come dipendenze nel file POM. Rimuovere le librerie dal server Tomcat (in genere dalla directory *tomcat/lib*) e verificare che l'applicazione venga eseguita con funzionalità complete prima di procedere.

1. Aggiungere il file POM padre di Spring Boot al file POM. Per altre informazioni, vedere la sezione relativa alla [creazione del file POM](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#getting-started-first-application-pom) nella documentazione di Spring Boot.

1. Aggiungere Spring Boot Starter di Tomcat come dipendenza al file POM:

    ```xml
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    ```

    Anche se si tratta precedentemente di un'applicazione Tomcat, non aggiungere `war` come pacchetto di destinazione.

1. Sostituire le origini dati Tomcat con le origini dati Spring. [Configurare gli oggetti DataSource Spring](https://docs.spring.io/spring-boot/docs/current/reference/html/howto.html#howto-configure-a-datasource) per tutti i database usati dall'applicazione. Se il codice esegue query SQL dirette, modificarlo per l'[uso di JdbcTemplate](https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-using-jdbc-template). Per informazioni sulle funzionalità di accesso ai dati aggiuntive, ad esempio la gestione delle transazioni e gli strumenti CRUD, vedere la [documentazione di Spring Framework](https://docs.spring.io/spring/docs/current/spring-framework-reference/data-access.html#jdbc) e la [documentazione di Spring Data](https://docs.spring.io/spring-data/jdbc/docs/current/reference/html/#reference).

1. Anche se è possibile avere implementazioni servlet all'interno di un [contenitore di servlet incorporato](https://docs.spring.io/spring-boot/docs/current-SNAPSHOT/reference/html/spring-boot-features.html#boot-features-embedded-container), non è una scelta consigliata. Sostituire invece le [implementazioni servlet](https://docs.oracle.com/javaee/7/api/javax/servlet/http/HttpServletRequest.html) con i [controller REST](https://spring.io/guides/gs/rest-service/#_create_a_resource_controller) di Spring. Se l'applicazione usa un framework non Spring MVC, sostituirlo con Spring MVC. Per altre informazioni, vedere le [informazioni di riferimento sul controller annotato Spring MVC](https://docs.spring.io/spring-framework/docs/current/spring-framework-reference/web.html#mvc-controller).

1. Ricreare tutte le altre dipendenze JNDI con [Spring Bean](https://docs.spring.io/spring-boot/docs/current/reference/html/using-spring-boot.html#using-boot-spring-beans-and-dependency-injection). Preferire l'uso di meccanismi idiomatici di Spring, ad esempio l'uso di [Spring JMS](https://spring.io/guides/gs/messaging-jms/) per la messaggistica.

1. Sostituire le aree di autenticazione Tomcat con [Spring Security](https://docs.spring.io/spring-security/site/docs/current/reference/html5/#servlet-filters-review). Valutare se usare Azure Active Directory per la gestione delle autorizzazioni tramite [Spring Boot Starter per Active Directory](/azure/developer/java/spring-framework/spring-boot-starters-for-azure#azure-active-directory).

1. Ricreare i filtri servlet configurati in *web.xml* con [Spring Bean](https://docs.spring.io/spring-boot/docs/current/reference/html/howto.html#howto-add-a-servlet-filter-or-listener-as-spring-bean) o con l'[analisi del classpath](https://docs.spring.io/spring-boot/docs/current/reference/html/howto.html#howto-add-a-servlet-filter-or-listener-using-scanning).

1. Se l'applicazione contiene o fa riferimento a contenuti statici, ad esempio immagini o file JavaScript, è necessario spostarli in *src/main/resources* nel codice sorgente del progetto. Dopo aver spostato i file, aggiornare il codice sorgente per rimuovere tutti i riferimenti al file system locale. Usare la classe `ClassPathResource` di Spring per accedere a tali file.

Testare l'applicazione eseguendo `mvn spring-boot:run`. Verificare che l'applicazione risultante venga eseguita con funzionalità complete prima di procedere.

[!INCLUDE [migrate-steps-spring-boot-azure-spring-cloud](includes/migrate-steps-spring-boot-azure-spring-cloud.md)]

## <a name="post-migration"></a>Post-migrazione

[!INCLUDE [post-migration-spring-boot-azure-spring-cloud](includes/post-migration-spring-boot-azure-spring-cloud.md)]
