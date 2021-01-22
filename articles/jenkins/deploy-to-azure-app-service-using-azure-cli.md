---
title: "Esercitazione: Eseguire la distribuzione nel servizio app di Azure con Jenkins e l'interfaccia della riga di comando di Azure"
description: Informazioni su come usare l'interfaccia della riga di comando di Azure per distribuire un'app web di Java in Azure in Jenkins Pipeline
keywords: jenkins, azure, devops, servizio app, interfaccia della riga di comando
ms.topic: tutorial
ms.date: 01/06/2021
ms.custom: devx-track-jenkins, devx-track-azurecli
ms.openlocfilehash: 1f73da29b6b1bff2abf92383d672afd5af92abe4
ms.sourcegitcommit: 0eb25e1fdafcd64118843748dc061f60e7e48332
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/21/2021
ms.locfileid: "98669167"
---
# <a name="tutorial-deploy-to-azure-app-service-with-jenkins-and-the-azure-cli"></a>Esercitazione: Distribuire nel servizio app di Azure con Jenkins e l'interfaccia della riga di comando di Azure

[!INCLUDE [jenkins-integration-with-azure.md](includes/jenkins-integration-with-azure.md)]

Per distribuire un'app Web di Java in Azure, è possibile usare l'interfaccia della riga di comando di Azure in [Jenkins Pipeline](https://jenkins.io/doc/book/pipeline/). In questa esercitazione si eseguiranno le attività seguenti:

> [!div class="checklist"]
> * Creare una macchina virtuale Jenkins
> * Configurare Jenkins
> * Creare un'app Web in Azure
> * Preparare un repository GitHub
> * Creare una pipeline Jenkins
> * Eseguire la pipeline e verificare l'app Web

## <a name="prerequisites"></a>Prerequisiti

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../includes/open-source-devops-prereqs-azure-subscription.md)]

- **Jenkins** - [Installare Jenkins in una VM Linux](configure-on-linux-vm.md)
- **Interfaccia della riga di comando di Azure**: Installare l'interfaccia della riga di comando di Azure (versione 2.0.67 o successiva) nel server Jenkins.

## <a name="configure-jenkins"></a>Configurare Jenkins

I passaggi seguenti illustrano come installare il pacchetto Java JDK e Maven necessario sul controller Jenkins:

1. Accedere al controller Jenkins tramite SSH.

1. [Scaricare e installare i JDK Azul Zulu da un repository apt-get](../java/fundamentals/java-jdk-install.md#download-and-install-the-azul-zulu-jdks-from-an-apt-get-repository):

    ```bash
    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 0xB1998361219BD9C9
    sudo apt-add-repository "deb http://repos.azul.com/azure-only/zulu/apt stable main"
    sudo apt-get -q update
    sudo apt-get -y install zulu-8-azure-jdk
    ```
    
1. Eseguire il comando seguente per installare Maven:

    ```bash
    sudo apt-get install -y maven
    ```
    
## <a name="add-azure-service-principal-to-a-jenkins-credential"></a>Aggiungere l'entità servizio di Azure a una credenziale di Jenkins

La procedura seguente illustra come specificare le credenziali di Azure:

1. Verificare che il [plug-in Credentials](https://plugins.jenkins.io/credentials/) sia installato.

1. Nel dashboard di Jenkins selezionare **Credentials -> System ->** (Credenziali -> Sistema). 

1. Selezionare **Global credentials (unrestricted)** (Credenziali globali - senza restrizioni).

1. Selezionare **Add Credentials** (Aggiungi credenziali) per aggiungere un'[entità servizio di Microsoft Azure](/cli/azure/create-an-azure-service-principal-azure-cli?toc=%252fazure%252fazure-resource-manager%252ftoc.json). Verificare che il tipo di credenziali sia *_Username with password_* _ (Nome utente con password) e immettere gli elementi seguenti:

    _ **Username** (Nome utente): `appId` dell'entità servizio
    * **Password**: `password` dell'entità servizio
    * **ID**: identificatore di credenziali (ad esempio `AzureServicePrincipal`)

## <a name="create-an-azure-app-service-for-deploying-the-java-web-app"></a>Creare un servizio app di Azure per la distribuzione dell'app Web di Java

Usare il comando [az appservice plan create](/cli/azure/appservice/plan#az-appservice-plan-create) per creare un piano di servizio app di Azure con il piano tariffario **GRATUITO**:

```azurecli
az appservice plan create \
    --name <app_service_plan> \ 
    --resource-group <resource_group> \
    --sku FREE
```

**Note**:

- Il piano di servizio app definisce le risorse fisiche usate per ospitare le app.
- Tutte le applicazioni assegnate a un piano di servizio app condividono queste risorse.
- I piani di servizio app consentono di risparmiare sui costi in caso di hosting di più app.

## <a name="create-an-azure-web-app"></a>Creare un'app Web di Azure

Usare il comando [az webapp create](/cli/azure/webapp#az-webapp-create) per creare una definizione di app Web nel piano di servizio app `myAppServicePlan`.

```azurecli
az webapp create \
    --name <app_name> \ 
    --resource-group <resource_group> \
    --plan <app_service_plan>
```

**Note**:

- La definizione dell'app Web fornisce un URL con cui accedere all'applicazione e configura diverse opzioni per distribuire il codice in Azure.
- Sostituire il segnaposto `<app_name>` con un nome univoco per l'app.
- Il nome dell'app fa parte del nome di dominio predefinito dell'app Web. Deve pertanto essere univoco in tutte le app di Azure.
- È possibile eseguire il mapping di una voce di nome di dominio personalizzata all'app Web prima di esporla agli utenti.


## <a name="configure-java"></a>Configurare Java

Usare il comando [az appservice web config update](/cli/azure/webapp/config) per impostare la configurazione del runtime Java per l'app:

```azurecli
az webapp config set \ 
    --name <app_name> \
    --resource-group <resource_group> \ 
    --java-version 1.8 \ 
    --java-container Tomcat \
    --java-container-version 8.0
```

## <a name="prepare-a-github-repository"></a>Preparare un repository GitHub

1. Aprire il repository [Simple Java Web App for Azure](https://github.com/azure-devops/javawebappsample).

1. Selezionare il pulsante **Fork** per creare una copia tramite fork del repository nel proprio account GitHub.

1. Aprire il file **Jenkinsfile** facendo clic sul relativo nome.

1. Selezionare l'icona della matita per modificare il file.

1. Aggiornare l'ID sottoscrizione e l'ID tenant.
    
    ```groovy
      withEnv(['AZURE_SUBSCRIPTION_ID=<subscription_id>',
            'AZURE_TENANT_ID=<tenant_id>']) 
    ```
    
1. Aggiornare il gruppo di risorse e il nome dell'app Web nella riga 22 e 23 rispettivamente.

    ```groovy
    def resourceGroup = '<resource_group>'
    def webAppName = '<app_name>'
    ```

1. Aggiornare l'ID delle credenziali nell'istanza di Jenkins

    ```groovy
    withCredentials([usernamePassword(credentialsId: '<service_princial>', passwordVariable: 'AZURE_CLIENT_SECRET', usernameVariable: 'AZURE_CLIENT_ID')]) {
    ```
    
## <a name="create-jenkins-pipeline"></a>Creare una pipeline Jenkins

Per creare una pipeline di Jenkins, seguire questa procedura:

1. Aprire Jenkins in un Web browser.

1. Selezionare **Nuovo elemento**.

1. Immettere un nome per il processo.

1. Selezionare **Pipeline**.

1. Selezionare **OK**.

1. Selezionare **Pipeline**.

1. Per **Definition** selezionare **Pipeline script from SCM**.

1. Per **SCM** selezionare **Git**.

1. Immettere l'URL di GitHub per il repository copiato tramite con fork: `https:\<forked_repo\>.git`

1. Selezionare **Salva**

## <a name="test-your-pipeline"></a>Testare la pipeline

1. Passare alla pipeline creata

1. Selezionare **Build now** (Compila)

1. Al termine della compilazione, selezionare **Output console** per visualizzare i relativi dettagli.

## <a name="verify-your-web-app"></a>Verificare l'app Web

Per verificare che il file WAR sia stato distribuito correttamente nell'app Web, seguire questa procedura:

1. Passare all'URL seguente: `http://&lt;app_name>.azurewebsites.net/api/calculator/ping`

1. L'output dovrebbe essere simile al testo seguente:

    ```output
    Welcome to Java Web App!!! This is updated!
    Today's date
    ```

1. Passare all'URL seguente (sostituire &lt;x> e &lt;y> con due valori da sommare): http://&lt;app_name>.azurewebsites.net/api/calculator/add?x=&lt;x>&y=&lt;y>.

    ![Esempio di esecuzione dell'app demo](./media/deploy-to-azure-app-service-using-azure-cli/calculator-add.png)

## <a name="deploy-to-azure-app-service-on-linux"></a>Eseguire la distribuzione nel Servizio app di Azure in Linux

Il servizio app può anche ospitare le app Web in modo nativo in Linux per gli stack di applicazioni supportate. Può anche eseguire contenitori Linux personalizzati, noti anche come app Web per contenitori.

È possibile modificare lo script per eseguire la distribuzione in un servizio app di Azure in Linux. Il Servizio app di Azure in Linux supporta Docker. Di conseguenza, specificare un Dockerfile che assembla l'app Web con il runtime del servizio in un'immagine Docker. Il plug-in crea l'immagine, ne esegue il push in un registro Docker e la distribuisce nell'app Web.

1. Per informazioni su come creare un servizio app di Azure in Linux e un registro Azure Container, vedere [Eseguire la migrazione di un software personalizzato al Servizio app di Azure usando un contenitore personalizzato](/azure/app-service/tutorial-custom-container?pivots=container-linux#configure-app-service-to-deploy-the-image-from-the-registry).

    ```azurecli
        az group create --name myResourceGroup2 --location westus2
        az acr create --name myACRName --resource-group myResourceGroup2 --sku Basic --admin-enabled true
        az appservice plan create --name myAppServicePlan --resource-group  myResourceGroup2 --is-linux
        az webapp create --resource-group myResourceGroup2 --plan myAppServicePlan --name myApp --deployment-container-image-name myACRName.azurecr.io/calculator:latest
    ```

1. [Installare Docker in Jenkins](https://docs.docker.com/engine/installation/linux/ubuntu/).

1. Verificare che il [plug-in Docker Pipeline](https://plugins.jenkins.io/docker-workflow/) sia installato.

1. Nello stesso repository [Simple Java Web App for Azure](https://github.com/azure-devops/javawebappsample) copiato tramite fork modificare il file **Jenkinsfile2** come segue:

    1. Aggiornare l'ID sottoscrizione e l'ID tenant.

        ```groovy
         withEnv(['AZURE_SUBSCRIPTION_ID=<mySubscriptionId>',
                'AZURE_TENANT_ID=<myTenantId>']) {
        ```

    1. Aggiornare i nomi del gruppo di risorse, dell'app Web e di Registro Azure Container, sostituendo i segnaposto con i propri valori.

        ```bash
        def webAppResourceGroup = '<resource_group>'
        def webAppName = '<app_name>'
        def acrName = '<registry>'
        ```

    1. Aggiornare `<azsrvprincipal\>` in base all'ID credenziali

        ```bash
        withCredentials([usernamePassword(credentialsId: '<service_principal>', passwordVariable: 'AZURE_CLIENT_SECRET', usernameVariable: 'AZURE_CLIENT_ID')]) {
        ```

1. Creare una nuova pipeline Jenkins come è già stato fatto per la distribuzione dell'app Web di Azure in Windows usando `Jenkinsfile2`.

1. Eseguire il nuovo processo.

1. Per verifica, eseguire il comando seguente nell'interfaccia della riga di comando di Azure:

    ```azurecli
    az acr repository list -n <myRegistry> -o json
    ```

    Verranno visualizzati risultati simili ai seguenti:
    
    ```output
    [
    "calculator"
    ]
    ```
    
1. Passare a `http://<app_name>.azurewebsites.net/api/calculator/ping` (sostituendo il segnaposto). I risultati visualizzati saranno simili ai seguenti: 

    ```output
    Welcome to Java Web App!!! This is updated!
    Today's date
    ```

1. Passare a `http://<app_name>.azurewebsites.net/api/calculator/add?x=<x>&y=<y>` (sostituendo i segnaposto). I valori specificati per `x` e `y` vengono sommati e visualizzati.
    
## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"]
> [Jenkins in Azure](/azure/jenkins)