---
title: 'Esercitazione: Usare Istanze di Azure Container come agente di compilazione Jenkins'
description: Informazioni su come configurare un server Jenkins per l'esecuzione di processi di compilazione in Istanze di Azure Container
keywords: jenkins, azure, devops, istanze di contenitore, agente di compilazione
ms.topic: article
ms.date: 01/08/2021
ms.custom: devx-track-jenkins,devx-track-azurecli
ms.openlocfilehash: 7633d88897d76f4ed75fa1d7d6c5b0c620db4919
ms.sourcegitcommit: 593d177cfb5f56f236ea59389e43a984da30f104
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/18/2021
ms.locfileid: "98561597"
---
# <a name="tutorial-use-azure-container-instances-as-a-jenkins-build-agent"></a>Esercitazione: Usare Istanze di Azure Container come agente di compilazione Jenkins

[!INCLUDE [jenkins-integration-with-azure.md](includes/jenkins-integration-with-azure.md)]

Istanze di Azure Container offre un ambiente isolato on demand utilizzabile in modalità burst per eseguire carichi di lavoro in contenitori. Grazie a questi attributi, le istanze di contenitore di Azure sono un'ottima piattaforma per l'esecuzione di processi di compilazione di Jenkins su vasta scala. Questo articolo illustra come distribuire un servizio Istanze di Azure Container e aggiungerlo come agente di compilazione permanente per un controller Jenkins.

Per altre informazioni su Istanze di Azure Container, vedere [Informazioni su Istanze di Azure Container](/azure/container-instances/container-instances-overview).

## <a name="prerequisites"></a>Prerequisiti

- **Sottoscrizione di Azure**: Se non si ha una sottoscrizione di Azure, [creare un account Azure gratuito](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) prima di iniziare.
- **Server Jenkins**: se non è installato un server Jenkins, [creare un server Jenkins in Azure](./configure-on-linux-vm.md).

## <a name="prepare-the-jenkins-controller"></a>Preparare il controller Jenkins

1. Passare al portale di Jenkins.

1. Nel menu selezionare **Manage Jenkins** (Gestisci Jenkins).

1. In **System Configuration** (Configurazione del sistema) selezionare **Configure System** (Configura sistema).

1. Verificare che l'opzione **Jenkins URL** (URL di Jenkins) sia impostata sull'indirizzo HTTP dell'installazione di Jenkins, ossia `http://<your_host>.<your_domain>:8080/`.

1. Nel menu selezionare **Manage Jenkins** (Gestisci Jenkins).

1. In **Security** (Sicurezza) selezionare **Configure Global Security (Configura sicurezza globale)** .

1. In **Agents** (Agenti) specificare la porta **Fixed** (Fissa) e immettere il numero di porta appropriato per l'ambiente.

    Esempio di configurazione:  ![Configurare la porta TCP](./media/azure-container-instances-as-jenkins-build-agent/agent-port.png)

1. Selezionare **Salva**.

## <a name="create-jenkins-work-agent"></a>Creare l'agente di lavoro di Jenkins

1. Passare al portale di Jenkins.

1. Nel menu selezionare **Manage Jenkins** (Gestisci Jenkins).

1. In **System Configuration** (Configurazione del sistema) selezionare **Manage Nodes and Clouds** (Gestisci nodi e cloud).

1. Scegliere **New Node** (Nuovo nodo) dal menu.

1. Immettere un valore per **Node Name** (Nome nodo).

1. Selezionare **Permanent Agent** (Agente permanente).

1. Selezionare **OK**.

1. Immettere un valore per **Remote root directory** (Directory radice remota). Ad esempio: `/home/jenkins/work`

1. Se si vuole, immettere un'etichetta. Le etichette vengono usate per raggruppare più agenti in un unico gruppo logico. Ad esempio, si può usare l'etichetta `linux` per raggruppare gli agenti Linux.

1. Impostare **Launch method** (Metodo di avvio) su **Launch agent by connecting to the master** (Avvia agente tramite connessione al master).

1. Verificare che tutti i campi obbligatori siano stati specificati o immessi:

    ![Esempio di configurazione di un agente Jenkins](./media/azure-container-instances-as-jenkins-build-agent/agent-config.png)

1. Selezionare **Salva**.

1. Nella pagina di stato dell'agente dovrebbero essere visualizzati i valori `JENKINS_SECRET` e `AGENT_NAME`. Lo screenshot seguente mostra come identificare i valori. Entrambi i valori sono necessari per creare l'istanza di Azure Container.

    ![Se la creazione riesce, viene visualizzato il segreto dell'agente di compilazione.](./media/azure-container-instances-as-jenkins-build-agent/jenkins-secret.png)

## <a name="create-azure-container-instance-with-cli"></a>Creare un'istanza di Azure Container con l'interfaccia della riga di comando

1. Usare il comando [az group create](/cli/azure/group?#az_group_create) per creare un gruppo di risorse di Azure.

      ```azurecli
      az group create --name my-resourcegroup --location westus
      ```

1. Usare il comando[az container create](/cli/azure/container#az_container_create) per creare un'istanza di Azure Container. Sostituire i segnaposto con i valori ottenuti durante la creazione dell'agente di lavoro.

    ```azurecli
    az container create \
      --name my-dock \
      --resource-group my-resourcegroup \
      --ip-address Public --image jenkins/inbound-agent:latest \
      --os-type linux \
      --ports 80 \
      --command-line "jenkins-agent -url http://jenkinsserver:port <JENKINS_SECRET> <AGENT_NAME>"
    ```

    Dopo l'avvio, il contenitore si connetterà automaticamente al server del controller Jenkins.

    ![L'agente è stato avviato correttamente](./media/azure-container-instances-as-jenkins-build-agent/agent-start.png)

## <a name="create-a-build-job"></a>Creare un'attività di compilazione

A questo punto, viene creato un processo di compilazione Jenkins per illustrare le compilazioni Jenkins in un'istanza di contenitore di Azure.

1. Selezionare **New item** (Nuovo elemento), assegnare un nome al progetto di compilazione, ad esempio **aci-demo**, selezionare **Freestyle project** (Progetto Freestyle) e fare clic su **OK**.

   ![Finestra per il nome del processo di compilazione e l'elenco dei tipi di progetto](./media/azure-container-instances-as-jenkins-build-agent/jenkins-new-job.png)

2. In **General** (Generale) verificare che sia selezionata l'opzione **Restrict where this project can be run** (Limita i casi in cui eseguire il progetto). Immettere **linux** per Label Expression (Espressione etichetta). Questa configurazione assicura che il processo di compilazione venga eseguito nel cloud delle istanze di contenitore di Azure.

   ![Scheda "General" (Generale) con i dettagli di configurazione](./media/azure-container-instances-as-jenkins-build-agent/jenkins-job-01.png)

3. In **Build** (Compilazione) selezionare **Add build step** (Aggiungi passaggio di compilazione) e quindi selezionare **Execute Shell** (Esegui shell). Immettere `echo "aci-demo"` come comando.

   ![Scheda "Build" (Compilazione) con le selezioni per l'istruzione di compilazione](./media/azure-container-instances-as-jenkins-build-agent/jenkins-job-02.png)

5. Selezionare **Salva**.

## <a name="run-the-build-job"></a>Eseguire il processo di compilazione

Per testare il processo di compilazione e osservare Istanze di Azure Container come piattaforma di compilazione, avviare manualmente una compilazione.

1. Selezionare **Build Now** (Compila) per avviare un processo di compilazione. Per avviare il processo sono necessari alcuni minuti. Verrà visualizzato uno stato simile all'immagine seguente:

   ![Informazione sulla "Build History" (Cronologia di compilazione) con lo stato del processo](./media/azure-container-instances-as-jenkins-build-agent/jenkins-job-status.png)

2. Durante l'esecuzione del processo, aprire il portale di Azure ed esaminare il gruppo di risorse Jenkins. Si noterà che è stata creata un'istanza di contenitore. Il processo Jenkins viene eseguito all'interno di questa istanza.

   ![Istanza di contenitore nel gruppo di risorse](./media/azure-container-instances-as-jenkins-build-agent/jenkins-aci.png)

3. Poiché Jenkins esegue più processi rispetto al numero configurato di executor Jenkins (2 per impostazione predefinita), vengono create più istanze di contenitore.

   ![Istanze di contenitore appena create](./media/azure-container-instances-as-jenkins-build-agent/jenkins-aci-multi.png)

4. Dopo aver completato tutti i processi di compilazione, le istanze di contenitore vengono rimosse.

   ![Gruppo di risorse con istanze di contenitore rimosse](./media/azure-container-instances-as-jenkins-build-agent/jenkins-aci-none.png)

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"]
> [Integrazione continua e distribuzione continua per i Siti Web di Microsoft Azure](/azure/jenkins/tutorial-jenkins-deploy-web-app-azure-app-service)