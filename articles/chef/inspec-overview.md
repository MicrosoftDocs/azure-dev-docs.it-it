---
title: Usare InSpec per l'automazione della conformità dell'infrastruttura di Azure
description: Informazioni su come usare InSpec per rilevare problemi nelle distribuzioni di Azure
keywords: azure, chef, devops, macchine virtuali, panoramica, automazione, inspec
ms.date: 03/19/2019
ms.topic: article
ms.custom: devx-track-chef
ms.openlocfilehash: 0c50cd07473565609084db24b9e537519194a0c2
ms.sourcegitcommit: 16ce1d00586dfa9c351b889ca7f469145a02fad6
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/14/2020
ms.locfileid: "88240723"
---
# <a name="use-inspec-for-compliance-automation-of-your-azure-infrastructure"></a>Usare InSpec per l'automazione della conformità dell'infrastruttura di Azure

[InSpec](https://www.chef.io/inspec/) è il linguaggio open source di Chef per descrivere le regole di sicurezza e conformità che è possibile condividere tra gli ingegneri software, le operazioni e i tecnici di sicurezza. InSpec confronta lo stato reale dell'infrastruttura con lo stato desiderato, definito in codice InSpec di facile scrittura e lettura. InSpec rileva le violazioni e visualizza i risultati in un report, ma consente anche all'utente di assumere il controllo della correzione.

È possibile usare InSpec per convalidare lo stato delle risorse e dei gruppi di risorse in una sottoscrizione, tra cui macchine virtuali, configurazioni di rete, impostazioni di Azure Active Directory e altro ancora.

Questo articolo descrive i vantaggi dell'uso di InSpec per semplificare la sicurezza e la conformità in Azure.

## <a name="make-compliance-easy-to-understand-and-assess"></a>Semplificare la comprensione e la valutazione della conformità

I requisiti contenuti nella documentazione sulla conformità scritta in fogli di calcolo o in documenti Word sono soggetti a interpretazioni diverse. Con InSpec i requisiti operativi vengono trasformati in codice eseguibile, provvisto di versione e leggibile dall'utente. Il codice elimina le divergenze sugli elementi da valutare a favore di test tangibili con finalità chiare.

## <a name="detect-fleet-wide-issues-and-prioritize-their-remediation"></a>Rilevare i problemi estesi e assegnare priorità alla loro correzione

La modalità di rilevamento senza agenti di InSpec consente di valutare rapidamente e su larga scala il livello di esposizione. I metadati predefiniti per i punteggi di impatto/gravità consentono di determinare le aree sulle quali concentrare le correzioni. È anche possibile scrivere rapidamente regole in risposta a nuove vulnerabilità o normative e implementarle immediatamente.

## <a name="audit-azure-virtual-machines-with-policy-guest-configuration"></a>Controllare le macchine virtuali di Azure con Configurazione guest dei criteri

Azure supporta direttamente l'uso di definizioni di Chef InSpec per controllare macchine virtuali di Azure tramite [Configurazione guest di Criteri di Azure](https://docs.microsoft.com/azure/governance/policy/concepts/guest-configuration). La funzionalità Configurazione guest valuta una macchina virtuale Linux in base a una definizione di Chef InSpec specificata e segnala la conformità tramite Criteri di Azure. I risultati di questi controlli vengono segnalati anche tramite i log di Monitoraggio di Azure, abilitando gli avvisi e altri scenari di automazione.

## <a name="satisfy-audits"></a>Soddisfare i controlli

InSpec consente di rispondere alle richieste di controllo in qualsiasi momento e non solo a intervalli predeterminati, ad esempio con frequenza trimestrale o annuale. Con l'esecuzione continua di test InSpec, si inizia un ciclo di controllo sulla base di conoscenze dettagliate della cronologia e della postura di conformità, evitando così risultati inattesi derivanti dalle conclusioni di un revisore.

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"] 
> [Provare InSpec in Azure Cloud Shell](https://shell.azure.com)