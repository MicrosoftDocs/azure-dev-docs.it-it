---
title: Usare Habitat per distribuire l'applicazione in Azure
description: Informazioni su come distribuire in modo coerente l'applicazione nei contenitori e nelle macchine virtuali di Azure
keywords: azure, chef, devops, macchine virtuali, panoramica, automazione, habitat
ms.date: 05/15/2018
ms.topic: article
ms.custom: devx-track-chef
ms.openlocfilehash: d2a834c631986b70a13c95f1403e84e82886a5f2
ms.sourcegitcommit: 95fdc444c424f4a7d7d53437837e9532a0b897e9
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/20/2020
ms.locfileid: "88662942"
---
# <a name="use-habitat-to-deploy-your-application-to-azure"></a>Usare Habitat per distribuire l'applicazione in Azure

[Habitat](https://www.habitat.sh/) è un sistema di runtime e creazione di pacchetti di applicazione che raggruppa l'applicazione e la sua automazione come unità di distribuzione. Viene così creata la massima portabilità per l'applicazione, in modo che possa essere distribuita a contenitori, macchine virtuali, computer, bare metal o PaaS senza riscrittura o riassemblaggio.

Questo articolo descrive i vantaggi principali dell'uso di Habitat.

## <a name="modernize-and-move-legacy-applications"></a>Eseguire lo spostamento e la modernizzazione delle applicazioni legacy

Con Habitat è possibile creare nuovi pacchetti per le applicazioni legacy, così che non sia più necessario usarle con sistemi operativi o middleware meno recenti. L'elemento ottenuto risulta sia portabile che facilmente riprogrammabile in infrastrutture più recenti, come le macchine virtuali o i contenitori in esecuzione nel cloud.

## <a name="accelerate-container-adoption"></a>Accelerare l'adozione di contenitore

Habitat risolve la distribuzione continua di applicazioni complesse e orientate ai microservizi rappresentando accuratamente le dipendenze di runtime. Permette inoltre di andare oltre la semplice distribuzione blu/verde dei singoli componenti e di progettare comportamenti di distribuzione sofisticati senza generare flussi di orchestrazione complessa.

## <a name="run-any-application-anywhere"></a>Eseguire qualsiasi applicazione da qualsiasi posizione

Con Habitat le applicazioni possono essere eseguite senza modifiche in qualsiasi ambiente di runtime. Sono supportati tutti gli ambienti, da macchine virtuali e ambienti bare metal a contenitori, come Docker, sistemi di gestione cluster, come Mesosphere o Kubernetes, e sistemi PaaS, come VMware Tanzu Application Service (in precedenza Pivotal Cloud Foundry).

## <a name="integrate-into-the-chef-devops-workflow"></a>Integrazione nel flusso di lavoro DevOps di Chef

Il progetto Habitat è parte di un progetto open source del software Chef, che vanta una forte community di supporto. Habitat si avvale dell'esperienza di Chef nell'automazione di infrastrutture per offrire alle applicazioni funzionalità di automazione senza precedenti. Chef gestisce il supporto commerciale per Habitat e garantisce un'integrazione uniforme tra Habitat e Chef Automate per l'automatizzazione completa del ciclo di rilascio dell'applicazione, dallo sviluppo alla distribuzione.

## <a name="next-steps"></a>Passaggi successivi

* [Provare Habitat](https://www.habitat.sh/learn/)
