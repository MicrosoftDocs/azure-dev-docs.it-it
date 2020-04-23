---
title: Distribuire un'app Web Spring Boot in Linux nel Servizio app di Azure
description: Questa esercitazione illustra in modo dettagliato la procedura per la distribuzione di un'applicazione Spring Boot come app Web Linux in Microsoft Azure.
services: azure app service
documentationcenter: java
ms.date: 12/31/2019
ms.service: app-service
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: web
ms.custom: mvc
ms.openlocfilehash: 5e6204d773ee8e140832361ad587e850e36b75f6
ms.sourcegitcommit: 0af39ee9ff27c37ceeeb28ea9d51e32995989591
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/21/2020
ms.locfileid: "81668817"
---
# <a name="deploy-a-spring-boot-application-to-linux-on-azure-app-service"></a>Distribuire un'applicazione Spring Boot in Linux nel Servizio app di Azure

Questa esercitazione illustra l'uso di [Docker] per distribuire l'applicazione [Spring Boot] in contenitori e distribuire l'immagine Docker in un host Linux nel [servizio app di Azure](/azure/app-service/containers/app-service-linux-intro).

## <a name="prerequisites"></a>Prerequisiti

Per completare la procedura di questa esercitazione, sono necessari i prerequisiti seguenti:

* Sottoscrizione di Azure; se non si ha una sottoscrizione di Azure, è possibile attivare i [vantaggi per i sottoscrittori di MSDN] oppure iscriversi per ottenere un [account Azure gratuito].
* [Interfaccia della riga di comando di Azure].
* Java Development Kit (JDK) supportato. Per altre informazioni sulle versioni di JDK utilizzabili per lo sviluppo in Azure, vedere <https://aka.ms/azure-jdks>.
* Lo strumento di compilazione [Maven] di Apache (versione 3).
* Un client [Git].
* Un client [Docker].

> [!NOTE]
>
> A causa dei requisiti di virtualizzazione di questa esercitazione, non è possibile seguire la procedura illustrata in questo articolo in una macchina virtuale. È necessario usare un computer fisico in cui sono abilitate le funzionalità di virtualizzazione.

## <a name="create-the-spring-boot-on-docker-getting-started-web-app"></a>Creare l'app Web introduttiva di Spring Boot in Docker

I passaggi seguenti illustrano i passaggi necessari per creare una semplice applicazione Web Spring Boot e testarla localmente.

1. Aprire un prompt dei comandi e creare una directory locale in cui contenere l'applicazione, quindi passare a tale directory. Ad esempio:

   ```bash
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. Clonare il progetto di esempio [Spring Boot on Docker Getting Started] (Introduzione a Spring Boot in Docker) nella directory appena creata, ad esempio:

   ```bash
   git clone https://github.com/spring-guides/gs-spring-boot-docker.git
   ```

1. Passare alla directory del progetto completato. Ad esempio:

   ```bash
   cd gs-spring-boot-docker/complete
   ```

1. Compilare il file JAR usando Maven. Ad esempio:

   ```bash
   mvn package
   ```

1. Dopo aver creato l'app Web, passare alla directory `target` in cui si trova il file JAR e avviare l'app Web. Ad esempio:

   ```bash
   cd target
   java -jar gs-spring-boot-docker-0.1.0.jar --server.port=80
   ```

1. Testare l'app Web esplorandola localmente tramite un Web browser. Ad esempio, se è disponibile un cURL e il server Tomcat è configurato per l'esecuzione sulla porta 80:

   ```bash
   curl http://localhost
   ```

1. Dovrebbe essere visualizzato il messaggio **Hello Docker World**

   ![Esplorare l'app di esempio in locale][SB01]

## <a name="create-an-azure-container-registry-to-use-as-a-private-docker-registry"></a>Creare un'istanza di Registro Azure Container da usare come registro Docker privato

La procedura seguente illustra come usare il portale di Azure per creare un'istanza di Registro Azure Container.

> [!NOTE]
>
> Se si vuole usare l'interfaccia della riga di comando di Azure anziché il portale di Azure, seguire i passaggi in [Creare un registro per contenitori Docker privati usando l'interfaccia della riga di comando di Azure 2.0](/azure/container-registry/container-registry-get-started-azure-cli).

1. Aprire il [Azure portal] ed effettuare l'accesso.

   Dopo aver effettuato l'accesso all'account nel portale di Azure, è possibile seguire la procedura illustrata nell'articolo [Creare un registro per contenitori Docker privati con il portale di Azure], parafrasata per semplicità nei passaggi seguenti.

1. Fare clic sull'icona di menu **+ Nuovo**, su **Contenitori** e quindi su **Registro Azure Container**.

   ![Creare una nuova istanza di Registro Azure Container][AR01]

1. Quando viene visualizzata la pagina **Crea registro contenitori**, immettere **Nome del registro**, **Sottoscrizione**, **Gruppo di risorse** e **Percorso**. Selezionare **Abilita** per **Utente amministratore**. Fare quindi clic su **Crea**.

   ![Configurare le impostazioni di Registro Azure Container][AR03]

1. Dopo avere creato il registro contenitori, passare al registro contenitori stesso nel portale di Azure e fare clic su **Chiavi di accesso**. Prendere nota del nome utente e della password per i passaggi successivi.

   ![Chiavi di accesso a Registro Azure Container][AR04]

## <a name="configure-maven-to-use-your-azure-container-registry-access-keys"></a>Configurare Maven per l'uso delle chiavi di accesso di Registro Azure Container

1. Passare alla directory del progetto completato per l'applicazione Spring Boot (ad esempio "*C:\SpringBoot\gs-spring-boot-docker\complete*" o " */users/robert/SpringBoot/gs-spring-boot-docker/complete*") e aprire il file *pom.xml* con un editor di testo.

1. Aggiornare la raccolta `<properties>` nel file *pom.xml* con l'ultima versione di [jib-maven-plugin](https://github.com/GoogleContainerTools/jib/tree/master/jib-maven-plugin), oltre che con il valore del server di accesso e le impostazioni di accesso per Registro Azure Container della sezione precedente di questa esercitazione. Ad esempio:

   ```xml
   <properties>
      <jib-maven-plugin.version>1.7.0</jib-maven-plugin.version>
      <docker.image.prefix>wingtiptoysregistry.azurecr.io</docker.image.prefix>
      <java.version>1.8</java.version>
      <username>wingtiptoysregistry</username>
      <password>{put your Azure Container Registry access key here}</password>
   </properties>
   ```

1. Aggiungere [jib-maven-plugin](https://github.com/GoogleContainerTools/jib/tree/master/jib-maven-plugin) alla raccolta `<plugins>` nel file *pom.xml*.  Questo esempio usa la versione 1.8.0.

   Specificare l'immagine di base in `<from>/<image>`, qui `mcr.microsoft.com/java/jre:8-zulu-alpine`. Specificare il nome dell'immagine finale da creare dalla base in `<to>/<image>`.  

   Il valore `{docker.image.prefix}` dell'autenticazione è il **Server di accesso** nella pagina del registro visualizzata in precedenza. Il valore `{project.artifactId}` indica il nome e il numero di versione del file JAR dalla prima compilazione Maven del progetto.

   Specificare il nome utente e la password dal riquadro del registro nel nodo `<to>/<auth>`. Ad esempio:

   ```xml
   <plugin>
     <artifactId>jib-maven-plugin</artifactId>
     <groupId>com.google.cloud.tools</groupId>
     <version>1.8.0</version>
     <configuration>
        <from>
            <image>mcr.microsoft.com/java/jre:8-zulu-alpine</image>
        </from>
        <to>
            <image>${docker.image.prefix}/${project.artifactId}</image>
            <auth>
               <username>${username}</username>
               <password>${password}</password>
            </auth>
        </to>
     </configuration>
   </plugin>
   ```

1. Passare alla directory del progetto completato per l'applicazione Spring Boot ed eseguire il comando seguente per ricompilare l'applicazione ed effettuare il push del contenitore in Registro Azure Container:

   ```bash
   mvn compile jib:build
   ```

> [!NOTE]
>
> Quando si usa Jib per eseguire il push dell'immagine nel Registro Azure Container, l'immagine non userà *Dockerfile*. Per i dettagli, vedere [questo](https://cloudplatform.googleblog.com/2018/07/introducing-jib-build-java-docker-images-better.html) documento.
>

## <a name="create-a-web-app-on-linux-on-azure-app-service-using-your-container-image"></a>Creare un'app Web in Linux nel servizio app di Azure usando l'immagine del contenitore

1. Aprire il [Azure portal] ed effettuare l'accesso.

2. Fare clic sull'icona di menu **+ Crea una risorsa**, quindi su **Calcolo** e infine su **App Web per contenitori**.
   
   ![Creare una nuova app Web nel portale di Azure][LX01]

3. Quando viene visualizzata la pagina **App Web in Linux**, immettere le informazioni seguenti:

   * Scegliere una **Sottoscrizione** dall'elenco a discesa.

   * Selezionare un **Gruppo di risorse** esistente o specificare un nome per creare uno nuovo.

   * Immettere un nome univoco per **Nome app**, ad esempio "*wingtiptoyslinux*"

   * Specificare `Docker Container` per **Pubblica**.

   * Scegliere *Linux* come **Sistema operativo**.

   * Selezionare **Area**.

   * Accettare il **Piano Linux** e scegliere un **Piano di servizio app** esistente oppure fare clic su **Create new** per creare un nuovo piano di servizio app.

   * Fare clic su **Avanti: Docker**.

   ![Configurare le impostazioni dell'app Web][LX02]

      Nella pagina **App Web** selezionare **Docker** e immettere le informazioni seguenti:

   * Selezionare **Contenitore singolo**.

   * **Registro di sistema**: Scegliere il contenitore, ad esempio: "*wingtiptoysregistry*"

   * **Immagine**: Selezionare l'immagine creata in precedenza, ad esempio: "*gs-spring-boot-docker*"

   * **Tag**: scegliere il tag per l'immagine, ad esempio "*latest*"

   * **Comando di avvio**: lasciare vuoto questo campo perché l'immagine include già un comando di avvio

   Dopo aver immesso tutte le informazioni riportate sopra, fare clic su **Rivedi e crea**.

   ![Configurare le impostazioni dell'app Web][LX02-A]

   * Fare clic su **Rivedi e crea**.

Esaminare le informazioni e fare clic su **Crea**.

Una volta completata la distribuzione, fare clic su **Vai alla risorsa**.  La pagina di distribuzione mostra l'URL per l'accesso all'applicazione.

   ![Ottenere l'URL della distribuzione][LX02-B]

> [!NOTE]
>
> Azure eseguirà automaticamente il mapping delle richieste Internet al server Tomcat incorporato in esecuzione sulla porta - 80. Se tuttavia il server Tomcat incorporato è stato configurato per l'esecuzione sulla porta - 8080 o su una porta personalizzata, è necessario aggiungere all'app Web una variabile di ambiente che definisce tale porta. A tale scopo, seguire questa procedura:
>
> 1. Aprire il [Azure portal] ed effettuare l'accesso.
>
> 2. Fare clic sull'icona per **App Web** e selezionare l'app dalla pagina **Servizi app**.
>
> 3. Fare clic su **Configurazione** nel riquadro di spostamento a sinistra.
>
> 4. Nella sezione **Impostazioni applicazione** aggiungere una nuova impostazione denominata **WEBSITES_PORT** e immettere il numero di porta personalizzato come valore.
>
> 5. Fare clic su **OK**. Fare quindi clic su **Salva**.
>
> ![Salvataggio di un numero di porta personalizzato nel portale di Azure][LX03]

<!--
##  OPTIONAL: Configure the embedded Tomcat server to run on a different port

The embedded Tomcat server in the sample Spring Boot application is configured to run on port 8080 by default. However, if you want to run the embedded Tomcat server to run on a different port, such as port 80 for local testing, you can configure the port by using the following steps.

1. Go to the *resources* directory (or create the directory if it does not exist); for example:
   ```shell
   cd src/main/resources
   ```

1. Open the *application.yml* file in a text editor if it exists, or create a new YAML file if it does not exist.

1. Modify the **server** setting so that the server runs on port 80; for example:
   ```yaml
   server:
      port: 80
   ```

1. Save and close the *application.yml* file.
-->

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni su Spring e Azure, passare al centro di documentazione di Spring in Azure.

> [!div class="nextstepaction"]
> [Spring in Azure](/azure/developer/java/spring-framework)

### <a name="additional-resources"></a>Risorse aggiuntive

Per altre informazioni sull'uso delle applicazioni Spring Boot in Azure, vedere gli articoli seguenti:

* [Distribuire un'applicazione Spring Boot nel servizio app di Azure](deploy-spring-boot-java-app-from-container-registry-using-maven-plugin.md)
* [Distribuire un'applicazione Spring Boot in un cluster Kubernetes nel servizio Azure Container](deploy-spring-boot-java-app-on-kubernetes.md)

Per altre informazioni sull'uso di Azure con Java, vedere [Azure per sviluppatori Java] e la documentazione relativa all'[uso di Azure DevOps e Java].

Per maggiori dettagli sul progetto di esempio di Spring Boot in Docker, vedere [Spring Boot on Docker Getting Started] (Introduzione a Spring Boot in Docker).

Per iniziare a usare proprie applicazioni Spring Boot, vedere **Spring Initializr** all'indirizzo https://start.spring.io/.

Per altre informazioni su come iniziare a creare una semplice applicazione Spring Boot, vedere Spring Initializr all'indirizzo https://start.spring.io/.

Per altri esempi sull'uso delle immagini personalizzate di Docker con Azure, vedere [Uso di un'immagine Docker personalizzata per App Web di Azure in Linux].

<!-- URL List -->

[Interfaccia della riga di comando di Azure]: /cli/azure/overview
[Azure Container Service (AKS)]: https://azure.microsoft.com/services/container-service/
[Azure per sviluppatori Java]: /azure/developer/java/
[Azure portal]: https://portal.azure.com/
[Creare un registro per contenitori Docker privati con il portale di Azure]: /azure/container-registry/container-registry-get-started-portal
[Uso di un'immagine Docker personalizzata per App Web di Azure in Linux]: /azure/app-service-web/app-service-linux-using-custom-docker-image
[Docker]: https://www.docker.com/
[Account Azure gratuito]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Uso di Azure DevOps e Java]: /azure/devops/java/
[Maven]: http://maven.apache.org/
[vantaggi per i sottoscrittori di MSDN]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Boot on Docker Getting Started]: https://github.com/spring-guides/gs-spring-boot-docker (Introduzione a Spring Boot in Docker)
[Spring Framework]: https://spring.io/

[Java Development Kit (JDK)]: https://aka.ms/azure-jdks
<!-- http://www.oracle.com/technetwork/java/javase/downloads/ -->

<!-- IMG List -->

[SB01]: media/deploy-spring-boot-java-app-on-linux/SB01.png
[SB02]: media/deploy-spring-boot-java-app-on-linux/SB02.png
[AR01]: media/deploy-spring-boot-java-app-on-linux/AR01.png
[AR03]: media/deploy-spring-boot-java-app-on-linux/AR03.png
[AR04]: media/deploy-spring-boot-java-app-on-linux/AR04.png
[LX01]: media/deploy-spring-boot-java-app-on-linux/LX01.png
[LX02]: media/deploy-spring-boot-java-app-on-linux/LX02.png
[LX02-A]: media/deploy-spring-boot-java-app-on-linux/LX02-A.png
[LX02-B]: media/deploy-spring-boot-java-app-on-linux/LX02-B.png
[LX03]: media/deploy-spring-boot-java-app-on-linux/LX03.png
