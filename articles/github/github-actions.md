---
title: Distribuire in Azure con GitHub Actions
description: Creare flussi di lavoro all'interno del repository per compilare, testare, creare un pacchetto, rilasciare e distribuire in Azure.
ms.author: kaelli
author: KathrynEE
ms.topic: conceptual
ms.service: azure
ms.date: 05/05/2020
ms.openlocfilehash: 16ebc2d8013b2199911c0648f4bb8364c4d6de06
ms.sourcegitcommit: a631b36ec1277ee9397a860c597ffdd5495d88e7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/13/2020
ms.locfileid: "83369928"
---
# <a name="deploy-to-azure-using-github-actions"></a>Distribuire in Azure con GitHub Actions

Con GitHub Actions per Azure è possibile automatizzare i flussi di lavoro per la distribuzione in Azure. Per iniziare, vedere le azioni di GitHub Actions seguenti: 

- [Azure/webapps-deploy](https://github.com/Azure/webapps-deploy), per eseguire la distribuzione in App Web di Azure e nell'app Web per contenitori di Azure. 
- [Azure/appservice-settings](https://github.com/Azure/appservice-settings), per configurare in blocco le impostazioni dell'app, le stringhe di connessione e altre impostazioni generali usando la sintassi JSON in App Web di Azure (Windows o Linux) o uno qualsiasi dei relativi slot di distribuzione.

> [!NOTE]   
> I collegamenti forniti in questo articolo si riferiscono a un articolo di GitHub o a un repository GitHub. 

## <a name="key-concepts"></a>Concetti chiave

Con GitHub Actions è possibile creare flussi di lavoro personalizzati del ciclo di vita di sviluppo software (SDLC) direttamente nel repository GitHub. Per una panoramica di GitHub Actions e dei concetti di base, vedere gli articoli seguenti: 

- [Informazioni su GitHub Actions](https://help.github.com/actions/getting-started-with-github-actions/about-github-actions)
- [Concetti di base](https://help.github.com/actions/getting-started-with-github-actions/core-concepts-for-github-actions)
- [Informazioni sulla creazione di pacchetti con GitHub Actions](https://help.github.com/en/actions/publishing-packages-with-github-actions/about-packaging-with-github-actions)

## <a name="get-started"></a>Introduzione 

GitHub Actions include modelli preconfigurati e azioni di Marketplace. 

- [Usare modelli preconfigurati](https://help.github.com/actions/getting-started-with-github-actions/starting-with-preconfigured-workflow-templates)  
- [Usare azioni di GitHub Marketplace](https://help.github.com/en/actions/getting-started-with-github-actions/using-actions-from-github-marketplace)  
- [Azioni di GitHub Marketplace: distribuzione in Azure](https://github.com/marketplace?type=actions&query=Azure)  
  
Per i collegamenti a tutte le azioni di GitHub Actions per Azure, vedere la pagina seguente: 
   
- [Azioni di Azure](https://github.com/marketplace?query=Azure&type=actions)  

## <a name="connect-to-azure"></a>Connettersi ad Azure

Per i flussi di lavoro di esempio per la connessione ad Azure, vedere le azioni di GitHub seguenti:  

- [Accesso ad Azure](https://github.com/Azure/login)  
- [Interfaccia della riga di comando di Azure](https://github.com/Azure/CLI)  


## <a name="starter-templates-and-end-to-end-cicd-workflow-samples"></a>Modelli di base ed esempi di flusso di lavoro CI/CD end-to-end 

Gli esempi seguenti forniscono flussi di lavoro end-to-end per la distribuzione di app Web in Azure. 

- [Distribuire un'app Web con supporto ASP.NET](https://github.com/Azure-Samples/dotnet-sample)  
- [Distribuire un'app ASP.NET Core](https://github.com/Azure-Samples/dotnet_core_sample)  
- [Distribuire un'app Web Node.js](https://github.com/Azure-Samples/node_express_app)  
- [Distribuire un'app Web Java](https://github.com/Azure-Samples/java-spring-petclinic)  
- [Distribuire un'app Java Spring](https://github.com/Azure-Samples/Java-application-petstore-ee7)  
- [Distribuire un'app Web Python](https://github.com/Azure-Samples/pythonSample_thecatsaidno)  
- [Eseguire la distribuzione con Docker](https://github.com/Azure-Samples/Node_express_container)  


## <a name="deploy-a-web-app"></a>Distribuire un'app Web

- [App Web di Azure](https://github.com/Azure/webapps-deploy)  
- [App Web per contenitori di Azure](https://github.com/Azure/webapps-container-deploy)  
- [Impostazioni di Servizio app di Azure](https://github.com/Azure/appservice-settings)  

## <a name="deploy-a-serverless-app"></a>Distribuire un'app serverless

- [Funzioni di Azure](https://github.com/Azure/functions-action)  
- [Funzioni di Azure per contenitori](https://github.com/Azure/webapps-container-deploy)  
 
## <a name="build-and-deploy-containerized-apps"></a>Creare e distribuire app in contenitori

- [Accesso a Docker](https://github.com/Azure/docker-login)  

## <a name="deploy-to-kubernetes"></a>Eseguire la distribuzione in Kubernetes

- [Programma di installazione dello strumento Kubectl](https://github.com/Azure/setup-kubectl)  
- [Impostazione del contesto di Kubernetes](https://github.com/Azure/k8s-set-context)  
- [Impostazione del contesto del servizio Azure Kubernetes](https://github.com/Azure/aks-set-context)  
- [Creazione del segreto Kubernetes](https://github.com/Azure/k8s-create-secret)  
- [Distribuzione di Kubernetes](https://github.com/Azure/k8s-deploy)  
- [Installazione di Helm](https://github.com/Azure/setup-helm)  
- [Bake di Kubernetes](https://github.com/Azure/k8s-bake)  

## <a name="train-and-deploy-a-machine-learning-model"></a>Eseguire il training e distribuire un modello di Machine Learning 

- [Accesso](https://github.com/Azure/aml-workspace) 
- [Esecuzione del training](https://github.com/Azure/aml-run)
- [Distribuzione del modello](https://github.com/Azure/aml-deploy)

## <a name="deploy-to-databases"></a>Connettersi ai database

- [Database SQL di Azure](https://github.com/Azure/sql-action)  
- [Azione MySQL di Azure](https://github.com/Azure/mysql-action)  

## <a name="trigger-a-run-in-azure-pipelines"></a>Attivare un'esecuzione in Azure Pipelines

- [Azure Pipelines](https://github.com/Azure/pipelines)  
 
## <a name="utility-actions"></a>Azioni di utilità

- [Sostituzione di variabili](https://github.com/Microsoft/variable-substitution) 


## <a name="additional-resources"></a>Risorse aggiuntive

È possibile usare le risorse GitHub seguenti per facilitare l'uso di GitHub per la distribuzione delle app in Azure.  

- [Marketplace di GitHub Actions per Azure](https://github.com/marketplace?query=Azure&type=actions)
- [Laboratorio di formazione - Recapito continuo con Azure](https://lab.github.com/githubtraining/github-actions:-continuous-delivery-with-azure)
- [Flussi di lavoro azione di base per la distribuzione in Azure](https://github.com/Azure/actions-workflow-samples)
