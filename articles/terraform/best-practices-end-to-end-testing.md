---
title: 'Esercitazione: Configurare il test Terratest end-to-end nei progetti Terraform'
description: Altre informazioni sul test end-to-end con Terratest in un progetto Terraform.
ms.topic: tutorial
ms.date: 07/31/2020
ms.custom: devx-track-terraform
ms.openlocfilehash: 182d403ed227eca50961e9db2df0d6766c4b9f54
ms.sourcegitcommit: 16ce1d00586dfa9c351b889ca7f469145a02fad6
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/14/2020
ms.locfileid: "88241293"
---
# <a name="tutorial-setup-end-to-end-terratest-testing-on-terraform-projects"></a>Esercitazione: Configurare il test Terratest end-to-end nei progetti Terraform

[!INCLUDE [terraform-intro.md](includes/terraform-intro.md)]

In questo articolo si apprenderà come eseguire le attività seguenti:

> [!div class="checklist"]
> * Informazioni sulle nozioni di base dei test end-to-end con [Terratest](https://github.com/gruntwork-io/terratest)
> * Informazioni su come scrivere test end-to-end con Golang
> * Informazioni su come usare Azure DevOps per attivare automaticamente test end-to-end quando viene eseguito il commit del codice nel repository

[!INCLUDE [hashicorp-support.md](includes/hashicorp-support.md)]

## <a name="prerequisites"></a>Prerequisiti

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../includes/open-source-devops-prereqs-azure-subscription.md)]
- **Installare Terraform**: in base all'ambiente specifico, [scaricare e installare Terraform](https://www.terraform.io/downloads.html).
- **Creare una copia tramite fork degli esempi di test:** per iniziare rapidamente, è consigliabile creare una copia tramite fork di [questo repository](https://github.com/Azure/terraform) nella propria organizzazione GitHub.
- **Linguaggio di programmazione Go**: i test case Terraform sono scritti in [Go](https://golang.org/dl/). L'esempio in questo articolo usa i [moduli Go](https://blog.golang.org/using-go-modules). Per questo articolo è consigliabile usare Go 1.13 (o versione successiva).

## <a name="what-is-end-to-end-testing"></a>Che cos'è il test end-to-end

I test end-to-end verificano che un sistema funzioni come un insieme collettivo. Questo tipo di test è il contrario del test di moduli specifici. Per i progetti Terraform, il test end-to-end consente di verificare gli elementi che sono stati distribuiti. Questo tipo di test è diverso da molti altri tipi che testano gli scenari di pre-distribuzione. I test end-to-end sono fondamentali per il test di sistemi complessi che includono più moduli e operano su più risorse. In questi scenari il test end-to-end è l'unico modo per determinare se i vari moduli interagiscono correttamente.

Questo articolo illustra l'uso di [Terratest](https://github.com/gruntwork-io/terratest) per implementare il test end-to-end. Terratest fornisce tutta l'infrastruttura necessaria per eseguire le attività seguenti:

- Distribuire una configurazione Terraform
- Consente di scrivere un test usando il linguaggio Go per verificare che cosa è stato distribuito
- Orchestrare i test in fasi
- Disinstallare l'infrastruttura distribuita

## <a name="tutorial-scenario"></a>Scenario dell'esercitazione

Per questa esercitazione viene usato un esempio disponibile nel [repository di esempi di Azure/Terraform](https://github.com/Azure/terraform/blob/master/samples/end-to-end-testing/README.md).

Questo esempio definisce una configurazione Terraform che distribuisce due macchine virtuali Linux nella stessa rete virtuale. Una macchina virtuale denominata `vm-linux-1` ha un indirizzo IP pubblico. È aperta solo la porta 22 per consentire le connessioni SSH. La seconda macchina virtuale, `vm-linux-2`, non ha un indirizzo IP pubblico definito.

Il test deve convalidare gli scenari seguenti:

- L'infrastruttura è stata distribuita correttamente
- Usando la porta 22, è possibile aprire una sessione SSH per `vm-linux-1`
- Usando la sessione SSH in `vm-linux-1`, è possibile effettuare il ping di `vm-linux-2`

![Scenario di test end-to-end di esempio](media/best-practices-end-to-end-testing/scenario.png)

Se è stato [scaricato l'esempio](#prerequisites), la configurazione Terraform per questo scenario si trova nel file `src/main.tf`. Il file contiene tutti gli elementi necessari per distribuire l'infrastruttura di Azure rappresentata nella figura precedente.

Se non si ha familiarità con la creazione di macchine virtuali, vedere [Creare una VM Linux con infrastruttura in Azure tramite Terraform](create-linux-virtual-machine-with-infrastructure.md).

> [!CAUTION]
> Lo scenario di esempio presentato in questo articolo è esclusivamente a scopo illustrativo. Si tratta di uno scenario estremamente semplice, al fine di mantenere la concentrazione sui passaggi del test end-to-end. Non è consigliabile usare macchine virtuali di produzione che espongono le porte SSH tramite un indirizzo IP pubblico.

## <a name="end-to-end-test"></a>Test end-to-end

Il test end-to-end è scritto nel linguaggio Go e usa il framework Terratest. Se è stato [scaricato l'esempio](#prerequisites), è definito nel file `src/test/end2end_test.go`.

Il codice sorgente seguente mostra la struttura standard di un test Golang usando Terratest:

```Go
package test

import (
    "testing"

    "github.com/gruntwork-io/terratest/modules/terraform"
    test_structure "github.com/gruntwork-io/terratest/modules/test-structure"
)

func TestEndToEndDeploymentScenario(t *testing.T) {
    t.Parallel()

    fixtureFolder := "../"

    // User Terratest to deploy the infrastructure
    test_structure.RunTestStage(t, "setup", func() {
        terraformOptions := &terraform.Options{
            // Indicate the directory that contains the Terraform configuration to deploy
            TerraformDir: fixtureFolder,
        }

        // Save options for later test stages
        test_structure.SaveTerraformOptions(t, fixtureFolder, terraformOptions)

        // Triggers the terraform init and terraform apply command
        terraform.InitAndApply(t, terraformOptions)
    })

    test_structure.RunTestStage(t, "validate", func() {
        // run validation checks here
        terraformOptions := test_structure.LoadTerraformOptions(t, fixtureFolder)
            publicIpAddress := terraform.Output(t, terraformOptions, "public_ip_address")
    })

    // When the test is completed, teardown the infrastructure by calling terraform destroy
    test_structure.RunTestStage(t, "teardown", func() {
        terraformOptions := test_structure.LoadTerraformOptions(t, fixtureFolder)
        terraform.Destroy(t, terraformOptions)
    })
}
```

Come si può notare nel frammento di codice precedente, il test è composto da tre fasi:

- **setup**: esegue Terraform per distribuire la configurazione
- **validate**: esegue i controlli di convalida e le asserzioni
- **teardown**: pulisce l'infrastruttura dopo l'esecuzione del test

Nell'elenco seguente vengono illustrate alcune delle funzioni principali fornite dal framework Terratest:

- **terraform.InitAndApply**: abilita l'esecuzione di `terraform init` e `terraform apply` dal codice Go
- **terraform.Output**: recupera il valore della variabile di output della distribuzione.
- **terraform.Destroy**: esegue il comando `terraform destroy` dal codice Go.
- **test_structure.LoadTerraformOptions**: carica le opzioni di Terraform, ad esempio la configurazione e le variabili, dallo stato
- **test_structure.SaveTerraformOptions**: salva le opzioni di Terraform, ad esempio la configurazione e le variabili, nello stato

## <a name="run-the-end-to-end-test"></a>Eseguire il test end-to-end

In questa sezione viene eseguito il test sulla configurazione e la distribuzione di esempio. 

1. Aprire una finestra Bash o del terminale.

1. Accedere all'account Azure. Per informazioni su come accedere alla sottoscrizione di Azure, vedere la sezione [Autenticazione di Azure](get-started-cloud-shell.md#authenticate-to-azure).

1. Per eseguire questo test di esempio, è necessario un nome di coppia di chiavi privata/pubblica SSH `id_rsa` e `id_rsa.pub` nella directory principale. Sostituire `USER` con il nome della directory principale.

```bash
export TEST_SSH_KEY_PATH="/home/USER/.ssh/id_rsa"
```

1. Passare alla directory `test`.

1. Eseguire il test.

```go
go test -v ./ -timeout 10m
```

Il test visualizzerà i risultati con un output simile al seguente:

```output
--- PASS: TestEndToEndDeploymentScenario (390.99s)
PASS
ok      test    391.052s
```

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"]
> [Panoramica dei test di Terraform](best-practices-testing-overview.md)