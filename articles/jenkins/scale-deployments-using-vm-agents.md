---
title: Esercitazione - Ridimensionare le distribuzioni di Jenkins con macchine virtuali in esecuzione in Azure
description: Informazioni su come aggiungere altra capacità alle pipeline di Jenkins usando macchine virtuali di Azure
keywords: jenkins, azure, devops, macchina virtuale, agenti
ms.topic: tutorial
ms.date: 01/08/2021
ms.custom: devx-track-jenkins,devx-track-jenkins
ms.openlocfilehash: c498a43d5a8d57a75a5592de279b79a75b8e6360
ms.sourcegitcommit: 347bfa3b6c34579c567d1324efc63c1d6672a75b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/12/2021
ms.locfileid: "98109093"
---
# <a name="tutorial-scale-jenkins-deployments-with-vm-running-in-azure"></a>Esercitazione: Ridimensionare le distribuzioni di Jenkins con macchine virtuali in esecuzione in Azure

[!INCLUDE [jenkins-integration-with-azure.md](includes/jenkins-integration-with-azure.md)]

Questa esercitazione illustra come creare una macchina virtuale Linux in Azure e aggiungerla come nodo di lavoro in Jenkins.

In questa esercitazione si apprenderà come:

> [!div class="checklist"]
> * Creare il computer agente
> * Aggiungere l'agente a Jenkins
> * Creare un nuovo processo Freestyle Jenkins
> * Eseguire il processo in un agente di macchine virtuali di Azure

## <a name="prerequisites"></a>Prerequisites

- **Installazione di Jenkins**: se non è possibile accedere all'installazione di Jenkins, [configurare Jenkins con l'interfaccia della riga di comando di Azure](configure-on-linux-vm.md).

## <a name="configure-agent-virtual-machine"></a>Configurare la macchina virtuale agente

1. Usare il comando [az group create](/cli/azure/group?#az_group_create) per creare un gruppo di risorse di Azure.

    ```azurecli
    az group create --name <resource_group> --location <location>
    ```

1. Usare il comando [az vm create](/cli/azure/vm#az_vm_create) per creare una macchina virtuale.

    ```azurecli
    az vm create --resource-group <resource-group> --name <vm_name> --image UbuntuLTS --admin-username azureuser --admin-password "<password>"
    ```

    **Note**:

    - È anche possibile caricare la chiave SSH con il comando `--ssh-key-value <ssh_path>`.

1. Installare JDK.  

    #### <a name="linux"></a>[Linux](#tab/linux)
    
    1. Accedere alla macchina virtuale usando uno strumento SSH.
    
        ```bash
        ssh username@123.123.123.123
        ```
        
    1. Installare JDK con apt. Si può eseguire l'installazione anche con un altro strumento di gestione pacchetti, come yum o pacman.
    
        ```bash
        sudo apt-get install -y default-jdk
        ```
    
    1. Al termine dell'installazione, eseguire `java -version` per verificare l'ambiente Java. L'output includerà i numeri di versione associati alle varie parti di JDK.
    
    #### <a name="windows"></a>[Windows](#tab/windows)
    
    1. Accedere alla macchina virtuale tramite uno strumento SSH o Connessione Desktop remoto.
    
    1. [Scaricare la versione di JDK](https://www.oracle.com/java/technologies/javase-downloads.html) appropriata per l'ambiente.
    
    1. Installare JDK
    
## <a name="configure-jenkins-url"></a>Configurare l'URL di Jenkins

Se si usa JNLP, è necessario configurare l'URL di Jenkins.

1. Nel menu selezionare **Manage Jenkins** (Gestisci Jenkins).

1. In **System Configuration** (Configurazione del sistema) selezionare **Configure System** (Configura sistema).

1. Verificare che l'opzione **Jenkins URL** (URL di Jenkins) sia impostata sull'indirizzo HTTP dell'installazione di Jenkins, ossia `http://<your_host>.<your_domain>:8080/`.

1. Selezionare **Salva**.

## <a name="add-agent-to-jenkins"></a>Aggiungere l'agente a Jenkins

1. Nel menu selezionare **Manage Jenkins** (Gestisci Jenkins).

1. In **System Configuration** (Configurazione del sistema) selezionare **Manage Nodes and Clouds** (Gestisci nodi e cloud).

1. Scegliere **New Node** (Nuovo nodo) dal menu.

1. Immettere un valore per **Node Name** (Nome nodo).

1. Selezionare **Permanent Agent** (Agente permanente).

1. Selezionare **OK**.

1. Specificare i valori per i campi seguenti:

    - **Nome**: specificare un nome univoco che identifichi un agente all'interno della nuova installazione di Jenkins. Questo valore può essere diverso dal nome host dell'agente. È comunque più pratico usare lo stesso valore per entrambi. Per il valore del nome è consentito qualsiasi carattere speciale dell'elenco seguente: `?*/\%!@#$^&|<>[]:;`.

    - **Remote root directory** (Directory radice remota): un agente deve avere una directory dedicata a Jenkins. Specificare il percorso di questa directory nell'agente. È preferibile usare un percorso assoluto, come `/home/azureuser/work` o `c:\jenkins`. Deve essere un percorso locale nel computer agente. Non è necessario che questo percorso sia visibile dal master. Se si usa un percorso relativo, come ./jenkins-agent, il percorso sarà relativo alla directory di lavoro specificata dal metodo di avvio.

    - **Labels** (Etichette): le etichette vengono usate per raggruppare in modo semantico gli agenti correlati in un unico gruppo logico. Ad esempio, è possibile definire un'etichetta `UBUNTU` per tutti gli agenti che eseguono la distribuzione Ubuntu di Linux.

    - **Launch method** (Metodo di avvio): sono disponibili due opzioni per avviare il nodo Jenkins remoto: **Launch agents via SSH** (Avvia agenti tramite SSH) e **Launch agent via execution of command on the master** (Avvia agente tramite l'esecuzione di un comando nel master):

        - **Launch agents via SSH** (Avvia agenti tramite SSH): specificare i valori per i campi seguenti:

            - **Host**: indirizzo IP pubblico o nome di dominio della VM. Ad esempio, `123.123.123.123` o `example.com`

            - **Credenziali**: selezionare le credenziali da usare per accedere all'host remoto. Si può anche selezionare il pulsante **Add** (Aggiungi) per definire nuove credenziali e quindi selezionarle una volta create.

            - **Host Key Verification Strategy** (Strategia di verifica chiave host): consente di definire il modo in cui Jenkins deve verificare la chiave SSH presentata dall'host remoto durante la connessione.

            ![Esempio di configurazione di un nodo in cui è specificato il metodo di avvio Launch agents via SSH (Avvia agenti tramite SSH).](./media/scale-deployments-using-vm-agents/ssh2.png)

        - **Launch agent via execution of command on the master** (Avvia agente tramite l'esecuzione di un comando nel master):

            - Scaricare `agent.jar` da `https://<your_jenkins_host_name>/jnlpJars/agent.jar`. Ad esempio: `https://localhost:8443/jnlpJars/agent.jar`.

            - Caricare `agent.jar` nella macchina virtuale

            - Avviare Jenkins con il comando `ssh <node_host> java -jar <remote_agentjar_path>`. Ad esempio: `ssh azureuser@99.99.999.9 java -jar /home/azureuser/agent.jar`.

            ![Esempio di configurazione di un nodo in cui è specificato il metodo di avvio Launch agent via execution of command on the master (Avvia agente tramite l'esecuzione di un comando nel master).](./media/scale-deployments-using-vm-agents/config.png)

1. Selezionare **Salva**.

Una volta definite le configurazioni, Jenkins aggiunge la macchina virtuale come nuovo nodo di lavoro.

![Esempio di macchina virtuale come nuovo nodo di lavoro](./media/scale-deployments-using-vm-agents/commandstart.png)

## <a name="create-a-job-in-jenkins"></a>Creare un processo in Jenkins

1. Scegliere **New Item** (Nuovo elemento) dal menu.

1. Immettere `demoproject1` per il nome.

1. Selezionare **Freestyle project** (Progetto Freestyle).

1. Selezionare **OK**.

1. Nella scheda **Generale** selezionare **Restrict where project can be run** (Limitare l'area di esecuzione del progetto), quindi digitare `ubuntu` in **Espressione etichetta**. Verrà visualizzato un messaggio di conferma che l'etichetta viene servita dalla configurazione cloud creata nel passaggio precedente.

   ![Configurazione di un nuovo processo Jenkins](./media/scale-deployments-using-vm-agents/job-config.png)

1. Selezionare la scheda **Gestione del codice sorgente**, abilitare **Git** e immettere l'URL seguente nel campo **URL del repository**: `https://github.com/spring-projects/spring-petclinic.git`

1. Nella scheda **Genera** selezionare **Aggiungi istruzione di compilazione**, quindi scegliere **Invoke top-level Maven targets** (Richiama destinazioni Maven di primo livello). Immettere `package` nel campo **Obiettivi**.

1. Selezionare **Salva**.

## <a name="build-the-new-job-on-an-azure-vm-agent"></a>Generare un nuovo processo in un agente di macchine virtuali di Azure

1. Selezionare il processo creato nel passaggio precedente.

1. Selezionare **Build now** (Compila). Viene accodata una nuova compilazione, che non viene però avviata finché non viene creata una VM agente nella sottoscrizione di Azure.

1. Una volta completata la compilazione, passare a **Console output** (Output console). Si vedrà che la compilazione è stata eseguita in remoto in un agente di Azure.

    ![Output console](./media/scale-deployments-using-vm-agents/console-output.png)

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"]
> [Integrazione continua e distribuzione continua per i Siti Web di Microsoft Azure](deploy-from-github-to-azure-app-service.md)