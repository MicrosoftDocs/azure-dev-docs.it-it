---
title: 'Avvio rapido: Creare un server Jenkins in Azure'
description: Informazioni su come installare Jenkins in una macchina virtuale Linux di Azure dal modello di soluzione Jenkins e compilare un'applicazione Java di esempio.
keywords: jenkins, azure, devops, portale, linux, macchina virtuale, modello di soluzione
ms.topic: quickstart
ms.date: 03/03/2020
ms.openlocfilehash: 2ea038dad2294784804f45026ea26198a9b12d79
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/05/2020
ms.locfileid: "82170337"
---
# <a name="quickstart-create-a-jenkins-server-on-an-azure-linux-vm"></a>Guida introduttiva: Creare un server Jenkins in una macchina virtuale Linux di Azure

Questa guida introduttiva illustra come installare [Jenkins](https://jenkins.io) in una VM Ubuntu Linux con gli strumenti e i plug-in configurati per usare Azure. Al termine, si avrà un server Jenkins in esecuzione in Azure che compila un'app Java di esempio da [GitHub](https://github.com).

## <a name="prerequisites"></a>Prerequisiti

* Accesso a SSH nella riga di comando del computer (ad esempio, la shell Bash o [PuTTY](https://www.putty.org/))

## <a name="create-the-jenkins-vm-from-the-solution-template"></a>Creare la VM Jenkins dal modello di soluzione

Jenkins supporta un modello in cui il server Jenkins delega il lavoro a uno o più agenti per consentire a una singola installazione di Jenkins di ospitare un numero elevato di progetti o di fornire ambienti diversi necessari per le compilazioni o i test. I passaggi in questa sezione consentono di installare e configurare un server Jenkins in Azure.

1. Nel browser aprire l'[immagine di Azure Marketplace per Jenkins](https://azuremarketplace.microsoft.com/marketplace/apps/azure-oss.jenkins?tab=Overview).

1. Selezionare **SCARICA ADESSO**.

    ![Selezionare SCARICA ADESSO per avviare il processo di installazione per l'immagine del Marketplace per Jenkins.](./media/install-from-azure-marketplace-image/jenkins-install-get-it-now.png)

1. Dopo aver esaminato le informazioni sui prezzi e sulle condizioni, selezionare **Continua**.

    ![Informazioni su prezzi e condizioni dell'immagine del Marketplace per Jenkins.](./media/install-from-azure-marketplace-image/jenkins-install-pricing-and-terms.png)

1. Selezionare **Crea** per configurare il server Jenkins nel portale di Azure. 

    ![Installare l'immagine del Marketplace per Jenkins.](./media/install-from-azure-marketplace-image/jenkins-install-create.png)

1. Nella scheda **Informazioni di base** specificare i valori seguenti:

   - **Name** (Nome) - Immettere `Jenkins`.
   - **Nome utente** - Immettere il nome utente da usare per l'accesso alla macchina virtuale su cui è in esecuzione Jenkins. Il nome utente deve soddisfare [specifici requisiti](/azure/virtual-machines/linux/faq#what-are-the-username-requirements-when-creating-a-vm).
   - **Tipo di autenticazione** - Selezionare **Chiave pubblica SSH**.
   - **Chiave pubblica SSH** - Copiare e incollare una chiave pubblica RSA nel formato a riga singola (che inizia con `ssh-rsa`) o nel formato PEM su più righe. È possibile generare le chiavi SSH tramite ssh-keygen in Linux e macOS o con PuTTYGen in Windows. Per altre informazioni sulle chiavi SSH e Azure, vedere l'articolo [Come usare le chiavi SSH con Windows in Azure](/azure/virtual-machines/linux/ssh-from-windows).
   - **Sottoscrizione**: selezionare la sottoscrizione di Azure in cui si vuole installare Jenkins.
   - **Gruppo di risorse**: selezionare **Crea nuovo** e immettere un nome per il gruppo di risorse che funga da contenitore logico per la raccolta di risorse che costituiscono l'installazione di Jenkins.
   - **Località**: selezionare **Stati Uniti orientali**.

     ![Immettere le informazioni di autenticazione e sul gruppo di risorse per Jenkins nella scheda Informazioni di base.](./media/install-from-azure-marketplace-image/jenkins-configure-basic.png)

1. Selezionare **OK** per passare alla scheda **Impostazioni aggiuntive**. 

1. Nella scheda **Impostazioni aggiuntive** specificare i valori seguenti:

   - **Dimensioni**: selezionare l'opzione appropriata per le dimensioni per la macchina virtuale Jenkins.
   - **Tipo di disco della macchina virtuale**: specificare HDD (unità disco rigido) o SSD (unità SSD) per indicare il tipo di disco di archiviazione consentito per la macchina virtuale Jenkins.
   - **Rete virtuale** - (Facoltativo) Selezionare **Rete virtuale** per modificare le impostazioni predefinite.
   - **Subnet** - Selezionare **Subnet**, verificare le informazioni e selezionare **OK**.
   - **Indirizzo IP pubblico**: per impostazione predefinita, per il nome dell'indirizzo IP viene usato il nome di Jenkins specificato nella pagina precedente, preceduto dal suffisso -IP. È possibile selezionare l'opzione per modificare il valore predefinito.
   - **Etichetta del nome di dominio**: specificare il valore per l'URL completo della macchina virtuale Jenkins.
   - **Tipo di versione Jenkins** - Selezionare il tipo di versione desiderato tra le opzioni `LTS`, `Weekly build` o `Azure Verified`. Le opzioni `LTS` e `Weekly build` sono descritte nell'articolo [Jenkins LTS Release Line](https://jenkins.io/download/lts/) (Ciclo di rilascio LTS Jenkins). L'opzione `Azure Verified` fa riferimento a una [versione LTS di Jenkins](https://jenkins.io/download/lts/) verificata per l'esecuzione in Azure. 
   - **Tipo di JDK**: JDK da installare. L'impostazione predefinita prevede compilazioni certificate e testate con Zulu di OpenJDK.

     ![Immettere le impostazioni della macchina virtuale per Jenkins nella scheda Impostazioni.](./media/install-from-azure-marketplace-image/jenkins-configure-settings.png)

1. Selezionare **OK** per passare alla scheda **Impostazioni di integrazione**.

1. Nella scheda **Impostazioni di integrazione** specificare i valori seguenti:

    - **Entità servizio** - L'entità servizio viene aggiunta in Jenkins come credenziale per l'autenticazione con Azure. `Auto` significa che l'entità verrà creata dall'identità del servizio gestito. `Manual` significa che l'entità deve essere creata dall'utente. 
        - **ID applicazione** e **Segreto** - Se si seleziona l'opzione `Manual` per l'opzione **Entità servizio** è necessario specificare `Application ID` e `Secret` per l'entità servizio. Quando si [crea un'entità servizio](/cli/azure/create-an-azure-service-principal-azure-cli), si noti che il ruolo predefinito è **Collaboratore**, sufficiente per l'utilizzo delle risorse di Azure.
    - **Enable Cloud Agents** (Abilita agenti cloud) - Specificare il modello di cloud predefinito per gli agenti in cui `ACI` fa riferimento a Istanza di contenitore di Azure e `VM` si riferisce alle macchine virtuali. È anche possibile specificare `No` se non si desidera abilitare un agente cloud.

1. Selezionare **OK** per passare alla scheda **Riepilogo**.

1. Quando viene visualizzata la scheda **Riepilogo**, vengono convalidate le informazioni immesse. Quando viene visualizzato il messaggio **Convalida superata** nella parte superiore della scheda, selezionare **OK**. 

     ![La scheda Riepilogo visualizza e convalida le opzioni selezionate.](./media/install-from-azure-marketplace-image/jenkins-configure-summary.png)

1. Quando viene visualizzata la scheda **Crea**, selezionare **Crea** per creare la macchina virtuale Jenkins. Quando il server è pronto, viene visualizzata una notifica nel portale di Azure.

     ![Notifica che indica che Jenkins è pronto.](./media/install-from-azure-marketplace-image/jenkins-install-notification.png)

## <a name="connect-to-jenkins"></a>Connettersi a Jenkins

1. Passare alla macchina virtuale (ad esempio, `http://jenkins2517454.eastus.cloudapp.azure.com/` nel Web browser. Dato che la console Jenkins non è accessibile tramite HTTP non protetto, nella pagina verranno visualizzate istruzioni per accedere alla console Jenkins in modo sicuro dal computer usando un tunnel SSH.

    ![Sbloccare Jenkins](./media/install-solution-template-steps/jenkins-ssh-instructions.png)

1. Configurare il tunnel usando il comando `ssh` sulla pagina dalla riga di comando, sostituendo `username` con il nome dell'utente amministratore della macchina virtuale scelto in precedenza durante la configurazione della macchina virtuale dal modello di soluzione.

    ```bash
    ssh -L 127.0.0.1:8080:localhost:8080 jenkinsadmin@jenkins2517454.eastus.cloudapp.azure.com
    ```
    
1. Dopo aver avviato il tunnel, passare a `http://localhost:8080/` nel computer locale. 

1. Ottenere la password iniziale eseguendo questo comando nella riga di comando mentre si è connessi tramite SSH alla VM Jenkins:

    ```bash
    sudo cat /var/lib/jenkins/secrets/initialAdminPassword
    ```
    
1. Sbloccare il dashboard di Jenkins per la prima volta con questa password iniziale.

    ![Sbloccare Jenkins](./media/install-solution-template-steps/jenkins-unlock.png)

1. Selezionare **Install suggested plugins** (Installa plug-in consigliati) nella pagina successiva e quindi creare un utente amministratore di Jenkins che verrà usato per accedere al dashboard di Jenkins.

    ![Jenkins è pronto per l'uso.](./media/install-solution-template-steps/jenkins-welcome.png)

Il server Jenkins è ora pronto per la compilazione di codice.

## <a name="create-your-first-job"></a>Creare il primo processo

1. Selezionare **Create new jobs** (Crea nuovi processi) nella console Jenkins, assegnare il nome **mySampleApp** e selezionare **Freestyle project** (Progetto Freestyle) e quindi **OK**.

    ![Creare un nuovo processo](./media/install-solution-template-steps/jenkins-new-job.png) 

1. Selezionare la scheda **Source Code Management** (Gestione del codice sorgente), abilitare **Git** e immettere l'URL seguente nel campo **Repository URL** (URL del repository): `https://github.com/spring-guides/gs-spring-boot.git`

    ![Definire il repository Git](./media/install-solution-template-steps/jenkins-job-git-configuration.png) 

1. Selezionare la scheda **Build** (Compilazione) e quindi **Add build step** (Aggiungi passaggio di compilazione) e **Invoke Gradle script** (Richiama script Gradle). Selezionare **Use Gradle Wrapper** (Usa wrapper di Gradle) e quindi immettere `complete` in **Wrapper location** (Percorso wrapper) e `build` in **Tasks** (Attività).

    ![Usare il wrapper di Gradle per la compilazione](./media/install-solution-template-steps/jenkins-job-gradle-config.png) 

1. Selezionare **Advanced** (Avanzate) e quindi immettere `complete` nel campo **Root Build script** (Script di compilazione radice). Selezionare **Salva**.

    ![Configurare le impostazioni avanzate nel passaggio di compilazione del wrapper di Gradle](./media/install-solution-template-steps/jenkins-job-gradle-advances.png) 

## <a name="build-the-code"></a>Compilare il codice

1. Selezionare **Build Now** (Compila) per compilare il codice e creare il pacchetto dell'app di esempio. Al termine della compilazione, selezionare il collegamento **Workspace** (Area di lavoro) per il progetto.

    ![Passare all'area di lavoro per ottenere il file JAR della compilazione](./media/install-solution-template-steps/jenkins-access-workspace.png) 

1. Passare a `complete/build/libs` e verificare che sia presente il file `gs-spring-boot-0.1.0.jar` per assicurarsi che la compilazione sia stata completata correttamente. Il server Jenkins è ora pronto per la compilazione dei progetti dell'utente in Azure.

## <a name="troubleshooting-the-jenkins-solution-template"></a>Risoluzione dei problemi del modello di soluzione Jenkins

Se si rilevano bug relativi al modello di soluzione Jenkins, segnalare un problema nel [repository Jenkins di GitHub](https://github.com/azure/jenkins/issues).

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"]
> [Jenkins in Azure](/azure/developer/jenkins)