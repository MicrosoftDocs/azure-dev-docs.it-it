---
title: 'Esercitazione: Eseguire la distribuzione da GitHub nel servizio Azure Kubernetes con Jenkins'
description: Informazioni su come configurare Jenkins per l'integrazione continua (CI) da GitHub e la distribuzione continua (CD) nel servizio Azure Kubernetes
keywords: jenkins, azure, devops, aks, servizio azure kubernetes, github
ms.topic: article
ms.date: 02/05/2021
ms.custom: devx-track-jenkins, devx-track-azurecli
ms.openlocfilehash: d6a45e227436970fa34505ccfbf28dc6e8c2a5d8
ms.sourcegitcommit: 5f4a041aede2a0b7e2cf479185f62885209eaf9f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/09/2021
ms.locfileid: "102517175"
---
# <a name="tutorial-deploy-from-github-to-azure-kubernetes-service-using-jenkins"></a>Esercitazione: Eseguire la distribuzione da GitHub nel servizio Azure Kubernetes con Jenkins

[!INCLUDE [jenkins-integration-with-azure.md](includes/jenkins-integration-with-azure.md)]

Questa esercitazione distribuisce un'app di esempio da GitHub in un cluster del [servizio Azure Kubernetes (AKS)](/azure/aks/intro-kubernetes) configurando l'integrazione continua (CI) e la distribuzione continua (CD) in Jenkins.

In questa esercitazione si completeranno le attività seguenti:

> [!div class="checklist"]
> * Distribuire un'app Azure Vote di esempio in un cluster servizio Azure Kubernetes.
> * Creare un progetto di Jenkins di base.
> * Configurare le credenziali per Jenkins per l'interazione con il Registro Azure Container.
> * Creare un processo di compilazione Jenkins e un webhook GitHub per compilazioni automatizzate.
> * Testare la pipeline CI/CD per aggiornare un'applicazione in servizio Azure Kubernetes in base ai commit del codice di GitHub.

## <a name="prerequisites"></a>Prerequisiti

Per completare questa esercitazione, è necessario quanto segue:

- Conoscenze di base di Kubernetes, Git, CI/CD e delle immagini di contenitore

- Un [cluster del servizio Azure Kubernetes](/azure/aks/kubernetes-walkthrough) e `kubectl` configurati con le [credenziali del cluster del servizio Azure Kubernetes](/cli/azure/aks#az-aks-get-credentials).

- Un [registro del servizio Registro Azure Container](/azure/container-registry/container-registry-get-started-azure-cli), il nome del server di accesso al servizio Registro Azure Container e il cluster del servizio Azure Kubernetes configurati per [eseguire l'autenticazione con il registro del servizio Registro Azure Container](/azure/aks/cluster-container-registry-integration).

- Un [controller Jenkins](https://docs.microsoft.com/azure/developer/jenkins/configure-on-linux-vm) distribuito in una macchina virtuale di Azure.

- Interfaccia della riga di comando di Azure versione 2.0.46 o successiva installata e configurata. Eseguire `az --version` per trovare la versione. Se è necessario eseguire l'installazione o l'aggiornamento, vedere [Installare l'interfaccia della riga di comando di Azure](/cli/azure/install-azure-cli).

- [Docker installato](https://docs.docker.com/install/) nel sistema di distribuzione in uso

- Un account GitHub, un [token di accesso personale a GitHub](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/) e un client Git installato nel sistema di distribuzione

- Se si fornisce la propria istanza di Jenkins invece di questo esempio di script per distribuire Jenkins, l'istanza di Jenkins richiede [l'installazione e la configurazione di Docker](https://docs.docker.com/install/) e [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/).

## <a name="prepare-your-app"></a>Preparare l'app

In questo articolo si usa un'applicazione Azure vote di esempio che contiene un'interfaccia Web e Redis per l'archiviazione di dati temporanei.

Prima di integrare Jenkins e il servizio Azure Kubernetes per le distribuzioni automatizzate, preparare e distribuire l'applicazione di voto di Azure nel cluster servizio Azure Kubernetes. Questa distribuzione manuale consente di visualizzare l'applicazione in azione.

> [!NOTE]
> L'applicazione di voto di Azure di esempio usa un pod di Linux pianificato per l'esecuzione in un nodo di Linux. Il flusso descritto in questo articolo funziona anche per un pod di Windows Server pianificato in un nodo di Windows Server.

Creare il fork del repository GitHub seguente per l'applicazione esempio - [https://github.com/Azure-Samples/azure-voting-app-redis](https://github.com/Azure-Samples/azure-voting-app-redis). Per creare il fork del repository nel proprio account GitHub, selezionare il pulsante **Fork** nell'angolo superiore destro.

Clonare il fork nel proprio sistema di sviluppo. Verificare di usare l'URL del fork durante la clonazione del repository:

```console
git clone https://github.com/<your-github-account>/azure-voting-app-redis.git
```

Passare alla directory del fork clonato:

```console
cd azure-voting-app-redis
```

Per creare le immagini di contenitore necessarie per l'applicazione di esempio, usare il file *docker-compose.yaml* con `docker-compose`:

```console
docker-compose up -d
```

Viene eseguito il pull delle immagini di base necessarie e vengono compilati i contenitori dell'applicazione. È quindi possibile usare il comando [docker images](https://docs.docker.com/engine/reference/commandline/images/) per visualizzare l'immagine creata. Sono state scaricate o create tre immagini. L'immagine `azure-vote-front` contiene l'applicazione e usa l'immagine `nginx-flask` come base. L'immagine `redis` viene usata per avviare un'istanza di Redis:

```
$ docker images

REPOSITORY                   TAG        IMAGE ID            CREATED             SIZE
azure-vote-front             latest     9cc914e25834        40 seconds ago      694MB
redis                        latest     a1b99da73d05        7 days ago          106MB
tiangolo/uwsgi-nginx-flask   flask      788ca94b2313        9 months ago        694MB
```

Accedere all'istanza di contenitore di Azure.

```azurecli
az login -n <acrLoginServer>
```

Sostituire `<acrLoginServer>` con il server di accesso di ACR.

Usare il comando [docker tag](https://docs.docker.com/engine/reference/commandline/tag/) per assegnare tag all'immagine con il nome del server di accesso di Registro Azure Container e il numero di versione `v1`. Usare il proprio `<acrLoginServer>` nome ottenuto nel passaggio precedente:

```console
docker tag azure-vote-front <acrLoginServer>/azure-vote-front:v1
```

Eseguire infine il push dell'immagine *azure-vote-front* in Registro Azure Container. Anche in questo caso, sostituire `<acrLoginServer>` con il nome del server di accesso di Registro Azure Container, ad esempio `myacrregistry.azurecr.io`:

```console
docker push <acrLoginServer>/azure-vote-front:v1
```

## <a name="deploy-the-sample-application-to-aks"></a>Distribuire l'applicazione di esempio nel servizio Azure Kubernetes

Per distribuire l'applicazione di esempio nel cluster servizio Azure Kubernetes, è possibile usare il file manifesto Kubernetes nella radice del repository di voto di Azure. Aprire il `azure-vote-all-in-one-redis.yaml` file manifesto con un editor, ad esempio `vi` . Sostituire `microsoft` con il nome del server di accesso di Registro Azure Container. Questo valore è disponibile nella riga **60** del file manifesto:

```yaml
containers:
- name: azure-vote-front
  image: azuredocs/azure-vote-front
```

Successivamente, usare il comando [kubectl apply](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply) per distribuire l'applicazione nel cluster del servizio Azure Kubernetes:

```console
kubectl apply -f azure-vote-all-in-one-redis.yaml
```

Viene creato un servizio di bilanciamento del carico di Kubernetes per esporre l'applicazione a Internet. Il processo potrebbe richiedere alcuni minuti. Per monitorare l'avanzamento della distribuzione del servizio di bilanciamento del carico, usare il comando [kubectl get service](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get) con l'argomento `--watch`. Dopo il passaggio di *EXTERNAL-IP* da *pending* a un *indirizzo IP*, usare `Control + C` per arrestare il processo kubectl watch.

```console
$ kubectl get service azure-vote-front --watch

NAME               TYPE           CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE
azure-vote-front   LoadBalancer   10.0.215.27   <pending>     80:30747/TCP   22s
azure-vote-front   LoadBalancer   10.0.215.27   40.117.57.239   80:30747/TCP   2m
```

Per vedere l'applicazione in azione, aprire un Web browser all'indirizzo IP esterno del servizio. Viene visualizzata l'applicazione di voto di Azure, come illustrato nell'esempio seguente:

![Applicazione di voto di Azure di esempio in esecuzione nel servizio Azure Kubernetes](media/deploy-from-github-to-aks/azure-vote.png)

## <a name="configure-jenkins-controller"></a>Configurare il controller Jenkins

Applicare le seguenti modifiche per abilitare le distribuzioni AKS dal controller Jenkins:

Aprire la porta in `80` ingresso.

```azurecli
az vm open-port \
--resource-group <Resource_Group_name> \
--name <Jenkins_Controller_VM>  \
--port 80 --priority 1020
```

Sostituire `<Resource_Group_name>` e `<Jenkins_Controller_VM>` con i valori appropriati.

SSH nel controller Jenkins

```azurecli
ssh azureuser@<PublicIPAddres>
```

Sostituire `<PublicIPAddress>` con l'indirizzo IP del controller Jenkins.

### <a name="install--log-into-azcli"></a>Installare & accedere a AzCLI

```azurecli
curl -L https://aka.ms/InstallAzureCli | bash
``````

```azurecli
az login
```

   > [!NOTE]
   > Per installare manualmente AzCLI, seguire queste [istruzioni](https://docs.microsoft.com/cli/azure/install-azure-cli).

### <a name="install-docker"></a>Installazione di Docker

```bash
sudo apt-get install apt-transport-https ca-certificates curl software-properties-common -y;
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -;
sudo apt-key fingerprint 0EBFCD88;
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable";
sudo apt-get update;
sudo apt-get install docker-ce -y;
```

### <a name="install-kubectl-and-connect-to-aks"></a>Installare Kubectl e connettersi a AKS

```azurecli
sudo az aks install-cli
sudo az aks get-credentials --resource-group <Resource_Group> --name <AKS_Name>
```

Sostituire `<Resource_Group>` e `<AKS_Name>` con i valori appropriati.

### <a name="configure-access"></a>Configurare l'accesso

```bash
sudo usermod -aG docker jenkins;
sudo usermod -aG docker azureuser;
sudo touch /var/lib/jenkins/jenkins.install.InstallUtil.lastExecVersion;
sudo service jenkins restart;
sudo cp ~/.kube/config /var/lib/jenkins/.kube/
sudo chmod 777 /var/lib/jenkins/
sudo chmod 777 /var/lib/jenkins/config
```

## <a name="create-a-jenkins-environment-variable"></a>Creare una variabile di ambiente di Jenkins

Per contenere il nome del server di accesso di Registro Azure Container, viene usata una variabile di ambiente di Jenkins. A tale variabile viene fatto riferimento durante il processo di compilazione di Jenkins. Per creare questa variabile di ambiente, completare i passaggi seguenti:

- Nella parte sinistra del portale di Jenkins selezionare **Manage Jenkins** > **Configure System** (Gestisci Jenkins > Configura sistema)
- In **Global Properties** (Proprietà globali) selezionare **Environment variables** (Variabili di ambiente). Aggiungere una variabile con il nome `ACR_LOGINSERVER` e il valore del server di accesso di Registro Azure Container in uso.

    ![Variabili di ambiente di Jenkins](media/deploy-from-github-to-aks/env-variables.png)

- Al termine, selezionare **Salva** nella parte inferiore della pagina.

## <a name="create-a-jenkins-credential-for-acr"></a>Creare una credenziale di Jenkins per il Registro Azure Container

Durante il processo CI/CD, Jenkins compila nuovi immagini del contenitore in base agli aggiornamenti delle applicazioni e deve quindi eseguire il *push* di tali immagini in Registro Azure Container.

Per consentire a Jenkins di inserire le immagini del contenitore aggiornate in ACR, è necessario specificare le credenziali per ACR. 

Per la separazione dei ruoli e delle autorizzazioni, configurare un'entità servizio per Jenkins con le autorizzazioni di *collaboratore* per il registro ACR.

### <a name="create-a-service-principal-for-jenkins-to-use-acr"></a>Creare un'entità servizio per Jenkins per usare il Registro Azure Container

Creare in primo luogo un'entità servizio usando il comando [az ad sp create-for-rbac](/cli/azure/ad/sp#az-ad-sp-create-for-rbac):

```azurecli
az ad sp create-for-rbac --skip-assignment
```

```bash
{
  "appId": "626dd8ea-042d-4043-a8df-4ef56273670f",
  "displayName": "azure-cli-2018-09-28-22-19-34",
  "name": "http://azure-cli-2018-09-28-22-19-34",
  "password": "1ceb4df3-c567-4fb6-955e-f95ac9460297",
  "tenant": "72f988bf-86f1-41af-91ab-2d7cd011db48"
}
```

Prendere nota di *appId* e *password*. Tali valori vengono usati nella procedura seguente per configurare la risorsa credenziale in Jenkins.

Ottenere l'ID risorsa di Registro Azure Container usando il comando [az acr show](/cli/azure/acr#az-acr-show) e archiviarlo come variabile.

```azurecli
ACR_ID=$(az acr show --resource-group <Resource_Group> --name <acrLoginServer> --query "id" --output tsv)
```

Sostituire `<Resource_Group>` e `<acrLoginServer>` con i valori appropriati.

Creare un'assegnazione di ruolo per assegnare i diritti di *collaboratore* dell'entità servizio al registro ACR.

```azurecli
az role assignment create --assignee <appID> --role Contributor --scope $ACR_ID
```

Sostituire `<appId>` con il valore specificato nell'output del comando precedente usare per creare l'entità servizio.

### <a name="create-a-credential-resource-in-jenkins-for-the-acr-service-principal"></a>Creare una risorsa credenziale in Jenkins per l'entità servizio del servizio Registro Azure Container

Con l'assegnazione di ruolo creata in Azure, archiviare le credenziali di Registro Azure Container in un oggetto credenziale di Jenkins. Durante il processo di compilazione di Jenkins si farà riferimento a queste credenziali.

Tornare al lato sinistro del portale di Jenkins, selezionare **Manage Jenkins**  >  **Manage Credentials**  >  **Jenkins Store**  >  **Global Credentials (Unrestricted)**  >  **Add Credentials**

Verificare che il tipo di credenziali sia **Username with password** (Nome utente con password) e immettere gli elementi seguenti:

- **Nome utente** - *appId* dell'entità servizio creata per l'autenticazione con il Registro Azure Container.
- **Password** - *password* dell'entità utente creata per l'autenticazione con il Registro Azure Container.
- **ID** - identificatore di credenziali, ad esempio *acr-credentials*

Al termine, il modulo delle credenziali è analogo all'esempio seguente:

![Creare un oggetto credenziale di Jenkins con le informazioni dell'entità servizio](media/deploy-from-github-to-aks/acr-credentials.png)

Selezionare **OK** e tornare al portale di Jenkins.

## <a name="create-a-jenkins-project"></a>Creare un progetto di Jenkins

Nella home page del portale di Jenkins selezionare **New item** (Nuovo elemento) nella parte sinistra:

1. Immettere *azure-vote* come nome del processo. Scegliere **Freestyle project** (Progetto Freestyle) e quindi selezionare **OK**
1. Nella sezione **General** (Generale) selezionare **GitHub project** (Progetto GitHub) e immettere l'URL del repository copiato tramite fork, ad esempio *https:\//github.com/\<your-github-account\>/azure-voting-app-redis*
1. Nella sezione **gestione del codice sorgente** selezionare **git**, immettere l'URL del repository con fork `.git` , ad esempio *https: \/ /github.com/ \<your-github-account\> /Azure-voting-app-Redis.git*

1. Nella sezione **Build Triggers** (Trigger di compilazione) selezionare **GitHub hook trigger for GITScm polling** (Trigger di hook GitHub per polling GITScm).
1. In **Build Environment** (Ambiente di compilazione) selezionare **Use secret texts or files** (Usa testi o file segreti).
1. In **Bindings** (Associazioni) selezionare **Add** (Aggiungi) > **Username and password (separated)** (Nome utente e password separati).
   - Immettere `ACR_ID` per **Username Variable** (Variabile nome utente) e `ACR_PASSWORD` per **Password Variable** (Variabile password).

     ![Associazioni di Jenkins](media/deploy-from-github-to-aks/bindings.png)

1. Aggiungere un'**istruzione di compilazione** di tipo **Execute shell** (Esegui shell) e usare il testo seguente. Questo script compila una nuova immagine del contenitore e la inserisce nel Registro Azure Container.

    ```bash
    # Build new image and push to ACR.
    WEB_IMAGE_NAME="${ACR_LOGINSERVER}/azure-vote-front:kube${BUILD_NUMBER}"
    docker build -t $WEB_IMAGE_NAME ./azure-vote
    docker login ${ACR_LOGINSERVER} -u ${ACR_ID} -p ${ACR_PASSWORD}
    docker push $WEB_IMAGE_NAME
    ```

1. Aggiungere un'altra **istruzione di compilazione** di tipo **Execute shell** (Esegui shell) e usare il testo seguente. Questo script aggiorna la distribuzione dell'applicazione nel servizio Azure Kubernetes con la nuova immagine del contenitore da Registro Azure Container.

    ```bash
    # Update kubernetes deployment with new image.
    WEB_IMAGE_NAME="${ACR_LOGINSERVER}/azure-vote-front:kube${BUILD_NUMBER}"
    kubectl set image deployment/azure-vote-front azure-vote-front=$WEB_IMAGE_NAME
    ```

1. Al termine, fare clic su **Save** (Salva).

## <a name="test-the-jenkins-build"></a>Testare la build di Jenkins

Prima di automatizzare il processo in base ai commit di GitHub, testare manualmente la compilazione di Jenkins.

Questa compilazione convalida che il processo è stato configurato correttamente. Conferma che è presente il file di autenticazione di Kubernetes appropriato e che l'autenticazione per il registro di sistema ACR funziona.

Nel menu a sinistra del progetto selezionare **Build Now** (Compila).

![Build di test di Jenkins](media/deploy-from-github-to-aks/test-build.png)

La prima build è più lunga perché i livelli di immagine Docker vengono estratti nel server Jenkins.

Le compilazioni eseguono le attività seguenti:

1. Clona il repository GitHub
1. Compila una nuova immagine del contenitore
1. Inserisce l'immagine del contenitore nel registro ACR
1. Aggiorna l'immagine usata dalla distribuzione di AKS

Poiché non sono state apportate modifiche al codice dell'applicazione, l'interfaccia utente Web è invariata.

Al termine del processo di compilazione, selezionare **compila #1** in cronologia di compilazione. Selezionare **Console output** (Output console) e visualizzare l'output del processo di compilazione. La riga finale deve indicare che la build ha avuto esito positivo.

## <a name="create-a-github-webhook"></a>Creare un webhook GitHub

Dopo aver completato la compilazione manuale, integrare GitHub nella compilazione Jenkins. Usare un webhook per eseguire il processo di compilazione Jenkins ogni volta che viene eseguito il commit del codice in GitHub.

Per creare il webhook GitHub, completare questi passaggi:

1. Passare al repository GitHub con fork in un Web browser.
1. Selezionare **Impostazioni** e quindi **Webhook** sul lato sinistro.
1. Scegli **Aggiungi webhook**. Come valore per *Payload URL* (URL di payload) immettere `http://<publicIp:8080>/github-webhook/` dove `<publicIp>` è l'indirizzo IP del server di Jenkins. Assicurarsi di includere l'oggetto finale `/` . Lasciare le altre impostazioni predefinite per il tipo di contenuto e attivare gli eventi *push*.
1. Selezionare **Aggiungi webhook**.

    ![Creare un webhook GitHub per Jenkins](media/deploy-from-github-to-aks/webhook.png)

## <a name="test-the-complete-cicd-pipeline"></a>Testare la pipeline CI/CD completata

È ora possibile testare l'intera pipeline di integrazione CI/CD. Quando si esegue il push di un commit di codice da GitHub, vengono eseguite queste operazioni:

1. Il webhook GitHub informa Jenkins.
1. Jenkins avvia il processo di compilazione ed esegue il pull del commit di codice più recente da GitHub.
1. Viene avviata una compilazione di Docker usando il codice aggiornato e la nuova immagine di contenitore è contrassegnata con il numero di build più recente.
1. Viene eseguito il push di tale immagine in Registro Azure Container.
1. L'applicazione in esecuzione in Azure Kubernetes Service viene aggiornata con l'immagine più recente di Azure Container Registry.

Nel computer di sviluppo aprire l'applicazione clonata con un editor di codice. Nella directory */azure-vote/azure-vote* aprire il file denominato **config_file.cfg**. Aggiornare i valori di voto in questo file su un valore rilevante, come illustrato nell'esempio seguente:

```
# UI Configurations
TITLE = 'Azure Voting App'
VOTE1VALUE = 'Blue'
VOTE2VALUE = 'Purple'
SHOWHOST = 'false'
```

Al termine dell'aggiornamento, salvare il file, eseguire il commit delle modifiche ed eseguirne il push nel fork del repository GitHub. Il webhook GitHub attiva un nuovo processo di compilazione in Jenkins. Nel dashboard Web di Jenkins monitorare il processo di compilazione. Sono necessari pochi secondi per eseguire il pull del codice più recente, creare ed eseguire il push dell'immagine aggiornata e distribuire l'applicazione aggiornata nel servizio Azure Kubernetes.

Al termine della compilazione, aggiornare il Web browser dell'applicazione di voto di Azure di esempio. Le modifiche vengono visualizzate, come illustrato nell'esempio seguente:

![Voto di Azure di esempio nel servizio Azure Kubernetes aggiornato dal processo di compilazione di Jenkins](media/deploy-from-github-to-aks/azure-vote-updated.png)

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"]
> [Jenkins in Azure](/azure/jenkins)
