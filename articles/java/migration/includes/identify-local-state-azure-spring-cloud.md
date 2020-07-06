---
author: yevster
ms.author: yebronsh
ms.date: 5/28/2020
ms.openlocfilehash: 70f54f911d97febf30b78888e6922e2d4ac3eb54
ms.sourcegitcommit: 81577378a4c570ced1e9c6765f4a9eee8453c889
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/08/2020
ms.locfileid: "84512620"
---
#### <a name="identify-local-state"></a>Identificare lo stato locale

Negli ambienti PaaS, non è garantito che un'applicazione venga eseguita esattamente una volta in un determinato momento. Anche se si configura un'applicazione per l'esecuzione in una singola istanza, è possibile che venga creata un'istanza duplicata nei casi seguenti:

* L'applicazione deve essere riallocata in un host fisico a causa di un errore o di un aggiornamento di sistema.
* L'applicazione viene aggiornata.

In uno di questi casi, l'istanza originale rimarrà in esecuzione fino al completamento dell'avvio della nuova istanza. Questo comportamento presenta implicazioni potenzialmente significative per l'applicazione:

* Non è possibile garantire che un [singleton](https://en.wikipedia.org/wiki/Singleton_pattern) sia effettivamente singolo.
* Tutti i dati che non sono stati salvati in modo permanente in un archivio esterno andranno probabilmente persi molto prima rispetto a quanto avverrebbe in un singolo server fisico o in una VM.

Prima di eseguire la migrazione ad Azure Spring Cloud, assicurarsi che il codice non contenga uno stato locale che non deve andare perso o non deve essere duplicato. Se lo stato locale esiste, cambiare il codice per archiviarlo all'esterno dell'applicazione. Lo stato delle applicazioni predisposte per il cloud viene in genere archiviato in posizioni come le seguenti:

* [Cache Redis di Azure](/azure/azure-cache-for-redis/cache-java-get-started)
* [Azure CosmosDB](/azure/cosmos-db/create-sql-api-java)
* Un altro database esterno, ad esempio [SQL di Azure](/azure/azure-sql/azure-sql-iaas-vs-paas-what-is-overview), [Database di Azure per MySQL](/azure/mysql/overview) o [Database di Azure per PostgreSQL](/azure/postgresql/overview).
