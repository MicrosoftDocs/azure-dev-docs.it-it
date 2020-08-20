---
title: Distribuire app in Azure Spring Cloud con Jenkins e l'interfaccia della riga di comando di Azure
description: Informazioni su come usare l'interfaccia della riga di comando di Azure in una pipeline di integrazione e distribuzione continue per distribuire i microservizi nel servizio Azure Spring Cloud
keywords: jenkins, azure, devops, azure spring cloud, interfaccia della riga di comando di azure
ms.topic: tutorial
ms.date: 08/10/2020
ms.custom: devx-track-jenkins,devx-track-azurecli
ms.openlocfilehash: 769067c03fce08e462364314d2e4712ab6ffe155
ms.sourcegitcommit: 16ce1d00586dfa9c351b889ca7f469145a02fad6
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/14/2020
ms.locfileid: "88240833"
---
# <a name="tutorial-deploy-apps-to-azure-spring-cloud-using-jenkins-and-the-azure-cli"></a>Esercitazione: Distribuire app in Azure Spring Cloud con Jenkins e l'interfaccia della riga di comando di Azure

[Azure Spring Cloud](/spring-cloud/spring-cloud-overview) è uno sviluppo di microservizi completamente gestito con funzionalità predefinite di individuazione dei servizi e gestione della configurazione. Il servizio semplifica la distribuzione di applicazioni di microservizi basate su Spring Boot in Azure. Questa esercitazione illustra come usare l'interfaccia della riga di comando di Azure in Jenkins per automatizzare l'integrazione e la distribuzione continue (CI/CD) per Azure Spring Cloud.

In questa esercitazione si completeranno le attività seguenti:

> [!div class="checklist"]
> * Effettuare il provisioning di un'istanza del servizio e avviare un'applicazione Java Spring
> * Preparare il server Jenkins
> * Usare l'interfaccia della riga di comando di Azure in una pipeline Jenkins per compilare e distribuire le applicazioni di microservizi 

>[!Note]
> Azure Spring Cloud è attualmente disponibile come anteprima pubblica. Le offerte di anteprima pubblica consentono ai clienti di sperimentare le nuove funzionalità prima del rilascio della versione ufficiale.  I servizi e le funzionalità di anteprima pubblica non sono destinati all'uso in produzione.  Per altre informazioni sul supporto durante le anteprime, vedere le [domande frequenti](https://azure.microsoft.com/support/faq/) o inviare una [richiesta di supporto](/azure-supportability/how-to-create-azure-support-request).

## <a name="prerequisites"></a>Prerequisiti

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../includes/open-source-devops-prereqs-azure-subscription.md)]

**Jenkins**: [Installare Jenkins in una VM Linux](configure-on-linux-vm.md)

**Account GitHub**: Se non si ha un account GitHub, crearne [uno gratuito](https://github.com/) prima di iniziare.

## <a name="provision-a-service-instance-and-launch-a-java-spring-application"></a>Effettuare il provisioning di un'istanza del servizio e avviare un'applicazione Java Spring

Per l'applicazione del servizio Microsoft di esempio si usa [Piggy Metrics](https://github.com/Azure-Samples/piggymetrics) e viene seguita la stessa procedura descritta in [Avvio rapido: Avviare un'applicazione Java Spring usando l'interfaccia della riga di comando di Azure](/spring-cloud/spring-cloud-quickstart-launch-app-cli.md) per effettuare il provisioning dell'istanza del servizio e configurare le applicazioni. Se questa procedura è già stata completata, è possibile passare alla sezione successiva. In caso contrario, di seguito sono inclusi i comandi dell'interfaccia della riga di comando di Azure. Per altre informazioni generali, vedere [Avvio rapido: Avviare un'applicazione Java Spring usando l'interfaccia della riga di comando di Azure](/spring-cloud/spring-cloud-quickstart-launch-app-cli.md).

Il computer locale deve soddisfare lo stesso prerequisito del server di compilazione Jenkins. Verificare che siano installati i componenti seguenti per compilare e distribuire le applicazioni di microservizi:
    * [Git](https://git-scm.com/)
    * [JDK 8](/java/azure/jdk/?view=azure-java-stable)
    * [Maven 3.0 o versione successiva](https://maven.apache.org/download.cgi)
    * L'[interfaccia della riga di comando di Azure](/cli/azure/install-azure-cli?view=azure-cli-latest) versione 2.0.67 o superiore installata

1. Installare l'estensione Azure Spring Cloud:

    ```Azure CLI
    az extension add --name spring-cloud
    ```

1. Creare un gruppo di risorse in cui includere il servizio Azure Spring Cloud:

    ```Azure CLI
    az group create --location eastus --name <resource group name>
    ```

1. Effettuare il provisioning di un'istanza di Azure Spring Cloud:

    ```Azure CLI
    az spring-cloud create -n <service name> -g <resource group name>
    ```

1. Creare una copia tramite fork del repository [Piggy Metrics](https://github.com/Azure-Samples/piggymetrics) per il proprio account GitHub. Nel computer locale clonare il repository in una directory denominata `source-code`:

    ```bash
    mkdir source-code
    git clone https://github.com/<your GitHub id>/piggymetrics
    ```

1. Configurare il server di configurazione. Assicurarsi di sostituire il &lt;proprio ID GitHub&gt; con il valore corretto.

    ```Azure CLI
    az spring-cloud config-server git set -n <your-service-name> --uri https://github.com/<your GitHub id>/piggymetrics --label config
    ```

1. Compilare il progetto:

    ```bash
    cd piggymetrics
    mvn clean package -D skipTests
    ```

1. Creare i tre microservizi **gateway**, **auth-service** e **account-service**:

    ```Azure CLI
    az spring-cloud app create --n gateway -s <service name> -g <resource group name>
    az spring-cloud app create --n auth-service -s <service name> -g <resource group name>
    az spring-cloud app create --n account-service -s <service name> -g <resource group name>
    ```

1. Distribuire le applicazioni:

    ```Azure CLI
    az spring-cloud app deploy -n gateway -s <service name> -g <resource group name> --jar-path ./gateway/target/gateway.jar
    az spring-cloud app deploy -n account-service -s <service name> -g <resource group name> --jar-path ./account-service/target/account-service.jar
    az spring-cloud app deploy -n auth-service -s <service name> -g <resource group name> --jar-path ./auth-service/target/auth-service.jar
    ```

1. Assegnare un endpoint pubblico al gateway:

    ```Azure CLI
    az spring-cloud app update -n gateway -s <service name> -g <resource group name> --is-public true
    ```

1. Eseguire una query sull'applicazione gateway per ottenere il relativo URL in modo da verificare se è in esecuzione.

    ```Azure CLI
    az spring-cloud app show --name gateway | grep url
    ```
    
 1. Passare all'URL specificato dal comando precedente per eseguire l'applicazione PiggyMetrics.

## <a name="prepare-jenkins-server"></a>Preparare il server Jenkins

In questa sezione verrà preparato il server Jenkins per eseguire una compilazione corretta per i test. Tuttavia, per motivi di sicurezza, è necessario usare un [agente di VM di Azure](https://plugins.jenkins.io/azure-vm-agents) o un [agente di contenitore di Azure](https://plugins.jenkins.io/azure-container-agents) per creare un agente in Azure che esegua le compilazioni. Per ulteriori informazioni, vedere l'articolo Jenkins sulle [implicazioni per la sicurezza di compilazione su master](https://wiki.jenkins.io/display/JENKINS/Security+implication+of+building+on+master).

### <a name="install-plug-ins"></a>Installare i plug-in

1. Accedere al server Jenkins. Scegliere **Gestisci Jenkins > Gestisci plug-in**.

1. Nella scheda **Disponibili** selezionare i plug-in seguenti:

    * [Integrazione GitHub](https://plugins.jenkins.io/github-pullrequest)
    * [Credenziali di Azure](https://plugins.jenkins.io/azure-credentials)

    Se questi plug-in non sono inclusi nell'elenco, passare alla scheda **Installati** per verificare se sono già installati.

1. Per installare i plug-in, scegliere **Download now and install after restart** (Scarica subito e installa dopo il riavvio).

1. Riavviare il server Jenkins per completare l'installazione.

### <a name="add-your-azure-service-principal-credential-in-jenkins-credential-store"></a>Aggiungere le credenziali dell'entità servizio di Azure nell'archivio delle credenziali di Jenkins

1. Per eseguire la distribuzione in Azure, è necessaria un'entità servizio di Azure. Per altre informazioni, vedere la sezione  [Creare l'entità servizio](deploy-from-github-to-azure-app-service.md#create-service-principal) dell'esercitazione Eseguire la distribuzione in Servizio app di Azure. L'output di `az ad sp create-for-rbac` è simile al seguente:

    ```
    {
        "appId": "xxxxxx-xxx-xxxx-xxx-xxxxxxxxxxxx",
        "displayName": "xxxxxxxjenkinssp",
        "name": "http://xxxxxxxjenkinssp",
        "password": "xxxxxx-xxx-xxxx-xxx-xxxxxxxxxxxx",
        "tenant": "xxxxxx--xxx-xxxx-xxx-xxxxxxxxxxxx"
    }
    ```

1. Nel dashboard di Jenkins selezionare **Credentials** > **System** (Credenziali > Sistema). Quindi, selezionare **Global credentials(unrestricted)** (Credenziali globali senza restrizioni).

1. Selezionare **Aggiungi credenziali**.

1. Selezionare **Entità servizio di Microsoft Azure** come tipo.

1. Fornire i valori per i campi seguenti:

    - **ID sottoscrizione**: ID sottoscrizione di Azure
    - **ID client**: ID app dell'entità servizio
    - **Segreto client**: Password dell'entità servizio
    - **ID tenant**: ID tenant dell'account Microsoft
    - **Ambiente di Azure**: selezionare il valore appropriato per l'ambiente. Usare ad esempio **Azure** per Azure globale
    - **ID**: impostare questo valore come `azure_service_principal`. L'ID verrà usato in un passaggio successivo dell'articolo
    - **Descrizione**: questo valore è facoltativo ma consigliato.
    
### <a name="install-maven-and-az-cli-spring-cloud-extension"></a>Installare Maven e l'estensione spring-cloud dell'interfaccia della riga di comando di Azure

La pipeline di esempio usa Maven per la compilazione e l'interfaccia della riga di comando di Azure per la distribuzione nell'istanza del servizio. Quando viene installato, Jenkins crea un account amministratore denominato *jenkins*. Verificare che l'utente *jenkins* abbia l'autorizzazione per eseguire l'estensione spring-cloud.

1. Connettersi al master Jenkins tramite SSH.

1. Installare Maven.

    ```bash
    sudo apt-get install maven
    ```

1. Verificare che l'interfaccia della riga di comando di Azure sia installata immettendo `az version`. Se l'interfaccia della riga di comando di Azure non è installata, vedere [Installazione dell'interfaccia della riga di comando di Azure](/cli/azure/install-azure-cli).

1. Passare all'utente `jenkins`:

    ```bash
    sudo su jenkins
    ```

1. Aggiungere l'estensione **spring-cloud**:

    ```bash
    az extension add --name spring-cloud
    ```

## <a name="create-a-jenkinsfile"></a>Creare un Jenkinsfile

1. Nel proprio repository (https://github.com/&lt ;ID GitHub&gt; /piggymetrics) creare un **Jenkinsfile** nella radice.

1. Aggiornare il file come segue. Assicurarsi di sostituire i valori di **\<resource group name>** e **\<service name>** . Sostituire **azure_service_principal** con l'ID corretto se è stato usato un valore diverso quando sono state aggiunte le credenziali in Jenkins.

   ```groovy
       node {
         stage('init') {
           checkout scm
         }
         stage('build') {
           sh 'mvn clean package'
         }
         stage('deploy') {
           withCredentials([azureServicePrincipal('azure_service_principal')]) {
             // login to Azure
             sh '''
               az login --service-principal -u $AZURE_CLIENT_ID -p $AZURE_CLIENT_SECRET -t $AZURE_TENANT_ID
               az account set -s $AZURE_SUBSCRIPTION_ID
             '''  
             // Set default resource group name and service name. Replace <resource group name> and <service name> with the right values
             sh 'az configure --defaults group=<resource group name>'
             sh 'az configure --defaults spring-cloud=<service name>'
             // Deploy applications
             sh 'az spring-cloud app deploy -n gateway --jar-path ./gateway/target/gateway.jar'
             sh 'az spring-cloud app deploy -n account-service --jar-path ./account-service/target/account-service.jar'
             sh 'az spring-cloud app deploy -n auth-service --jar-path ./auth-service/target/auth-service.jar'
             sh 'az logout'
           }
         }
       }
   ```

1. Salvare la modifica ed eseguirne il commit.

## <a name="create-the-job"></a>Creare il processo

1. Nel dashboard di Jenkins fare clic su **New Item** (Nuovo elemento).

1. Specificare il nome *Deploy-PiggyMetrics* per il processo e selezionare **Pipeline**. Fare clic su OK.

1. Quindi fare clic sulla scheda **Pipeline**.

1. Per **Definition** selezionare **Pipeline script from SCM**.

1. Per **SCM** selezionare **Git**.

1. Immettere l'URL di GitHub per il repository copiato tramite fork: **https://github.com/&lt ;ID GitHub&gt; /piggymetrics.git**

1. Assicurarsi che l'opzione **Branch Specifier (black for 'any')** (Specifica ramo - vuoto per 'qualsiasi') sia impostata su * **/Azure**

1. Mantenere il **percorso dello script** impostato su **Jenkinsfile**

1. Fare clic su **Save** (Salva).

## <a name="validate-and-run-the-job"></a>Convalidare ed eseguire il processo

Prima di eseguire il processo, aggiornare il testo nella casella di input di accesso specificando **enter login ID**.

1. Nel repository aprire `index.html` in **/gateway/src/main/resources/static/**

1. Cercare il testo "enter your login" e sostituirlo con "enter login ID"

    ```HTML
    <input class="frontforms" id="frontloginform" name="username" placeholder="enter login ID" type="text" autocomplete="off"/>
    ```

1. Eseguire il commit delle modifiche

1. Eseguire manualmente il processo in Jenkins. Nel dashboard di Jenkins fare clic sul processo *Deploy-PiggyMetrics* e quindi su **Build Now** (Compila).

Al termine del processo, passare all'indirizzo IP pubblico dell'applicazione **gateway** e verificare che sia stata aggiornata. 

![Piggy Metrics aggiornata](./media/deploy-to-azure-spring-cloud-using-azure-cli/piggymetrics.png)

## <a name="clean-up-resources"></a>Pulire le risorse

Quando non sono più necessarie, eliminare le risorse create in questo articolo:

```bash
az group delete -y --no-wait -n <resource group name>
```

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"]
> [Jenkins in Azure](/azure/jenkins/)