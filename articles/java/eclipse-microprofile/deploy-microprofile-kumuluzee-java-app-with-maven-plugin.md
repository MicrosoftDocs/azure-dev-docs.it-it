---
title: Distribuire un'app Web KumuluzEE in Servizio app di Azure con Maven
description: Informazioni su come distribuire un'app KumuluzEE in Servizio app in Linux con il plug-in Maven per App Web di Azure.
services: app-service
documentationcenter: java
ms.date: 06/10/2020
ms.service: app-service
ms.topic: article
ms.custom: ''
ms.openlocfilehash: fee8a9b121914e3360b19726b6069a6778cfea41
ms.sourcegitcommit: 0d2ea78f18430c845a32e0d2311427ab81033465
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/23/2020
ms.locfileid: "97754177"
---
# <a name="deploy-a-kumuluzee-web-app-to-azure-app-service-with-maven"></a>Distribuire un'app Web KumuluzEE in Servizio app di Azure con Maven

In questa guida di avvio rapido si userà il [plug-in Maven per app Web del servizio app di Azure](https://github.com/microsoft/azure-maven-plugins/blob/develop/azure-webapp-maven-plugin/README.md) per distribuire un'applicazione KumuluzEE in [Servizio app di Azure in Linux](/azure/app-service/containers/). La distribuzione con Java SE sarà preferibile rispetto a [Tomcat e file WAR](/azure/app-service/containers/quickstart-java) quando si vogliono consolidare le dipendenze, il runtime e la configurazione dell'app in un singolo artefatto distribuibile.

Se non si ha una sottoscrizione di Azure, creare un [account gratuito](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) prima di iniziare.

## <a name="prerequisites"></a>Prerequisiti

* [Interfaccia della riga di comando di Azure](/cli/azure/), in locale o tramite [Azure Cloud Shell](https://shell.azure.com).
* Java Development Kit (JDK) supportato. Per altre informazioni sulle versioni di JDK utilizzabili per lo sviluppo in Azure, vedere <https://aka.ms/azure-jdks>.
* Apache [Maven](https://maven.apache.org/), versione 3.

## <a name="install-and-sign-in-to-azure-cli"></a>Installare l'interfaccia della riga di comando di Azure ed eseguire l'accesso

Il modo più semplice e facile per distribuire l'applicazione KumuluzEE tramite il plug-in Maven consiste nell'usare l'[interfaccia della riga di comando di Azure](/cli/azure/).

Accedere all'account Azure con l'interfaccia della riga di comando di Azure:

```shell
az login
```

Seguire le istruzioni per completare il processo di accesso.

## <a name="create-sample-app-from-microprofile-starter"></a>Creare un'app di esempio da MicroProfile Starter

In questa sezione si creerà un'applicazione KumuluzEE e la si testerà in locale.

### <a name="create-java-se-8-base-project"></a>Creare un progetto di base Java SE 8

1. Aprire il Web browser e accedere al sito [MicroProfile Starter](https://start.microprofile.io/).

   ![MicroProfile Starter per KumuluzEE](./media/kumuluzee/microprofile-starter-kumuluzee.png)

2. Immettere o selezionare i valori per i campi come indicato di seguito.  

   |  Campo  |  Valore  |
   | ---- | ---- |
   |  groupId  |  com.microsoft.azure.samples.kumuluzee  |
   |  artifactId  |  kumuluzEE-hello-azure  |
   |  MicroProfile Version (Versione di MicroProfile)  |  MP 3.2  |
   |  Java SE Version (Versione di Java SE)  |  Java 8  |
   |  MicroProfile Runtime (Runtime di MicroProfile)  |  KumuluzEE  |
   |  Examples for Specifications (Esempi per le specifiche)  |  Metrics, OpenAPI  |

3. Selezionare il pulsante **DOWNLOAD** per scaricare il progetto.

4. Decomprimere il file di archivio, ad esempio:

   ```bash
   unzip kumuluzEE-hello-azure.zip
   ```

### <a name="run-the-application-in-local-environment"></a>Eseguire l'applicazione nell'ambiente locale

1. Passare alla directory del progetto completato. Ad esempio:

   ```bash
   cd kumuluzEE-hello-azure/
   ```

2. Compilare il progetto usando Maven, ad esempio:

   ```bash
   mvn clean package
   ```

3. Eseguire l'applicazione usando il comando seguente:

   ```bash
   java -jar target/kumuluzEE-hello-azure.jar
   ```

4. Testare l'app Web esplorandola localmente tramite un Web browser. Se è disponibile curl, ad esempio, è possibile usare il comando seguente:

   ```bash
   curl http://localhost:8080/data/hello
   ```

5. Dovrebbe essere visualizzato il messaggio **Hello World**.

## <a name="configure-maven-plugin-for-azure-app-service"></a>Configurare il plug-in Maven per il servizio app di Azure

In questa sezione si configurerà il file *pom.xml* del progetto KumuluzEE in modo che Maven possa distribuire l'app in Servizio app di Azure in Linux.

1. Aprire il file *pom.xml* in un editor di codice.

2. Nella sezione `<build>` del file *pom.xml* inserire la voce `<plugin>` seguente all'interno del tag `<plugins>`.

   ```xml
   <build>
     <finalName>kumuluzEE-hello-azure</finalName>
     <plugins>
       <plugin>
         <groupId>com.microsoft.azure</groupId>
         <artifactId>azure-webapp-maven-plugin</artifactId>
         <version>1.10.0</version>
       </plugin>
     </plugins>
   </build>
   ```

3. È quindi possibile configurare la distribuzione, eseguire il comando maven seguente al prompt dei comandi e usare il **numero** scegliere queste opzioni nel prompt:

   ```bash
   mvn azure-webapp:config
   ```

   Parametro delle opzioni:  

   |  Campo di input  |  Valore di input/selezione  |
   | ---- | ---- |
   |  Define value for OS(Default: Linux):  | 1. linux  |
   |  Define value for javaVersion(Default: Java 8):   | 2. Java 8  |
   |  Confirm (Y/N) | y |

   È possibile eseguire la configurazione con il comando seguente:

   ```bash
   $ mvn azure-webapp:config
   [INFO] Scanning for projects...
   [INFO] 
   [INFO] ----< com.microsoft.azure.samples.kumuluzee:kumuluzEE-hello-azure >-----
   [INFO] Building kumuluzEE-hello-azure 1.0-SNAPSHOT
   [INFO] --------------------------------[ jar ]---------------------------------
   [INFO] 
   [INFO] --- azure-webapp-maven-plugin:1.10.0:config (default-cli) @ kumuluzEE-hello-azure ---
   1. linux [*]
   2. windows
   3. docker
   Enter index to use: 1
   Define value for javaVersion(Default: Java 8): 
   1. Java 11
   2. Java 8 [*]
   Enter index to use: 2
   Please confirm webapp properties
   AppName : kumuluzEE-hello-azure-1601006602397
   ResourceGroup : kumuluzEE-hello-azure-1601006602397-rg
   Region : westeurope
   PricingTier : PremiumV2_P1v2
   OS : Linux
   RuntimeStack : JAVA 8-jre8
   Deploy to slot : false
   Confirm (Y/N)? : y
   [INFO] Saving configuration to pom.
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  44.223 s
   [INFO] Finished at: 2020-09-25T13:04:02+09:00
   [INFO] ------------------------------------------------------------------------
   ```

4. Aggiungere la sezione `<appSettings>` alla sezione `<configuration>` di `PORT`, `WEBSITES_PORT` e `WEBSITES_CONTAINER_START_TIME_LIMIT`.  
 È infine possibile visualizzare la voce XML seguente per `azure-webapp-maven-plugin`.

   ```xml
   <plugin>
     <groupId>com.microsoft.azure</groupId>
     <artifactId>azure-webapp-maven-plugin</artifactId>
     <version>1.10.0</version>
     <configuration>
       <schemaVersion>V2</schemaVersion>
       <resourceGroup>microprofile</resourceGroup>
       <appName>kumuluzEE-hello-azure-1601006602397</appName>
       <pricingTier>P1v2</pricingTier>
       <region>japaneast</region>
       <runtime>
         <os>linux</os>
         <javaVersion>jre8</javaVersion>
         <webContainer>jre8</webContainer>
       </runtime>
       <appSettings>
         <property>
           <name>PORT</name>
           <value>8080</value>
         </property>
         <property>
           <name>WEBSITES_PORT</name>
           <value>8080</value>
         </property>
         <property>
           <name>WEBSITES_CONTAINER_START_TIME_LIMIT</name>
           <value>600</value>
         </property>
       </appSettings>
       <deployment>
         <resources>
           <resource>
             <directory>${project.basedir}/target</directory>
             <includes>
               <include>*.jar</include>
             </includes>
           </resource>
         </resources>
       </deployment>
     </configuration>
   </plugin>
   ```

## <a name="deploy-the-app-to-azure"></a>Distribuire l'app in Azure

Dopo aver configurato tutte le impostazioni nelle sezioni precedenti di questo articolo, è possibile distribuire l'app Web in Azure. A tale scopo, seguire questa procedura:

1. Se sono state apportate modifiche al file *pom.xml*, dalla finestra del terminale o al prompt dei comandi usato in precedenza ricompilare il file JAR usando Maven. Ad esempio:

   ```bash
   mvn clean package
   ```

2. Distribuire l'app Web in Azure usando Maven. Ad esempio:

   ```bash
   mvn azure-webapp:deploy
   ```

   Se la distribuzione ha avuto esito positivo, è possibile visualizzare il messaggio seguente nella console.

   ```bash
   mvn azure-webapp:deploy

   [INFO] Successfully deployed the artifact to https://kumuluzee-hello-azure-1601006602397.azurewebsites.net
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  01:23 min
   [INFO] Finished at: 2020-09-25T13:13:14+09:00
   [INFO] ------------------------------------------------------------------------
   ```

   Maven distribuirà l'app Web in Azure. Se l'app Web o il piano dell'app Web non esiste già, verrà creato. Potrebbero essere necessari alcuni minuti prima che l'app Web sia visibile nell'URL indicato nell'output. Passare all'URL in un Web browser.  Verrà visualizzata la schermata seguente.

   ![Pagina iniziale di KumuluzEE](./media/kumuluzee/kumuluzee-front-page.png)

   Dopo che è stata distribuita, l'app Web potrà essere gestita tramite il [portale di Azure].

   * L'app Web verrà elencata nel gruppo di risorse **microprofile**:

   ![App Web elencata in Servizi app nel portale di Azure](./media/kumuluzee/kumuluzee-azure-portal-rg.png)

   * È possibile accedere all'app Web facendo clic sul pulsante `Browse` nella **panoramica** dell'app Web.  
   Verificare che la distribuzione sia riuscita e in esecuzione. Verrà visualizzata la schermata seguente:

   ![Trovare l'URL per l'app Web in Servizi app nel portale di Azure](./media/kumuluzee/kumuluzee-azure-portal-manage.png)

## <a name="confirm-the-log-stream-from-running-app-service"></a>Verificare il flusso di log dall'esecuzione di Servizio app
 
È possibile visualizzare i log del servizio app in esecuzione. Qualsiasi chiamata effettuata a `console.log` nel codice del sito viene visualizzata nel terminale.
 
   ```azurecli
   az webapp log tail -g microprofile -n kumuluzEE-hello-azure-1601006602397
   ```
 
   ![Verificare il flusso di log](./media/kumuluzee/azure-cli-app-service-log-stream.png)
 

## <a name="clean-up-resources"></a>Eseguire la pulizia delle risorse

Quando non sono più necessarie, eseguire la pulizia delle risorse di Azure distribuite eliminando il gruppo di risorse.

* Nel portale di Azure selezionare Gruppo di risorse nel menu a sinistra.
* Immettere **microprofile** nel campo **Filtra per nome**. Il gruppo di risorse creato in questa esercitazione dovrebbe avere questo prefisso.
* Selezionare il gruppo di risorse creato in questa esercitazione.
* Selezionare Elimina gruppo di risorse nel menu in alto.

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni su MicroProfile e Azure, passare al centro di documentazione di MicroProfile in Azure.

> [!div class="nextstepaction"]
> [MicroProfile in Azure](./index.yml)

### <a name="additional-resources"></a>Risorse aggiuntive

Per altre informazioni sulle varie tecnologie illustrate in questo articolo, vedere gli articoli seguenti:

* [Maven Plugin for Azure Web Apps (Plug-in Maven per App Web di Azure)]

* [Creare un'entità servizio di Azure con l'interfaccia della riga di comando di Azure 2.0](/cli/azure/create-an-azure-service-principal-azure-cli)

* [Informazioni di riferimento sulle impostazioni di Maven](https://maven.apache.org/settings.html)

<!-- URL List -->

[Azure Command-Line Interface (CLI)]: /cli/azure/overview
[Azure for Java Developers]: ../index.yml
[Portale di Azure]: https://portal.azure.com/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Working with Azure DevOps and Java]: /azure/devops/
[Maven]: http://maven.apache.org/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Maven Plugin for Azure Web Apps (Plug-in Maven per App Web di Azure)]: https://github.com/microsoft/azure-maven-plugins/blob/develop/azure-webapp-maven-plugin/README.md

[Java Development Kit (JDK)]: ../fundamentals/java-jdk-long-term-support.md
<!-- http://www.oracle.com/technetwork/java/javase/downloads/ -->

<!-- IMG List -->

[AP01]: media/deploy-spring-boot-java-app-with-maven-plugin/web-app-listed-azure-portal.png
[AP02]: media/deploy-spring-boot-java-app-with-maven-plugin/determine-web-app-url.png