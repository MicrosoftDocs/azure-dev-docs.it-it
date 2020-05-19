---
author: mriem
ms.author: manriem
ms.date: 2/28/2020
ms.openlocfilehash: 08389c5802d2f6fef5360ac09adb11bb6494beb6
ms.sourcegitcommit: 226ebca0d0e3b918928f58a3a7127be49e4aca87
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/08/2020
ms.locfileid: "82988813"
---
### <a name="create-a-docker-image-for-wildfly"></a>Creare un'immagine Docker per WildFly

Per creare un Dockerfile, è necessario soddisfare i prerequisiti seguenti:

* Una versione supportata di JDK.
* Un'installazione di WildFly.
* Le opzioni di runtime della JVM.
* Un modo per passare le variabili di ambiente (se applicabile).

È quindi possibile eseguire i passaggi descritti nelle sezioni seguenti, ove applicabile. È possibile usare il [repository di avvio rapido di contenitori WildFly](https://github.com/Azure/wildfly-container-quickstart) come punto di partenza per il Dockerfile e l'applicazione Web.

1. [Configurare KeyVault FlexVolume](#configure-keyvault-flexvolume)
2. [Configurare le origini dati](#set-up-data-sources)
3. [Configurare le risorse JNDI](#set-up-jndi-resources)
4. [Verificare la configurazione di WildFly](#review-wildfly-configuration)

#### <a name="configure-keyvault-flexvolume"></a>Configurare KeyVault FlexVolume

Creare un'istanza di Azure KeyVault e popolarla con i segreti necessari. Per altre informazioni, vedere [Avvio rapido: Impostare e recuperare un segreto da Azure Key Vault usando l'interfaccia della riga di comando di Azure](/azure/key-vault/quick-create-cli). Configurare quindi [KeyVault FlexVolume](https://github.com/Azure/kubernetes-keyvault-flexvol/blob/master/README.md) per rendere tali segreti accessibili ai pod.

Sarà anche necessario aggiornare lo script di avvio usato per eseguire il bootstrap di WildFly. Questo script deve importare i certificati nell'archivio chiavi usato da WildFly prima di avviare il server.

#### <a name="set-up-data-sources"></a>Configurare le origini dati

Per configurare WildFly per l'accesso a un'origine dati, è necessario aggiungere il file JAR del driver JDBC all'immagine Docker e quindi eseguire i comandi dell'interfaccia della riga di comando di JBoss appropriati. Questi comandi devono configurare l'origine dati durante la creazione dell'immagine Docker.

La procedura seguente fornisce le istruzioni per PostgreSQL, MySQL e SQL Server.

1. Scaricare il driver JDBC per [PostgreSQL](https://jdbc.postgresql.org/download.html), [MySQL](https://dev.mysql.com/downloads/connector/j/) o [SQL Server](/sql/connect/jdbc/download-microsoft-jdbc-driver-for-sql-server).

    Decomprimere l'archivio scaricato per ottenere il file con estensione jar del driver.

1. Creare un file con un nome come `module.xml` e aggiungere il markup seguente. Sostituire il segnaposto `<module name>` (incluse le parentesi angolari) con `org.postgres` per PostgreSQL, `com.mysql` per MySQL o `com.microsoft` per SQL Server. Sostituire `<JDBC .jar file path>` con il nome del file con estensione jar del passaggio precedente, incluso il percorso completo della posizione in cui si vuole inserire il file nell'immagine Docker, ad esempio in `/opt/database`.

    ```xml
    <?xml version="1.0" ?>
    <module xmlns="urn:jboss:module:1.1" name="<module name>">
        <resources>
           <resource-root path="<JDBC .jar file path>" />
        </resources>
        <dependencies>
            <module name="javax.api"/>
            <module name="javax.transaction.api"/>
        </dependencies>
    </module>
    ```

1. Creare un file con un nome come `datasource-commands.cli` e aggiungere il codice seguente. Sostituire `<JDBC .jar file path>` con il valore usato nel passaggio precedente. Sostituire `<module file path>` con il nome file e il percorso del passaggio precedente, ad esempio `/opt/database/module.xml`.

    **PostgreSQL**

    ```console
    batch
    module add --name=org.postgres --resources=<JDBC .jar file path> --module-xml=<module file path>
    /subsystem=datasources/jdbc-driver=postgres:add(driver-name=postgres,driver-module-name=org.postgres,driver-class-name=org.postgresql.Driver,driver-xa-datasource-class-name=org.postgresql.xa.PGXADataSource)
    data-source add --name=postgresDS --driver-name=postgres --jndi-name=java:jboss/datasources/postgresDS --connection-url=$DATABASE_CONNECTION_URL --user-name=$DATABASE_SERVER_ADMIN_FULL_NAME --password=$DATABASE_SERVER_ADMIN_PASSWORD --use-ccm=true --max-pool-size=5 --blocking-timeout-wait-millis=5000 --enabled=true --driver-class=org.postgresql.Driver --exception-sorter-class-name=org.jboss.jca.adapters.jdbc.extensions.postgres.PostgreSQLExceptionSorter --jta=true --use-java-context=true --valid-connection-checker-class-name=org.jboss.jca.adapters.jdbc.extensions.postgres.PostgreSQLValidConnectionChecker
    reload
    run batch
    shutdown
    ```

    **MySQL**

    ```console
    batch
    module add --name=com.mysql --resources=<JDBC .jar file path> --module-xml=<module file path>
    /subsystem=datasources/jdbc-driver=mysql:add(driver-name=mysql,driver-module-name=com.mysql,driver-class-name=com.mysql.cj.jdbc.Driver)
    data-source add --name=mysqlDS --jndi-name=java:jboss/datasources/mysqlDS --connection-url=$DATABASE_CONNECTION_URL --driver-name=mysql --user-name=$DATABASE_SERVER_ADMIN_FULL_NAME --password=$DATABASE_SERVER_ADMIN_PASSWORD --use-ccm=true --max-pool-size=5 --blocking-timeout-wait-millis=5000 --enabled=true --driver-class=com.mysql.cj.jdbc.Driver --jta=true --use-java-context=true --exception-sorter-class-name=com.mysql.cj.jdbc.integration.jboss.ExtendedMysqlExceptionSorter
    reload
    run batch
    shutdown
    ```

    **SQL Server**

    ```console
    batch
    module add --name=com.microsoft --resources=<JDBC .jar file path> --module-xml=<module file path>
    /subsystem=datasources/jdbc-driver=sqlserver:add(driver-name=sqlserver,driver-module-name=com.microsoft,driver-class-name=com.microsoft.sqlserver.jdbc.SQLServerDriver,driver-datasource-class-name=com.microsoft.sqlserver.jdbc.SQLServerDataSource)
    data-source add --name=sqlDS --jndi-name=java:jboss/datasources/sqlDS --driver-name=sqlserver --connection-url=$DATABASE_CONNECTION_URL --validate-on-match=true --background-validation=false --valid-connection-checker-class-name=org.jboss.jca.adapters.jdbc.extensions.mssql.MSSQLValidConnectionChecker --exception-sorter-class-name=org.jboss.jca.adapters.jdbc.extensions.mssql.MSSQLExceptionSorter
    reload
    run batch
    shutdown
    ```

1. Aggiornare la configurazione dell'origine dati JTA per l'applicazione:

    Aprire il file `src/main/resources/META-INF/persistence.xml` per l'app e trovare l'elemento `<jta-data-source>`. Sostituire il relativo contenuto come illustrato di seguito:

    **PostgreSQL**

    ```xml
    <jta-data-source>java:jboss/datasources/postgresDS</jta-data-source>
    ```

    **MySQL**

    ```xml
    <jta-data-source>java:jboss/datasources/mysqlDS</jta-data-source>
    ```

    **SQL Server**

    ```xml
    <jta-data-source>java:jboss/datasources/postgresDS</jta-data-source>
    ```

1. Aggiungere quanto segue al `Dockerfile` in modo che l'origine dati venga creata quando si crea l'immagine Docker

    ```console
    RUN /bin/bash -c '<WILDFLY_INSTALL_PATH>/bin/standalone.sh --start-mode admin-only &' && \
    sleep 30 && \
    <WILDFLY_INSTALL_PATH>/bin/jboss-cli.sh -c --file=/opt/database/datasource-commands.cli && \
    sleep 30
    ```

1. Determinare il valore di `DATABASE_CONNECTION_URL` da usare perché è diverso per ogni server di database ed è diverso dai valori nel portale di Azure. I formati di URL riportati di seguito sono necessari per l'uso da parte di WildFly:

    **PostgreSQL**

    ```console
    jdbc:postgresql://<database server name>:5432/<database name>?ssl=true
    ```

    **MySQL**

    ```console
    jdbc:mysql://<database server name>:3306/<database name>?ssl=true\&useLegacyDatetimeCode=false\&serverTimezone=GMT
    ```

    **SQL Server**

    ```console
    jdbc:sqlserver://<database server name>:1433;database=<database name>;user=<admin name>;password=<admin password>;encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;
    ```

1. Quando si crea la distribuzione YAML in una fase successiva, sarà necessario passare le variabili di ambiente `DATABASE_CONNECTION_URL`, `DATABASE_SERVER_ADMIN_FULL_NAME` e `DATABASE_SERVER_ADMIN_PASSWORD` con i valori appropriati.

Per altre informazioni sulla configurazione della connettività del database con WildFly, vedere [PostgreSQL](https://developer.jboss.org/blogs/amartin-blog/2012/02/08/how-to-set-up-a-postgresql-jdbc-driver-on-jboss-7), [MySQL](https://docs.jboss.org/jbossas/docs/Installation_And_Getting_Started_Guide/5/html/Using_other_Databases.html#Using_other_Databases-Using_MySQL_as_the_Default_DataSource) o [SQL Server](https://docs.jboss.org/jbossas/docs/Installation_And_Getting_Started_Guide/5/html/Using_other_Databases.html#d0e3898).

#### <a name="set-up-jndi-resources"></a>Configurare le risorse JNDI

Per impostare ogni risorsa JNDI che è necessario configurare in WildFly, si useranno in genere i passaggi seguenti:

1. Scaricare i file JAR necessari e copiarli nell'immagine Docker.
2. Creare un file *module.xml* di WildFly che fa riferimento a tali file JAR.
3. Creare qualsiasi configurazione necessaria per la risorsa JNDI specifica.
4. Creare uno script dell'interfaccia della riga di comando di JBoss da usare durante la compilazione Docker per registrare la risorsa JNDI.
5. Aggiungere tutto al Dockerfile.
6. Passare le variabili di ambiente appropriate nella distribuzione YAML.

L'esempio seguente illustra i passaggi necessari per creare la risorsa JNDI per la connettività JMS al bus di servizio di Azure.

1. Scaricare il [provider JMS di Apache Qpid](https://qpid.apache.org/components/jms/index.html)

    Decomprimere l'archivio scaricato per ottenere il file con estensione jar.

1. Creare un file con un nome come `module.xml` e aggiungere il markup seguente in `/opt/servicebus`. Verificare che i numeri di versione dei file JAR siano allineati ai nomi dei file JAR del passaggio precedente.

    ```xml
    <?xml version="1.0" ?>
    <module xmlns="urn:jboss:module:1.1" name="org.jboss.genericjms.provider">
     <resources>
      <resource-root path="proton-j-0.31.0.jar"/>
      <resource-root path="qpid-jms-client-0.40.0.jar"/>
      <resource-root path="slf4j-log4j12-1.7.25.jar"/>
      <resource-root path="slf4j-api-1.7.25.jar"/>
      <resource-root path="log4j-1.2.17.jar"/>
      <resource-root path="netty-buffer-4.1.32.Final.jar" />
      <resource-root path="netty-codec-4.1.32.Final.jar" />
      <resource-root path="netty-codec-http-4.1.32.Final.jar" />
      <resource-root path="netty-common-4.1.32.Final.jar" />
      <resource-root path="netty-handler-4.1.32.Final.jar" />
      <resource-root path="netty-resolver-4.1.32.Final.jar" />
      <resource-root path="netty-transport-4.1.32.Final.jar" />
      <resource-root path="netty-transport-native-epoll-4.1.32.Final-linux-x86_64.jar" />
      <resource-root path="netty-transport-native-kqueue-4.1.32.Final-osx-x86_64.jar" />
      <resource-root path="netty-transport-native-unix-common-4.1.32.Final.jar" />
      <resource-root path="qpid-jms-discovery-0.40.0.jar" />
     </resources>
     <dependencies>
      <module name="javax.api"/>
      <module name="javax.jms.api"/>
     </dependencies>
    </module>
    ```

1. Crea un file `jndi.properties` in `/opt/servicebus`.

    ```console
    connectionfactory.${MDB_CONNECTION_FACTORY}=amqps://${DEFAULT_SBNAMESPACE}.servicebus.windows.net?amqp.idleTimeout=120000&jms.username=${SB_SAS_POLICY}&jms.password=${SB_SAS_KEY}
    queue.${MDB_QUEUE}=${SB_QUEUE}
    topic.${MDB_TOPIC}=${SB_TOPIC}
    ```

1. Creare un file con un nome come `servicebus-commands.cli` e aggiungere il codice seguente.

    ```console
    batch
    /subsystem=ee:write-attribute(name=annotation-property-replacement,value=true)
    /system-property=property.mymdb.queue:add(value=myqueue)
    /system-property=property.connection.factory:add(value=java:global/remoteJMS/SBF)
    /subsystem=ee:list-add(name=global-modules, value={"name" => "org.jboss.genericjms.provider", "slot" =>"main"}
    /subsystem=naming/binding="java:global/remoteJMS":add(binding-type=external-context,module=org.jboss.genericjms.provider,class=javax.naming.InitialContext,environment=[java.naming.factory.initial=org.apache.qpid.jms.jndi.JmsInitialContextFactory,org.jboss.as.naming.lookup.by.string=true,java.naming.provider.url=/opt/servicebus/jndi.properties])
    /subsystem=resource-adapters/resource-adapter=generic-ra:add(module=org.jboss.genericjms,transaction-support=XATransaction)
    /subsystem=resource-adapters/resource-adapter=generic-ra/connection-definitions=sbf-cd:add(class-name=org.jboss.resource.adapter.jms.JmsManagedConnectionFactory, jndi-name=java:/jms/${MDB_CONNECTION_FACTORY})
    /subsystem=resource-adapters/resource-adapter=generic-ra/connection-definitions=sbf-cd/config-properties=ConnectionFactory:add(value=${MDB_CONNECTION_FACTORY})
    /subsystem=resource-adapters/resource-adapter=generic-ra/connection-definitions=sbf-cd/config-properties=JndiParameters:add(value="java.naming.factory.initial=org.apache.qpid.jms.jndi.JmsInitialContextFactory;java.naming.provider.url=/opt/servicebus/jndi.properties")
    /subsystem=resource-adapters/resource-adapter=generic-ra/connection-definitions=sbf-cd:write-attribute(name=security-application,value=true)
    /subsystem=ejb3:write-attribute(name=default-resource-adapter-name, value=generic-ra)
    run-batch
    reload
    shutdown
    ```

1. Aggiungere quanto segue a `Dockerfile` in modo che la risorsa JNDI venga creata quando si crea l'immagine Docker

    ```console
    RUN /bin/bash -c '<WILDFLY_INSTALL_PATH>/bin/standalone.sh --start-mode admin-only &' && \
    sleep 30 && \
    <WILDFLY_INSTALL_PATH>/bin/jboss-cli.sh -c --file=/opt/servicebus/servicebus-commands.cli && \
    sleep 30
    ```

1. Quando si crea la distribuzione YAML in una fase successiva, sarà necessario passare le variabili di ambiente `MDB_CONNECTION_FACTORY`, `DEFAULT_SBNAMESPACE`, `SB_SAS_POLICY`, `SB_SAS_KEY`, `MDB_QUEUE`, `SB_QUEUE`, `MDB_TOPIC` e `SB_TOPIC` con i valori appropriati.

#### <a name="review-wildfly-configuration"></a>Verificare la configurazione di WildFly

Leggere la [guida dell'amministratore di WildFly](https://docs.wildfly.org/18/Admin_Guide.html) per informazioni su eventuali passaggi di premigrazione necessari non illustrati nelle istruzioni precedenti.
