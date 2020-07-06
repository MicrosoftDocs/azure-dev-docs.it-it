---
title: Acquisire informazioni sul comportamento degli utenti e l'utilizzo delle applicazioni per dispositivi mobili con Visual Studio App Center e servizi di Azure
description: Informazioni sui servizi come App Center che consentono di prendere decisioni aziendali intelligenti in base al modo in cui gli utenti usano l'applicazione per dispositivi mobili.
author: codemillmatt
ms.assetid: 34a8a070-9b3c-4faf-8588-ccff02097224
ms.service: mobile-services
ms.topic: article
ms.date: 06/08/2020
ms.author: masoucou
ms.openlocfilehash: 744e41200a5f7e5ff898e7a0786a4284de176b7a
ms.sourcegitcommit: f02ff55cef47581303a217cf1d310879cd722237
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/09/2020
ms.locfileid: "84632593"
---
# <a name="analyze-and-understand-mobile-application-use"></a>Analizzare e comprendere l'uso delle applicazioni per dispositivi mobili

In che modo gli utenti usano le applicazioni? Qual è il numero di utenti attivi dell'applicazione e in che modo cambia l'utilizzo nel tempo? Quali sono le funzionalità che usano e quali sono le funzionalità più utilizzate? Dove risiedono gli utenti? Quanti utenti usano la versione più recente dell'applicazione? È importante trovare una risposta a tutte queste domande per trasformare l'app in un progetto di successo. Per rispondere a queste tipologie di domande relative all'analisi di utilizzo, è necessario raccogliere i dati di utilizzo dalle app.

Più si esaminano i dati, più è possibile trovare modi per migliorare l'applicazione e garantire la soddisfazione degli utenti nel tempo. È importante usare i dati per individuare informazioni dettagliate di utilità pratica e mantenere la soddisfazione degli utenti.

## <a name="importance-of-analytics"></a>Importanza dell'analisi

- Acquisire informazioni sugli utenti, il modo in cui interagiscono con l'applicazione e ciò che interessa loro per ottimizzare l'applicazione e offrire esperienze straordinarie e far crescere il business.
- Monitorare le metriche di utilizzo per prendere decisioni informate su come commercializzare l'applicazione e soddisfare al meglio i clienti.
- Misurare le prestazioni dell'applicazione.
- Scoprire quali sono le parti dell'applicazione che generano valore e prestazioni.
- Ottenere informazioni dettagliate basate sui dati sui problemi relativi ad abbandono e fidelizzazione.

Usare i servizi seguenti per abilitare l'analisi delle applicazioni per dispositivi mobili.

## <a name="visual-studio-app-center"></a>Visual Studio App Center

[App Center Analytics](/appcenter/analytics/) consente di incentivare la crescita dei destinatari concentrandosi sugli aspetti più importanti. Offre funzionalità di creazione di report e informazioni dettagliate su sessioni utente, dispositivi principali, versioni del sistema operativo e analisi comportamentale. Permette di creare facilmente eventi personalizzati per monitorare qualsiasi aspetto con analisi delle applicazioni estese.

### <a name="visual-studio-app-center-features"></a>Funzionalità di Visual Studio App Center

- Monitorare i modelli di utilizzo, l'adozione degli utenti e altre metriche di engagement, gratuitamente.
- Identificare tendenze, comportamenti degli utenti ed engagement tramite eventi personalizzati.
- Metriche predefinite e informazioni dettagliate sull'utilizzo delle applicazioni (giornaliero, settimanale, mensile), sessioni, proprietà dei dispositivi e dati demografici degli utenti in un unico dashboard.
- Esportare in modo continuo tutti i dati di Analisi App Center in Azure per tempi di conservazione dei dati senza limiti. App Center Analytics supporta l'esportazione nell'archiviazione BLOB di Azure e in Azure Application Insights.
- Integrazione con Azure Application Insights per informazioni dettagliate ancora più approfondite, ad esempio fidelizzazione, analisi della canalizzazione e delle coorti.
- Usare l'integrazione dell'SDK a una riga per iniziare a usare il servizio in pochi minuti.
- Supporto della piattaforma per iOS, Android, macOS, tvOS, Xamarin, React Native, Unity e Cordova.

### <a name="visual-studio-app-center-references"></a>Informazioni di riferimento per Visual Studio App Center

- [Effettuare l'iscrizione ad App Center](https://appcenter.ms/signup)
- [Introduzione ad App Center Analytics](/appcenter/analytics/)

## <a name="azure-monitor"></a>Monitoraggio di Azure

Monitoraggio di Azure include [Application Insights](/azure/azure-monitor/app/app-insights-overview), che offre strumenti per raccogliere e analizzare i dati di telemetria per ottimizzare le prestazioni e monitorare l'applicazione per dispositivi mobili. È possibile sfruttare i vantaggi di Application Insights usando App Center Analytics per configurare l'esportazione in Application Insights. Application Insights può eseguire query, segmentare, filtrare e analizzare i dati di telemetria relativi agli eventi personalizzati delle applicazioni, oltre agli strumenti di analisi offerti da App Center.

### <a name="azure-monitor-features"></a>Funzionalità di Monitoraggio di Azure

- Eseguire query sui dati di telemetria relativi agli eventi personalizzati.
- Filtrare i dati di telemetria degli eventi con potenti funzionalità di segmentazione.
- Analizzare i modelli di conversione, fidelizzazione e navigazione nell'applicazione. È possibile usare:
  - Canalizzazioni per analizzare e monitorare i tassi di conversione.
  - Fidelizzazione per analizzare il modo in cui l'applicazione fidelizza gli utenti nel tempo.
  - Cartelle di lavoro per la combinazione di visualizzazioni e testo in un report condivisibile.
  - Coorti per l'assegnazione di nomi e il salvataggio di specifici gruppi di utenti o eventi per farvi facilmente riferimento da altri strumenti di analisi.

### <a name="azure-monitor-references"></a>Informazioni di riferimento per Monitoraggio di Azure

- [Azure portal](https://portal.azure.com/)
- [Analizzare l'applicazione per dispositivi mobili con App Center e Application Insights](/azure/azure-monitor/learn/mobile-center-quickstart)
- [Usare App Center Analytics con Application Insights](/azure/azure-monitor/app/usage-overview)

## <a name="azure-playfab"></a>Azure PlayFab

[Azure PlayFab](https://playfab.com/) offre una piattaforma back-end completa con servizi di gioco, analisi in tempo reale e funzionalità LiveOps necessari per creare giochi di qualità elevata e connessi al cloud. Questi servizi riducono gli ostacoli al lancio per gli sviluppatori di giochi. Offrono soluzioni di sviluppo convenienti per case di produzione di piccole e grandi dimensioni in grado di evolversi insieme ai giochi. I servizi consentono alle case di produzione di coinvolgere, fidelizzare e monetizzare i giocatori. Con PlayFab, gli sviluppatori possono usare il cloud intelligente per creare e gestire giochi, analizzare i dati di gioco e migliorare le esperienze di gioco complessive.

### <a name="azure-playfab-features"></a>Funzionalità di Azure PlayFab

- Monitorare i dashboard in tempo reale.
- Valutare le prestazioni del gioco attraverso le metriche più importanti.
- Analizzare i riepiloghi delle prestazioni quotidiane e mensili del gioco tramite report generati automaticamente. È possibile visualizzare i report in Game Manager per il download o l'invio quotidiano tramite posta elettronica.
- Usare i test A/B per eseguire esperimenti e determinare l'impostazione ottimale per una determinata variabile.
- Usare la segmentazione per i giocatori per definire raggruppamenti di giocatori automatizzati.

### <a name="azure-playfab-references"></a>Informazioni si riferimenti per Azure PlayFab

- [Portale di PlayFab](https://developer.playfab.com/en-US/sign-up)
- [Analisi](/gaming/playfab/#pivot=documentation&panel=analytics)
- [Guide introduttive](/gaming/playfab/#pivot=documentation&panel=quickstarts)
