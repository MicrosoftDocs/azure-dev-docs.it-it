---
title: Avvio rapido - Configurare Jenkins con l'interfaccia della riga di comando di Azure
description: Informazioni su come installare Jenkins in una macchina virtuale Linux di Azure e compilare un'applicazione Java di esempio.
keywords: jenkins, azure, devops, portale, linux, macchina virtuale
ms.topic: quickstart
ms.date: 08/21/2020
ms.custom: devx-track-jenkins, devx-track-azurecli
ms.openlocfilehash: 6fc5eafbec8917b517b38d7a02c3149512675ac9
ms.sourcegitcommit: 1ddcb0f24d2ae3d1f813ec0f4369865a1c6ef322
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/27/2020
ms.locfileid: "92689137"
---
# <a name="quickstart-configure-jenkins-using-azure-cli"></a>Avvio rapido: Configurare Jenkins con l'interfaccia della riga di comando di Azure

Questa guida introduttiva illustra come installare [Jenkins](https://jenkins.io) in una VM Ubuntu Linux con gli strumenti e i plug-in configurati per usare Azure.

In questo avvio rapido verranno completate le attività seguenti:

> [!div class="checklist"]
> * Creare un file di configurazione che scarica e installa Jenkins
> * Creare un gruppo di risorse
> * Crea una macchina virtuale con il file di configurazione
> * Aprire la porta 8080 per accedere a Jenkins nella macchina virtuale
> * Connettersi alla macchina virtuale tramite SSH
> * Configurare un processo Jenkins di esempio basato su una semplice app Java in GitHub
> * Compilare il processo Jenkins di esempio

## <a name="prerequisites"></a>Prerequisiti

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../includes/open-source-devops-prereqs-azure-subscription.md)]

## <a name="troubleshooting"></a>Risoluzione dei problemi

Se si verificano problemi durante la configurazione di Jenkins, vedere la [pagina di installazione di Cloudbees Jenkins](https://www.jenkins.io/doc/book/installing/) per le istruzioni più recenti e i problemi noti.

## <a name="create-a-virtual-machine"></a>Creare una macchina virtuale

1. Accedere al [portale di Azure](https://portal.azure.com).

1. Aprire [Azure Cloud Shell](/azure/cloud-shell/overview) e, se non è già stato fatto, passare a **Bash** .

1. Creare un file denominato `cloud-init-jenkins.txt`.

    ```bash
    code cloud-init-jenkins.txt
    ```

1. Incollare il codice seguente nel nuovo file:

    ```json
    #cloud-config
    package_upgrade: true
    runcmd:
      - apt install openjdk-8-jdk -y
      - wget -qO - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
      - sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
      - apt-get update && apt-get install jenkins -y
      - service jenkins restart
    ```

1. Salvare il file ( **&lt;CTRL+S** ) e uscire dall'editor ( **&lt;CTRL+Q** ).

1. Creare un gruppo di risorse usando [az group create](/cli/azure/group#az-group-create). Potrebbe essere necessario sostituire il parametro `--location` con il valore appropriato per l'ambiente.

    ```azurecli
    az group create \
    --name QuickstartJenkins-rg \
    --location eastus
    ```

1. Creare una macchina virtuale con [az vm create](/cli/azure/vm#az-vm-create).

    ```azurecli
    az vm create \
    --resource-group QuickstartJenkins-rg \
    --name QuickstartJenkins-vm \
    --image UbuntuLTS \
    --admin-username "azureuser" \
    --generate-ssh-keys \
    --custom-data cloud-init-jenkins.txt
    ```

1. Verificare la creazione e lo stato della nuova macchina virtuale usando [az vm list](/cli/azure/vm#az-vm-list).

    ```azurecli
    az vm list -d -o table --query "[?name=='QuickstartJenkins-vm']"
    ```

1. Per impostazione predefinita, Jenkins viene eseguito sulla porta 8080. Aprire quindi la porta 8080 nella nuova macchina virtuale usando il comando [az vm open](/cli/azure/vm#az-vm-open-port).

    ```azurecli
    az vm open-port \
    --resource-group QuickstartJenkins-rg \
    --name QuickstartJenkins-vm  \
    --port 8080 --priority 1010
    ```

## <a name="configure-jenkins"></a>Configurare Jenkins

1. Ottenere l'indirizzo IP pubblico per la macchina virtuale di esempio usando [az vm show](/cli/azure/vm#az-vm-show).

    ```azurecli
    az vm show \
    --resource-group QuickstartJenkins-rg \
    --name QuickstartJenkins-vm -d \
    --query [publicIps] \
    --output tsv
    ```

    **Note** :

    - Il parametro `--query` limita l'output agli indirizzi IP pubblici per la macchina virtuale.

1. Usando l'indirizzo IP recuperato nel passaggio precedente, stabilire una sessione SSH con la macchina virtuale. Sarà necessario confermare la richiesta di connessione.

    ```azurecli
    ssh azureuser@<ip_address>
    ```

    **Note** :

    - quando la connessione viene stabilita, il prompt di Cloud Shell include il nome utente e il nome macchina virtuale: `azureuser@QuickstartJenkins-vm`.

1. Verificare che Jenkins sia in esecuzione recuperando lo stato del servizio Jenkins.

    ```bash
    service jenkins status
    ```

1. Ottenere la password generata automaticamente di Jenkins.

    ```bash
    sudo cat /var/lib/jenkins/secrets/initialAdminPassword
    ```

1. Usando l'indirizzo IP, aprire l'URL seguente in un browser: `http://<ip_address>:8080`

1. Immettere la password recuperata in precedenza e selezionare **Continue** (Continua).

    ![Pagina iniziale per sbloccare Jenkins](./media/configure-on-linux-vm/unlock-jenkins.png)

1. Selezionare **Select plugins to install** (Selezionare i plug-in da installare).

    ![Selezionare l'opzione per installare i plug-in selezionati](./media/configure-on-linux-vm/select-plugins.png)

1. Nella casella del filtro nella parte superiore della pagina immettere `github`. Selezionare il plug-in di GitHub e quindi selezionare **Install** (Installa).

    ![Installare i plug-in di GitHub](./media/configure-on-linux-vm/install-github-plugin.png)

1. Immettere le informazioni per il primo utente amministratore e quindi selezionare **Save and Continue** (Salva e continua).

    ![Immettere le informazioni per il primo utente amministratore](./media/configure-on-linux-vm/create-first-user.png)

1. Nella pagina **Instance Configuration** (Configurazione istanza) selezionare **Save and Finish** (Salva e fine).

    ![Pagina di conferma per la configurazione dell'istanza](./media/configure-on-linux-vm/instance-configuration.png)

1. Selezionare **Start using Jenkins** (Inizia a usare Jenkins).

    ![Jenkins è pronto per l'uso.](./media/configure-on-linux-vm/start-using-jenkins.png)

## <a name="create-your-first-job"></a>Creare il primo processo

1. Nella home page di Jenkins selezionare **Create a job** (Crea un processo).

    ![Home page della console di Jenkins](./media/configure-on-linux-vm/jenkins-home-page.png)

1. Immettere il nome `mySampleApp` per il processo, selezionare **Freestyle project** (Progetto Freestyle) e quindi selezionare **OK** .

    ![Creazione di un nuovo processo](./media/configure-on-linux-vm/new-job.png)

1. Selezionare la scheda **Source Code Management** (Gestione del codice sorgente). Abilitare **Git** e immettere l'URL seguente per il valore **Repository URL** (URL repository): `https://github.com/spring-guides/gs-spring-boot.git`

    ![Definire il repository Git](./media/configure-on-linux-vm/source-code-management.png)

1. Selezionare la scheda **Build** (Compilazione) e quindi **Add build step** (Aggiungi passaggio di compilazione)

    ![Aggiungere un nuovo passaggio di compilazione](./media/configure-on-linux-vm/add-build-step.png)

1. Dall'elenco a discesa selezionare **Invoke Gradle script** (Richiama script Gradle).

    ![Selezionare l'opzione relativa allo script Gradle](./media/configure-on-linux-vm/invoke-gradle-script-option.png)

1. Selezionare **Use Gradle Wrapper** (Usa wrapper di Gradle) e quindi immettere `complete` in **Wrapper location** (Percorso wrapper) e `build` in **Tasks** (Attività).

    ![Opzioni dello script Gradle](./media/configure-on-linux-vm/gradle-script-options.png)

1. Selezionare **Advanced** (Avanzate) e immettere `complete` nel campo **Root Build script** (Script di compilazione radice).

    ![Opzioni avanzate dello script Gradle](./media/configure-on-linux-vm/root-build-script.png)

1. Scorrere fino alla parte inferiore della pagina e selezionare **Save** (Salva).

## <a name="build-the-sample-java-app"></a>Compilare l'app Java di esempio

1. Quando viene visualizzata la home page per il progetto, selezionare **Build Now** (Compila adesso) per compilare il codice e creare il pacchetto dell'app di esempio.

    ![Home page progetto](./media/configure-on-linux-vm/project-home-page.png)

1. Un grafico sotto l'intestazione **Build History** (Cronologia compilazione) indica che il processo è in fase di compilazione.

    ![Compilazione del processo](./media/configure-on-linux-vm/job-currently-building.png)

1. Al termine della compilazione selezionare il collegamento **Workspace** (Area di lavoro).

    ![Selezionare il collegamento dell'area di lavoro](./media/configure-on-linux-vm/job-workspace.png)

1. Passare a `complete/build/libs` per verificare che il file `.jar` sia stato compilato correttamente.

    ![La libreria di destinazione verifica che la compilazione sia stata completata](./media/configure-on-linux-vm/successful-build.png)

1. Il server Jenkins è ora pronto per la compilazione dei progetti dell'utente in Azure.

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"]
> [Jenkins in Azure](./index.yml)
