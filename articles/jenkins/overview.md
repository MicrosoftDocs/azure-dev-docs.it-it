---
title: Panoramica di Jenkins e Azure
description: Ospitare il server di automazione di compilazione e distribuzione Jenkins in Azure e usare le risorse di calcolo e archiviazione di Azure per estendere le pipeline di integrazione continua e distribuzione continua (CI/CD).
keywords: jenkins, azure, devops, panoramica
ms.topic: overview
ms.date: 08/08/2020
ms.custom: devx-track-jenkins
ms.openlocfilehash: 43e38bd609f00257859c5976dc0b4e0fa4ae9e8f
ms.sourcegitcommit: 16ce1d00586dfa9c351b889ca7f469145a02fad6
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/14/2020
ms.locfileid: "88240633"
---
# <a name="azure-and-jenkins"></a>Azure e Jenkins

[Jenkins](https://jenkins.io/) è un server di automazione open source molto diffuso usato per configurare l'integrazione continua e la distribuzione continua per i progetti software. È possibile ospitare la distribuzione di Jenkins in Azure oppure estendere la configurazione di Jenkins esistente usando le risorse di Azure. Sono anche disponibili plug-in di Jenkins per semplificare l'integrazione continua e la distribuzione continua delle applicazioni in Azure.

Questo articolo offre un'introduzione all'uso di Azure con Jenkins, con dettagli delle principali funzionalità di Azure disponibili per gli utenti di Jenkins. Per altre informazioni sulle operazioni preliminari con il server Jenkins in Azure, vedere [Creare un server Jenkins in Azure](configure-on-linux-vm.md).

## <a name="host-your-jenkins-servers-in-azure"></a>Ospitare i server Jenkins in Azure

Ospitare Jenkins in Azure per centralizzare l'automazione della compilazione e ridimensionare la distribuzione con l'aumentare dei requisiti dei progetti software. Per informazioni su come installare e configurare Jenkins in una VM Linux, vedere [Avvio rapido - Introduzione a Jenkins](configure-on-linux-vm.md). Monitorare e gestire la distribuzione di Jenkins di Azure tramite i [log di Monitoraggio di Azure](/azure/log-analytics/log-analytics-overview) e l'[interfaccia della riga di comando di Azure](/cli/azure).

## <a name="scale-your-build-automation-on-demand"></a>Ridimensionare l'automazione della compilazione su richiesta

Aggiungere agenti di compilazione alla distribuzione di Jenkins esistente per ridimensionare la capacità di compilazione di Jenkins con l'aumentare del numero di compilazioni e della complessità di processi e pipeline. È possibile eseguire questi agenti di compilazione in macchine virtuali di Azure tramite il [plug-in degli agenti di macchine virtuali di Azure](https://plugins.jenkins.io/azure-vm-agents). Vedere l'[esercitazione ](/azure/jenkins/jenkins-azure-vm-agents) per altri dettagli.

Una volta configurati con un'[entità servizio di Azure](/azure/azure-resource-manager/resource-group-overview), i processi e le pipeline di Jenkins possono usare questa credenziale per:

- Archiviare in modo sicuro gli artefatti della compilazione in [Archiviazione di Azure](/azure/storage/common/storage-introduction) usando il [plug-in di Archiviazione di Azure](https://plugins.jenkins.io/windows-azure-storage). Vedere la [procedura di archiviazione Jenkins](azure-storage-blobs-as-build-artifact-repository.md) per altre informazioni.
- Gestire e configurare le risorse di Azure con l'[interfaccia della riga di comando di Azure](deploy-to-azure-app-service-using-azure-cli.md).

## <a name="deploy-your-code-into-azure-services"></a>Distribuire il codice nei servizi di Azure

Usare i plug-in di Jenkins per distribuire le applicazioni in Azure nell'ambito delle pipeline di integrazione continua e distribuzione continua. La distribuzione in [Servizio app di Azure](/azure/app-service/) e nel [servizio Azure Container](/azure/container-service/kubernetes/) consente di inserire temporaneamente, testare e rilasciare aggiornamenti delle applicazioni senza gestire l'infrastruttura sottostante.

 Sono disponibili plug-in per la distribuzione nei servizi e negli ambienti seguenti:

- [Servizio app di Azure in Linux](/azure/app-service/containers/app-service-linux-intro). Vedere l'[esercitazione](deploy-from-github-to-azure-app-service.md) per iniziare.
- [Servizio app di Azure](/azure/app-service/overview). Vedere la [procedura](deploy-to-azure-app-service-using-plugin.md) per iniziare.