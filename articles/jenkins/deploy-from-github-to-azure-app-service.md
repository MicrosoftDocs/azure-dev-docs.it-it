---
title: 'Esercitazione: Eseguire la distribuzione da GitHub nel servizio app di Azure con Jenkins'
description: Informazioni su come configurare Jenkins per l'integrazione continua (CI) da GitHub e la distribuzione continua (CD) nel servizio app di Azure per le app Web Java
keywords: jenkins, azure, devops, servizio app
ms.topic: tutorial
ms.date: 08/10/2020
ms.custom: devx-track-jenkins
ms.openlocfilehash: ef404f1d2e3d1ed042a99eccd2469fdd112e931b
ms.sourcegitcommit: 39f3f69e3be39e30df28421a30747f6711c37a7b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/21/2020
ms.locfileid: "90832017"
---
# <a name="tutorial-deploy-from-github-to-azure-app-service-using-jenkins"></a>Esercitazione: Eseguire la distribuzione da GitHub nel servizio app di Azure con Jenkins

Questa esercitazione distribuisce un'app Web Java di esempio da GitHub in [Servizio app di Azure in Linux](/azure/app-service/containers/app-service-linux-intro) configurando l'integrazione continua (CI) e la distribuzione continua (CD) in Jenkins. Quando si aggiorna l'app eseguendo il push di commit in GitHub, Jenkins compila automaticamente l'app e la pubblica di nuovo in Servizio app di Azure. L'app di esempio in questa esercitazione è stata sviluppata usando il framework [Spring Boot](https://projects.spring.io/spring-boot/). 

![Panoramica della distribuzione da GitHub al Servizio app di Azure](media/deploy-from-github-to-azure-app-service/azure-continuous-integration-deployment-overview.png)

In questa esercitazione si completeranno le attività seguenti:

> [!div class="checklist"]
> * Installare i plug-in Jenkins per eseguire la compilazione da GitHub, eseguire la distribuzione in Servizio app di Azure ed eseguire altre attività correlate.
> * Creare una copia tramite fork del repository GitHub di esempio in modo da disporre di una copia di lavoro.
> * Connettere Jenkins a GitHub.
> * Creare un'entità servizio di Azure in modo che Jenkins possa accedere ad Azure senza usare le credenziali dell'utente.
> * Aggiungere l'entità servizio a Jenkins.
> * Creare la pipeline Jenkins che compila e distribuisce l'app di esempio ogni volta che si aggiorna l'app in GitHub.
> * Creare file di compilazione e distribuzione per la pipeline Jenkins.
> * Puntare la pipeline Jenkins allo script di compilazione e distribuzione.
> * Distribuire l'app di esempio in Azure eseguendo una compilazione manuale.
> * Eseguire il push di un aggiornamento dell'app in GitHub, per fare in modo che Jenkins esegua la compilazione e la ridistribuzione in Azure.

## <a name="prerequisites"></a>Prerequisiti

Per completare questa esercitazione, è necessario quanto segue:

* Un server [Jenkins](https://jenkins.io/) con Java Development Kit (JDK) e gli strumenti Maven installati in una macchina virtuale Linux di Azure

  Se non si ha un server Jenkins, completare questa procedura nel portale di Azure: [Creare un server Jenkins in una macchina virtuale Linux di Azure](/azure/jenkins/install-jenkins-solution-template)

* Un account [GitHub](https://github.com) per ottenere una copia di lavoro (fork) per l'app Web Java di esempio. 

* [Interfaccia della riga di comando di Azure](/cli/azure/install-azure-cli), che è possibile eseguire dalla riga di comando locale o da [Azure Cloud Shell](/azure/cloud-shell/overview)

## <a name="install-jenkins-plug-ins"></a>Installare i plug-in Jenkins

1. Accedere alla console Web Jenkins da questo percorso:

   `https://<Jenkins-server-name>.<Azure-region>.cloudapp.azure.com`

1. Nella pagina principale di Jenkins selezionare **Manage Jenkins** > **Manage Plugins** (Gestisci Jenkins > Gestisci plug-in).

   ![Gestire i plug-in Jenkins](media/deploy-from-github-to-azure-app-service/manage-plug-ins-for-azure.png)

1. Nella scheda **Available** (Disponibile) selezionare questi plug-in:

   - [Servizio app di Azure](https://plugins.jenkins.io/azure-app-service)
   - [GitHub Branch Source](https://plugins.jenkins.io/github-branch-source)
   - [Environment Injector Plug-in](https://plugins.jenkins.io/envinject) Jenkins
   - [Azure Credentials](https://plugins.jenkins.io/azure-credentials)

   Se i plug-in non vengono visualizzati, verificare che non siano già installati controllando la scheda **Installed** (Installato).

1. Per installare i plug-in selezionati, selezionare **Download now and install after restart** (Scarica subito e installa dopo il riavvio).

1. Al termine, dal menu di Jenkins scegliere **Manage Jenkins** (Gestisci Jenkins) per tornare alla pagina di gestione di Jenkins per i passaggi successivi.

## <a name="fork-sample-github-repo"></a>Creare una copia tramite fork del repository GitHub di esempio

1. [Accedere al repository GitHub per l'app di esempio Spring Boot](https://github.com/spring-guides/gs-spring-boot). 

1. Nell'angolo superiore destro in GitHub selezionare **Fork**.

   ![Creare una copia tramite fork del repository di esempio in GitHub](media/deploy-from-github-to-azure-app-service/fork-github-repo.png)

1. Seguire le istruzioni per selezionare l'account GitHub e completare la creazione di una copia tramite fork.

Configurare quindi Jenkins con le proprie credenziali di GitHub.

## <a name="connect-jenkins-to-github"></a>Connettere Jenkins a GitHub

Per fare in modo che Jenkins monitori GitHub e risponda quando viene eseguito il push di nuovi commit nell'app Web nel fork di GitHub, abilitare i [webhook di GitHub](https://developer.github.com/webhooks/) in Jenkins.

> [!NOTE]
> 
> Questi passaggi creano credenziali di token di accesso personale per consentire a Jenkins di usare GitHub tramite nome utente e password di GitHub dell'utente. 
> Se tuttavia l'account GitHub usa l'autenticazione a due fattori, è necessario creare il token in GitHub e configurare Jenkins per usare tale token. 
> Pe altre informazioni, vedere la documentazione del [plug-in di GitHub per Jenkins](https://wiki.jenkins.io/display/JENKINS/GitHub+Plugin).

1. Nella pagina **Manage Jenkins** (Gestisci Jenkins) selezionare **Configure System** (Configura sistema). 

   ![Configurare il sistema in Jenkins](media/deploy-from-github-to-azure-app-service/manage-jenkins-configure-system.png)

1. Nella sezione **GitHub** specificare i dettagli per il server GitHub. Nell'elenco **Add GitHub Server** (Aggiungi server GitHub) selezionare **GitHub Server** (Server GitHub). 

   ![Aggiungere il server GitHub in Jenkins](media/deploy-from-github-to-azure-app-service/add-GitHub-server.png)

1. Se la proprietà **Manage hooks** (Gestisci hook) non è selezionata, selezionarla. Selezionare **Advanced** (Avanzate) per specificare altre impostazioni. 

   ![Specificare le impostazioni avanzate di Jenkins per il server GitHub](media/deploy-from-github-to-azure-app-service/advanced-GitHub-settings.png)

1. Nell'elenco **Manage additional GitHub actions** (Gestisci azioni GitHub aggiuntive) selezionare **Convert login and password to token** (Converti account di accesso e password in token).

   ![Convertire l'account di accesso e la password nel token per GitHub](media/deploy-from-github-to-azure-app-service/manage-additional-actions.png)

1. Selezionare **From login and password** (Da account di accesso e password) e immettere il nome utente e la password di GitHub. Al termine, selezionare **Create token credentials** (Crea credenziali token) per creare un [token di accesso personale di GitHub](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/).   

   ![Creare un token di accesso personale di GitHub da account di accesso e password](media/deploy-from-github-to-azure-app-service/create-github-token-credentials.png)

1. Nell'elenco **Credentials** (Credenziali) della sezione **GitHub Server** (Server GitHub) selezionare il nuovo token. Per controllare che l'autenticazione funzioni, scegliere **Test connection** (Test connessione).

   ![Controllare la connessione al server GitHub con il nuovo token di accesso personale](media/deploy-from-github-to-azure-app-service/check-github-connection.png)

Creare quindi l'entità servizio di Azure usata da Jenkins per l'autenticazione e l'accesso alle risorse di Azure.

## <a name="create-service-principal"></a>Creare un'entità servizio

In una sezione successiva si creerà un processo di pipeline Jenkins che compila l'app da GitHub e la distribuisce in Servizio app di Azure. Per fare in modo che Jenkins acceda ad Azure senza dover immettere le credenziali dell'utente è necessaria un'[entità servizio](/azure/active-directory/develop/app-objects-and-service-principals). Se si ha già un'entità servizio da usare per questo articolo, è possibile ignorare questa sezione.

Per creare l'entità servizio, eseguire il comando [az ad sp create-for-rbac](/cli/azure/ad/sp?#az-ad-sp-create-for-rbac) dell'interfaccia della riga di comando di Azure.

```azurecli-interactive
az ad sp create-for-rbac
```

**Note**:

- Al termine, `az ad sp create-for-rbac` visualizza diversi valori. I valori `name`, `password` e `tenant` vengono usati nel passaggio successivo.
- Per impostazione predefinita, un'entità servizio viene creata con il ruolo **Collaboratore**, che include autorizzazioni complete per la lettura e la scrittura in un account Azure. Per altre informazioni sul controllo degli accessi in base al ruolo e i ruoli, vedere [Controllo degli accessi in base al ruolo: ruoli predefiniti](/azure/active-directory/role-based-access-built-in-roles).
- Se viene persa, questa password non può essere recuperata. Di conseguenza, è consigliabile archiviarla in un luogo sicuro. Se si dimentica la password, è necessario [reimpostare le credenziali dell'entità servizio](/cli/azure/create-an-azure-service-principal-azure-cli#reset-credentials).


## <a name="add-service-principal-to-jenkins"></a>Aggiungere l'entità servizio a Jenkins

1. Nella pagina principale di Jenkins selezionare **Credentials** > **System** (Credenziali > Sistema). 

1. Nella pagina **System** (Sistema) in **Domain** (Dominio) selezionare **Global credentials (unrestricted)** (Credenziali globali - senza restrizioni).

1. Dal menu a sinistra scegliere **Add Credentials** (Aggiungi credenziali).

1. Nell'elenco **Kind** (Tipo) selezionare **Azure Service Principal** (Entità servizio di Azure).

1. Fornire le informazioni per l'entità servizio e la sottoscrizione di Azure nelle proprietà descritte dalla tabella in questo passaggio:

   ![Aggiungere le credenziali dell'entità servizio di Azure](media/deploy-from-github-to-azure-app-service/add-service-principal-credentials.png)

   | Proprietà | Valore | Descrizione | 
   |----------|-------|-------------| 
   | **ID sottoscrizione** | <*ID-SottoscrizioneAzure*> | Valore GUID per la sottoscrizione di Azure <p>**Suggerimento**: se non si conosce l'ID sottoscrizione di Azure, eseguire questo comando dell'interfaccia della riga di comando di Azure dalla riga di comando o in Cloud Shell e quindi usare il valore GUID `id`: <p>`az account list` | 
   | **ID client** | <*ID-EntitàServizioAzure*> | Valore GUID `appId` generato in precedenza per l'entità servizio di Azure | 
   | **Segreto client** | <*PasswordSicura*> | Valore `password` o "segreto" specificato per l'entità servizio di Azure | 
   | **ID tenant** | <*ID-TenantAzureActiveDirectory*> | Valore GUID `tenant` per il tenant di Azure Active Directory | 
   | **ID** | <*NomeEntitàServizioAzure*> | Valore `displayName` per l'entità servizio di Azure | 

1. Per verificare il corretto funzionamento dell'entità servizio, selezionare **Verify Service Principal** (Verifica entità servizio). Al termine, fare clic su **OK**.

Creare quindi la pipeline Jenkins che compila e distribuisce l'app.

## <a name="create-jenkins-pipeline"></a>Creare una pipeline Jenkins

In Jenkins creare il processo di pipeline per la compilazione e la distribuzione dell'app.

1. Tornare alla home page di Jenkins e selezionare **New Item** (Nuovo elemento). 

   ![Creare una pipeline Jenkins](media/deploy-from-github-to-azure-app-service/jenkins-select-new-item.png)

1. Specificare un nome per il processo di pipeline, ad esempio, "My-Java-Web-App" e selezionare **Pipeline**. Nella parte inferiore selezionare **OK**.  

   ![Assegnare un nome al processo della pipeline Jenkins](media/deploy-from-github-to-azure-app-service/jenkins-select-pipeline.png)

1. Configurare Jenkins con l'entità servizio in modo che Jenkins possa eseguire la distribuzione in Azure senza usare le credenziali dell'utente.

   1. Nella scheda **Generale** selezionare **Prepare an environment for the run** (Preparare un ambiente per l'esecuzione). 

   1. Nella casella **Properties Content** (Contenuto proprietà) visualizzata aggiungere queste variabili di ambiente e i relativi valori. 

      ```ini
      AZURE_CRED_ID=yourAzureServicePrincipalName
      RES_GROUP=yourWebAppAzureResourceGroupName
      WEB_APP=yourWebAppName
      ```

      ![Preparare un ambiente per l'esecuzione e impostare le variabili di ambiente](media/deploy-from-github-to-azure-app-service/prepare-environment-for-jenkins-run.png)

1. Al termine, selezionare **Salva**.

Creare quindi gli script di compilazione e distribuzione per Jenkins.

## <a name="create-build-and-deployment-files"></a>Creare file di compilazione e distribuzione

Creare ora i file usati da Jenkins per la compilazione e la distribuzione dell'app.

1. Nella cartella `src/main/resources/` del fork di GitHub creare il file di configurazione dell'app denominato `web.config`, contenente questo codice XML, ma sostituire `$(JAR_FILE_NAME)` con `gs-spring-boot-0.1.0.jar`:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <configuration>
      <system.webServer>
         <handlers>
            <add name="httpPlatformHandler" path="*" verb="*" modules="httpPlatformHandler" resourceType="Unspecified" />
         </handlers>
         <httpPlatform processPath="%JAVA_HOME%\bin\java.exe" arguments="-Djava.net.preferIPv4Stack=true -Dserver.port=%HTTP_PLATFORM_PORT% -jar &quot;%HOME%\site\wwwroot\${JAR_FILE_NAME}&quot;"></httpPlatform>
      </system.webServer>
   </configuration>
   ```

1. Nella cartella radice del fork di GitHub creare questo script di compilazione e distribuzione denominato `Jenkinsfile`, contenente questo testo ([codice sorgente disponibile in GitHub qui](https://github.com/Microsoft/todo-app-java-on-azure/blob/master/doc/resources/jenkins/Jenkinsfile-webapp-se)):

   ```groovy
   node {
      stage('init') {
         checkout scm
      }
      stage('build') {
         sh '''
            mvn clean package
            cd target
            cp ../src/main/resources/web.config web.config
            cp todo-app-java-on-azure-1.0-SNAPSHOT.jar app.jar 
            zip todo.zip app.jar web.config
         '''
      }
      stage('deploy') {
         azureWebAppPublish azureCredentialsId: env.AZURE_CRED_ID,
         resourceGroup: env.RES_GROUP, appName: env.WEB_APP, filePath: "**/todo.zip"
      }
   }
   ```

1. Eseguire il commit dei file `web.config` e `Jenkinsfile` nel fork di GitHub ed eseguire il push delle modifiche.

## <a name="point-pipeline-at-script"></a>Puntare la pipeline allo script

Specificare ora lo script di compilazione e distribuzione che Jenkins deve usare.

1. In Jenkins selezionare il processo di pipeline creato in precedenza. 

   ![Selezionare il processo della pipeline Jenkins per l'app Web](media/deploy-from-github-to-azure-app-service/select-pipeline-job.png)

1. Nel menu a sinistra selezionare **Configure** (Configura).

1. Nell'elenco **Definition** (Definizione) della scheda **Pipeline** selezionare **Pipeline script from SCM** (Script di pipeline da SCM).

   1. Nella casella **SCM** visualizzata selezionare **Git** come controllo del codice sorgente. 

   1. Nella sezione **Repositories** (Repository), per **Repository URL** (URL del repository) immettere l'URL del fork di GitHub, ad esempio: 

      `https://github.com/<your-GitHub-username>/gs-spring-boot`

   1. Per **Credentials** (Credenziali) selezionare il token di accesso personale di GitHub creato in precedenza.

   1. Nella casella **Script Path** (Percorso script) aggiungere il percorso dello script "Jenkinsfile".

   Al termine, la definizione della pipeline avrà un aspetto simile a questo esempio: 

   ![Impostare la pipeline Jenkins in modo che punti allo script](media/deploy-from-github-to-azure-app-service/set-up-jenkins-github.png)

1. Al termine, selezionare **Salva**.

Compilare e distribuire quindi l'app in Servizio app di Azure. 

## <a name="build-and-deploy-to-azure"></a>Eseguire la compilazione e la distribuzione in Azure

1. Con l'interfaccia della riga di comando di Azure, dalla riga di comando o da Azure Cloud Shell, creare un'[app Web di Servizio app di Azure in Linux](/azure/app-service/containers/app-service-linux-intro) dove Jenkins distribuirà l'app Web dopo il completamento della compilazione. Assicurarsi che l'app Web abbia un nome univoco.

   ```azurecli-interactive
   az group create --name yourWebAppAzureResourceGroupName --location yourAzureRegion
   az appservice plan create --name yourLinuxAppServicePlanName --resource-group yourWebAppAzureResourceGroupName --is-linux
   az webapp create --name yourWebAppName --resource-group yourWebAppAzureResourceGroupName --plan yourLinuxAppServicePlanName --runtime "java|1.8|Tomcat|8.5"
   ```

   Per altre informazioni su questi comandi dell'interfaccia della riga di comando di Azure, vedere queste pagine:

   * [**`az group create`**](/cli/azure/group?view=azure-cli-latest#az-group-create)

   * [**`az appservice plan create`**](/cli/azure/appservice/plan?view=azure-cli-latest#az-appservice-plan-create)

   * [**`az webapp create`**](/cli/azure/webapp?view=azure-cli-latest#az-webapp-create)

1. In Jenkins selezionare il processo di pipeline e quindi **Build Now** (Compila).

   Al termine della compilazione, Jenkins distribuisce l'app, che è ora disponibile in Azure nell'URL di pubblicazione, ad esempio: 

   `http://<your-Java-web-app>.azurewebsites.net`

   ![Visualizzare l'app distribuita in Azure](media/deploy-from-github-to-azure-app-service/greetings-unedited.png)

## <a name="push-changes-and-redeploy"></a>Eseguire il push delle modifiche e ripetere la ridistribuzione

1. Nel Web browser passare a questo percorso nel fork di GitHub dell'app Web:

   `complete/src/main/java/Hello/Application.java`
   
1. Nell'angolo superiore destro in GitHub selezionare **Edit this file** (Modifica questo file).

1. Apportare la modifica seguente al metodo `commandLineRunner()` ed eseguirne il commit nel ramo `master` del repository. Questo commit nel ramo `master` avvia una compilazione in Jenkins. 
   
   ```java
   System.out.println("Let's inspect the beans provided by Spring Boot on Azure");
   ```

1. Al termine della compilazione, dopo che Jenkins ha eseguito la ridistribuzione in Azure, aggiornare l'app, in cui sarà ora presente l'aggiornamento apportato.

   ![Visualizzare l'app distribuita in Azure](media/deploy-from-github-to-azure-app-service/greetings-edited.png)

## <a name="troubleshooting-the-jenkins-plug-in"></a>Risoluzione dei problemi del plug-in Jenkins

Se si rilevano bug con i plug-in Jenkins, segnalare il problema in [Jenkins JIRA](https://issues.jenkins-ci.org/) per il componente specifico.

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"]
> [Usare macchine virtuali di Azure come agenti di compilazione](/azure/jenkins/jenkins-azure-vm-agents)