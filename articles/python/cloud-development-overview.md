---
title: Sviluppo cloud con Azure - Che cos'è Azure?
description: Panoramica dello sviluppo di applicazioni cloud in Microsoft Azure, a partire dalla correlazione tra data center, servizi e risorse.
ms.date: 02/16/2021
ms.topic: conceptual
ms.custom: devx-track-python
ms.openlocfilehash: c6bfc9a9de48ab474b5bf99d5804a111ea10f62d
ms.sourcegitcommit: b882128a763f81dba83913bfff1e9cd1ec70818f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/17/2021
ms.locfileid: "100642297"
---
# <a name="cloud-development-on-azure"></a>Sviluppo cloud in Azure

Per gli sviluppatori Python che vogliono sviluppare applicazioni cloud per Microsoft Azure, questa serie di tre articoli consentiranno di orientarsi tra i concetti di base correlati allo sviluppo cloud in Azure per prepararsi a una carriera lunga e produttiva.

## <a name="what-is-azure-data-centers-services-and-resources"></a>Cos'è Azure? Data center, servizi e risorse

Satya Nadella, CEO di Microsoft, si riferisce spesso ad Azure come "computer del mondo". Un computer, come è noto, è una raccolta di hardware gestito da un sistema operativo, che fornisce una piattaforma su cui è possibile creare software che consente agli utenti di *applicare* la potenza di calcolo del sistema a un numero qualsiasi di attività. È per questo motivo che si usa il termine "applicazione" per descrivere tale software.

Nel caso di Azure l'hardware del computer non è costituito da un singolo computer, ma da un enorme pool di computer server virtualizzati contenuti in [decine di data center di grandi dimensioni in tutto il mondo](https://azure.microsoft.com/global-infrastructure/regions/). Il "sistema operativo" di Azure è quindi costituito da *servizi* che allocano e deallocano in modo dinamico parti diverse del pool di risorse in base alle esigenze delle applicazioni. Queste allocazioni dinamiche consentono alle applicazioni di rispondere rapidamente a un numero qualsiasi di condizioni mutevoli, ad esempio la domanda dei clienti.

Ogni allocazione è denominata *risorsa* e a ogni risorsa viene assegnato un identificatore di *oggetto* univoco (Guid) e un URL univoco. I tipi di risorse includono macchine virtuali (core CPU e memoria), archiviazione, database, reti virtuali, registri contenitori, agenti di orchestrazione dei contenitori, host Web, motori di AI e analisi e così via.

![Livelli di Azure, dal data center ai servizi di Azure per l'allocazione delle risorse](media/cloud-development/azure-layers.png)

Le risorse rappresentano gli elementi costitutivi di un'applicazione cloud. Il processo di sviluppo cloud inizia quindi con la creazione dell'ambiente appropriato in cui è possibile distribuire le diverse parti dell'applicazione. In poche parole, non è possibile distribuire codice o dati in Azure finché non è stato allocato e configurato &mdash; il *provisioning* &mdash; delle risorse di destinazione appropriate.

Il processo di creazione dell'ambiente per l'applicazione, quindi, comporta l'identificazione dei servizi e dei tipi di risorse rilevanti coinvolti, quindi il provisioning di tali risorse. Per processo di provisioning si intende essenzialmente la modalità di costruzione del sistema di elaborazione in cui si distribuisce l'applicazione. Il provisioning è anche il punto in cui si inizia a noleggiare tali risorse da Azure.

È possibile scegliere tra centinaia di tipi di risorse diversi, dalle risorse di base per l'"infrastruttura", come le macchine virtuali, in cui si mantiene il controllo completo e la responsabilità del software distribuito, fino a servizi della "piattaforma" di livello superiore che forniscono un ambiente più gestito in cui è necessario preoccuparsi solo dei dati e del codice delle applicazioni.

Trovare i servizi appropriati per l'applicazione e bilanciare i costi relativi può risultare complesso, ma fa anche parte del divertimento creativo dello sviluppo cloud. Per informazioni sulle numerose opzioni disponibili, vedere la [guida per gli sviluppatori di Azure](/azure/guides/developer/azure-developer-guide). In questa sede verrà illustrato come usare tutti questi servizi e queste risorse.

> [!NOTE]
> Probabilmente si è visto che è possibile che si sia cresciuta l'usura dei termini *IaaS* (Infrastructure-as-a-Service), *PaaS* (piattaforma distribuita come servizio) e così via. La parte *As-a-Service* riflette la realtà che in genere non si ha accesso fisico ai Data Center. Si usano invece strumenti come il portale di Azure, l'interfaccia della riga di comando di Azure o l'API REST di Azure per eseguire il provisioning di risorse di *infrastruttura* , risorse della *piattaforma* e così via. Azure è sempre in attesa di ricevere le richieste.
>
> In questo centro per sviluppatori non verranno usati i termini IaaS, PaaS e così via perché l'espressione "come servizio" è riferita esclusivamente al cloud.

> [!NOTE]
> Un *cloud ibrido* si riferisce alla combinazione di computer privati e data center con risorse cloud come Azure e presenta le proprie considerazioni oltre a quanto descritto nella discussione precedente. Questa discussione presuppone inoltre lo sviluppo di nuove applicazioni, di conseguenza in questa sede non vengono trattati gli scenari che prevedono la riprogettazione e la migrazione di applicazioni locali esistenti.

> [!NOTE]
> Si potrebbero sentire i termini applicazioni *cloud native* e *abilitate* per il cloud, spesso discusse come la stessa cosa. Esistono tuttavia alcune differenze. Un'applicazione abilitata per il cloud è spesso una migrata, nel suo complesso, da un data center locale a server basati su cloud. Spesso, tali applicazioni mantengono la struttura originale e vengono semplicemente distribuite alle macchine virtuali nel cloud (e quindi in tutte le aree geografiche). Tale migrazione consente la scalabilità dell'applicazione in modo da soddisfare la domanda globale senza dover effettuare il provisioning di un nuovo hardware nel proprio data center. Tuttavia, il ridimensionamento deve essere eseguito a livello di macchina virtuale (o di infrastruttura), anche se solo una parte dell'applicazione richiede un miglioramento delle prestazioni.
>
> Un'applicazione cloud *nativa* , d'altra parte, viene scritta dall'inizio per sfruttare i numerosi servizi scalabili indipendenti disponibili in un cloud, ad esempio Azure. Le applicazioni cloud native sono, ad esempio, strutturate in modo più flessibile (usando le architetture di microservizi), che consente di configurare più precisamente la distribuzione e il ridimensionamento per ogni parte. Tale struttura semplifica la manutenzione e spesso riduce i costi perché è necessario pagare per i servizi Premium solo se necessario.
>
> Per ulteriori informazioni, vedere la pagina relativa alla [compilazione di applicazioni native del cloud in Azure](https://azure.microsoft.com/overview/cloudnative/) e all' [architettura di applicazioni .NET native cloud per Azure](/dotnet/architecture/cloud-native/), i cui principi si applicano alle applicazioni scritte in qualsiasi linguaggio.

## <a name="next-step"></a>Passaggio successivo

> [!div class="nextstepaction"]
> [Provisioning, accesso e gestione delle risorse >>>](cloud-development-provisioning.md)
