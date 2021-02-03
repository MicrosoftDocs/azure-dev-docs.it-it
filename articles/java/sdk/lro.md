---
title: Operazioni con esecuzione prolungata in Azure SDK per Java
description: Panoramica dei concetti relativi a Azure SDK per Java correlati a operazioni a esecuzione prolungata
author: anuchandy
ms.date: 02/02/2021
ms.topic: conceptual
ms.custom: devx-track-java
ms.author: anuchan
ms.openlocfilehash: a20fb7a638fefd0182bea89e3d5c4555442a4547
ms.sourcegitcommit: 71847ee0a1fee3f3320503629d9a8c82319a1f6a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/03/2021
ms.locfileid: "99522108"
---
# <a name="long-running-operations-in-the-azure-sdk-for-java"></a>Operazioni con esecuzione prolungata in Azure SDK per Java

Questo articolo fornisce una panoramica dell'uso di operazioni con esecuzione prolungata con Azure SDK per Java.

Alcune operazioni in Azure possono richiedere una quantità di tempo estesa per il completamento. Queste operazioni non rientrano nello stile HTTP standard del flusso di richiesta/risposta veloce. Ad esempio, la copia di dati da un URL di origine a un BLOB di archiviazione o il training di un modello per riconoscere i moduli, sono operazioni che potrebbero richiedere alcuni secondi a diversi minuti. Tali operazioni sono denominate Long-Running operazioni e sono spesso abbreviate come ' è. Un e può richiedere secondi, minuti, ore, giorni o più, a seconda dell'operazione richiesta e del processo che deve essere eseguito sul lato server.

Nelle librerie client Java per Azure, esiste una convenzione che tutte le operazioni a esecuzione prolungata iniziano con il `begin` prefisso. Questo prefisso indica che questa operazione è con esecuzione prolungata e che il mezzo di interazione con questa operazione è leggermente diverso da quello del normale flusso di richiesta/risposta. Insieme al `begin` prefisso, il tipo restituito dall'operazione è anche diverso dal consueto, per consentire l'intera gamma di funzionalità dell'operazione a esecuzione prolungata. Come per la maggior parte degli elementi in Azure SDK per Java, sono disponibili API sincrone e asincrone per le operazioni a esecuzione prolungata:

* Nei client sincroni, le operazioni a esecuzione prolungata restituiscono un' `SyncPoller` istanza di.
* Nei client asincroni, le operazioni a esecuzione prolungata restituiscono un' `PollerFlux` istanza di.

Sia `SyncPoller` che `PollerFlux` sono le astrazioni lato client progettate per semplificare l'interazione con le operazioni lato server a esecuzione prolungata. Nella parte restante di questo articolo vengono descritte le procedure consigliate per l'utilizzo di questi tipi.

## <a name="synchronous-long-running-operations"></a>Operazioni sincrone a esecuzione prolungata

La chiamata di qualsiasi API che restituisce un oggetto avvierà `SyncPoller` immediatamente l'operazione con esecuzione prolungata. L'API restituirà `SyncPoller` immediatamente, consentendo di monitorare lo stato di avanzamento dell'operazione a esecuzione prolungata e di recuperare il risultato finale. Nell'esempio seguente viene illustrato come monitorare lo stato di avanzamento di un'operazione con esecuzione prolungata utilizzando `SyncPoller` .

```java
SyncPoller<UploadBlobProgress, UploadedBlobProperties> poller = syncClient.beginUploadFromUri(<URI to upload from>)
PollResponse<UploadBlobProgress> response;

do {
    response = poller.poll();
    System.out.println("Status of long running upload operation: " + response.getStatus());
    Duration pollInterval = response.getRetryAfter();
    TimeUnit.MILLISECONDS.sleep(pollInterval.toMillis());
} while (!response.getStatus().isComplete());
```

In questo esempio viene utilizzato il `poll()` metodo su `SyncPoller` per recuperare informazioni sullo stato dell'operazione a esecuzione prolungata. Questo codice stampa lo stato nella console, ma un'implementazione migliore prenderà decisioni rilevanti in base a questo stato.

Il `getRetryAfter()` metodo restituisce informazioni sul tempo di attesa prima del polling successivo. La maggior parte delle operazioni con esecuzione prolungata di Azure restituisce il ritardo di polling come parte della risposta HTTP, ovvero l'intestazione di uso comune `retry-after` . Se la risposta non contiene il ritardo del polling, il `getRetryAfter()` metodo restituisce la durata specificata al momento della chiamata dell'operazione a esecuzione prolungata.

Nell'esempio precedente viene utilizzato un `do..while` ciclo per eseguire ripetutamente il polling fino al completamento dell'operazione a esecuzione prolungata. Se non si è interessati a questi risultati intermedi, è invece possibile chiamare `waitForCompletion()` . Questa chiamata bloccherà il thread corrente finché l'operazione a esecuzione prolungata non viene completata e restituisce l'ultima risposta di polling:

```java
PollResponse<UploadBlobProgress> response = poller.waitForCompletion();
```

Se l'ultima risposta di polling indica che l'operazione a esecuzione prolungata è stata completata correttamente, è possibile recuperare il risultato finale usando `getFinalResult()` :

```java
if (LongRunningOperationStatus.SUCCESSFULLY_COMPLETED == response.getStatus()) {
    UploadedBlobProperties result = poller.getFinalResult();
}
```

Altre API utili in `SyncPoller` includono:

1. `waitForCompletion(Duration)`: attendere il completamento dell'operazione a esecuzione prolungata per la durata di timeout specificata.
1. `waitUntil(LongRunningOperationStatus)`: attendere fino a quando non viene ricevuto lo stato dell'operazione a esecuzione prolungata specificata.
1. `waitUntil(LongRunningOperationStatus, Duration)`: attendere che venga ricevuto lo stato dell'operazione a esecuzione prolungata specificata o fino alla scadenza della durata del timeout specificata.

## <a name="asynchronous-long-running-operations"></a>Operazioni asincrone a esecuzione prolungata

Nell'esempio seguente viene illustrato come `PollerFlux` è possibile osservare un'operazione a esecuzione prolungata. Nelle API asincrone, le chiamate di rete si verificano in un thread diverso rispetto al thread principale che chiama `subscribe()` . Ciò significa che il thread principale può terminare prima che il risultato sia disponibile. È necessario assicurarsi che l'applicazione non venga chiusa prima del completamento dell'operazione asincrona.

L'API asincrona restituisce immediatamente un oggetto `PollerFlux` , ma l'operazione a esecuzione prolungata non viene avviata fino a quando non si sottoscrive `PollerFlux` . Questo processo rappresenta il `Flux` funzionamento di tutte le API basate su. Nell'esempio seguente viene illustrata un'operazione asincrona a esecuzione prolungata:

```java
asyncClient.beginUploadFromUri(...)
    .subscribe(response -> System.out.println("Status of long running upload operation: " + response.getStatus()));
```

Nell'esempio seguente si otterranno aggiornamenti di stato intermittenti per l'operazione a esecuzione prolungata. È possibile utilizzare questi aggiornamenti per determinare se l'operazione a esecuzione prolungata funziona ancora nel modo previsto. In questo esempio lo stato viene stampato nella console, ma un'implementazione migliore renderebbe le decisioni relative alla gestione degli errori in base a questo stato.

Se non si è interessati agli aggiornamenti di stato intermedi e si vuole solo ricevere una notifica del risultato finale all'arrivo, è possibile usare codice simile all'esempio seguente:

```java
asyncClient.beginUploadFromUri(...)
    .last()
    .flatMap(response -> {
        if (LongRunningOperationStatus.SUCCESSFULLY_COMPLETED == response.getStatus()) {
            return response.getFinalResult();
        }
        return Mono.error(new IllegalStateException("Polling completed unsuccessfully with status: "+ response.getStatus()));
    })
    .subscribe(
        finalResult -> processFormPages(finalResult),
        ex -> countDownLatch.countDown(),
        () -> countDownLatch.countDown());
```

In questo codice si recupera il risultato finale dell'operazione a esecuzione prolungata chiamando `last()` . Questa chiamata indica al `PollerFlux` che si vuole attendere il completamento di tutto il polling, a quel punto l'operazione a esecuzione prolungata ha raggiunto uno stato finale ed è possibile esaminarne lo stato per determinare il risultato. Se il Poller indica che l'operazione a esecuzione prolungata è stata completata correttamente, è possibile recuperare il risultato finale e passarlo al consumer nella chiamata di sottoscrizione.

## <a name="next-steps"></a>Passaggi successivi

Ora che si ha familiarità con le API con esecuzione prolungata in Azure SDK per Java, vedere [configurare proxy in Azure SDK per Java](proxying.md) per informazioni su come personalizzare ulteriormente il client http.
