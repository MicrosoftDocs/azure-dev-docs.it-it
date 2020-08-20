---
title: Esercitazione - Test di conformità con Terraform e Azure
description: Informazioni su come applicare il test di conformità dello stile di sviluppo basato su comportamento (BDD) alle configurazioni di Terraform
ms.topic: tutorial
ms.date: 07/31/2020
ms.custom: devx-track-terraform
ms.openlocfilehash: 7abb4072d923d4d5ec4fa3df6251f07576dba3bc
ms.sourcegitcommit: 16ce1d00586dfa9c351b889ca7f469145a02fad6
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/14/2020
ms.locfileid: "88241313"
---
# <a name="tutorial-compliance-testing-with-terraform-and-azure"></a>Esercitazione: Test di conformità con Terraform e Azure

[!INCLUDE [terraform-intro.md](includes/terraform-intro.md)]

In questo articolo si apprenderà come eseguire le attività seguenti:

> [!div class="checklist"]
> * Quando usare i test di conformità
> * Come eseguire un controllo di conformità

[!INCLUDE [hashicorp-support.md](includes/hashicorp-support.md)]

## <a name="prerequisites"></a>Prerequisiti

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../includes/open-source-devops-prereqs-azure-subscription.md)]
- **Installare Terraform**: in base all'ambiente specifico, [scaricare e installare Terraform](https://www.terraform.io/downloads.html).
- **Docker**: [Installare Docker](https://docs.docker.com/get-docker/)
- **Terraform-compliance**: [Installare lo strumento di conformità Terraform](https://terraform-compliance.com/pages/installation/docker).
- **Creare una copia tramite fork degli esempi di test**: creare una copia tramite fork del [progetto di esempio di Terraform in GitHub](https://github.com/Azure/terraform) e clonarlo nel computer di sviluppo/test.

## <a name="what-is-compliance-testing"></a>Che cos'è il test di conformità

Il test di conformità è una tecnica di test non funzionale per determinare se un sistema soddisfa gli standard prestabiliti. Il test di conformità è anche noto come *verifica della conformità*.

La maggior parte dei team software esegue un'analisi per verificare che gli standard siano imposti e implementati correttamente. Spesso lavorano simultaneamente per migliorare gli standard che, a loro volta, generano una qualità più elevata.

Il test di conformità garantisce che l'output di ogni fase del ciclo di vita di sviluppo sia conforme ai requisiti concordati.

I controlli di conformità devono essere integrati nel ciclo di sviluppo all'inizio dei progetti. Il tentativo di aggiungere i controlli di conformità in una fase successiva diventa sempre più difficile quando il requisito stesso non è documentato in modo adeguato.

## <a name="understanding-compliance-checks"></a>Informazioni sui controlli di conformità

L'esecuzione dei controlli di conformità è un'operazione semplice. Un set di standard e procedure viene sviluppato e documentato per ogni fase del ciclo di vita di sviluppo. L'output di ogni fase viene confrontato con i requisiti documentati. I risultati del test sono costituiti dagli eventuali "scostamenti" non conformi agli standard predeterminati. Il test di conformità viene eseguito tramite il processo di ispezione e il risultato del processo di revisione deve essere documentato.

Ora verrà esaminato un esempio specifico.

Un problema comune è costituito dall'interruzione degli ambienti quando più sviluppatori applicano modifiche incompatibili. Si supponga che uno sviluppatore stia implementando una modifica e applichi risorse quali la creazione di una macchina virtuale in un ambiente di test. Un altro sviluppatore applica quindi una versione diversa del codice che effettua il provisioning di una versione diversa di tale macchina virtuale. In questo caso serve una supervisione per garantire la conformità alle regole definite.

Un modo per risolvere questo problema consiste nel definire un criterio di assegnazione di tag alle risorse, ad esempio con i tag `role` e `creator`. Dopo aver definito i criteri, viene usato uno strumento come [Terraform-compliance](https://terraform-compliance.com) per garantire che i criteri vengano seguiti.

Terraform-compliance esegue il *test negativo*. Il test negativo è il processo che garantisce che un sistema possa gestire normalmente un input imprevisto o un comportamento indesiderato. Il *test con dati casuali* è un esempio di test negativo. Con il test con dati casuali, viene testato un sistema che riceve input per assicurarsi che possa gestire in modo sicuro l'input imprevisto.

Fortunatamente, Terraform è un livello di astrazione per qualsiasi API che crei, aggiorni o elimini entità dell'infrastruttura cloud. Terraform garantisce inoltre che la configurazione locale e le risposte API remote siano sincronizzate. Poiché Terraform viene usato principalmente per le API cloud, è comunque necessario un modo per garantire che il codice distribuito nell'infrastruttura segua criteri specifici. Terraform-compliance, uno strumento gratuito e open source, fornisce questa funzionalità per le configurazioni di Terraform.

Usando l'esempio relativo alla macchina virtuale, i criteri di conformità potrebbero essere i seguenti: *"Se si sta creando una risorsa di Azure, deve contenere un tag"* .

Lo strumento Terraform-compliance fornisce un framework di test in cui è possibile creare criteri come quello illustrato nell'esempio. Questi criteri vengono quindi eseguiti nel piano di esecuzione di Terraform.

Terraform-compliance consente di applicare i principi di *sviluppo basato su comportamento* o BDD. Lo sviluppo basato su comportamento è un processo di collaborazione in cui tutti gli stakeholder cooperano insieme per definire le operazioni che un sistema deve eseguire. Tali stakeholder includono in genere gli sviluppatori, i tester e tutti coloro che hanno un interesse specifico o che saranno interessati dal sistema in fase di sviluppo. Lo sviluppo basato su comportamento ha l'obiettivo di incoraggiare i team a creare esempi concreti che esprimono principi comuni sulle modalità di comportamento del sistema.

## <a name="looking-at-an-example"></a>Osservare un esempio

In precedenza in questo articolo è stato descritto un esempio di test di conformità relativo alla creazione di una macchina virtuale per un ambiente di test. In questa sezione viene illustrato come convertire questo esempio in una funzionalità e in uno scenario di sviluppo basato su comportamento. La regola viene innanzitutto espressa con *Cucumber*, che è uno strumento usato per supportare lo sviluppo basato su comportamento.

```Cucumber
when creating Azure resources, every new resource should have a tag
```

La regola precedente viene convertita come indicato di seguito:

```Cucumber
If the resource supports tags
Then it must contain a tag
And its value must not be null
```

Il codice HCL Terraform aderirebbe alla regola, come indicato di seguito.

```hcl
resource "random_uuid" "uuid" {}

resource "azurerm_resource_group" "rg" {
  name     = "rg-hello-tf-${random_uuid.uuid.result}"
  location = var.location

  tags = {
    environment = "dev"
    application = "Azure Compliance"
  } 
}
```

Il primo criterio può essere scritto come uno [scenario di funzionalità BDD](https://cucumber.io/docs/gherkin/reference/) come indicato di seguito:

```Cucumber
Feature: Test tagging compliance  # /target/src/features/tagging.feature
    Scenario: Ensure all resources have tags
        If the resource supports tags
        Then it must contain a tag
        And its value must not be null
```

Il codice seguente mostra un test per un tag specifico:

```Cucumber
Scenario Outline: Ensure that specific tags are defined
    If the resource supports tags
    Then it must contain a tag <tags>
    And its value must match the "<value>" regex

    Examples:
      | tags        | value              |
      | Creator     | .+                 |
      | Application | .+                 |
      | Role        | .+                 |
      | Environment | ^(prod\|uat\|dev)$ |
```

## <a name="running-the-sample"></a>Esecuzione dell'esempio

In questa sezione verrà scaricato e testato l'esempio.

1. [Scaricare l'esempio di test di conformità](https://github.com/Azure/terraform/tree/master/samples/compliance-testing).

1. Passare alla directory `src`.

1. Eseguire [terraform init](https://www.terraform.io/docs/commands/init.html) per inizializzare la directory di lavoro. Questo passaggio scarica i moduli di Azure necessari per creare un gruppo di risorse di Azure.

    ```bash
    terraform init
    ```
    
1. Eseguire [terraform validate](https://www.terraform.io/docs/commands/validate.html) per convalidare la sintassi dei file di configurazione.

    ```bash
    terraform validate
    ```
    
1. Eseguire [terraform plan](https://www.terraform.io/docs/commands/plan.html) per creare un piano di esecuzione.

    ```bash
    terraform plan -out tf.out
    ```
    
1. Eseguire [terraform apply](https://www.terraform.io/docs/commands/apply.html) per applicare il piano di esecuzione.

    ```bash
    terraform apply -target=random_uuid.uuid
    ```
    
1. Eseguire [docker pull](https://docs.docker.com/engine/reference/commandline/pull/) per scaricare l'immagine di terraform-compliance.

    ```bash
    docker pull eerkunt/terraform-compliance
    ```
    
1. Eseguire [docker run](https://docs.docker.com/engine/reference/commandline/run/) per eseguire i test in un contenitore Docker. **Il test avrà esito negativo**. La prima regola che richiede l'esistenza dei tag ha esito positivo. Tuttavia, la seconda regola ha esito negativo in quanto mancano i tag `Role` e `Creator`.

    ```bash
    docker run --rm -v $PWD:/target -it eerkunt/terraform-compliance -f features -p tf.out
    ```
    
    ![Esempio di test non superato](media/best-practices-compliance-testing/best-practices-compliance-testing-tagging-fail.png)

1. Modificare `main.tf` come indicato di seguito per correggere l'errore.

    ```terraform
      tags = {
        Environment = "dev"
        Application = "Azure Compliance"
        Creator     = "Azure Compliance"
        Role        = "Azure Compliance"
      } 
    
    ```
    
1. Eseguire di nuovo `terraform validate` per verificare la sintassi.

    ```bash
    terraform validate
    ```
    
1. Eseguire di nuovo `terraform plan` per creare un nuovo piano di esecuzione.

    ```bash
    terraform plan -out tf.out
    ```
    
1. Eseguire di nuovo [docker run](https://docs.docker.com/engine/reference/commandline/run/) per testare la configurazione. Questa volta il test ha esito positivo perché è stata implementata la specifica completa.

    ```bash
    docker run --rm -v $PWD:/target -it eerkunt/terraform-compliance -f features -p tf.out
    ```

    ![Esempio di test superato](media/best-practices-compliance-testing/best-practices-compliance-testing-tagging-succeed.png)

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"]
> [Creare ed eseguire test end-to-end in progetti Terraform](best-practices-end-to-end-testing.md)
