---
title: Configurare la registrazione in Azure SDK per Java
description: Panoramica dei concetti relativi alla registrazione di Azure SDK per Java
author: srnagar
ms.date: 02/02/2021
ms.topic: conceptual
ms.custom: devx-track-java
ms.author: srnagar
ms.openlocfilehash: bbb9733a21eabf5d13a698a0785908dcd997af45
ms.sourcegitcommit: 71847ee0a1fee3f3320503629d9a8c82319a1f6a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/03/2021
ms.locfileid: "99522107"
---
# <a name="configure-logging-in-the-azure-sdk-for-java"></a>Configurare la registrazione in Azure SDK per Java

Questo articolo fornisce una panoramica su come abilitare la registrazione nelle applicazioni che usano Azure SDK per Java. Le librerie client di Azure per Java hanno due opzioni di registrazione:

* Un Framework di registrazione incorporato a scopo di debug temporaneo.
* Supporto per la registrazione tramite l'interfaccia [SLF4J](https://www.slf4j.org/) .

Si consiglia di usare SLF4J perché è noto nell'ecosistema Java ed è ben documentato. Per altre informazioni, vedere il [manuale dell'utente di SLF4J](https://www.slf4j.org/manual.html).

Questo articolo contiene collegamenti ad altri articoli che coprono molti dei framework di registrazione Java più diffusi. Questi altri articoli forniscono esempi di configurazione e descrivono in che modo le librerie client di Azure possono usare i Framework di registrazione.

Indipendentemente dalla configurazione di registrazione usata, lo stesso output del log è disponibile in entrambi i casi, perché tutto l'output di registrazione nelle librerie client di Azure per Java viene indirizzato tramite un'astrazione di Azure Core `ClientLogger` .

Il resto di questo articolo illustra in dettaglio la configurazione di tutte le opzioni di registrazione disponibili.

## <a name="default-logger-for-temporary-debugging"></a>Logger predefinito (per il debug temporaneo)

Come indicato, tutte le librerie client di Azure usano SLF4J per la registrazione, ma è presente un logger predefinito incorporato nelle librerie client di Azure per Java. Questo logger predefinito viene fornito per i casi in cui un'applicazione è stata distribuita ed è necessaria la registrazione, ma non è possibile ridistribuire l'applicazione con un logger di SLF4J incluso. Per abilitare questo logger, è innanzitutto necessario essere certi che non esista alcun logger SLF4J (poiché avrà la precedenza) e quindi impostare la `AZURE_LOG_LEVEL` variabile di ambiente. Nella tabella seguente sono illustrati i valori consentiti per la variabile di ambiente:

| Livello registro              | Valori delle variabili di ambiente consentiti     |
|------------------------|-----------------------------------------|
| DETTAGLIATO                | "verbose", "debug"                      |
| INFORMATIVO          | "info", "information", "informational"  |
| WARNING                | "warn", "warning"                       |
| ERRORE                  | "err", "error"                          |

Dopo aver impostato la variabile di ambiente, riavviare l'applicazione per rendere effettiva la variabile di ambiente. Questo logger registrerà nella console di e non fornirà le funzionalità di personalizzazione avanzate di un'implementazione di SLF4J, ad esempio il rollover e la registrazione su file. Per riattivare la disconnessione, è sufficiente rimuovere la variabile di ambiente e riavviare l'applicazione.

## <a name="slf4j-logging"></a>Registrazione SLF4J

Per impostazione predefinita, è consigliabile configurare la registrazione usando un Framework di registrazione supportato da SLF4J. In primo luogo, includere un'implementazione della registrazione di SLF4J pertinente come dipendenza dal progetto. Per altre informazioni, vedere [dichiarazione delle dipendenze di progetto per la registrazione](http://www.slf4j.org/manual.html#projectDep) nel manuale dell'utente di SLF4J. Configurare quindi il logger in modo che funzioni come necessario nell'ambiente in uso, ad esempio l'impostazione dei livelli di registrazione, la configurazione delle classi che eseguono e non registrano e così via. Alcuni esempi vengono forniti tramite i collegamenti seguenti, ma per maggiori dettagli, vedere la documentazione relativa al Framework di registrazione scelto.

## <a name="next-steps"></a>Passaggi successivi

Ora che si è appreso come funziona la registrazione in Azure SDK per Java, provare a esaminare i collegamenti riportati di seguito per istruzioni su come configurare alcuni dei framework di registrazione Java più diffusi per lavorare con SLF4J e le librerie client Java:

* [Java. util. Logging](logging-jul.md)
* [Logback](logging-logback.md)
* [Log4J](logging-log4j.md)
