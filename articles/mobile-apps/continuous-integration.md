---
title: Automatizzare il ciclo di vita delle app con Visual Studio App Center e i servizi di Azure
description: Informazioni sui servizi come App Center che consentono di configurare la compilazione e l'integrazione continue per le applicazioni per dispositivi mobili.
author: codemillmatt
ms.assetid: 34a8a070-9b3c-4faf-8588-ccff02097224
ms.service: mobile-services
ms.topic: article
ms.date: 06/08/2020
ms.author: masoucou
ms.openlocfilehash: 82ce8246b916549d2207a10d96de89ad26cde49c
ms.sourcegitcommit: 3d906f265b748fbc0a070fce252098675674c8d9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/22/2021
ms.locfileid: "98699889"
---
# <a name="automate-the-lifecycle-of-your-apps-with-continuous-build-and-integration"></a>Automatizzare il ciclo di vita delle app con compilazione e integrazione continue

Gli sviluppatori scrivono codice e lo archiviano nel repository del codice, ma i commit archiviati nel repository potrebbero non essere sempre coerenti. Quando più sviluppatori collaborano allo stesso progetto, è possibile che si verifichino problemi di integrazione. I team possono trovarsi in situazioni in cui il codice non funziona come previsto, i bug si accumulano e lo sviluppo del progetto viene ritardato. Gli sviluppatori devono quindi attendere che l'intero codice software venga compilato e testato per verificare la presenza di errori, rallentando così il processo e riducendone l'iteratività. 

La compilazione continua e l'integrazione continua consentono agli sviluppatori di semplificare le compilazioni e testare il codice tramite il commit delle modifiche nel repository del codice sorgente e collocando i test e le verifiche nell'ambiente di compilazione. In questo modo, eseguono sempre test sul codice. Tutte le modifiche apportate al codice sorgente vengono compilate continuamente ogni volta che viene eseguito un commit nel repository. A ogni archiviazione, il server di integrazione continua convalida ed esegue tutti i test creati dallo sviluppatore. Se i test non vengono superati, il codice viene inviato di nuovo allo sviluppatore per ulteriori modifiche. In questo modo, gli sviluppatori non interrompono le compilazioni create e non devono eseguire tutti i test localmente nei propri computer, aumentando così la produttività. 

## <a name="key-benefits"></a>Vantaggi principali

- Automatizzare compilazioni, test e distribuzioni per le pipeline.
- Rilevare di bug e correggere tempestivamente i problemi per garantire velocità di rilascio più rapide.
- Eseguire il commit del codice con maggiore frequenza e compilare rapidamente le applicazioni.
- Usufruire della flessibilità per modificare rapidamente il codice senza problemi.
- Accelerare il time-to-market affinché solo il codice di qualità elevata proceda alle fasi successive.
- Apportare modifiche ridotte al codice più efficienti grazie alla possibilità di integrare in una volta sola piccoli frammenti di codice.
- Aumentare la trasparenza e la responsabilità del team in modo da ottenere feedback continuo dai clienti e dal team.

Usare i servizi seguenti per abilitare una pipeline di integrazione continua nelle app per dispositivi mobili.

## <a name="visual-studio-app-center"></a>Visual Studio App Center

[App Center Build](/appcenter/build/) consente di compilare applicazioni native e multipiattaforma a cui il team collabora usando un'infrastruttura cloud sicura. È possibile connettere facilmente il repository in Visual Studio App Center e iniziare a compilare l'app nel cloud a ogni commit. Non è necessario preoccuparsi della configurazione dei server di compilazione in locale, di configurazioni complesse e del codice compilato nel computer di un collaboratore, ma non sul proprio.

Grazie alla potenza aggiuntiva dei servizi di Visual Studio App Center, è possibile automatizzare ulteriormente il flusso di lavoro e rilasciare automaticamente le compilazioni ai tester e negli App Store pubblici con Distribuzione App Center. È anche possibile eseguire test automatizzati dell'interfaccia utente su migliaia di configurazioni di dispositivi e del sistema operativo reali nel cloud con Test App Center.

### <a name="visual-studio-app-center-features"></a>Funzionalità di Visual Studio App Center

- Configurare l'integrazione continua in pochi minuti e compilare le applicazioni con maggiore frequenza e rapidità.
- Integrazione con GitHub, BitBucket, Azure DevOps e GitLab.
- Creare compilazioni veloci e sicure in computer gestiti e ospitati nel cloud.
- Abilitare le compilazioni per l'avvio di test e verificare la corretta compilazione in dispositivi iOS e Android reali.
- Supporto nativo e multipiattaforma per iOS, Android, macOS, Windows, Xamarin e React Native.
- Personalizzare le compilazioni tramite l'aggiunta di script post-clonazione, pre-compilazione e post-compilazione.

### <a name="visual-studio-app-center-references"></a>Informazioni di riferimento per Visual Studio App Center

- [Effettuare l'iscrizione per Visual Studio App Center](https://appcenter.ms/signup?utm_source=Mobile%20Development%20Docs&utm_medium=Azure&utm_campaign=New%20azure%20docs)
- [Introduzione ad App Center Build](/appcenter/build/)

## <a name="azure-pipelines"></a>Azure Pipelines

 [Azure Pipelines](https://azure.microsoft.com/services/devops/pipelines/), disponibile in Azure DevOps, è un servizio completo per l'integrazione continua e la distribuzione continua, che funziona con il provider Git preferito. Può essere distribuito nella maggior parte dei principali servizi cloud, incluso Azure. È possibile iniziare a scrivere codice in GitHub, GitHub Enterprise Server, GitLab, Bitbucket cloud o Azure Repos. Quindi, automatizzare la compilazione, il test e la distribuzione del codice a Microsoft Azure, Google Cloud Platform o Amazon Web Services.

### <a name="azure-pipelines-features"></a>Funzionalità di Azure Pipelines

- **Esperienza basata su attività semplificata per la configurazione di un server di integrazione continua:** Configurare un server di integrazione continua per le applicazioni per dispositivi mobili native (Android, iOS e Windows) e multipiattaforma (Xamarin, Cordova e React Native), oltre alle tecnologie server basate su Node.js/Java Microsoft e non Microsoft.
- **Tutti i linguaggi, le piattaforme e i cloud:** Compilare, testare e distribuire applicazioni Node.js, Python, Java, PHP, Ruby, Go, C/C++, C#, Android e iOS. Eseguire le app in parallelo in Linux, macOS e Windows. Eseguire la distribuzione nei provider di servizi cloud come Azure, AWS e Google Cloud Platform. Eseguire la distribuzione di applicazioni per dispositivi mobili tramite canali beta e App Store.
- **Supporto nativo per i contenitori:** Creare nuovi contenitori con facilità ed eseguirne il push in qualsiasi registro. Distribuire i contenitori in host indipendenti o in Kubernetes.
- **Flussi di lavoro avanzati:** Creare con facilità catene di compilazioni e le compilazioni in più fasi. Supporto per YAML, integrazione di test, attività di controllo di rilascio, creazione di report e altro ancora.
- **Estensibilità:** Usare una gamma di attività di compilazione, test e distribuzione create dalla community, che include centinaia di estensioni da Slack a SonarCloud. È anche possibile eseguire la distribuzione da altri sistemi di integrazione continua, ad esempio Jenkins. I webhook e le API REST possono agevolare l'integrazione.
- **Compilazioni gratuite ospitate nel cloud:** Queste compilazioni sono disponibili per i repository pubblici e privati.
- **Supporto per la distribuzione in altri fornitori di servizi cloud:** Tra questi fornitori sono inclusi AWS e Google Cloud Platform.

### <a name="azure-pipelines-references"></a>Informazioni di riferimento per Azure Pipelines

- [Guida introduttiva ad Azure Pipelines](/azure/devops/pipelines/get-started/pipelines-get-started)
- [Introduzione ad Azure DevOps](https://app.vsaex.visualstudio.com/signup/)
- [Guide introduttive](/azure/devops/pipelines/create-first-pipeline?tabs=tfs-2018-2)

Per facilitare la scelta del servizio appropriato per le compilazioni dell'applicazione, vedere l'articolo con il confronto tra [App Center Build e Azure Pipelines](/appcenter/build/choose-between-services).
