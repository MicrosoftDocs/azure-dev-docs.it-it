---
title: Automatizzare la distribuzione e il rilascio di applicazioni per dispositivi mobili con Visual Studio App Center e i servizi di Azure
description: Informazioni sui servizi come App Center che consentono di configurare la pipeline di recapito continuo per le applicazioni per dispositivi mobili.
author: codemillmatt
ms.assetid: 34a8a070-9b3c-4faf-8588-ccff02097224
ms.service: mobile-services
ms.topic: article
ms.date: 06/08/2020
ms.author: masoucou
ms.openlocfilehash: 6cb18fed3db2ad46c96b7ff20684727aa2e35064
ms.sourcegitcommit: 3d906f265b748fbc0a070fce252098675674c8d9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/22/2021
ms.locfileid: "98699769"
---
# <a name="automate-the-deployment-and-release-of-your-mobile-applications-with-continuous-delivery-services"></a>Automatizzare la distribuzione e il rilascio di applicazioni per dispositivi mobili con servizi di recapito continuo

Gli sviluppatori scrivono codice e lo archiviano nel repository del codice, ma i commit archiviati nel repository potrebbero non essere sempre coerenti. Quando più sviluppatori collaborano allo stesso progetto, è possibile che si verifichino problemi di integrazione. I team possono trovarsi in situazioni in cui il codice non funziona come previsto, i bug si accumulano e lo sviluppo del progetto viene ritardato. Gli sviluppatori devono quindi attendere che l'intero codice software venga compilato e testato per verificare la presenza di errori, rallentando così il processo e riducendone l'iteratività.

Con il recapito continuo è possibile automatizzare la distribuzione e il rilascio di applicazioni per dispositivi mobili. Non è importante se l'applicazione viene distribuita a un gruppo di tester o di dipendenti della società (per il beta test) o a un App Store (per la produzione). Il recapito continuo rende le distribuzioni meno rischiose e favorisce le iterazioni rapide. È anche possibile rilasciare nuove modifiche ai clienti in modo continuo.

## <a name="distribute-application-binaries-to-beta-testers"></a>Distribuire i file binari dell'applicazione ai beta tester

Il beta testing dell'applicazione per dispositivi mobili è uno dei passaggi fondamentali durante il processo di sviluppo dell'applicazione. Consente di individuare tempestivamente i bug e i problemi nell'applicazione. Il feedback migliora la qualità dell'applicazione quando viene preparata per l'uso in produzione.

Usare i servizi seguenti per abilitare una pipeline di recapito continuo nelle app per dispositivi mobili.

### <a name="visual-studio-app-center-distribute"></a>Visual Studio - Distribuzione App Center

[Distribuzione App Center](/appcenter/distribution/) è uno strumento che consente agli sviluppatori di rilasciare rapidamente le compilazioni nei dispositivi. Con un portale di installazione che offre un'esperienza completa, Distribuzione App Center è una soluzione avanzata per la distribuzione ai beta tester delle app. Si tratta anche di una comoda alternativa alla distribuzione tramite App Store pubblici. Gli sviluppatori possono automatizzare ulteriormente il flusso di lavoro di distribuzione con App Center Build e le integrazioni di App Store pubblici.

#### <a name="visual-studio-app-center-distribute-features"></a>Funzionalità di Visual Studio - Distribuzione App Center

- Distribuire l'app a beta tester e utenti e assicurarsi che tutti i tester usino la versione più recente dell'applicazione.
- Inviare ai tester una notifica relativa alla disponibilità di nuove versioni senza che i tester debbano eseguire nuovamente il flusso di download.
- Gestire i gruppi di distribuzioni per versioni diverse dell'applicazione.
- Distribuire l'app negli Store: 
  - [Apple](/appcenter/distribution/stores/apple)
  - [Google Play](/appcenter/distribution/stores/googleplay)
  - [Intune](/appcenter/distribution/stores/intune)
- Supporto della piattaforma per iOS, Android, macOS, tvOS, Xamarin, React Native, Unity e Cordova.
- Registrare automaticamente i dispositivi iOS nel profilo di provisioning.

#### <a name="visual-studio-app-center-distribute-references"></a>Informazioni di riferimento per Visual Studio - Distribuzione App Center

- [Effettuare l'iscrizione per Visual Studio App Center](https://appcenter.ms/signup)
- [Introduzione a Distribuzione App Center](/appcenter/build/)

### <a name="azure-pipelines"></a>Azure Pipelines

[Azure Pipelines](https://azure.microsoft.com/services/devops/pipelines/) è un servizio completo per l'integrazione continua e la distribuzione continua, che funziona con il provider Git preferito. Con Azure Pipelines è possibile eseguire la distribuzione nella maggior parte dei servizi cloud principali, ad esempio i servizi di Azure. È possibile iniziare a scrivere codice in GitHub, GitHub Enterprise Server, GitLab, Bitbucket cloud o Azure Repos. Quindi, automatizzare la compilazione, il test e la distribuzione del codice a Microsoft Azure, Google Cloud Platform o Amazon Web Services.

#### <a name="azure-pipelines-features"></a>Funzionalità di Azure Pipelines

- **Esperienza basata su attività semplificata per la configurazione di un server di integrazione continua:** Configurare un server di integrazione continua per le applicazioni per dispositivi mobili native (Android, iOS e Windows) e multipiattaforma (Xamarin, Cordova e React Native).
- **Tutti i linguaggi, le piattaforme e i cloud:** Compilare, testare e distribuire app Node.js, Python, Java, PHP, Ruby, Go, C/C++, C#, Android e iOS. Eseguire le app in parallelo in Linux, macOS e Windows. Eseguire la distribuzione nei provider di servizi cloud come Azure, AWS e Google Cloud Platform. Eseguire la distribuzione di applicazioni per dispositivi mobili tramite canali beta e App Store.
- **Supporto nativo per i contenitori:** Creare nuovi contenitori con facilità ed eseguirne il push in qualsiasi registro. Distribuire i contenitori in host indipendenti o in Kubernetes.
- **Flussi di lavoro e funzionalità avanzati:** Creare con facilità catene di compilazioni e le compilazioni in più fasi. Supporto per YAML, integrazione di test, attività di controllo di rilascio, creazione di report e altro ancora.
- **Estensibilità:** Usare una gamma di attività di compilazione, test e distribuzione create dalla community, che include centinaia di estensioni da Slack a SonarCloud. È anche possibile eseguire la distribuzione da altri sistemi di integrazione continua, ad esempio Jenkins. I webhook e le API REST possono agevolare l'integrazione.
- **Compilazioni gratuite ospitate nel cloud:** Queste compilazioni sono disponibili per i repository pubblici e privati.
- **Supporto per la distribuzione in altri fornitori di servizi cloud:** Tra questi fornitori sono inclusi AWS e Google Cloud Platform.

#### <a name="azure-pipelines-references"></a>Informazioni di riferimento per Azure Pipelines

- [Guida introduttiva ad Azure Pipelines](/azure/devops/pipelines/get-started/pipelines-get-started)
- [Introduzione ad Azure DevOps](https://app.vsaex.visualstudio.com/signup/)
  
## <a name="distribute-your-application-directly-to-app-stores"></a>Distribuire l'applicazione direttamente negli App Store

Quando l'applicazione è pronta per l'uso nell'ambiente di produzione e si vuole renderla disponibile pubblicamente, è necessario inviarla agli App Store da cui potrà essere scaricata dai clienti. Esistono diversi modi per distribuire l'applicazione direttamente negli App Store. 

### <a name="visual-studio-app-center-distribute-stores"></a>Visual Studio - Distribuzione App Center per gli Store

Con [Distribuzione App Center](/appcenter/distribution/stores/), è possibile pubblicare le applicazioni per dispositivi mobili direttamente negli App Store. Quando l'applicazione è pronta per essere scaricata dagli utenti, è possibile pubblicare i file binari dell'applicazione direttamente dal portale di Visual Studio App Center. 

È possibile distribuire direttamente l'app in:

- [App Store di Apple](/appcenter/distribution/stores/apple)
- [Google Play Store](/appcenter/distribution/stores/googleplay)
- [Microsoft Intune](/appcenter/distribution/stores/intune)

### <a name="apple-app-store"></a>App Store di Apple

Nell'App Store sviluppato e gestito da Apple, gli utenti possono esplorare e scaricare le applicazioni sviluppate per i dispositivi iOS, MacOS, WatchOS e tvOS. Gli sviluppatori devono inviare le app iOS all'App Store di Apple per affinché vengano usate pubblicamente.

### <a name="google-play"></a>Google Play

Google Play è l'App Store ufficiale per il sistema operativo Android, in cui gli utenti possono esplorare e scaricare le applicazioni sviluppate per i dispositivi Android pubblicate tramite Google.

### <a name="intune"></a>Intune

[Microsoft Intune](/intune/app-management) è un servizio basato sul cloud nel settore di gestione della mobilità aziendale che consente alla forza lavoro di aumentare la produttività, garantendo al tempo stesso la protezione dei dati aziendali. Con Intune, è possibile:

- Gestire i dispositivi mobili e i PC usati dalla forza lavoro per accedere ai dati aziendali.
- Gestire le applicazioni per dispositivi mobili usate dalla forza lavoro.
- Proteggere le informazioni aziendali grazie alla possibilità di controllare le modalità di accesso e condivisione dei dati da parte della forza lavoro.
- Assicurarsi che i dispositivi e le applicazioni siano conformi ai requisiti di sicurezza aziendali.

## <a name="deploy-updates-directly-to-users-devices"></a>Distribuire gli aggiornamenti direttamente nei dispositivi degli utenti

### <a name="codepush"></a>CodePush

Con [CodePush](/appcenter/distribution/codepush/) in App Center, gli sviluppatori di Apache Cordova e React Native possono distribuire gli aggiornamenti delle applicazioni per dispositivi mobili direttamente nei dispositivi degli utenti. Funge da repository centrale in cui gli sviluppatori possono pubblicare determinati aggiornamenti, ad esempio le modifiche a JavaScript, HTML, CSS e immagini. Le applicazioni possono quindi eseguire query per gli aggiornamenti dal repository usando gli SDK client forniti. In questo modo, è possibile usufruire di un modello di engagement più deterministico e diretto con gli utenti mentre si risolvono i bug o si aggiungono funzionalità minori. Non è necessario ricompilare un file binario o ridistribuirlo tramite un App Store pubblico.

#### <a name="codepush-key-features"></a>Funzionalità principali di CodePush

- Gli sviluppatori di Cordova e React Native possono distribuire gli aggiornamenti delle applicazioni per dispositivi mobili direttamente nei dispositivi degli utenti senza procedere al rilascio in uno Store.
- È utile per la correzione di bug o per l'aggiunta e la rimozione di funzionalità minori che non richiedono la ricompilazione dei file binari e la ridistribuzione tramite i rispettivi Store.

#### <a name="codepush-references"></a>Informazioni di riferimento per CodePush

- [Effettuare l'iscrizione per Visual Studio App Center](https://appcenter.ms/signup)
- [Introduzione a CodePush in App Center](/appcenter/distribution/codepush/)
- [Interfaccia della riga di comando di CodePush](/appcenter/distribution/codepush/cli)
