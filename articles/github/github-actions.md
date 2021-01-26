---
title: Che cos'è la funzionalità GitHub Actions per Azure?
description: Creare flussi di lavoro all'interno del repository per compilare, testare, assemblare, rilasciare e distribuire software in Azure.
author: N-Usha
ms.author: ushan
ms.topic: conceptual
ms.service: azure
ms.date: 10/30/2020
ms.custom: github-actions-azure
ms.openlocfilehash: 7309bd16cdecf8b148b89eb40649864590b552a6
ms.sourcegitcommit: 8eb1c379b2bbc2acdd82fc9d24d8ed948e5a6847
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/26/2021
ms.locfileid: "98811090"
---
# <a name="what-is-github-actions-for-azure"></a>Che cos'è la funzionalità GitHub Actions per Azure?

[GitHub Actions](https://docs.github.com/en/free-pro-team@latest/actions) consente di automatizzare i flussi di lavoro dello sviluppo di software all'interno di GitHub. È possibile distribuire i flussi di lavoro nella stessa posizione in cui si archivia il codice, oltre a collaborare alle richieste pull e ai problemi.

In GitHub Actions un [flusso di lavoro](https://docs.github.com/en/free-pro-team@latest/actions/learn-github-actions/introduction-to-github-actions) è un processo automatizzato che si configura nel repository GitHub. Con un flusso di lavoro è possibile compilare, testare, assemblare o distribuire qualsiasi progetto in GitHub.

Ogni flusso di lavoro è costituito da singole [azioni](https://docs.github.com/en/actions/learn-github-actions/introduction-to-github-actions) eseguite dopo un evento specifico, ad esempio una richiesta pull.  Le singole azioni sono script inseriti in un pacchetto che automatizzano le attività di sviluppo di software.

Con GitHub Actions per Azure è possibile creare flussi di lavoro configurabili nel repository per compilare, testare, assemblare, rilasciare e distribuire software in Azure. GitHub Actions per Azure supporta servizi di Azure come il servizio app di Azure, Funzioni di Azure e Azure Key Vault.

GitHub Actions include anche il supporto per utilità come i modelli di Azure Resource Manager, l'interfaccia della riga di comando di Azure e Criteri di Azure.

Guardare questo video di GitHub Universe 2020 per altre informazioni sulla distribuzione continua con GitHub Actions.  

> [!VIDEO https://www.youtube.com/embed/36hY0-O4STg]

## <a name="why-should-i-use-github-actions-for-azure"></a>Perché usare GitHub Actions per Azure?

Le azioni di GitHub Actions sono sviluppate da Microsoft e progettate per essere usate con Azure. È possibile visualizzare tutte le azioni di GitHub Actions in [GitHub Marketplace](https://github.com/marketplace?query=Azure&type=actions). Per altre informazioni su come incorporare le azioni nei flussi di lavoro, vedere [Ricerca e personalizzazione di azioni](https://docs.github.com/en/actions/learn-github-actions/finding-and-customizing-actions).

## <a name="what-is-the-difference-between-github-actions-and-azure-pipelines"></a>Qual è la differenza tra GitHub Actions e Azure Pipelines?

Azure Pipelines e GitHub Actions consentono di automatizzare i flussi di lavoro di sviluppo di software. Per altre informazioni sulle differenze tra i servizi e su come eseguire la migrazione da Azure Pipelines a GitHub Actions, vedere [qui](https://docs.github.com/en/actions/learn-github-actions/migrating-from-azure-pipelines-to-github-actions).

## <a name="what-do-i-need-to-use-github-actions-for-azure"></a>Quali sono i requisiti per usare GitHub Actions per Azure?

Sono necessari account Azure e GitHub:

* Un account Azure con una sottoscrizione attiva. [Creare un account gratuitamente](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Un account GitHub. Se non è disponibile, iscriversi per riceverne uno [gratuito](https://github.com/join).  

## <a name="how-do-i-connect-github-actions-and-azure"></a>Come si connette GitHub Actions ad Azure?

A seconda dell'azione, si userà un'entità servizio o un profilo di pubblicazione per connettersi ad Azure da GitHub. Si userà un'entità servizio ogni volta che si usa l'azione di [accesso di Azure](https://github.com/marketplace/actions/azure-login). L'azione del [servizio app di Azure](https://github.com/marketplace/actions/azure-webapp) supporta l'uso di un profilo di pubblicazione o di un'entità servizio. Per altre informazioni sulle entità servizio, vedere [Oggetti applicazione e oggetti entità servizio in Azure Active Directory](/azure/active-directory/develop/app-objects-and-service-principals#service-principal-object).  

È possibile usare l'azione di accesso di Azure in combinazione con le azioni dell'[interfaccia della riga di comando di Azure](https://github.com/marketplace/actions/azure-cli-action) e di [Azure PowerShell](https://github.com/marketplace/actions/azure-powershell-action). L'azione di accesso di Azure funziona anche con la maggior parte delle altre azioni di GitHub per Azure, tra cui la [distribuzione in app Web](https://github.com/marketplace/actions/azure-webapp) e l'[accesso ai segreti dell'insieme di credenziali delle chiavi](https://github.com/marketplace/actions/azure-key-vault-get-secrets).

## <a name="what-is-included-in-a-github-actions-workflow"></a>Che cosa è incluso in un flusso di lavoro di GitHub Actions?

I flussi di lavoro sono costituiti da uno o più processi. Un processo include passaggi costituiti da singole azioni. Per altre informazioni sui concetti relativi a GitHub Actions, vedere [Introduzione a GitHub Actions](https://docs.github.com/en/actions/learn-github-actions/introduction-to-github-actions).  

## <a name="where-can-i-see-complete-workflow-examples"></a>Dove è possibile trovare esempi completi di flussi di lavoro?

Il [repository di flussi di lavoro di azioni di avvio di Azure](https://github.com/Azure/actions-workflow-samples) include flussi di lavoro end-to-end per creare e distribuire app Web in qualsiasi linguaggio e qualsiasi ecosistema in Azure.

## <a name="where-can-i-see-all-the-available-actions"></a>Dove è possibile vedere tutte le azioni disponibili?

Per vedere tutte le azioni di GitHub Actions disponibili per Azure, visitare il [marketplace per GitHub Actions per Azure](https://github.com/marketplace?query=Azure&type=actions).

* [Distribuire in un'app Web statica](/azure/static-web-apps/getting-started?tabs=angular)
* [Impostazioni di Servizio app di Azure](https://github.com/Azure/appservice-settings)  
* [Distribuire in Funzioni di Azure](https://github.com/Azure/functions-action)  
* [Distribuire in Funzioni di Azure per contenitori](https://github.com/Azure/webapps-container-deploy)  
* [Accesso a Docker](https://github.com/Azure/docker-login)  
* [Eseguire la distribuzione in Istanze di Azure Container](https://github.com/Azure/aci-deploy)
* [Azione di analisi dei contenitori](https://github.com/Azure/container-scan)
* [Programma di installazione dello strumento Kubectl](https://github.com/Azure/setup-kubectl)  
* [Impostazione del contesto di Kubernetes](https://github.com/Azure/k8s-set-context)  
* [Impostazione del contesto del servizio Azure Kubernetes](https://github.com/Azure/aks-set-context)  
* [Creazione del segreto Kubernetes](https://github.com/Azure/k8s-create-secret)  
* [Distribuzione di Kubernetes](https://github.com/Azure/k8s-deploy)  
* [Installazione di Helm](https://github.com/Azure/setup-helm)  
* [Bake di Kubernetes](https://github.com/Azure/k8s-bake)  
* [Creare immagini di macchine virtuali di Azure](https://github.com/Azure/build-vm-image)
* [Accesso a Machine Learning](https://github.com/Azure/aml-workspace)
* [Training di Machine Learning](https://github.com/Azure/aml-run)
* [Distribuzione di un modello di Machine Learning](https://github.com/Azure/aml-deploy)
* [Distribuire in Database SQL di Azure](https://github.com/Azure/sql-action)  
* [Distribuire in un'azione MySQL di Azure](https://github.com/Azure/mysql-action)  
* [Analisi della conformità di Criteri di Azure](https://github.com/Azure/policy-compliance-scan)
* [Gestire Criteri di Azure](https://github.com/Azure/manage-azure-policy)
* [Attivare un'esecuzione in Azure Pipelines](https://github.com/Azure/pipelines)  
* [Sostituzione di variabili](https://github.com/Microsoft/variable-substitution)

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"]
> [Percorso di apprendimento: Automatizzare il flusso di lavoro con GitHub Actions](/learn/modules/github-actions-automate-tasks/)

> [!div class="nextstepaction"]
> [Laboratorio di formazione - Recapito continuo con Azure](https://lab.github.com/githubtraining/github-actions:-continuous-delivery-with-azure)