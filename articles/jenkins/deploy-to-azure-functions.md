---
title: 'Esercitazione: Eseguire la distribuzione in Funzioni di Azure con Jenkins'
description: Informazioni su come eseguire la distribuzione in Funzioni di Azure tramite il plug-in Jenkins per Funzioni di Azure
keywords: jenkins, azure, devops, java, funzioni di azure
ms.topic: tutorial
ms.date: 01/11/2021
ms.custom: devx-track-jenkins,devx-track-cli
ms.openlocfilehash: 51807b1a3038d17278a6015d387b84e68aac71f5
ms.sourcegitcommit: 347bfa3b6c34579c567d1324efc63c1d6672a75b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/12/2021
ms.locfileid: "98109030"
---
# <a name="tutorial-deploy-to-azure-functions-using-jenkins"></a>Esercitazione: Eseguire la distribuzione in Funzioni di Azure con Jenkins

[!INCLUDE [jenkins-integration-with-azure.md](includes/jenkins-integration-with-azure.md)]

[Funzioni di Azure](/azure/azure-functions/) è un servizio di calcolo senza server. Usando Funzioni di Azure è possibile eseguire codice on demand senza provisioning o gestione dell'infrastruttura. Questa esercitazione mostra come distribuire una funzione Java in Funzioni di Azure tramite il plug-in per Funzioni di Azure.

## <a name="prerequisites"></a>Prerequisiti

- **Sottoscrizione di Azure**: Se non si ha una sottoscrizione di Azure, creare un [account gratuito](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) prima di iniziare.
- **Server Jenkins**: se non è installato un server Jenkins, vedere l'articolo [Creare un server Jenkins in Azure](./configure-on-linux-vm.md).

## <a name="view-the-source-code"></a>Visualizzare il codice sorgente

Il codice sorgente usato per questa esercitazione si trova nel [repository GitHub Visual Studio China](https://github.com/VSChina/odd-or-even-function/blob/master/src/main/java/com/microsoft/azure/Function.java).

## <a name="create-a-java-function"></a>Creare una funzione Java

Per creare una funzione Java con lo stack di runtime Java, usare il [portale di Azure](https://portal.azure.com) o l'[interfaccia della riga di comando di Azure](/cli/azure/).

La procedura seguente mostra come creare una funzione Java tramite l'interfaccia della riga di comando di Azure:

1. Creare un gruppo di risorse, sostituendo il segnaposto **&lt;resource_group>** con il nome del gruppo di risorse.

    ```azurecli
    az group create --name <resource_group> --location eastus
    ```

1. Creare un account di archiviazione di Azure, sostituendo i segnaposto con i valori appropriati.
 
    ```azurecli
    az storage account create --name <storage_account> --location eastus --resource-group <resource_group> --sku Standard_LRS    
    ```

1. Creare l'app per le funzioni di test, sostituendo i segnaposto con i valori appropriati.

    ```azurecli
    az functionapp create --resource-group <resource_group> --runtime java --consumption-plan-location eastus --name <function_app> --storage-account <storage_account> --functions-version 2
    ```

## <a name="prepare-jenkins-server"></a>Preparare il server Jenkins

La procedura seguente descrive come preparare il server Jenkins:

1. Distribuire un [server Jenkins](https://azuremarketplace.microsoft.com/marketplace/apps/bitnami.production-jenkins) in Azure. Se non è già installata un'istanza del server Jenkins, l'articolo [Creare un server Jenkins in Azure](./configure-on-linux-vm.md) descrive in modo dettagliato questo processo.

1. Accedere all'istanza di Jenkins con SSH.

1. Nelll'istanza di Jenkins installare l'interfaccia della riga di comando di Azure versione 2.0.67 o successiva.

1. Installare Maven usando il comando seguente:

    ```bash
    sudo apt install -y maven
    ```

1. Nell'istanza di Jenkins installare [Azure Functions Core Tools](/azure/azure-functions/functions-run-local) eseguendo i comandi seguenti da un prompt del terminale:

    ```bash
    curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
    sudo mv microsoft.gpg /etc/apt/trusted.gpg.d/microsoft.gpg
    sudo sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/repos/microsoft-ubuntu-$(lsb_release -cs)-prod $(lsb_release -cs) main" > /etc/apt/sources.list.d/dotnetdev.list'
    cat /etc/apt/sources.list.d/dotnetdev.list
    sudo apt-get update
    sudo apt-get install azure-functions-core-tools-3
    ```

1. Jenkins richiede un'entità servizio di Azure per l'autenticazione e l'accesso alle risorse di Azure. Per istruzioni dettagliate, fare riferimento a [Eseguire la distribuzione in Servizio app di Azure](./deploy-from-github-to-azure-app-service.md).

1. Verificare che il [plug-in Credentials](https://plugins.jenkins.io/credentials/) sia installato.

    1. Nel menu selezionare **Manage Jenkins** (Gestisci Jenkins).

    1. In **System Configuration** (Configurazione del sistema) selezionare **Manage Plugins** (Gestisci plug-in).

    1. Selezionare la scheda **Installati**.

    1. Nel campo del **filtro** immettere `credentials`.
    
    1. Verificare che il **plug-in Credentials** sia installato. Se non lo è, occorre installarlo dalla scheda **Available** (Disponibili).

    ![Il plug-in Credentials deve essere installato.](./media/deploy-to-azure-functions/credentials-plugin.png)

1. Nel menu selezionare **Manage Jenkins** (Gestisci Jenkins).

1. In **Security** (Sicurezza) selezionare **Manage Credentials** (Gestisci credenziali).

1. In **Credentials** (Credenziali) selezionare **(global)** .

1. Scegliere **Add Credentials** (Aggiungi credenziali) dal menu.

1. Immettere i valori seguenti per l'[entità servizio di Microsoft Azure](/cli/azure/create-an-azure-service-principal-azure-cli?toc=%252fazure%252fazure-resource-manager%252ftoc.json):

    - **Tipologia**: verificare che il tipo sia **_Username with password_* _ (Nome utente con password).
    - _*Username**: **_appId_*_ dell'entità servizio creata.
    - _*Password**: **_password_*_ dell'entità servizio creata.
    - _*ID**: identificatore di credenziali, ad esempio `as azuresp`.

1. Selezionare **OK**.

## <a name="fork-the-sample-github-repo"></a>Creare una copia tramite fork del repository GitHub di esempio

1. [Accedere al repository GitHub per l'app di esempio odd-or-even](https://github.com/VSChina/odd-or-even-function.git).

1. Nell'angolo superiore destro in GitHub scegliere **Fork**.

1. Seguire le istruzioni per selezionare l'account GitHub e completare la creazione di una copia tramite fork.

## <a name="create-a-jenkins-pipeline"></a>Creare una pipeline Jenkins

In questa sezione viene creata la [pipeline Jenkins](https://jenkins.io/doc/book/pipeline/).

1. Nel dashboard di Jenkins creare una pipeline.

1. Selezionare **Prepare an environment for the run** (Preparare un ambiente per l'esecuzione).

1. Nella sezione **Pipeline > Definition** (Pipeline > Definizione) selezionare **Pipeline script from SCM** (Script di pipeline da SCM).

1. Immettere il percorso dello script e l'URL del fork di GitHub ("doc/resources/jenkins/JenkinsFile") da usare nell'[esempio JenkinsFile](https://github.com/VSChina/odd-or-even-function/blob/master/doc/resources/jenkins/JenkinsFile).

   ```nodejs
    node {
    withEnv(['AZURE_SUBSCRIPTION_ID=99999999-9999-9999-9999-999999999999',
            'AZURE_TENANT_ID=99999999-9999-9999-9999-999999999999']) {
        stage('Init') {
            cleanWs()
            checkout scm
        }

        stage('Build') {
            sh 'mvn clean package'
        }

        stage('Publish') {
            def RESOURCE_GROUP = '<resource_group>' 
            def FUNC_NAME = '<function_app>'
            // login Azure
            withCredentials([usernamePassword(credentialsId: 'azuresp', passwordVariable: 'AZURE_CLIENT_SECRET', usernameVariable: 'AZURE_CLIENT_ID')]) {
            sh '''
                az login --service-principal -u $AZURE_CLIENT_ID -p $AZURE_CLIENT_SECRET -t $AZURE_TENANT_ID
                az account set -s $AZURE_SUBSCRIPTION_ID
            '''
            }
            sh 'cd $PWD/target/azure-functions/odd-or-even-function-sample && zip -r ../../../archive.zip ./* && cd -'
            sh "az functionapp deployment source config-zip -g $RESOURCE_GROUP -n $FUNC_NAME --src archive.zip"
            sh 'az logout'
            }
        }
    }
    ```

## <a name="build-and-deploy"></a>Eseguire la compilazione e la distribuzione

È ora possibile eseguire il processo Jenkins.

1. Prima di tutto, ottenere la chiave di autorizzazione tramite le istruzioni contenute nell'articolo [Trigger e associazioni HTTP di Funzioni di Azure](/azure/azure-functions/functions-bindings-http-webhook-trigger#authorization-keys).

1. Nel browser immettere l'URL dell'app. Sostituire i segnaposto con i valori appropriati e specificare un valore numerico per **&lt;input_number>** come input per la funzione Java.

    ```
    https://<function_app>.azurewebsites.net/api/HttpTrigger-Java?code=<authorization_key>&number=<input_number>
    ```
1. Verranno visualizzati risultati simili all'output di esempio seguente (in cui è stato usato un numero dispari, 365, come test):

    ```output
    The number 365 is Odd.
    ```

## <a name="clean-up-resources"></a>Pulire le risorse

Se non si intende continuare a usare questa applicazione, eliminare le risorse associate seguendo questa procedura:

```azurecli
az group delete -y --no-wait -n <resource_group>
```

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"]
> [Funzioni di Azure](/azure/azure-functions/)