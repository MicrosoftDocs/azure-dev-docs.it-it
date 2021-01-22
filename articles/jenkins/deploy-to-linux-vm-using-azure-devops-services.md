---
title: Eseguire la distribuzione in macchine virtuali Linux con Jenkins e Azure DevOps Services
description: Informazioni su come facilitare l'integrazione e la distribuzione continue (CI/CD) per distribuire app in macchine virtuali Linux con Jenkins e Azure DevOps Services
keywords: jenkins, azure, devops, macchina virtuale, cicd, azure devops services
ms.topic: tutorial
ms.date: 07/31/2018
ms.custom: devx-track-jenkins
ms.openlocfilehash: 2261db025f48ef6df5e0ad3d8518ef3fc966f13d
ms.sourcegitcommit: 3d906f265b748fbc0a070fce252098675674c8d9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/22/2021
ms.locfileid: "98699939"
---
# <a name="tutorial-deploy-to-linux-virtual-machine-using-jenkins-and-azure-devops-services"></a>Esercitazione: Eseguire la distribuzione in macchine virtuali Linux con Jenkins e Azure DevOps Services

L'integrazione continua (CI) e la distribuzione continua (CD) formano una pipeline con cui è possibile compilare, rilasciare e distribuire codice. Azure DevOps Services offre un set di strumenti di automazione di integrazione continua/distribuzione continua completo per la distribuzione in Azure. Jenkins è uno strumento di terze parti diffuso basato su server per CI/CD che offre anche l'automazione CI/CD. È possibile usare insieme Azure DevOps Services e Jenkins per personalizzare la distribuzione dell'app o del servizio cloud.

In questa esercitazione si compila un'app Web Node.js usando Jenkins. Usare quindi Azure DevOps per la distribuzione

in un [gruppo di distribuzione](/azure/devops/pipelines/release/deployment-groups/index) che contiene macchine virtuali Linux. Si apprenderà come:

> [!div class="checklist"]
> * Ottenere l'app di esempio.
> * Configurare i plug-in Jenkins.
> * Configurare un progetto Freestyle di Jenkins per Node.js.
> * Configurare Jenkins per l'integrazione con Azure DevOps Services.
> * Creare un endpoint di servizio di Jenkins.
> * Creare un gruppo di distribuzione per le macchine virtuali di Azure.
> * Creare una pipeline di versione in Azure Pipelines.
> * Eseguire distribuzioni manuali e attivate da CI.

## <a name="prerequisites"></a>Prerequisiti

- **Sottoscrizione di Azure**: Se non si ha una sottoscrizione di Azure, [creare un account Azure gratuito](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) prima di iniziare.
- **Server Jenkins**: se non è installato un server Jenkins, [creare un server Jenkins in Azure](./configure-on-linux-vm.md).

  > [!NOTE]
  > Per altre informazioni, vedere [Connettersi ad Azure DevOps Services](/azure/devops/organizations/projects/connect-to-projects).

*  È necessaria una macchina virtuale Linux per una destinazione di distribuzione.  Per altre informazioni, vedere [Creare e gestire VM Linux con l'interfaccia della riga di comando di Azure](/azure/virtual-machines/linux/tutorial-manage-vm).

*  Aprire la porta 80 in ingresso per la macchina virtuale. Per altre informazioni, vedere [Creare gruppi di sicurezza di rete mediante il portale di Azure](/azure/virtual-network/tutorial-filter-network-traffic).

## <a name="get-the-sample-app"></a>Ottenere l'app di esempio

È necessario disporre di un'app da distribuire archiviata in un repository Git.
Per questa esercitazione, si consiglia di usare [questa app di esempio disponibile in GitHub](https://github.com/azure-devops/fabrikam-node). Questa esercitazione contiene uno script di esempio usato per installare Node.js e un'applicazione. Per usare il proprio repository, è consigliabile configurare un esempio simile.

Creare un fork di questa app e prendere nota del percorso (URL) per l'uso nei passaggi successivi di questa esercitazione. Per altre informazioni, vedere [Creare una copia tramite fork di un repository](https://help.github.com/articles/fork-a-repo/).    

> [!NOTE]
> L'app è stata compilata tramite [Yeoman](https://yeoman.io/learning/index.html). Usa Express, bower e grunt. Include alcuni pacchetti npm come dipendenze.
> L'esempio contiene anche uno script che configura Nginx e distribuisce l'app. Viene eseguito sulle macchine virtuali. In particolare, lo script:
> 1. Installa Node, Nginx e PM2.
> 2. Configura Nginx e PM2.
> 3. Avvia l'app Node.

## <a name="configure-jenkins-plug-ins"></a>Configurare i plug-in Jenkins

Prima di tutto, è necessario configurare due plug-in Jenkins: **NodeJS** e **VS Team Services Continuous Deployment**.

1. Aprire l'account Jenkins e selezionare **Manage Jenkins** (Gestisci Jenkins).
2. Nella pagina **Manage Jenkins** (Gestisci Jenkins) selezionare **Manage Plugins** (Gestisci plug-in).
3. Filtrare l'elenco per individuare il plug-in **NodeJS** e selezionare l'opzione **Install without restart** (Installa senza riavvio).
    ![Aggiunta del plug-in NodeJS a Jenkins](media/deploy-to-linux-vm-using-azure-devops-services/jenkins-nodejs-plugin.png)
4. Filtrare l'elenco per trovare il plug-in **VS Team Services Continuous Deployment** (Distribuzione continua di VS Team Services) e selezionare l'opzione **Installa senza riavvio**.
5. Tornare al dashboard di Jenkins e selezionare **Manage Jenkins** (Gestisci Jenkins).
6. Selezionare **Global Tool Configuration** (Configurazione strumenti globale). Trovare **NodeJS** e selezionare **NodeJS installations** (Installazioni NodeJS).
7. Selezionare l'opzione **Install automatically** (Installa automaticamente) e quindi immettere un valore per **Name** (Nome).
8. Selezionare **Salva**.

## <a name="configure-a-jenkins-freestyle-project-for-nodejs"></a>Configurare un progetto Freestyle di Jenkins per Node.js

1. Selezionare **Nuovo elemento**. Immettere un nome di elemento.
2. Selezionare **Freestyle project** (Progetto Freestyle). Selezionare **OK**.
3. Nella scheda **Source Code Management** (Gestione codice sorgente) selezionare **Git** e immettere i dettagli del repository e del ramo contenenti il codice dell'app.    
    ![Aggiungere un repository alla build](media/deploy-to-linux-vm-using-azure-devops-services/jenkins-git.png)
4. Nella scheda **Build Triggers** (Trigger build) selezionare **Poll SCM** (Polling SCM) e immettere la pianificazione `H/03 * * * *` per il polling delle modifiche al repository Git ogni tre minuti. 
5. Nella scheda **Build Environment** (Ambiente build) selezionare **Provide Node &amp; npm bin/ folder PATH** (Fornisci nodo e PERCORSO cartella npm bin/) e selezionare il valore **NodeJS Installation** (Installazione NodeJS). Lasciare **npmrc file** impostato su **use system default** (usa impostazioni predefinite di sistema).
6. Nella scheda **Build** (Compila) selezionare **Execute shell** (Esegui shell) e immettere il comando `npm install` per assicurarsi che tutte le dipendenze siano aggiornate.


## <a name="configure-jenkins-for-azure-devops-services-integration"></a>Configurare Jenkins per l'integrazione con Azure DevOps Services

> [!NOTE]
> Assicurarsi che il token di accesso personale usato per i passaggi seguenti contenga l'autorizzazione di *versione* (lettura, scrittura, esecuzione e gestione) in Azure DevOps Services.
 
1.  Creare un token di accesso personale nell'organizzazione di Azure DevOps Services, se non è già disponibile. Jenkins richiede questa informazione per accedere all'organizzazione di Azure DevOps Services. Assicurarsi di archiviare le informazioni del token per i passaggi successivi di questa sezione.
  
    Per informazioni su come generare un token, vedere [How do I create a personal access token for Azure DevOps Services?](/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate) (Come creare un token di accesso personale per Azure DevOps Services).
2. Nella scheda **Post-build Actions** (Azioni post-compilazione) selezionare **Add post-build action** (Aggiungi azione post-compilazione). Selezionare **Archive the artifacts** (Archivia gli elementi).
3. Per **Files to archive** (File da archiviare) immettere `**/*` per includere tutti i file.
4. Per creare un'altra azione, selezionare **Add post-build action** (Aggiungi azione post-compilazione).
5. Selezionare **Trigger release in TFS/Team Services** (Attiva rilascio in TFS/Team Services). Immettere l'URI per l'organizzazione di Azure DevOps Services, ad esempio **https://{your-organization-name}.visualstudio.com**.
6. Immettere il nome del **progetto**.
7. Scegliere un nome per la pipeline di versione. Si crea questa pipeline di versione in un momento successivo in Azure DevOps Services.
8. Scegliere le credenziali per connettersi all'ambiente Azure DevOps Services o Azure DevOps Server:
   - Lasciare vuoto **Username** (Nome utente) se si usa Azure DevOps Services. 
   - Immettere nome utente e password se si usa una versione locale diAzure DevOps Server.    
   ![Configurazione delle azioni di post-compilazione in Jenkins](media/deploy-to-linux-vm-using-azure-devops-services/trigger-release-from-jenkins.png)
5. Salvare il progetto Jenkins.


## <a name="create-a-jenkins-service-endpoint"></a>Creare un endpoint servizio di Jenkins

Un endpoint di servizio consente ad Azure DevOps Services di connettersi a Jenkins.

1. Aprire la pagina **Servizi** in Azure DevOps Services, aprire l'elenco **Nuovo endpoint servizio** e selezionare **Jenkins**.
   ![Aggiungere un endpoint di Jenkins](media/deploy-to-linux-vm-using-azure-devops-services/add-jenkins-endpoint.png)
2. Immettere un nome per la connessione.
3. Immettere l'URL del server Jenkins e selezionare l'opzione **Accetta i certificati SSL non attendibili**. Un esempio di URL è **http://{URLJenkinspersonale}.westcentralus.cloudapp.azure.com**.
4. Immettere nome utente e password per l'account Jenkins.
5. Selezionare **Verifica connessione** per assicurarsi che le informazioni siano corrette.
6. Selezionare **OK** per creare l'endpoint servizio.

## <a name="create-a-deployment-group-for-azure-virtual-machines"></a>Creare un gruppo di distribuzione per Macchine virtuali di Azure

È necessario un [gruppo di distribuzione](https://www.visualstudio.com/docs/build/concepts/definitions/release/deployment-groups/) per registrare l'agente di Azure DevOps Services in modo che la pipeline di versione possa essere distribuita nella macchina virtuale. I gruppi di distribuzione facilitano la definizione di gruppi logici di computer di destinazione e l'installazione dell'agente necessario in ogni computer.

   > [!NOTE]
   > Nella procedura seguente assicurarsi di installare i prerequisiti e di *non eseguire lo script con privilegi sudo*.

1. Aprire la scheda **Versioni** nell'hub **Compilazione e versione**, quindi aprire **Gruppi di distribuzione** e selezionare **+ Nuovo**.
2. Immettere un nome per il gruppo di distribuzione e una descrizione facoltativa, Selezionare quindi **Crea**.
3. Scegliere il sistema operativo per la macchina virtuale di destinazione di distribuzione. Ad esempio selezionare **Ubuntu 16.04+** .
4. Selezionare **Usa un token di accesso personale nello script per l'autenticazione**.
5. Selezionare il collegamento **Prerequisiti di sistema**. Installare i prerequisiti per il sistema operativo.
6. Selezionare **Copia script negli appunti** per copiare lo script.
7. Accedere alla macchina virtuale di destinazione di distribuzione ed eseguire lo script. Non eseguire lo script con privilegi sudo.
8. Dopo l'installazione, vengono chiesti i tag dei gruppi di distribuzione. Accettare i valori predefiniti.
9. In Azure DevOps Services cercare la nuova macchina virtuale registrata in **Destinazioni** sotto **Gruppi di distribuzione**.

## <a name="create-an-azure-pipelines-release-pipeline"></a>Creare una pipeline di versione in Azure Pipelines

Una pipeline di versione specifica il processo usato da Azure Pipelines per distribuire l'app. In questo esempio si esegue uno script della shell.

Per creare la pipeline di versione in Azure Pipelines:

1. Aprire la scheda **Versioni** nell'hub **Compilazione e versione** e selezionare **Crea pipeline di versione**. 
2. Selezionare il modello **Vuoto** scegliendo l'avvio con un **processo vuoto**.
3. Nella sezione **Elementi** selezionare **+ Aggiungi elemento** e scegliere **Jenkins** per **Tipo di origine**. Selezionare la connessione all'endpoint servizio Jenkins, quindi selezionare il processo di origine Jenkins e selezionare **Aggiungi**.
4. Selezionare i puntini di sospensione accanto ad **Ambiente 1**. Selezionare **Aggiungi fase per gruppo di distribuzione**.
5. Scegliere il gruppo di distribuzione.
5. Selezionare **+** per aggiungere un'attività alla **fase gruppo di distribuzione**.
6. Selezionare l'attività **Script della shell** e selezionare **Aggiungi**. L'attività **Script della Shell** fornisce la configurazione di esecuzione di uno script in ogni server al fine di installare Node.js e avviare l'app.
8. Per **Percorso script** immettere **$(System.DefaultWorkingDirectory)/Fabrikam-Node/deployscript.sh**.
9. Selezionare **Avanzate** e quindi abilitare **Specifica directory di lavoro**.
10. Per **Directory di lavoro** immettere **$(System.DefaultWorkingDirectory)/Fabrikam-Node**.
11. Modificare il nome della pipeline di versione in quello specificato nella scheda **Post-build Actions** (Azioni post-compilazione) nella compilazione in Jenkins. Jenkins richiede questo nome per poter attivare una nuova versione quando vengono aggiornati gli elementi di origine.
12. Selezionare **Salva** e quindi **OK** per salvare la pipeline di versione.

## <a name="execute-manual-and-ci-triggered-deployments"></a>Eseguire distribuzioni manuali e attivate da CI

1. Selezionare **+ Versione** e selezionare **Crea versione**.
2. Selezionare la build completata nell'elenco a discesa evidenziato e quindi **Accoda**.
3. Scegliere il collegamento di versione nel messaggio popup. Ad esempio: "Versione **Versione-1** creata".
4. Aprire la scheda **Log** per osservare l'output della console della versione.
5. Nel browser aprire l'URL di uno dei server aggiunti al gruppo di distribuzione. Ad esempio, immettere **http://{indirizzo-ip-server-personale}** .
6. Passare al repository Git di origine e modificare il contenuto del titolo **h1** nel file app/views/index.jade con un testo modificato.
7. Eseguire il commit delle modifiche.
8. Dopo alcuni minuti, si noterà una nuova versione creata nella pagina **Versioni** di Azure DevOps. Aprire la versione per visualizzare la distribuzione in corso. Congratulazioni!

## <a name="troubleshooting-the-jenkins-plug-in"></a>Risoluzione dei problemi del plug-in Jenkins

Se si rilevano bug con i plug-in Jenkins, segnalare il problema in [Jenkins JIRA](https://issues.jenkins-ci.org/) per il componente specifico.

## <a name="next-steps"></a>Passaggi successivi

In questa esercitazione è stata automatizzata la distribuzione di un'app in Azure usando Jenkins per la compilazione e Azure DevOps Services per la versione. Si è appreso come:

> [!div class="checklist"]
> * Creare l'app in Jenkins.
> * Configurare Jenkins per l'integrazione con Azure DevOps Services.
> * Creare un gruppo di distribuzione per le macchine virtuali di Azure.
> * Creare una pipeline di Azure che configura le VM e distribuisce l'app.

Per informazioni su come usare Azure Pipelines per i passaggi di compilazione e versioni, vedere [questo articolo](/azure/devops/pipelines/apps/cd/deploy-linuxvm-deploygroups).

Per informazioni su come creare una pipeline CI/CD basata su YAML per la distribuzione in macchine virtuali, passare all'esercitazione successiva.

> [!div class="nextstepaction"]
> [Jenkins in Azure](/azure/Jenkins/)