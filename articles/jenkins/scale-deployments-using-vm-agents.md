---
title: 'Esercitazione: Scalabilità delle distribuzioni di Jenkins con agenti di macchine virtuali di Azure'
description: Informazioni su come aggiungere altra capacità alle pipeline di Jenkins usando macchine virtuali di Azure con il plug-in Jenkins per agenti di VM di Azure.
keywords: jenkins, azure, devops, macchina virtuale, agenti
ms.topic: tutorial
ms.date: 08/19/2020
ms.custom: devx-track-jenkins
ms.openlocfilehash: 4918b548fb98f27fffaa8d836ec125cc325da79d
ms.sourcegitcommit: 4dac39849ba2e48034ecc91ef578d11aab796e58
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/20/2020
ms.locfileid: "96035469"
---
# <a name="tutorial-scale-jenkins-deployments-with-azure-vm-agents"></a>Esercitazione: Scalabilità delle distribuzioni di Jenkins con agenti di macchine virtuali di Azure

[!INCLUDE [jenkins-integration-with-azure.md](includes/jenkins-integration-with-azure.md)]

In questa esercitazione viene illustrato come usare il [plug-in di Agente di macchine virtuali di Azure](https://plugins.jenkins.io/azure-vm-agents) di Jenkins per aggiungere capacità su richiesta con macchine virtuali Linux in esecuzione in Azure.

In questa esercitazione si apprenderà come:

> [!div class="checklist"]
> * Installare il plug-in di Agente di macchine virtuali di Azure
> * Configurare il plug-in per creare risorse nella sottoscrizione di Azure
> * Impostare le risorse di calcolo disponibili per ogni agente
> * Impostare il sistema operativo e gli strumenti installati in ogni agente
> * Creare un nuovo processo Freestyle Jenkins
> * Eseguire il processo in un agente di macchine virtuali di Azure

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Continuous-Integration-with-Jenkins-Using-Azure-VM-Agents/player]

## <a name="prerequisites"></a>Prerequisites

- **Installazione di Jenkins**: se non è possibile accedere all'installazione di Jenkins, [configurare Jenkins con l'interfaccia della riga di comando di Azure](configure-on-linux-vm.md).

## <a name="install-azure-vm-agents-plugin"></a>Installare il plug-in Agenti di macchine virtuali di Azure

1. Dal dashboard di Jenkins selezionare **Manage Jenkins** (Gestione Jenkins), quindi selezionare **Manage Plugins** (Gestione plug-in).

1. Selezionare la scheda **Disponibili**, quindi cercare **Azure VM Agents** (Agenti di macchine virtuali di Azure). Selezionare la casella di controllo accanto alla voce del plug-in, quindi selezionare **Install without restart** (Installa senza riavviare) nella parte inferiore del dashboard.

## <a name="configure-the-azure-vm-agents-plugin"></a>Configurare il plug-in di Agente di macchine virtuali di Azure

1. Dal dashboard di Jenkins selezionare **Manage Jenkins** (Gestione Jenkins), quindi selezionare **Configurazione sistema**.

1. Scorrere fino alla fine della pagina per trovare la sezione **Cloud** con l'elenco a discesa **Add new cloud** (Aggiungi nuovo cloud), quindi scegliere **Microsoft Azure VM Agents** (Agenti di macchine virtuali di Azure).

1. Selezionare un'entità servizio esistente dal menu a discesa **Aggiungi** nella sezione **Azure Credentials** (Credenziali di Azure). Se non è elencata alcuna entità servizio, eseguire la procedura seguente per [creare un'entità servizio](/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager) per un account di Azure e aggiungerla alla configurazione di Jenkins:

    a. Selezionare **Aggiungi** accanto ad **Azure Credentials** (Credenziali di Azure), quindi scegliere **Jenkins**.
    b. Nella finestra di dialogo **Aggiungi credenziali**, selezionare **Microsoft Azure Service Principal** (Entità servizio di Microsoft Azure) dall'elenco a discesa **Tipologia**.
    c. Creare un'entità servizio Active Directory dall'interfaccia della riga di comando di Azure o da [Cloud Shell](/azure/cloud-shell/overview).
    
    ```azurecli-interactive
    az ad sp create-for-rbac --name jenkins_sp --password secure_password
    ```

    ```json
    {
        "appId": "BBBBBBBB-BBBB-BBBB-BBBB-BBBBBBBBBBB",
        "displayName": "jenkins_sp",
        "name": "http://jenkins_sp",
        "password": "secure_password",
        "tenant": "CCCCCCCC-CCCC-CCCC-CCCCCCCCCCC"
    }
    ```
    d. Immettere le credenziali dall'entità servizio nella finestra di dialogo **Aggiungi credenziali**. Se non si conosce l'ID sottoscrizione di Azure, è possibile eseguire una query dall'interfaccia della riga di comando:
     
     ```azurecli-interactive
     az account list
     ```

     ```json
        {
            "cloudName": "AzureCloud",
            "id": "AAAAAAAA-AAAA-AAAA-AAAA-AAAAAAAAAAAA",
            "isDefault": true,
            "name": "Visual Studio Enterprise",
            "state": "Enabled",
            "tenantId": "CCCCCCCC-CCCC-CCCC-CCCC-CCCCCCCCCCC",
            "user": {
            "name": "raisa@fabrikam.com",
            "type": "user"
            }
     ```

    L'entità servizio completata dovrebbe usare il campo `id` per **ID sottoscrizione**, il valore `appId` per **ID client**, `password` per **Segreto client** e `tenant` per **ID tenant**. Selezionare **Aggiungi** per aggiungere l'entità servizio, quindi configurare il plug-in per usare le nuove credenziali create.

    ![Configurare un'entità servizio di Azure](./media/scale-deployments-using-vm-agents/new-service-principal.png)

1. Nella sezione **Nome gruppo di risorse**, mantenere selezionato **Crea nuovo oggetto** e immettere `myJenkinsAgentGroup`.
1. Selezionare **Verifica configurazione** per connettersi ad Azure e verificare le impostazioni del profilo.
1. Selezionare **Applica** per aggiornare la configurazione del plug-in.

## <a name="configure-agent-resources"></a>Configurare le risorse di un agente

Configurare un modello da usare per definire un agente di macchine virtuali di Azure. Questo modello definisce le risorse di calcolo di cui dispone ogni agente quando viene creato.

1. Selezionare **Aggiungi** vicino a **Add Azure Virtual Machine Template** (Aggiungi un modello di macchina virtuale di Azure).
1. Immettere `defaulttemplate` per il **Nome**
1. Immettere `ubuntu` per l'**Etichetta**
1. Selezionare l'[area di Azure](https://azure.microsoft.com/regions/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) desiderata dalla casella combinata.
1. Selezionare le [dimensioni macchina virtuale](/azure/virtual-machines/linux/sizes) dall'elenco a discesa sotto **Dimensioni macchina virtuale**. Per questa esercitazione sono appropriate delle dimensioni `Standard_DS1_v2` per un uso generico.   
1. Lasciare un **Periodo di memorizzazione** di `60`. Questa impostazione definisce il numero di minuti per cui Jenkins può attendere prima di deallocare un agente inattivo. Se non si vuole eliminare automaticamente gli agenti inattivi, immettere 0.

   ![Configurazione generale di una macchina virtuale](./media/scale-deployments-using-vm-agents/general-config.png)

## <a name="configure-agent-operating-system-and-tools"></a>Configurare il sistema operativo e gli strumenti di un agente

Nella sezione **Image Configuration** (Configurazione dell'immagine) della configurazione del plug-in, selezionare **Ubuntu 16.04 LTS**. Selezionare le caselle accanto a **Install Git (Latest)** (Installare Git, versione più recente) e **installare Maven (V3.5.0)**, e **Installare Docker** per installare questi strumenti per gli agenti appena creati.

![Configurazione del sistema operativo e degli strumenti della macchina virtuale](./media/scale-deployments-using-vm-agents/jenkins-os-config.png)

Selezionare **Aggiungi** accanto a **Credenziali amministratore**, quindi selezionare **Jenkins**. Immettere un nome utente e una password usati per accedere agli agenti, assicurandosi che soddisfino i [criteri di nome utente e password](/azure/virtual-machines/linux/faq#what-are-the-username-requirements-when-creating-a-vm) per gli account amministrativi su macchine virtuali di Azure.

Selezionare **Verify Template** (Verifica modello) per verificare la configurazione, quindi selezionare **Salva** per salvare le modifiche e tornare al dashboard di Jenkins.

## <a name="create-a-job-in-jenkins"></a>Creare un processo in Jenkins

1. Nel dashboard di Jenkins fare clic su **New Item**. 
1. Immettere `demoproject1` per il nome e selezionare **Freestyle project** (Progetto Freestyle), quindi selezionare **OK**.
1. Nella scheda **Generale** selezionare **Restrict where project can be run** (Limitare l'area di esecuzione del progetto), quindi digitare `ubuntu` in **Espressione etichetta**. Verrà visualizzato un messaggio di conferma che l'etichetta viene servita dalla configurazione cloud creata nel passaggio precedente. 
   ![Configurazione del processo](./media/scale-deployments-using-vm-agents/job-config.png)
1. Selezionare la scheda **Gestione del codice sorgente**, abilitare **Git** e immettere l'URL seguente nel campo **URL del repository**: `https://github.com/spring-projects/spring-petclinic.git`
1. Nella scheda **Genera** selezionare **Aggiungi istruzione di compilazione**, quindi scegliere **Invoke top-level Maven targets** (Richiama destinazioni Maven di primo livello). Immettere `package` nel campo **Obiettivi**.
1. Selezionare **Salva** per salvare la configurazione del processo.

## <a name="build-the-new-job-on-an-azure-vm-agent"></a>Generare un nuovo processo in un agente di macchine virtuali di Azure

1. Tornare al dashboard di Jenkins.
1. Selezionare il processo creato nel passaggio precedente, quindi fare clic su **Build now** (Genera adesso). Viene accodata una nuova compilazione che tuttavia non è avviata finché non viene creato un agente di macchine virtuali nella sottoscrizione di Azure.
1. Una volta completata la compilazione, passare a **Console output** (Output console). Si vedrà che la compilazione è stata eseguita in remoto in un agente di Azure.

![Output console](./media/scale-deployments-using-vm-agents/console-output.png)

## <a name="troubleshooting-the-jenkins-plugin"></a>Risoluzione dei problemi del plug-in Jenkins

Se si rilevano bug con i plug-in Jenkins, segnalare un problema in [Jenkins JIRA](https://issues.jenkins-ci.org/) per il componente specifico.

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"]
> [Integrazione continua e distribuzione continua per i Siti Web di Microsoft Azure](deploy-from-github-to-azure-app-service.md)