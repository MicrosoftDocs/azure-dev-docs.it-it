---
title: 'Esercitazione: Panoramica dei test di Terraform'
description: Informazioni sulle diverse opzioni di test che è possibile configurare per convalidare i progetti di Terraform.
ms.topic: overview
ms.date: 07/31/2020
ms.custom: devx-track-terraform
adobe-target: true
ms.openlocfilehash: ede7fdfc1dafedacfdc74f657fee0a4ae11feb85
ms.sourcegitcommit: b380f6e637b47e6e3822b364136853e1d342d5cd
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/14/2021
ms.locfileid: "100395326"
---
# <a name="tutorial-terraform-testing-overview"></a>Esercitazione: Panoramica dei test di Terraform

[!INCLUDE [terraform-intro.md](includes/terraform-intro.md)]

Terraform è uno strumento di infrastruttura come codice (IaC, Infrastructure as Code). Questa categoria di strumenti si riferisce al fatto che i file di Terraform vengono considerati allo stesso modo del codice sorgente del progetto. Parte di tale processo include il controllo delle versioni e il controllo del codice sorgente. Inoltre, anche i test devono far parte del processo. Questo articolo fornisce una panoramica dei diversi tipi di test che è possibile eseguire per un progetto di Terraform.

## <a name="integration-testing"></a>Test di integrazione

I test di integrazione verificano che una modifica del codice appena introdotta non interrompa il codice esistente. In DevOps l'integrazione continua (CI) si riferisce a un processo che compila l'intero sistema ogni volta che viene modificata la codebase, ad esempio quando un utente vuole unire una richiesta pull in un repository Git. L'elenco seguente contiene esempi comuni di test di integrazione:

- Strumenti statici di analisi del codice, ad esempio lint e formato.
- Eseguire [terraform validate](https://www.terraform.io/docs/commands/validate.html) per verificare la sintassi del file di configurazione.
- Eseguire [terraform plan](https://www.terraform.io/docs/commands/validate.html) per verificare se la configurazione funziona come previsto.

> [!div class="nextstepaction"]
> [Altre informazioni sui test di integrazione](best-practices-integration-testing.md)

## <a name="unit-testing"></a>Testing unità

Gli unit test garantiscono un comportamento corretto di una parte o di una funzione specifica di un programma. Gli unit test vengono scritti dallo sviluppatore della funzionalità. Questo tipo di test, anche detto sviluppo basato su test, implica cicli di sviluppo brevi e continui. Nel contesto dei progetti di Terraform, il testing unità può essere costituito dall'uso di `terraform plan` per assicurarsi che i valori effettivi disponibili nel piano generato siano uguali ai valori previsti. 

Il testing unità può risultare particolarmente utile quando i moduli di Terraform iniziano a diventare più complessi:

- Generare blocchi dinamici
- Usare cicli
- Calcolare le variabili locali

Come per i test di integrazione, gli unit test vengono spesso inclusi nel processo di integrazione continua.

## <a name="compliance-testing"></a>Test di conformità

I test di conformità consentono di assicurarsi che la configurazione segua i criteri definiti per il progetto. Ad esempio, è possibile definire convenzioni di denominazione geopolitiche per le risorse di Azure. Oppure è possibile decidere che le macchine virtuali debbano essere create da un sottoinsieme definito di immagini. I test di conformità vengono usati per applicare queste regole.

Anche i test di conformità vengono in genere definiti come parte del processo di integrazione continua.

> [!div class="nextstepaction"]
> [Altre informazioni sui test di conformità](best-practices-compliance-testing.md)

## <a name="end-to-end-e2e-testing"></a>Test end-to-end

I test end-to-end verificano che un programma funzioni prima della distribuzione nell'ambiente di produzione. Uno scenario di esempio potrebbe essere un modulo di Terraform che distribuisce due macchine virtuali in una rete virtuale. Si vuole impedire l'esecuzione del ping tra le due macchine virtuali. In questo esempio è possibile definire un test per verificare il risultato previsto prima della distribuzione.

I test end-to-end sono in genere costituiti da un processo in tre passaggi. Prima di tutto, si applica la configurazione a un ambiente di test. Quindi viene eseguito il codice per verificare i risultati. Infine, l'ambiente di test viene reinizializzato o disattivato (ad esempio con la deallocazione di una macchina virtuale).

> [!div class="nextstepaction"]
> [Altre informazioni sui test end-to-end](best-practices-end-to-end-testing.md)

[!INCLUDE [terraform-troubleshooting.md](includes/terraform-troubleshooting.md)]
