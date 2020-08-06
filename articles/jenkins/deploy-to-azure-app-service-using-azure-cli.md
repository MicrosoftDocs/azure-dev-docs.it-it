---
title: "Esercitazione: Eseguire la distribuzione nel servizio app di Azure con Jenkins e l'interfaccia della riga di comando di Azure"
description: Informazioni su come usare l'interfaccia della riga di comando di Azure per distribuire un'app web di Java in Azure in Jenkins Pipeline
keywords: jenkins, azure, devops, servizio app, interfaccia della riga di comando
ms.topic: tutorial
ms.date: 04/25/2020
ms.custom: devx-track-azurecli
ms.openlocfilehash: 63a5097358001e0312af13053e3d7310fe413cc7
ms.sourcegitcommit: e451e4360d9c5956cc6a50880b3a7a55aa4efd2f
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/31/2020
ms.locfileid: "87478341"
---
# <a name="tutorial-deploy-to-azure-app-service-with-jenkins-and-the-azure-cli"></a>Esercitazione: Distribuire nel servizio app di Azure con Jenkins e l'interfaccia della riga di comando di Azure

Per distribuire un'app Web di Java in Azure, è possibile usare l'interfaccia della riga di comando di Azure in [Jenkins Pipeline](https://jenkins.io/doc/book/pipeline/). In questa esercitazione si eseguiranno le attività seguenti:

> [!div class="checklist"]
> * Creare una macchina virtuale Jenkins
> * Configurare Jenkins
> * Creare un'app Web in Azure
> * Preparare un repository GitHub
> * Creare una pipeline Jenkins
> * Eseguire la pipeline e verificare l'app Web

## <a name="create-and-configure-jenkins-instance"></a>Creare e configurare un'istanza di Jenkins

Se non si dispone ancora di un master Jenkins, installarlo con il [modello di soluzione Jenkins](configure-on-linux-vm.md). Per impostazione predefinita, il modello installa il plug-in per [credenziali di Azure](https://plugins.jenkins.io/azure-credentials) necessario. 

Il plug-in delle credenziali di Azure consente di archiviare le credenziali dell'entità servizio di Microsoft Azure in Jenkins. Nella versione 1.2 è stato aggiunto il supporto affinché Jenkins Pipeline possa ottenere le credenziali di Azure. 

Assicurarsi di disporre della versione 1.2 o versioni successive:

* Nel dashboard di Jenkins, fare clic su **Manage Jenkins -> Plugin Manager ->** e cercare **Azure Credential**. 
* Se la versione è meno recente della 1.2, aggiornare il plug-in.

Anche Java JDK e Maven sono necessari nel master di Jenkins. Per l'installazione, accedere al master di Jenkins usando SSH ed eseguire i comandi seguenti:

```bash
sudo apt-get install -y openjdk-7-jdk
sudo apt-get install -y maven
```

## <a name="add-azure-service-principal-to-a-jenkins-credential"></a>Aggiungere l'entità servizio di Azure a una credenziale di Jenkins

Per eseguire l'interfaccia della riga di comando di Azure è necessaria una credenziale di Azure.

* Nel dashboard di Jenkins fare clic su **Credentials -> System ->** . Fare clic su **Global credentials(unrestricted)** .
* Fare clic su **Add Credentials** per aggiungere un'[entità servizio di Microsoft Azure](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json), immettendo: Subscription ID (ID sottoscrizione), Client ID (ID client), Client Secret (Segreto client) e OAuth 2.0 Token Endpoint (Endpoint di token OAuth 2.0). Indicare un ID per l'uso nel passaggio successivo.

![Add Credentials](./media/deploy-to-azure-app-service-using-azure-cli/add-credentials.png)

## <a name="create-an-azure-app-service-for-deploying-the-java-web-app"></a>Creare un servizio app di Azure per la distribuzione dell'app Web di Java

Creare un piano di servizio app di Azure con il piano tariffario **GRATUITO** usando il comando dell'interfaccia della riga di comando [az appservice plan create](/cli/azure/appservice/plan#az-appservice-plan-create). Il piano di servizio app definisce le risorse fisiche usate per ospitare le app. Tutte le applicazioni assegnate a un piano di servizio app condividono queste risorse, per poter consentire un risparmio sui costi quando si ospitano più app. 

```azurecli-interactive
az appservice plan create \
    --name myAppServicePlan \ 
    --resource-group myResourceGroup \
    --sku FREE
```

Quando il piano è pronto, l'output dell'interfaccia della riga di comando di Azure è simile all'esempio seguente:

```json
{ 
  "adminSiteName": null,
  "appServicePlanName": "myAppServicePlan",
  "geoRegion": "North Europe",
  "hostingEnvironmentProfile": null,
  "id": "/subscriptions/0000-0000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/myAppServicePlan",
  "kind": "app",
  "location": "North Europe",
  "maximumNumberOfWorkers": 1,
  "name": "myAppServicePlan",
  ...
  < Output has been truncated for readability >
} 
``` 

### <a name="create-an-azure-web-app"></a>Creare un'app Web di Azure

 Usare il comando dell'interfaccia della riga di comando [az webapp create](/cli/azure/webapp?view=azure-cli-latest#az-webapp-create) per creare la definizione di un'app Web nel piano di servizio app `myAppServicePlan`. La definizione dell'app Web fornisce un URL con cui accedere all'applicazione e configura diverse opzioni per distribuire il codice in Azure. 

```azurecli-interactive
az webapp create \
    --name <app_name> \ 
    --resource-group myResourceGroup \
    --plan myAppServicePlan
```

Sostituire il segnaposto `<app_name>` con il nome univoco dell'app. Questo nome univoco fa parte del nome di dominio predefinito per l'app Web, quindi è necessario che sia univoco rispetto a tutte le app presenti in Azure. È possibile eseguire il mapping di una voce di nome di dominio personalizzata all'app Web prima di esporla agli utenti.

Quando la definizione dell'app Web è pronta, l'interfaccia della riga di comando di Azure visualizza informazioni simili all'esempio seguente: 

```json 
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app_name>.azurewebsites.net",
  "enabled": true,
   ...
  < Output has been truncated for readability >
}
```

### <a name="configure-java"></a>Configurare Java

Impostare la configurazione del runtime Java necessaria per l'app con il comando [az appservice web config update](/cli/azure/webapp/config).

Il comando seguente configura l'app Web per l'esecuzione in un'istanza recente di Java 8 JDK e in [Apache Tomcat](https://tomcat.apache.org/) 8.0.

```azurecli
az webapp config set \ 
    --name <app_name> \
    --resource-group myResourceGroup \ 
    --java-version 1.8 \ 
    --java-container Tomcat \
    --java-container-version 8.0
```

## <a name="prepare-a-github-repository"></a>Preparare un repository GitHub

1. Aprire il repository [Simple Java Web App for Azure](https://github.com/azure-devops/javawebappsample). Per creare il fork del repository nel proprio account GitHub, fare clic sul pulsante **Fork** nell'angolo superiore destro.

1. Nell'interfaccia utente Web di GitHub, aprire il file **Jenkinsfile**. Fare clic sull'icona della matita per modificare il file per aggiornare il gruppo di risorse e il nome dell'app Web nella riga 20 e 21 rispettivamente.

    ```java
    def resourceGroup = '<myResourceGroup>'
    def webAppName = '<app_name>'
    ```
    
1. Modificare la riga 23 per aggiornare l'ID delle credenziali nell'istanza di Jenkins

    ```java
    withCredentials([azureServicePrincipal('<mySrvPrincipal>')]) {
    ```
    
## <a name="create-jenkins-pipeline"></a>Creare una pipeline Jenkins

Aprire Jenkins in un Web browser, fare clic su **New Item**.

1. Immettere un nome per il processo.
1. Selezionare **Pipeline**. 
1. Selezionare **OK**.
1. Selezionare **Pipeline**.
1. Per **Definition** selezionare **Pipeline script from SCM**.
1. Per **SCM** selezionare **Git**.
1. Immettere l'URL di GitHub per il repository copiato tramite con fork: `https:\<your forked repo\>.git`
1. Selezionare **Salva**

## <a name="test-your-pipeline"></a>Testare la pipeline

1. Passare alla pipeline creata
1. Fare clic su **Compila**
1. Al termine della compilazione, selezionare **Output console** per visualizzare i relativi dettagli.

## <a name="verify-your-web-app"></a>Verificare l'app Web

Per verificare che il file WAR sia stato distribuito correttamente nell'app Web, procedere come segue. 

1. Aprire un Web browser:

1. Passare a `http://&lt;app_name>.azurewebsites.net/api/calculator/ping`

1. L'output dovrebbe essere simile al testo seguente:

    ```output
    Welcome to Java Web App!!! This is updated!
    Today's date
    ```

1. Passare a http://&lt;app_name>.azurewebsites.net/api/calculator/add?x=&lt;x>&y=&lt;y> (sostituire &lt;x> e &lt;y> con numeri qualsiasi) per ottenere la somma di x e y

    ![Calcolatrice: aggiungi](./media/deploy-to-azure-app-service-using-azure-cli/calculator-add.png)

## <a name="deploy-to-azure-web-app-on-linux"></a>Distribuire un'app Web di Azure in Linux

Dopo aver usato l'interfaccia della riga di comando di Azure nella pipeline Jenkins, modificare lo script per eseguire la distribuzione in un'app Web di Azure in Linux. Le app Web in Linux supportano Docker. Di conseguenza, specificare un Dockerfile che assembla l'app Web con il runtime del servizio in un'immagine Docker. Il plug-in crea l'immagine, ne esegue il push in un registro Docker e la distribuisce nell'app Web.

1. [Creare un'app Web di Azure eseguita in Linux](/azure/app-service/containers/quickstart-nodejs).

1. [Installare Docker in Jenkins](https://docs.docker.com/engine/installation/linux/ubuntu/).

1. [Creare un registro contenitori nel portale di Azure](/azure/container-registry/container-registry-get-started-azure-cli).

1. Nello stesso repository [Simple Java Web App for Azure](https://github.com/azure-devops/javawebappsample) copiato tramite fork modificare il file **Jenkinsfile2** come segue:

    1. Aggiornare i nomi del gruppo di risorse, dell'app Web e di Registro Azure Container, sostituendo i segnaposto con i propri valori.

        ```bash
        def webAppResourceGroup = '<myResourceGroup>'
        def webAppName = '<app_name>'
        def acrName = '<myRegistry>'
        ```

    1. Aggiornare `<azsrvprincipal\>` in base all'ID credenziali

        ```bash
        withCredentials([azureServicePrincipal('<mySrvPrincipal>')]) {
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
