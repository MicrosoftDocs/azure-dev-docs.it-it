---
title: Note sulla versione delle librerie di gestione di Azure per Java | Microsoft Docs
description: Informazioni sulle novità e sulle modifiche di rilievo nelle librerie di gestione di Azure per Java
keywords: Azure, Java, API, informazioni di riferimento, note, aggiornamenti, deprecare
ms.assetid: 1f128cf9-f747-4344-84e1-f9964709deb8
ms.topic: reference
ms.date: 3/06/2016
ms.openlocfilehash: 69c2c29935f9333dd1d31b26b0941e0446ca5504
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/05/2020
ms.locfileid: "82105202"
---
# <a name="release-notes"></a>Note sulla versione 

## <a name="october-5-2017---130"></a>5 ottobre 2017 - 1.3.0 

La versione 1.3.0 è compatibile con le versioni precedenti per l'uso di funzionalità e servizi per cui è stata raggiunta la fase di disponibilità generale (stabile) nelle versioni precedenti.

Eventuali modifiche significative rispetto alle versioni di anteprima per tali servizi sono contrassegnate con l'annotazione @Beta.


### <a name="generally-available-in-v13"></a>Disponibile a livello generale nella versione 1.3

Alcune API disponibili come Beta nelle versioni precedenti sono ora disponibili a livello generale, in particolare:

- Metodi asincroni
- Tutti i metodi della rete CDN disponibili in precedenza come versione Beta
- Tutti i metodi e tutte le interfacce nei gateway applicazione disponibili in precedenza come versione Beta

  Alcune parti della libreria sono ancora disponibili in anteprima. Per informazioni sullo stato attuale delle librerie, vedere la tabella seguente:

Servizio o funzionalità | Disponibile a livello generale | Disponibile come anteprima 
---------|---------|---------|-
Calcolo  | Macchine virtuali ed estensioni di VM, set di scalabilità di macchine virtuali, Managed Disks   | servizio Azure Container, Registro Azure Container 
Archiviazione   |  Account di archiviazione       |    Crittografia     
Database SQL  | Database, firewall, pool elastici              
Rete    |  Reti virtuali, interfacce di rete, indirizzi IP, tabelle di routing, gruppi di sicurezza di rete, DNS, strumenti di gestione traffico, gateway applicazione  |    Servizi di bilanciamento del carico, peering reti, gateway di rete virtuale, watcher di rete 
Altri servizi    |  Resource Manager, Key Vault, Redis, rete CDN, Batch       |  App Web, app per le funzioni, bus di servizio, controllo degli accessi in base al ruolo per Graph, Cosmos DB, Ricerca  
Nozioni fondamentali     |   Autenticazione: di base, metodi asincroni, identità del servizio gestito      |      |

> Le funzionalità in anteprima sono contrassegnate con l'annotazione `@Beta` a livello di classe, di interfaccia o di metodo nelle librerie. Queste funzionalità sono soggette a modifiche. In futuro potrebbero essere modificate in qualsiasi modo o addirittura rimosse.

### <a name="import-with-maven"></a>Eseguire l'importazione con Maven

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure</artifactId>
    <version>1.3.0</version>
</dependency>
```

### <a name="get-help-and-give-feedback"></a>Ottenere supporto e inviare commenti

Per ottenere supporto per l'uso delle librerie nel codice, visitare la community di [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-java-sdk). Se si rilevano bug o se si vogliono inviare suggerimenti per migliorare le librerie, inviare commenti tramite [GitHub](https://github.com/Azure/azure-sdk-for-java/issues).


