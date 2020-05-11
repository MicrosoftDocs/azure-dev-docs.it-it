---
title: Distribuire un'app Web Spring Boot basata su file JAR nel servizio app di Azure in Linux
description: Informazioni su come distribuire un file JAR di app Spring Boot in Linux con il plug-in Maven per app Web di Azure.
services: app-service
documentationcenter: java
ms.date: 12/19/2018
ms.service: app-service
ms.topic: article
ms.custom: seo-java-july2019, seo-java-august2019, seo-java-september2019
ms.openlocfilehash: da40d293d0ad42286e6dfb2fd47074a4ae218b6b
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/05/2020
ms.locfileid: "81673087"
---
# <a name="deploy-a-spring-boot-jar-file-app-to-azure-app-service-with-maven-and-azure-on-linux"></a>Distribuire un'app Spring Boot basata su file JAR nel servizio app di Azure con Maven e Azure in Linux

In questa guida di avvio rapido si userà il [plug-in Maven per app Web del servizio app di Azure](/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme) per distribuire un'applicazione Spring Boot inserita in un pacchetto JAR Java SE nel [servizio app di Azure in Linux](/azure/app-service/containers/). La distribuzione con Java SE sarà preferibile rispetto a [Tomcat e file WAR](/azure/app-service/containers/quickstart-java) quando si vogliono consolidare le dipendenze, il runtime e la configurazione dell'app in un singolo artefatto distribuibile.

Se non si ha una sottoscrizione di Azure, creare un [account gratuito](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) prima di iniziare.

## <a name="prerequisites"></a>Prerequisiti

Per completare i passaggi di questa esercitazione, devono essere installati e configurati gli strumenti seguenti:

* [Interfaccia della riga di comando di Azure](/cli/azure/), in locale o tramite [Azure Cloud Shell](https://shell.azure.com).
* Java Development Kit (JDK) supportato. Per altre informazioni sulle versioni di JDK utilizzabili per lo sviluppo in Azure, vedere <https://aka.ms/azure-jdks>.
* Apache [Maven](https://maven.apache.org/), versione 3.
* Un client [Git](https://git-scm.com/downloads).

## <a name="install-and-sign-in-to-azure-cli"></a>Installare l'interfaccia della riga di comando di Azure ed eseguire l'accesso

Il modo più semplice e facile per distribuire l'applicazione Spring Boot tramite il plug-in Maven consiste nell'usare l'[interfaccia della riga di comando di Azure](/cli/azure/).

Accedere all'account Azure con l'interfaccia della riga di comando di Azure:
   
   ```shell
   az login
   ```
   
Seguire le istruzioni per completare il processo di accesso.

## <a name="clone-the-sample-app"></a>Clonare l'app di esempio

In questa sezione sarà clonata e testata in locale un'applicazione Spring Boot completata.

1. Aprire un prompt dei comandi o una finestra del terminale e creare una directory locale in cui salvare l'applicazione Spring Boot, quindi passare a tale directory. Ad esempio:
   ```shell
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   -- o --
   ```shell
   mkdir ~/SpringBoot
   cd ~/SpringBoot
   ```

1. Clonare il progetto di esempio di [introduzione a Spring Boot] nella directory creata, ad esempio:
   ```shell
   git clone https://github.com/spring-guides/gs-spring-boot
   ```

1. Passare alla directory del progetto completato. Ad esempio:
   ```shell
   cd gs-spring-boot/complete
   ```

1. Compilare il file JAR usando Maven. Ad esempio:
   ```shell
   mvn clean package
   ```

1. Dopo che è stata creata, avviare l'app Web con Maven, ad esempio:
   ```shell
   mvn spring-boot:run
   ```

1. Testare l'app Web esplorandola localmente tramite un Web browser. Se è disponibile curl, ad esempio, è possibile usare il comando seguente:
   ```shell
   curl http://localhost:8080
   ```

1. Dovrebbe essere visualizzato il messaggio **Greetings from Spring Boot!**

## <a name="configure-maven-plugin-for-azure-app-service"></a>Configurare il plug-in Maven per il servizio app di Azure

In questa sezione si configurerà il progetto Spring Boot `pom.xml` in modo che Maven possa distribuire l'app nel servizio app di Azure in Linux.

1. Aprire `pom.xml` in un editor di codice.

2. Nella sezione `<build>` di pom.xml aggiungere la voce `<plugin>` seguente all'interno del tag `<plugins>`.

   ```xml
   <plugin>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-webapp-maven-plugin</artifactId>
    <version>1.9.0</version>
   </plugin>
   ```

3. È quindi possibile configurare la distribuzione, eseguire il comando maven seguente al prompt dei comandi e usare il **numero** scegliere queste opzioni nel prompt:
    * **OS**: linux  
    * **javaVersion**: Java 8    
    
   ```cmd
   mvn azure-webapp:config
   ```

   Quando viene visualizzato il prompt **Confirm (Y/N)** , premere **'y'** per completare la configurazione.

   ```cmd
   ~@Azure:~/gs-spring-boot/complete$ mvn azure-webapp:config
   [INFO] Scanning for projects...
   [INFO]
   [INFO] -----------------< org.springframework:gs-spring-boot >-----------------
   [INFO] Building gs-spring-boot 0.1.0
   [INFO] --------------------------------[ jar ]---------------------------------
   [INFO]
   [INFO] --- azure-webapp-maven-plugin:1.9.0:config (default-cli) @ gs-spring-boot ---
   [WARNING] The plugin may not work if you change the os of an existing webapp.
   Define value for OS(Default: Linux):
   1. linux [*]
   2. windows
   3. docker
   Enter index to use:
   Define value for javaVersion(Default: Java 8):
   1. Java 11
   2. Java 8 [*]
   Enter index to use:
   Please confirm webapp properties
   AppName : gs-spring-boot-1559091271202
   ResourceGroup : gs-spring-boot-1559091271202-rg
   Region : westeurope
   PricingTier : Premium_P1V2
   OS : Linux
   RuntimeStack : JAVA 8-jre8
   Deploy to slot : false
   Confirm (Y/N)? : Y
   ```

4. Aggiungere la sezione `<appSettings>` alla sezione `<configuration>` di `<azure-webapp-maven-plugin>` per essere in ascolto sulla porta *80*.

   ```xml
   <plugin>
       <groupId>com.microsoft.azure</groupId>
       <artifactId>azure-webapp-maven-plugin</artifactId>
       <version>1.9.0</version>
       <configuration>
          <schemaVersion>V2</schemaVersion>
          <resourceGroup>gs-spring-boot-1559091271202-rg</resourceGroup>
          <appName>gs-spring-boot-1559091271202</appName>
          <region>westeurope</region>
          <pricingTier>P1V2</pricingTier>
          <runtime>
            <os>linux</os>
            <javaVersion>jre8</javaVersion>
            <webContainer>jre8</webContainer>
          </runtime>
          <!-- Begin of App Settings  -->
          <appSettings>
             <property>
                   <name>JAVA_OPTS</name>
                   <value>-Dserver.port=80</value>
             </property>
          </appSettings>
          <!-- End of App Settings  -->
          <deployment>
            <resources>
              <resource>
                <directory>${project.basedir}/target</directory>
                <includes>
                  <include>*.jar</include>
               includes>
              </resource>
            </resources>
          </deployment>
         </configuration>
   </plugin>
   ```

## <a name="deploy-the-app-to-azure"></a>Distribuire l'app in Azure

Dopo aver configurato tutte le impostazioni nelle sezioni precedenti di questo articolo, è possibile distribuire l'app Web in Azure. A tale scopo, seguire questa procedura:

1. Se sono state apportate modifiche al file *pom.xml*, dalla finestra del terminale o al prompt dei comandi usato in precedenza ricompilare il file JAR usando Maven. Ad esempio:
   ```shell
   mvn clean package
   ```

1. Distribuire l'app Web in Azure usando Maven. Ad esempio:
   ```shell
   mvn azure-webapp:deploy
   ```

Maven distribuirà l'app Web in Azure. Se l'app Web o il piano dell'app Web non esiste già, verrà creato. Potrebbero essere necessari alcuni minuti prima che l'app Web sia visibile nell'URL indicato nell'output. Passare all'URL in un Web browser.  Dovrebbe essere visualizzato il messaggio Greetings from Spring Boot!

Dopo che è stata distribuita, l'app Web potrà essere gestita tramite il [Azure portal].

* L'app Web sarà elencata in **Servizi app**:

   ![App Web elencata in Servizi app nel portale di Azure][AP01]

* L'URL dell'app Web sarà riportato nella **panoramica** dell'app Web:

   ![Trovare l'URL per l'app Web in Servizi app nel portale di Azure][AP02]

Verificare il completamento della distribuzione con lo stesso comando curl eseguito in precedenza, usando l'URL dell'app Web riportato nel portale invece di `localhost`. Dovrebbe essere visualizzato il messaggio **Greetings from Spring Boot!** 

## <a name="clean-up-resources"></a>Pulire le risorse
Quando non sono più necessarie, eseguire la pulizia delle risorse di Azure distribuite eliminando il gruppo di risorse.

- Nel portale di Azure selezionare Gruppo di risorse nel menu a sinistra.
- Immettere **gs-spring-boot-** nel campo **Filtra per nome**. Il gruppo di risorse creato in questa esercitazione dovrebbe avere questo prefisso.
- Selezionare il gruppo di risorse creato in questa esercitazione.
- Selezionare Elimina gruppo di risorse nel menu in alto.

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni su Spring e Azure, passare al centro di documentazione di Spring in Azure.

> [!div class="nextstepaction"]
> [Spring in Azure](/azure/developer/java/spring-framework)

### <a name="additional-esources"></a>Risorse aggiuntive

Per altre informazioni sulle varie tecnologie illustrate in questo articolo, vedere gli articoli seguenti:

* [Maven Plugin for Azure Web Apps (Plug-in Maven per App Web di Azure)]

* [Come usare il plug-in Maven per App Web di Azure per distribuire un'app Spring Boot in contenitore in Azure](deploy-containerized-spring-boot-java-app-with-maven-plugin.md)

* [Creare un'entità servizio di Azure con l'interfaccia della riga di comando di Azure 2.0](/cli/azure/create-an-azure-service-principal-azure-cli)

* [Informazioni di riferimento sulle impostazioni di Maven](https://maven.apache.org/settings.html)

<!-- URL List -->

[Azure Command-Line Interface (CLI)]: /cli/azure/overview
[Azure for Java Developers]: /azure/developer/java/
[Azure portal]: https://portal.azure.com/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Working with Azure DevOps and Java]: /azure/devops/
[Maven]: http://maven.apache.org/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Introduzione a Spring Boot]: https://github.com/spring-guides/gs-spring-boot
[Spring Framework]: https://spring.io/
[Maven Plugin for Azure Web Apps (Plug-in Maven per App Web di Azure)]: /java/api/overview/azure/maven/azure-webapp-maven-plugin/readme

[Java Development Kit (JDK)]: https://aka.ms/azure-jdks
<!-- http://www.oracle.com/technetwork/java/javase/downloads/ -->

<!-- IMG List -->


[AP01]: media/deploy-spring-boot-java-app-with-maven-plugin/web-app-listed-azure-portal.png
[AP02]: media/deploy-spring-boot-java-app-with-maven-plugin/determine-web-app-url.png