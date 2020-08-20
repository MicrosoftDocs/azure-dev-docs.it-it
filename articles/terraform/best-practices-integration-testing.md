---
title: Esercitazione - Test di integrazione con Terraform e Azure
description: Informazioni sui test di integrazione e su come usare Azure DevOps per configurare l'integrazione continua per i progetti Terraform.
ms.topic: tutorial
ms.date: 07/31/2020
ms.custom: devx-track-terraform
ms.openlocfilehash: 3d305fb63deffb8f56ebd2cb1503bac543c5b84b
ms.sourcegitcommit: 16ce1d00586dfa9c351b889ca7f469145a02fad6
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/14/2020
ms.locfileid: "88241303"
---
# <a name="tutorial-configure-integration-tests-for-terraform-projects-in-azure"></a>Esercitazione: Configurare i test di integrazione per i progetti Terraform in Azure

[!INCLUDE [terraform-intro.md](includes/terraform-intro.md)]

In questo articolo si apprenderà come eseguire le attività seguenti:

> [!div class="checklist"]
> * Ottenere informazioni sui concetti di base dei test di integrazione per i progetti Terraform.
> * Usare Azure DevOps per configurare una pipeline di integrazione continua.
> * Eseguire l'analisi statica del codice nel codice di Terraform.
> * Eseguire `terraform validate` per convalidare i file di configurazione di Terraform nel computer locale.
> * Eseguire `terraform plan` per convalidare i file di configurazione di Terraform dal punto di vista dei servizi remoti.
> * Usare una pipeline di Azure per automatizzare l'integrazione continua.

[!INCLUDE [hashicorp-support.md](includes/hashicorp-support.md)]

## <a name="prerequisites"></a>Prerequisiti

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../includes/open-source-devops-prereqs-azure-subscription.md)]
- **Organizzazione e progetto di Azure DevOps**: se non è già disponibile, [creare un'organizzazione di Azure DevOps](https://docs.microsoft.com/azure/devops/organizations/projects/create-project?view=azure-devops&tabs=preview-page).
- **Estensione Terraform Build & Release Tasks**: [installare l'estensione Terraform build/release tasks](https://marketplace.visualstudio.com/items?itemName=charleszipp.azure-pipelines-tasks-terraform) nell'organizzazione di Azure DevOps.
- **Concedere l'accesso ad Azure DevOps alla sottoscrizione di Azure**: creare una [connessione del servizio di Azure](https://docs.microsoft.com/azure/devops/pipelines/library/connect-to-azure?view=azure-devops) denominata `terraform-basic-testing-azure-connection` per consentire ad Azure Pipelines di connettersi alle sottoscrizioni di Azure.
- **Installare Terraform**: in base all'ambiente specifico, [scaricare e installare Terraform](https://www.terraform.io/downloads.html).
- **Creare una copia tramite fork degli esempi di test**: creare una copia tramite fork del [progetto di esempio di Terraform in GitHub](https://github.com/Azure/terraform) e clonarlo nel computer di sviluppo/test.

## <a name="validate-alocal-terraform-configuration"></a>convalidare una configurazione locale di Terraform

Il comando [terraform validate](https://www.terraform.io/docs/commands/validate.html) viene eseguito dalla riga di comando nella directory contenente i file di Terraform. L'obiettivo principale di questo comando consiste nel convalidare la sintassi.

1. Aprire l'ambiente da riga di comando preferito. Molti editor di codice, ad esempio Visual Studio Code, forniscono un'interfaccia della riga di comando.

1. Passare alla directory `samples/integration-testing/src` del repository locale. Contiene un semplice progetto Terraform.

1. Inizializzare la distribuzione di Terraform con [terraform init](https://www.terraform.io/docs/commands/init.html). Questo passaggio scarica i moduli di Azure necessari per creare un gruppo di risorse di Azure.

    ```bash
    terraform init
    ```

1. Convalidare il file di test di Terraform con [terraform validate](https://www.terraform.io/docs/commands/validate.html).

    ```bash
    terraform validate
    ```

    Dovrebbe essere visualizzato un messaggio che indica che la configurazione è valida.

1. Nell'editor di codice aprire il file `main.tf`.

1. Nella riga 5 inserire un errore di ortografia che rende non valida la sintassi. Sostituire ad esempio `var.location` con `var.loaction`

1. Salvare il file.

1. Esegui di nuovo la convalida.

    ```bash
    terraform validate
    ```

    Dovrebbe essere ora visualizzato un messaggio di errore che indica la riga errata e fornisce una descrizione dell'errore.

Come si può notare, Terraform ha rilevato un problema nella sintassi del codice di configurazione. Questo problema impedisce la distribuzione della configurazione.

È consigliabile eseguire sempre `terraform validate` nei file di Terraform prima di eseguirne il push nel sistema di controllo della versione. È inoltre consigliabile includere questo livello di convalida nella pipeline di integrazione continua. Più avanti in questa esercitazione verrà illustrato come [configurare una pipeline di Azure per la convalida automatica](#automate-integration-tests-using-azure-pipeline).

## <a name="validate-terraform-configuration-can-be-deployed-on-azure"></a>Convalidare la configurazione di Terraform che può essere distribuita in Azure

Nella sezione precedente è stato illustrato come convalidare una configurazione di Terraform. Tale livello di test è specifico per la sintassi. Il test non ha preso in considerazione gli elementi che potrebbero essere già stati distribuiti in Azure.

Terraform è un *linguaggio dichiarativo*, ovvero si dichiara quello che si vuole ottenere come risultato finale. Si supponga ad esempio che in un gruppo di risorse siano presenti 10 macchine virtuali. Viene quindi creato un file di Terraform che definisce tre macchine virtuali. L'applicazione di questo piano non incrementa il conteggio totale a 13. Terraform elimina invece sette macchine virtuali, in modo da ottenere un numero totale di tre. L'esecuzione di `terraform plan` consente di confermare i risultati potenziali dell'applicazione di un piano di esecuzione per evitare sorprese.

Per generare il piano di esecuzione di Terraform, eseguire [terraform plan](https://www.terraform.io/docs/commands/plan.html). Questo comando stabilisce la connessione alla sottoscrizione di Azure per controllare quale parte della configurazione è già stata distribuita. Terraform determina quindi le modifiche necessarie per soddisfare i requisiti indicati nel file di Terraform. In questa fase Terraform non distribuisce alcun elemento. Fornisce informazioni sugli effetti dell'applicazione del piano.

Se si segue l'esercitazione e sono stati eseguiti i passaggi della sezione precedente, eseguire il comando `terraform plan`:

```bash
terraform plan
```

Dopo l'esecuzione di `terraform plan`, Terraform mostra il risultato potenziale dell'applicazione del piano di esecuzione. L'output indica le risorse di Azure che verranno aggiunte, modificate ed eliminate definitivamente.

Per impostazione predefinita, Terraform archivia lo stato nella stessa directory locale in cui si trova il file di Terraform. Questo criterio è ottimale negli scenari a utente singolo. Quando tuttavia più utenti lavorano sulle stesse risorse di Azure, è possibile che non si riesca a eseguire la sincronizzazione dei file di stato locali. Per risolvere questo problema, Terraform supporta la scrittura di file di stato in un archivio dati remoto, ad esempio Archiviazione di Azure. In questo scenario l'esecuzione di `terraform plan` in un computer locale con un computer remoto come destinazione potrebbe risultare problematica. Potrebbe quindi essere consigliabile [automatizzare la procedura di convalida come parte della pipeline di integrazione continua](#automate-integration-tests-using-azure-pipeline).

## <a name="run-static-code-analysis"></a>Eseguire l'analisi statica del codice

L'analisi statica del codice può essere eseguita direttamente nel codice di configurazione di Terraform, senza eseguirlo. Questa analisi può essere utile per rilevare problemi, ad esempio i problemi relativi alla sicurezza e il mancato rispetto della conformità.

Gli strumenti seguenti forniscono l'analisi statica per i file di Terraform:

- [Checkov](https://github.com/bridgecrewio/checkov/)
- [Terrascan](https://github.com/cesar-rodriguez/terrascan)
- [tfsec](https://github.com/liamg/tfsec) 
- [Deepsource](https://deepsource.io/blog/release-terraform-static-analysis/) 

L'analisi statica viene spesso eseguita come parte di una pipeline di integrazione continua. Questi test non richiedono la creazione di un piano di esecuzione o la distribuzione. L'esecuzione di questi test risulta quindi più rapida rispetto ad altri test e vengono in genere eseguiti per primi nel processo di integrazione continua.

## <a name="automate-integration-tests-using-azure-pipeline"></a>Automatizzare i test di integrazione con una pipeline di Azure

L'integrazione continua comporta l'esecuzione di test su un intero sistema quando viene apportata una modifica. In questa sezione verrà illustrata la configurazione di una pipeline di Azure usata per implementare l'integrazione continua.

1. Nell'editor preferito passare al clone locale del [progetto Terraform di esempio in GitHub](https://github.com/Azure/terraform).

1. Aprire il file `samples/integration-testing/src/azure-pipeline.yaml`.

1. Scorrere verso il basso fino alla sezione **steps**, che include un set standard di passaggi usati per eseguire diverse routine di installazione e convalida.

1. Esaminare la riga **Step 1: run the Checkov Static Code Analysis**. In questo passaggio il progetto `Checkov` indicato in precedenza esegue un'analisi statica del codice nella configurazione di esempio di Terraform. 

    ```yaml
    - bash: $(terraformWorkingDirectory)/checkov.sh $(terraformWorkingDirectory)
      displayName: Checkov Static Code Analysis
    ```
    
    Nota:
    
    - Questo script è responsabile per l'esecuzione di Checkov nell'area di lavoro di Terraform montata in un contenitore Docker. Gli agenti gestiti da Microsoft sono abilitati per Docker. L'esecuzione di strumenti in un contenitore Docker risulta più facile e consente di evitare la necessità di installare Checkov nell'agente di Azure Pipelines.
    - La variabile `$(terraformWorkingDirectory)` viene definita nel file `azure-pipeline.yaml`.

1. Esaminare la riga **Step 2: install Terraform on the Azure Pipelines agent**. L'[estensione Terraform Build & Release Task](https://marketplace.visualstudio.com/items?itemName=charleszipp.azure-pipelines-tasks-terraform) installata in precedenza include un comando per l'installazione di Terraform nell'agente che esegue la pipeline di Azure. Nel passaggio viene eseguita questa attività.

    ```yaml
    - task: charleszipp.azure-pipelines-tasks-terraform.azure-pipelines-tasks-terraform-installer.TerraformInstaller@0
      displayName: 'Install Terraform'
      inputs:
        terraformVersion: $(terraformVersion)
    ```
    
    Nota:

    - La versione di Terraform da installare viene specificata tramite una variabile di Azure Pipelines denominata `terraformVersion` e definita nel file `azure-pipeline.yaml`.

1. Esaminare la riga **Step 3: run Terraform init to initialize the workspace**. Dopo l'installazione di Terraform nell'agente sarà possibile inizializzare la directory di Terraform.

    ```yaml
    - task: charleszipp.azure-pipelines-tasks-terraform.azure-pipelines-tasks-terraform-cli.TerraformCLI@0
      displayName: 'Run terraform init'
      inputs:
        command: init
        workingDirectory: $(terraformWorkingDirectory)
    ```
    
    Note:

    - L'input `command` specifica il comando di Terraform da eseguire.
    - L'input `workingDirectory` indica il percorso della directory di Terraform.
    - La variabile `$(terraformWorkingDirectory)` viene definita nel file `azure-pipeline.yaml`.

1. Esaminare la riga **Step 4: run Terraform validate to validate HCL syntax**. Dopo l'inizializzazione della directory del progetto viene eseguito il comando`terraform validate` per convalidare la configurazione sul server.

    ```yaml
    - task: charleszipp.azure-pipelines-tasks-terraform.azure-pipelines-tasks-terraform-cli.TerraformCLI@0
      displayName: 'Run terraform validate'
      inputs:
        command: validate
        workingDirectory: $(terraformWorkingDirectory)
    ```
    
1. Esaminare la riga **Step 5: run Terraform plan to validate HCL syntax**. Come illustrato in precedenza, il piano di esecuzione viene generato per verificare la validità della configurazione di Terraform prima della distribuzione.

    ```yaml
    - task: charleszipp.azure-pipelines-tasks-terraform.azure-pipelines-tasks-terraform-cli.TerraformCLI@0
      displayName: 'Run terraform plan'
      inputs:
        command: plan
        workingDirectory: $(terraformWorkingDirectory)
        environmentServiceName: $(serviceConnection)
        commandOptions: -var location=$(azureLocation)
    ```
    
    Note:

    - L'input `environmentServiceName` fa riferimento al nome della connessione al servizio di Azure creata in [Prerequisiti](#prerequisites). La connessione consente a Terraform di accedere alla sottoscrizione di Azure.
    - L'input `commandOptions` viene usato per passare argomenti al comando di Terraform. In questo caso viene specificata una posizione. La variabile `$(azureLocation)` viene definita in precedenza nel file YAML.

### <a name="import-the-pipeline-into-azure-devops"></a>Importare la pipeline in Azure DevOps

1. Aprire il progetto di Azure DevOps e passare alla sezione Azure Pipelines.
 
1. Selezionare il pulsante **Crea pipeline**.

1. Per l'opzione **Dov'è il codice?** selezionare **GitHub (YAML)** .

    ![Dov'è il codice?](media/best-practices-integration-testing/new-pipeline-where-github-yaml.png)

1. A questo punto potrebbe essere necessario autorizzare Azure DevOps ad accedere l'organizzazione. Per altre informazioni su questo argomento, vedere l'articolo [Creare repository di GitHub](/azure/devops/pipelines/repos/github?view=azure-devops&tabs=yaml).

1. Nell'elenco di repository selezionare il fork del repository creato nell'organizzazione di GitHub.

1. Nel passaggio **Configura la pipeline** scegliere una pipeline YAML esistente come punto di partenza.

    ![Pipeline YAML esistente](media/best-practices-integration-testing/new-pipeline-existing-yaml.png)

1. Quando viene visualizzata la pagina **Seleziona un file YAML esistente** specificare il ramo `master` e immettere il percorso della pipeline YAML: `samples/integration-testing/src/azure-pipeline.yaml`.

    ![Seleziona un file YAML esistente](media/best-practices-integration-testing/select-existing-yaml-pipeline.png)

1. Selezionare **Continua** per caricare la pipeline YAML di Azure da GitHub.

1. Quando viene visualizzata la pagina **Esamina il codice YAML della pipeline**, selezionare **Esegui** per creare e attivare manualmente la pipeline per la prima volta.

    ![Eseguire una pipeline di Azure](media/best-practices-integration-testing/run-pipeline.png)

### <a name="run-the-pipeline"></a>Eseguire la pipeline

È possibile eseguire la pipeline manualmente dall'interfaccia utente di Azure DevOps. L'obiettivo dell'articolo consiste tuttavia nell'illustrare l'integrazione continua automatica. Testare il processo eseguendo il commit di una modifica nella cartella `samples/integration-testing/src` del repository con fork. La modifica attiverà automaticamente una nuova pipeline nel ramo in cui si esegue il push del codice.

![Pipeline in esecuzione da GitHub](media/best-practices-integration-testing/pipeline-running-from-github.png)

Dopo avere completato il passaggio, accedere ai dettagli in Azure DevOps per assicurarsi che non si siano verificati problemi durante l'esecuzione.

![Pipeline verde di Azure DevOps](media/best-practices-integration-testing/azure-devops-green-pipeline.png)

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"]
> [Creare ed eseguire test di conformità nei progetti Terraform](best-practices-compliance-testing.md)