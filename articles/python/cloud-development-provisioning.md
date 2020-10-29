---
title: Provisioning, accesso e gestione delle risorse in Azure
description: Panoramica dei metodi usati per usare le risorse di Azure, tra cui il portale di Azure, l'interfaccia della riga di comando di Azure e le librerie di Azure (SDK).
ms.date: 10/06/2020
ms.topic: conceptual
ms.custom: devx-track-python, devx-track-azurecli
ms.openlocfilehash: 6ccbfb953cb28cceec08a70af4431e66e6e8b8ca
ms.sourcegitcommit: 1ddcb0f24d2ae3d1f813ec0f4369865a1c6ef322
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/27/2020
ms.locfileid: "92688865"
---
# <a name="provisioning-accessing-and-managing-resources-on-azure"></a>Provisioning, accesso e gestione delle risorse in Azure

[Articolo precedente: Panoramica](cloud-development-overview.md)

Come descritto nell'articolo precedente di questa serie, una parte essenziale dello sviluppo di un'applicazione cloud consiste nell'effettuare il provisioning delle risorse necessarie in Azure, in cui sarà quindi possibile distribuire il codice e i dati. La creazione di un'applicazione cloud inizia con la creazione di quello che è essenzialmente il computer cloud di destinazione in cui distribuire l'applicazione. Per esaminare i tipi di risorse disponibili, vedere la [guida per sviluppatori di Azure](/azure/guides/developer/azure-developer-guide).

In che modo viene effettuato il provisioning? Come si chiede ad Azure di allocare le risorse per l'applicazione e come si configurano le risorse e si accede ad esse? In breve, come si comunica ad Azure di predisporre tutte queste risorse?

## <a name="means-of-communicating-with-azure"></a>Modalità di comunicazione con Azure

La risposta è semplice. Come per la maggior parte dei sistemi operativi, è possibile comunicare con Azure in tre modi diversi: un'interfaccia utente, un'interfaccia della riga di comando e un'API.

![Metodi di comunicazione disponibili in Azure per effettuare il provisioning delle risorse](media/cloud-development/communication-with-azure.png)

È possibile usare uno o tutti questi metodi complementari per creare, configurare e gestire tutte le risorse di Azure necessarie. In realtà, durante un progetto di sviluppo si usano in genere tutti e tre i metodi ed è quindi opportuno acquisire familiarità con ognuno di essi.

In questo centro per sviluppatori viene illustrato principalmente l'uso dell'interfaccia della riga di comando e del codice Python che usa le librerie di Azure, perché l'uso del portale è descritto approfonditamente nella documentazione di ogni singolo servizio.

> [!NOTE]
> Le librerie di Azure per Python sono talvolta denominate Azure SDK per Python. Tuttavia, non sono disponibili altri componenti dell'SDK oltre alle librerie, acquisite tramite lo strumento di gestione pacchetti Python, pip.

## <a name="azure-portal"></a>Portale di Azure

Il [portale di Azure](https://portal.azure.com) è l'interfaccia utente basata su browser completamente personalizzabile di Azure tramite la quale è possibile effettuare il provisioning delle risorse e gestirle con tutti i servizi di Azure. Per accedere al portale, è prima di tutto necessario accedere con un account Microsoft e quindi creare un account Azure gratuito con una sottoscrizione. Dopo aver eseguito l'accesso, è possibile selezionare l'icona **?** e selezionare **Avvia presentazione guidata** per una semplice presentazione delle principali funzionalità del portale.

**Vantaggi** : l'interfaccia utente permette di esplorare facilmente i servizi e tutte le relative opzioni di configurazione. L'impostazione dei valori di configurazione è sicura perché le informazioni non vengono archiviate nella workstation locale.

**Svantaggi** : l'uso del portale è un processo manuale e non può essere automatizzato facilmente. Per ricordare le operazioni eseguite per modificare una configurazione, ad esempio, è in genere necessario prendere nota dei passaggi in un documento separato.

## <a name="azure-cli"></a>Interfaccia della riga di comando di Azure

L'[interfaccia della riga di comando di Azure](/cli/azure/) è l'interfaccia della riga di comando [open source](https://github.com/Azure/azure-cli) di Azure. Dopo aver eseguito l'accesso all'interfaccia della riga di comando (con il comando `az login`), è possibile eseguire le stesse attività eseguibili tramite il portale.
  
**Vantaggi** : consente una facile automazione tramite script ed elaborazione dell'output. Offre comandi di livello superiore per effettuare il provisioning di più risorse contemporaneamente per attività comuni, ad esempio per la distribuzione di un'app Web. Gli script possono essere gestiti nel controllo del codice sorgente.

**Svantaggi** : la curva di apprendimento è più ripida rispetto all'uso del portale e i comandi sono soggetti a bug. I messaggi di errore non sono sempre utili.

In sostituzione dell'interfaccia della riga di comando di Azure, è anche possibile usare [Azure PowerShell](/powershell/), anche se gli sviluppatori di Python siano più a proprio agio con i comandi tipo Linux dell'interfaccia della riga di comando di Azure.

In sostituzione dell'interfaccia della riga di comando locale o di PowerShell, è possibile usare Azure Cloud Shell direttamente tramite [https://shell.azure.com/](https://shell.azure.com/). È una soluzione pratica in quanto questo servizio viene autenticato automaticamente con Azure all'apertura e offre le stesse funzionalità del portale di Azure. Cloud Shell è preconfigurato con numerosi strumenti che risultano scomodi da installare in locale, in particolare se è necessario eseguire solo uno o due comandi.

Dal momento che Cloud Shell non è un ambiente locale, è più adatto per singole operazioni per le quali si userebbe il portale invece dell'automazione tramite script. È tuttavia possibile clonare i repository di origine, ad esempio quelli GitHub, in Cloud Shell e di conseguenza sviluppare script di automazione in locale, archiviarli in un repository, clonare il repository in Cloud Shell e quindi eseguirli da tale posizione.

## <a name="azure-rest-api-and-azure-libraries"></a>API REST di Azure e librerie di Azure

L'[API REST di Azure](/rest/api/?view=Azure&preserve-view=true) è l'interfaccia a livello di codice di Azure, fornita tramite REST sicuro su HTTP perché i data center di Azure sono tutti intrinsecamente connessi a Internet. A ogni risorsa viene assegnato un URL univoco che supporta un'API specifica della risorsa, soggetta a rigorosi protocolli di autenticazione e criteri di accesso. Il funzionamento del portale di Azure e dell'interfaccia della riga di comando di Azure si basa, in realtà, proprio sull'API REST.

Per gli sviluppatori, le librerie di Azure (note anche come SDK di Azure) includono librerie specifiche del linguaggio che convertono le funzionalità dell'API REST in paradigmi di programmazione molto più pratici, come classi e oggetti. Per Python, si installano sempre singole librerie con `pip install` invece di installare l'intero SDK. Per altri linguaggi, vedere i [download di Azure SDK](https://azure.microsoft.com/downloads/).

**Vantaggi** : controllo accurato su tutte le operazioni, incluso un mezzo molto più diretto per usare l'output di un'operazione come input di un'altra rispetto all'interfaccia della riga di comando di Azure. Per gli sviluppatori Python, consente di lavorare all'interno di paradigmi di linguaggio noti invece di usare l'interfaccia della riga di comando. Può essere usato anche dal codice dell'applicazione per automatizzare scenari di gestione dettagliati.
  
**Svantaggi** : le operazioni che possono essere eseguite con un comando dell'interfaccia della riga di comando richiedono in genere più righe di codice, tutte soggette a bug. Non offre operazioni di livello superiore come l'interfaccia della riga di comando di Azure.

## <a name="automatic-on-demand-provisioning"></a>Provisioning su richiesta automatico

Molti servizi di Azure consentono di configurare caratteristiche di scalabilità per soddisfare richieste variabili. In tale caso Azure può effettuare automaticamente il provisioning di risorse aggiuntive quando necessario e deallocarle in base alle esigenze. La scalabilità automatica è uno dei vantaggi principali di una piattaforma cloud supportata dalle risorse di più data center. Invece di progettare l'ambiente per picchi di richieste, pagando per una capacità che in genere non si utilizzerà, è possibile progettare l'ambiente per un utilizzo iniziale o medio e pagare per la capacità aggiuntiva solo quando necessario.

Per altre informazioni, vedere le [Scalabilità automatica](/azure/architecture/best-practices/auto-scaling) in Centro architetture di Azure.

## <a name="subscriptions-resource-groups-and-regions"></a>Sottoscrizioni, gruppi di risorse e aree

Nel modello di risorse di Azure è possibile immaginare che, nel corso del tempo, si effettuerà il provisioning di molte risorse diverse in numerosi servizi di Azure per applicazioni diverse. È possibile organizzare queste risorse in tre livelli di gerarchia disponibili:

1. **Sottoscrizioni** : ogni sottoscrizione di Azure ha il proprio account di fatturazione e spesso rappresenta un team o un reparto distinto all'interno di un'organizzazione. In generale, si effettua il provisioning di tutte le risorse necessarie per una determinata applicazione all'interno della stessa sottoscrizione, in modo che possano trarre vantaggio da funzionalità quali l'autenticazione condivisa. Poiché, tuttavia, è possibile accedere a tutte le risorse tramite URL pubblici e i token di autorizzazione necessari, è certamente possibile distribuire le risorse tra più sottoscrizioni.

1. **Gruppi di risorse** : all'interno di una sottoscrizione, i gruppi di risorse costituiscono i contenitori per altre risorse, che possono quindi essere gestite come gruppo. Per questo motivo, un gruppo di risorse è in genere correlato a un progetto specifico. Di fatto, quando si effettua il provisioning di una risorsa, è necessario specificare il gruppo a cui appartiene. Il primo passaggio con un nuovo progetto consiste in genere nel creare un gruppo di risorse appropriato. Eliminando il gruppo di risorse, si deallocano tutte le risorse contenute nel gruppo invece di doverle eliminare singolarmente. La mancata organizzazione dei gruppi di risorse può causare non pochi problemi in seguito quando non si riesce a ricordare il progetto a cui appartiene una risorsa.

1. **Assegnazione di nomi alle risorse** : all'interno di un gruppo di risorse, è possibile usare qualsiasi strategia di denominazione per esprimere aspetti in comune o relazioni tra le risorse. Poiché il nome viene spesso usato nell'URL della risorsa, potrebbero essere previste limitazioni in merito ai caratteri che è possibile usare. Con alcuni nomi, ad esempio, sono consentiti solo lettere e numeri, mentre con altri anche trattini e caratteri di sottolineatura.

Durante l'uso di Azure, si svilupperanno preferenze personali per l'organizzazione delle risorse e convenzioni specifiche per l'assegnazione dei nomi a sottoscrizioni, gruppi di risorse e singole risorse.

### <a name="regions-and-geographies"></a>Aree e aree geografiche

Una particolarità di un gruppo di risorse è che è sempre associato a una specifica *area* di Azure, che corrisponde alla località in cui è ubicato il data center specifico. Tutte le risorse dello stesso gruppo condividono lo stesso data center e possono quindi interagire in modo molto più efficiente rispetto a quanto accadrebbe se si trovassero in aree diverse. Gli sviluppatori scelgono spesso le aree più vicine ai propri clienti, ottimizzando in tal modo la velocità di risposta di un'applicazione. Azure offre anche funzionalità di replica geografica per sincronizzare le copie dell'applicazione e dei database in più aree e soddisfare meglio una base di clienti globale.

A causa delle leggi e delle normative locali stabilite dall' *area geografica* in cui si crea una sottoscrizione, l'accesso potrebbe essere limitato solo ad alcune aree e tali aree potrebbero non supportare tutti i servizi di Azure. Per informazioni dettagliate, vedere [Infrastruttura globale di Azure](https://azure.microsoft.com/global-infrastructure/).

## <a name="next-step"></a>Passaggio successivo

> [!div class="nextstepaction"]
> [Flusso di sviluppo di Azure >>>](cloud-development-flow.md)
