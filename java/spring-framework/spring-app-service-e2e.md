---
title: Distribuire un'app Spring/Tomcat nel servizio app con Database di Azure per MySQL
description: Esercitazione end-to-end per il servizio app Java con MySQL
author: KarlErickson
manager: barbkess
ms.author: karler
ms.date: 08/38/2019
ms.service: app-service
ms.devlang: java
ms.topic: article
ms.openlocfilehash: 0a35692d44b4c0a3d587c11e8781f3a9a3d141c9
ms.sourcegitcommit: 8d9b68a214a4b92b722df9e71afca5bc6827ce9b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/05/2019
ms.locfileid: "70383836"
---
# <a name="deploy-a-spring-app-to-app-service-with-mysql"></a>Distribuire un'app Spring nel servizio app con MySQL

Questa esercitazione illustra il processo per compilare, configurare, distribuire e ridimensionare app Web Java nel servizio app per Linux e risolverne i problemi.

L'esercitazione si basa sulla popolare app di esempio Spring PetClinic. In questo argomento si testerà in locale e quindi si distribuirà nel [servizio app di Azure](/azure/app-service/containers) una versione HSQLDB dell'app. Successivamente si configurerà e si distribuirà una versione che usa [Database di Azure per MySQL](/azure/mysql). Infine verrà illustrato come accedere ai log dell'app e aumentare il numero di istanze incrementando il numero di ruoli di lavoro che eseguono l'app.

## <a name="prerequisites"></a>Prerequisiti

* [Interfaccia della riga di comando di Azure](http://docs.microsoft.com/cli/azure/overview)
* [Java 8](http://java.oracle.com/)
* [Maven 3](http://maven.apache.org/)
* [Git](https://github.com/)
* [Tomcat](https://tomcat.apache.org/download-80.cgi)
* [Interfaccia della riga di comando di MySQL](http://dev.mysql.com/downloads/mysql/)

## <a name="get-the-sample"></a>Ottenere l'esempio

Per iniziare a usare l'app di esempio, clonare e preparare il repository di origine con i comandi seguenti.

```bash
git clone --recurse-submodules https://github.com/Azure-Samples/e2e-java-experience-in-app-service-linux.git
cd e2e-java-experience-in-app-service-linux
yes | cp -rf .prep/* .
```

## <a name="build-and-run-the-hsqldb-sample-locally"></a>Compilare ed eseguire l'esempio HSQLDB in locale

Per prima cosa si testerà l'esempio in locale usando HSQLDB come database.

Passare alla versione HSQLDB dell'esempio e quindi compilarla.

```bash
cd initial-hsqldb/spring-framework-petclinic
mvn package
```

Successivamente, impostare la variabile di ambiente TOMCAT_HOME sul percorso dell'installazione di Tomcat.

```bash
export TOMCAT_HOME=<Tomcat install directory>
```

Aggiornare quindi il file *pom.xml* per configurare Maven per una distribuzione con file WAR Tomcat. Aggiungere il codice XML seguente come figlio dell'elemento `<plugins>` esistente. Se necessario, sostituire `1.7.7` con la versione corrente del [plug-in Cargo Maven 2](https://mvnrepository.com/artifact/org.codehaus.cargo/cargo-maven2-plugin).

```xml
<plugin>
    <groupId>org.codehaus.cargo</groupId>
    <artifactId>cargo-maven2-plugin</artifactId>
    <version>1.7.7</version>
    <configuration>
        <container>
            <containerId>tomcat8x</containerId>
            <type>installed</type>
            <home>${TOMCAT_HOME}</home>
        </container>
        <configuration>
            <type>existing</type>
            <home>${TOMCAT_HOME}</home>
        </configuration>
        <deployables>
            <deployable>
                <groupId>${project.groupId}</groupId>
                <artifactId>${project.artifactId}</artifactId>
                <type>war</type>
                <properties>
                    <context>/</context>
                </properties>
            </deployable>
        </deployables>
    </configuration>
</plugin>
```

Dopo aver implementato questa configurazione, è possibile distribuire l'app localmente in Tomcat.

```bash
mvn cargo:deploy
```

Avviare quindi Tomcat.

```bash
${TOMCAT_HOME}/bin/catalina.sh run
```

È ora possibile passare con il browser a [http://localhost:8080](http://localhost:8080) per visualizzare l'app in esecuzione e acquisire familiarità con il funzionamento. Al termine, premere CTRL+C al prompt di Bash per arrestare Tomcat.

## <a name="deploy-to-azure-app-service"></a>Distribuire nel Servizio app di Azure

Dopo averne osservato l'esecuzione in locale, si distribuirà l'app in Azure.

Per prima cosa, impostare le variabili di ambiente seguenti.

```bash
export RESOURCEGROUP_NAME=<resource group>
export WEBAPP_NAME=<web app>
export WEBAPP_PLAN_NAME=${WEBAPP_NAME}-appservice-plan
export REGION=<region>
```

Maven userà questi valori per creare le risorse di Azure con i nomi specificati. Usando le variabili di ambiente, è possibile mantenere i segreti dell'account al di fuori dei file di progetto.

Aggiornare quindi il file *pom.xml* per configurare Maven per una distribuzione di Azure. Aggiungere il codice XML seguente dopo l'elemento `<plugin>` aggiunto in precedenza. Se necessario, sostituire `1.7.0` con la versione corrente del [plug-in Maven per il servizio app di Azure](/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme).

```xml
<plugin>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-webapp-maven-plugin</artifactId>
    <version>1.7.0</version>
    <configuration>
        <resourceGroup>${RESOURCEGROUP_NAME}</resourceGroup>
        <appServicePlanName>${WEBAPP_PLAN_NAME}</appServicePlanName>
        <appName>${WEBAPP_NAME}</appName>
        <region>${REGION}</region>
        <linuxRuntime>tomcat 8.5-jre8</linuxRuntime>
    </configuration>
</plugin>
```

Successivamente, accedere ad Azure.

```azurecli
az login
```

Distribuire quindi l'app nel servizio app per Linux.

```bash
mvn azure-webapp:deploy
```

È ora possibile passare a `https://<app-name>.azurewebsites.net` (dopo aver sostituito `<app-name>`) per visualizzare l'app in esecuzione.

## <a name="set-up-azure-database-for-mysql"></a>Configurare Database di Azure per MySQL

Successivamente si userà MySQL al posto di HSQLDB. Si creerà un'istanza del server MySQL in Azure e si aggiungerà un database, quindi si aggiornerà la configurazione dell'app con le nuove informazioni di connessione al database.

Per prima cosa, impostare le variabili di ambiente seguenti, necessarie nei passaggi successivi.

```bash
export MYSQL_SERVER_NAME=<server>
export MYSQL_SERVER_FULL_NAME=${MYSQL_SERVER_NAME}.mysql.database.azure.com
export MYSQL_SERVER_ADMIN_LOGIN_NAME=<admin>
export MYSQL_SERVER_ADMIN_PASSWORD=<password>
export MYSQL_DATABASE_NAME=<database>
export DOLLAR=\$
```

Creare e inizializzare quindi il server di database. Usare [az mysql up](/cli/azure/ext/db-up/mysql?view=azure-cli-latest#ext-db-up-az-mysql-up) per la configurazione iniziale. Usare quindi [az mysql server configuration set](/cli/azure/mysql/server/configuration?view=azure-cli-latest#az-mysql-server-configuration-set) per aumentare il timeout della connessione e impostare il fuso orario del server.

```azurecli
az mysql up \
    --resource-group ${RESOURCEGROUP_NAME} \
    --server-name ${MYSQL_SERVER_NAME} \
    --database-name ${MYSQL_DATABASE_NAME} \
    --admin-user ${MYSQL_SERVER_ADMIN_LOGIN_NAME} \
    --admin-password ${MYSQL_SERVER_ADMIN_PASSWORD}

az mysql server configuration set --name wait_timeout \
    --resource-group ${RESOURCEGROUP_NAME} \
    --server ${MYSQL_SERVER_NAME} --value 2147483

az mysql server configuration set --name time_zone \
    --resource-group ${RESOURCEGROUP_NAME} \
    --server ${MYSQL_SERVER_NAME} --value -8:00
```

Usare quindi l'interfaccia della riga di comando di MySQL per creare il database.

```bash
mysql -u ${MYSQL_SERVER_ADMIN_LOGIN_NAME} \
 -h ${MYSQL_SERVER_FULL_NAME} -P 3306 -p
```

Al prompt dell'interfaccia della riga di comando di MySQL eseguire questo comando, sostituendo `<database name>` con lo stesso valore specificato in precedenza per la variabile di ambiente `MYSQL_DATABASE_NAME`:

```console
CREATE DATABASE <database name>;
```

MySQL è ora pronto per l'uso.

## <a name="configure-the-app-for-mysql"></a>Configurare l'app per MySQL

Successivamente si aggiungeranno le informazioni di connessione alla versione MySQL dell'app e quindi si eseguirà la distribuzione nel servizio app.

Per prima cosa, passare alla cartella corretta al prompt di Bash.

```bash
cd ../../initial-mysql/spring-framework-petclinic
```

Aggiornare quindi il file *pom.xml* per impostare MySQL come configurazione attiva. Rimuovere l'elemento `<activation>` dal profilo HSQLDB e inserirlo invece nel profilo MySQL, come illustrato di seguito. Il resto del frammento mostra la configurazione esistente. Si noti come le variabili di ambiente impostate in precedenza vengono usate da Maven per configurare l'accesso a MySQL.

```xml
<profile>
    <id>MySQL</id>
    <activation>
        <activeByDefault>true</activeByDefault>
    </activation>
    <properties>
        <db.script>mysql</db.script>
        <jpa.database>MYSQL</jpa.database>
        <jdbc.driverClassName>com.mysql.jdbc.Driver</jdbc.driverClassName>
        <jdbc.url>jdbc:mysql://${DOLLAR}{MYSQL_SERVER_FULL_NAME}:3306/${DOLLAR}{MYSQL_DATABASE_NAME}?useUnicode=true</jdbc.url>
        <jdbc.username>${DOLLAR}{MYSQL_SERVER_ADMIN_LOGIN_NAME}</jdbc.username>
        <jdbc.password>${DOLLAR}{MYSQL_SERVER_ADMIN_PASSWORD}</jdbc.password>
    </properties>
    ...
</profile>
```

Aggiornare quindi il file *pom.xml* per configurare Maven per una distribuzione di Azure e per l'uso di MySQL. Aggiungere il codice XML seguente dopo l'elemento `<plugin>` aggiunto in precedenza. Se necessario, sostituire `1.7.0` con la versione corrente del [plug-in Maven per il servizio app di Azure](/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme).

```xml
<plugin>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-webapp-maven-plugin</artifactId>
    <version>1.7.0</version>
    <configuration>

        <resourceGroup>${RESOURCEGROUP_NAME}</resourceGroup>
        <appServicePlanName>${WEBAPP_PLAN_NAME}</appServicePlanName>
        <appName>${WEBAPP_NAME}</appName>
        <region>${REGION}</region>

        <linuxRuntime>tomcat 8.5-jre8</linuxRuntime>

        <appSettings>
            <property>
                <name>MYSQL_SERVER_FULL_NAME</name>
                <value>${MYSQL_SERVER_FULL_NAME}</value>
            </property>
            <property>
                <name>MYSQL_SERVER_ADMIN_LOGIN_NAME</name>
                <value>${MYSQL_SERVER_ADMIN_LOGIN_NAME}</value>
            </property>
            <property>
                <name>MYSQL_SERVER_ADMIN_PASSWORD</name>
                <value>${MYSQL_SERVER_ADMIN_PASSWORD}</value>
            </property>
            <property>
                <name>MYSQL_DATABASE_NAME</name>
                <value>${MYSQL_DATABASE_NAME}</value>
            </property>
        </appSettings>

    </configuration>
</plugin>
```

Successivamente, compilare l'app e quindi testarla in locale distribuendola ed eseguendola con Tomcat.

```bash
mvn package
mvn cargo:deploy
${TOMCAT_HOME}/bin/catalina.sh run
```

È ora possibile visualizzare l'app in locale all'indirizzo [http://localhost:8080](http://localhost:8080). L'app avrà lo stesso aspetto e lo stesso comportamento osservati in precedenza, ma userà Database di Azure per MySQL anziché HSQLDB. Al termine, premere CTRL+C al prompt di Bash per arrestare Tomcat.

Distribuire infine l'app nel servizio app.

```bash
mvn azure-webapp:deploy
```

È ora possibile passare a `https://<app-name>.azurewebsites.net` per visualizzare l'app in esecuzione con il servizio app e Database di Azure per MySQL.

## <a name="access-the-app-logs"></a>Accedere ai log dell'app

Se è necessario risolvere problemi, è possibile esaminare i log dell'app. Per aprire il flusso remoto di log nel computer locale, usare il comando seguente.

```azurecli
az webapp log tail --name ${WEBAPP_NAME} \
    --resource-group ${RESOURCEGROUP_NAME}
```

Dopo aver visualizzato i log, selezionare CTRL+C per interrompere il flusso.

Il flusso di log è disponibile anche all'indirizzo `https://<app-name>.scm.azurewebsites.net/api/logstream`.

## <a name="scale-out"></a>Scalabilità orizzontale

Per supportare un maggiore traffico verso l'app, è possibile aumentare il numero di istanze con il comando seguente.

```azurecli
az appservice plan update --number-of-workers 2 \
    --name ${WEBAPP_PLAN_NAME} \
    --resource-group ${RESOURCEGROUP_NAME}
```

Congratulazioni! È stata compilata un'app Web Java e ne è stato aumentato il numero di istanze usando Spring Framework, JSP, Spring Data, Hibernate, JDBC, il servizio app per Linux e Database di Azure per MySQL.

## <a name="clean-up-resources"></a>Pulire le risorse

Nelle sezioni precedenti sono state create risorse di Azure in un gruppo di risorse. Se non si prevede di usare queste risorse in futuro, eliminare il gruppo di risorse eseguendo questo comando:

```azurecli
az group delete --name ${RESOURCEGROUP_NAME}
```

## <a name="next-steps"></a>Passaggi successivi

Successivamente, vedere le altre opzioni di configurazione e CI/CD disponibili per Java con il servizio app.

> [!div class="nextstepaction"]
> [Configurare un'app Java in Linux per il servizio app di Azure](/azure/app-service/containers/configure-language-java)
> [!div class="nextstepaction"]
> [Compilare e distribuire un'app Web Java con Azure Pipelines](/azure/devops/pipelines/ecosystems/java-webapp?view=azure-devops&tabs=java-tomcat)
> [!div class="nextstepaction"]
> [Eseguire la distribuzione nel servizio app di Azure con il plug-in Jenkins](/azure/jenkins/deploy-jenkins-app-service-plugin)
