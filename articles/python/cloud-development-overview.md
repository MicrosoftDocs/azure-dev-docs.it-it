---
title: Sviluppo cloud con Azure - Che cos'è Azure?
description: Panoramica dello sviluppo di applicazioni cloud in Microsoft Azure, a partire dalla correlazione tra data center, servizi e risorse.
ms.date: 05/12/2020
ms.topic: conceptual
ms.openlocfilehash: 815da765aaed1e8364c37f621f17f279212bf77f
ms.sourcegitcommit: 2cdf597e5368a870b0c51b598add91c129f4e0e2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/14/2020
ms.locfileid: "83404954"
---
# <a name="cloud-development-on-azure"></a>Sviluppo cloud in Azure

Se si è uno sviluppatore Python e si intende sviluppare applicazioni cloud per Microsoft Azure, questa serie di tre articoli consentiranno di orientarsi tra i concetti di base correlati allo sviluppo cloud in Azure per prepararsi a una carriera lunga e produttiva.

## <a name="what-is-azure-data-centers-services-and-resources"></a>Che cos'è Azure? Data center, servizi e risorse

Satya Nadella, CEO di Microsoft, si riferisce spesso ad Azure come "computer del mondo". Un computer, come è noto, è un insieme di componenti hardware gestito da un sistema operativo, che offre una piattaforma su cui è possibile creare software per consentire agli utenti di applicare la potenza di calcolo del sistema a un numero qualsiasi di attività. È per questo motivo che si usa il termine "applicazione" per descrivere tale software.

Nel caso di Azure l'hardware del computer non è costituito da un singolo computer, ma da un enorme pool di computer server virtualizzati contenuti in [decine di data center di grandi dimensioni in tutto il mondo](https://azure.microsoft.com/global-infrastructure/regions/). Il "sistema operativo" di Azure è quindi costituito da *servizi* che allocano e deallocano in modo dinamico parti diverse del pool di risorse in base alle esigenze delle applicazioni. Ogni allocazione, sia essa potenza di calcolo (memoria e core CPU), spazio di archiviazione, database, reti e così via, corrisponde a una *risorsa*. A ogni risorsa discreta vengono quindi assegnati un *identificatore oggetto* univoco (GUID) e un URL univoco.

![Livelli di Azure, dal data center ai servizi di Azure per l'allocazione delle risorse](media/cloud-development/azure-layers.png)

Le risorse rappresentano gli elementi costitutivi di un'applicazione cloud. Il processo di sviluppo cloud inizia quindi con la creazione dell'ambiente appropriato in cui è possibile distribuire le diverse parti dell'applicazione. In poche parole, non è possibile distribuire codice o dati in Azure finché non si esegue l'allocazione e la configurazione, ovvero il *provisioning*, di una risorsa di destinazione appropriata, ad esempio una macchina virtuale, un database, un account di archiviazione, un registro contenitori, un agente di orchestrazione dei contenitori, un host Web, una rete virtuale, i motori di analisi e intelligenza artificiale e così via.

Il processo di creazione dell'ambiente per l'applicazione implica quindi l'identificazione dei servizi e dei tipi di risorse pertinenti coinvolti e di conseguenza il provisioning di tali risorse, ovvero quando si inizia ad affittarle da Azure. È possibile scegliere tra centinaia di tipi di risorse diversi, dalle risorse di base per l'"infrastruttura", come le macchine virtuali, in cui si mantiene il controllo completo e la responsabilità del software distribuito, fino a servizi della "piattaforma" di livello superiore che forniscono un ambiente più gestito in cui è necessario preoccuparsi solo dei dati e del codice delle applicazioni.

Trovare i servizi appropriati per l'applicazione e bilanciare i costi relativi può risultare complesso, ma fa anche parte del divertimento creativo dello sviluppo cloud. Per altre informazioni su come scegliere i servizi più adatti, è possibile vedere altri articoli di questo centro per sviluppatori. Nel frattempo vediamo come usare tutti questi servizi e queste risorse.

> [!NOTE]
> Probabilmente si conoscono già i termini "IaaS" (infrastruttura distribuita come servizio), "PaaS" (piattaforma distribuita come servizio) e così via. L'espressione "come servizio" indica proprio che in genere queste soluzioni non prevedono in genere l'accesso fisico ai data center. Si usano invece strumenti come il portale di Azure, l'interfaccia della riga di comando di Azure o l'API REST di Azure per effettuare il provisioning di risorse di tipo "infrastruttura", "piattaforma" e così via. In quanto "servizio", Azure è sempre in attesa di ricevere le richieste degli utenti.
>
> In questo centro per sviluppatori non verranno usati i termini IaaS, PaaS e così via perché l'espressione "come servizio" è riferita esclusivamente al cloud.

> [!NOTE]
> Per "cloud ibrido" si intende la combinazione di computer privati e data center con risorse cloud come Azure. Tale soluzione implica considerazioni che esulano da quanto trattato nella discussione precedente. Questa discussione presuppone inoltre lo sviluppo di nuove applicazioni, di conseguenza in questa sede non vengono trattati gli scenari che prevedono la riprogettazione e la migrazione di applicazioni locali esistenti.

## <a name="next-step"></a>Passaggio successivo

> [!div class="nextstepaction"]
> [Provisioning, accesso e gestione delle risorse >>>](cloud-development-provisioning.md)
