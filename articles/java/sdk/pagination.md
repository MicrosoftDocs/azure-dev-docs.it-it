---
title: Impaginazione e iterazione in Azure SDK per Java
description: Panoramica dei concetti relativi a Azure SDK per Java correlati alla paginazione e all'iterazione
author: anuchandy
ms.date: 02/02/2021
ms.topic: conceptual
ms.custom: devx-track-java
ms.author: anuchan
ms.openlocfilehash: 0cb9e519931d24dcd97aa4a52742974427df5969
ms.sourcegitcommit: 71847ee0a1fee3f3320503629d9a8c82319a1f6a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/03/2021
ms.locfileid: "99522105"
---
# <a name="pagination-and-iteration-in-the-azure-sdk-for-java"></a>Impaginazione e iterazione in Azure SDK per Java

Questo articolo fornisce una panoramica su come usare Azure SDK per le funzionalità di impaginazione e iterazione Java per lavorare in modo efficiente e produttivo con set di dati di grandi dimensioni.

Molte operazioni fornite dalle librerie client nell'SDK Java di Azure restituiscono più di un risultato. Azure Java SDK definisce un set di tipi restituiti accettabili in questi casi per garantire che l'esperienza degli sviluppatori venga massimizzata tramite la coerenza. I tipi restituiti usati sono `PagedIterable` per le API di sincronizzazione e `PagedFlux` per le API asincrone. Le API differiscono leggermente per conto dei diversi casi d'uso, ma concettualmente hanno gli stessi requisiti:

- Consente di eseguire facilmente l'iterazione di ogni elemento della raccolta singolarmente, ignorando la necessità di eseguire manualmente l'impaginazione o il rilevamento dei token di continuazione. Sia `PagedIterable` che `PagedFlux` rendono questa attività semplice eseguendo l'iterazione di una risposta impaginata deserializzata in un tipo specificato `T` . `PagedIterable` implementa l' `Iterable` interfaccia e offre un'API per ricevere un oggetto `Stream` , mentre `PagedFlux` fornisce un oggetto `Flux` . In tutti i casi, l'azione di impaginazione è trasparente e l'iterazione continua mentre è ancora in corso l'iterazione dei risultati.

- Consente di eseguire l'iterazione in modo esplicito pagina per pagina. Questa operazione consente di comprendere più chiaramente quando vengono effettuate le richieste e di accedere alle informazioni di risposta per pagina. `PagedIterable`E `PagedFlux` dispongono di metodi che restituiscono i tipi appropriati per eseguire l'iterazione per pagina, anziché per singolo elemento.

Questo articolo è suddiviso tra le API sincrone e asincrone di Java Azure SDK. Le API di iterazione sincrone vengono visualizzate quando si utilizzano client sincroni e le API di iterazione asincrona quando si utilizzano client asincroni.

## <a name="synchronous-pagination-and-iteration"></a>Paginazione sincrona e iterazione

In questa sezione vengono illustrate le API sincrone.

### <a name="iterate-over-individual-elements"></a>Scorrere i singoli elementi

Come indicato, il caso d'uso più comune è quello di eseguire un'iterazione su ogni elemento singolarmente anziché per pagina. Gli esempi di codice seguenti illustrano come l' `PagedIterable` API consente di usare lo stile di iterazione che si preferisce implementare questa funzionalità.

#### <a name="use-a-for-each-loop"></a>Usare un ciclo for-each

Poiché `PagedIterable` implementa `Iterable` , è possibile scorrere gli elementi come illustrato nell'esempio seguente:

```java
PagedIterable<Secret> secrets = client.listSecrets();
for (Secret secret : secrets) {
   System.out.println("Secret is: " + secret);
}
```

#### <a name="use-stream"></a>USA flusso

Poiché `PagedIterable` è stato `stream()` definito un metodo, è possibile chiamarlo per usare le API di flusso Java standard, come illustrato nell'esempio seguente:

```java
client.listSecrets()
      .stream()
      .forEach(secret -> System.out.println("Secret is: " + secret));
```

#### <a name="use-iterator"></a>USA iteratore

Poiché `PagedIterable` implementa `Iterable` , dispone anche di un `iterator()` metodo per consentire lo stile di programmazione iteratore Java, come illustrato nell'esempio seguente:

```java
Iterator<Secret> secrets = client.listSecrets().iterator();
while (it.hasNext()) {
   System.out.println("Secret is: " + it.next());
}
```

### <a name="iterate-over-pages"></a>Scorrere le pagine

Quando si utilizzano singole pagine, è possibile eseguire l'iterazione per pagina, ad esempio quando sono necessarie informazioni sulla risposta HTTP o quando i token di continuazione sono importanti per conservare la cronologia delle iterazioni. Indipendentemente dall'iterazione per pagina o da ogni elemento, non esiste alcuna differenza di prestazioni o il numero di chiamate effettuate al servizio. L'implementazione sottostante carica la pagina successiva su richiesta e, se si annulla la sottoscrizione `PagedFlux` in qualsiasi momento, non sono presenti altre chiamate al servizio.

#### <a name="use-a-for-each-loop"></a>Usare un ciclo for-each

Quando si chiama `listSecrets()` , si ottiene un oggetto `PagedIterable` che ha un' `iterableByPage()` API. Questa API genera un oggetto `Iterable<PagedResponse<Secret>>` anziché un oggetto `Iterable<Secret>` . `PagedResponse`Fornisce i metadati della risposta e l'accesso al token di continuazione, come illustrato nell'esempio seguente:

```java
Iterable<PagedResponse<Secret>> secretPages = client.listSecrets().iterableByPage();
for (PagedResponse<Secret> page : secretPages) {
   System.out.println("Response code: " + page.getStatusCode());
   System.out.println("Continuation Token: " + page.getContinuationToken());
   page.getElements().forEach(secret -> System.out.println("Secret value: " + secret))
}
```

Esiste anche un `iterableByPage` Overload che accetta un token di continuazione. È possibile chiamare questo overload quando si desidera tornare allo stesso punto di iterazione in un secondo momento.

#### <a name="use-stream"></a>USA flusso

Nell'esempio seguente viene illustrato come il `streamByPage()` metodo esegue la stessa operazione, come illustrato in precedenza. Questa API dispone anche di un overload del token di continuazione per tornare allo stesso punto di iterazione in un secondo momento.

```java
client.listSecrets()
      .streamByPage()
      .forEach(page -> {
          System.out.println("Response code: " + page.getStatusCode());
          System.out.println("Continuation Token: " + page.getContinuationToken());
          page.getElements().forEach(secret -> System.out.println("Secret value: " + secret))
      });
```

## <a name="asynchronously-observe-pages-and-individual-elements"></a>Osservare in modo asincrono pagine e singoli elementi

In questa sezione vengono illustrate le API asincrone. Nelle API asincrone, le chiamate di rete si verificano in un thread diverso rispetto al thread principale che chiama `subscribe()` . Ciò significa che il thread principale può terminare prima che il risultato sia disponibile. È necessario assicurarsi che l'applicazione non venga chiusa prima del completamento dell'operazione asincrona.

### <a name="observe-individual-elements"></a>Osservare singoli elementi

Nell'esempio seguente viene illustrato come l' `PagedFlux` API consente di osservare i singoli elementi in modo asincrono. Esistono diversi modi per sottoscrivere un tipo Flux. Per altre informazioni, vedere [semplici modi per creare un flusso o mono e sottoscriverlo](https://projectreactor.io/docs/core/release/reference/#_simple_ways_to_create_a_flux_or_mono_and_subscribe_to_it) nella [Guida di riferimento a Reactor 3](https://projectreactor.io/docs/core/release/reference). Questo esempio è una varietà con tre espressioni lambda, una per ogni utente, il consumer di errore e il consumer completo. L'uso di tutte e tre le procedure è una procedura consigliata, ma in alcuni casi è necessario avere solo il consumer e, possibilmente, il consumer degli errori.

 ```java
asyncClient.listSecrets()
    .subscribe(secret -> System.out.println("Secret value: " + secret),
        ex -> System.out.println("Error listing secrets: " + ex.getMessage()),
        () -> System.out.println("Successfully listed all secrets"));
 ```

### <a name="observe-pages"></a>Osservare le pagine

 Nell'esempio seguente viene illustrato come l' `PagedFlux` API consente di osservare ogni pagina in modo asincrono, usando un' `byPage()` API e fornendo un consumer, un consumer di errore e un consumer di completamento.

  ```java
asyncClient.listSecrets().byPage()
    .subscribe(page -> {
            System.out.println("Response code: " + page.getStatusCode());
            System.out.println("Continuation Token: " + page.getContinuationToken());
            page.getElements().forEach(secret -> System.out.println("Secret value: " + secret))
        },
        ex -> System.out.println("Error listing pages with secret: " + ex.getMessage()),
        () -> System.out.println("Successfully listed all pages with secret"));
 ```

## <a name="next-steps"></a>Passaggi successivi

Ora che si ha familiarità con la paginazione e l'iterazione in Azure SDK per Java, provare a esaminare le [operazioni con esecuzione prolungata in Azure SDK per Java](lro.md). Le operazioni a esecuzione prolungata sono operazioni che vengono eseguite per un periodo più lungo rispetto alla maggior parte delle normali richieste HTTP, in genere perché richiedono un certo impegno sul lato server.
