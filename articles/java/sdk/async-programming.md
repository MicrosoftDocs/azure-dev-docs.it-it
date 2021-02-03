---
title: Programmazione asincrona in Azure SDK per Java
description: Panoramica del modello di programmazione asincrona in Azure SDK per Java
author: srnagar
ms.date: 02/02/2021
ms.topic: conceptual
ms.custom: devx-track-java
ms.author: srnagar
ms.openlocfilehash: 64a692b10175a7c08ff8a32e56a638209f239c19
ms.sourcegitcommit: 71847ee0a1fee3f3320503629d9a8c82319a1f6a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/03/2021
ms.locfileid: "99522134"
---
# <a name="asynchronous-programming-in-the-azure-sdk-for-java"></a>Programmazione asincrona in Azure SDK per Java

Questo articolo descrive il modello di programmazione asincrona in Azure SDK per Java.

Azure SDK conteneva inizialmente solo API asincrone non bloccanti per interagire con i servizi di Azure. Queste API consentono di usare Azure SDK per compilare applicazioni scalabili che usano le risorse di sistema in modo efficiente. Tuttavia, [Azure SDK per Java](https://github.com/Azure/azure-sdk-for-java#client-new-releases) contiene anche client sincroni per soddisfare un pubblico più ampio e rende anche le librerie client accessibili per gli utenti che non hanno familiarità con la programmazione asincrona. Vedere [approachable](https://azure.github.io/azure-sdk/general_introduction.html#approachable) nelle linee guida di progettazione di Azure SDK. Di conseguenza, tutte le librerie client Java in Azure SDK per Java offrono client sincroni e asincroni. È tuttavia consigliabile usare i client asincroni per i sistemi di produzione per ottimizzare l'uso delle risorse di sistema.

## <a name="reactive-streams"></a>Flussi reattivi

Se si esamina la sezione [client di servizi asincroni](https://azure.github.io/azure-sdk/java_introduction.html#async-service-clients) nelle [linee guida di progettazione di Java Azure SDK](https://azure.github.io/azure-sdk/java_introduction.html), si noterà che, anziché usare `CompletableFuture` fornito da Java 8, le API asincrone usano i tipi reattivi. Perché sono stati scelti i tipi reattivi rispetto ai tipi disponibili in modo nativo in JDK?

In Java 8 sono state introdotte funzionalità quali i [flussi](https://docs.oracle.com/javase/8/docs/api/java/util/stream/package-summary.html), le [espressioni lambda](https://docs.oracle.com/javase/tutorial/java/javaOO/lambdaexpressions.html)e [CompletableFuture](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/CompletableFuture.html). Queste funzionalità offrono molte funzionalità, ma presentano alcune limitazioni.

`CompletableFuture` fornisce funzionalità basate su callback e non bloccanti e l' `CompletionStage` interfaccia consentita per una semplice composizione di una serie di operazioni asincrone. Le espressioni lambda rendono più leggibili queste API basate su push. I flussi forniscono operazioni in stile funzionale per la gestione di una raccolta di elementi di dati. Tuttavia, i flussi sono sincroni e non possono essere riutilizzati. `CompletableFuture` consente di eseguire una singola richiesta, fornisce il supporto per un callback e prevede una _singola_ risposta. Molti servizi cloud, tuttavia, richiedono la possibilità di trasmettere Hub eventi dati per l'istanza.

I flussi reattivi possono contribuire a superare queste limitazioni mediante il flusso di elementi da un'origine a un Sottoscrittore. Quando un Sottoscrittore richiede dati da un'origine, l'origine invia un numero qualsiasi di risultati. Non è necessario inviarli tutti contemporaneamente. Il trasferimento avviene in un determinato periodo di tempo, ogni volta che l'origine dispone di dati da inviare.

In questo modello, il Sottoscrittore registra i gestori eventi per elaborare i dati all'arrivo. Queste interazioni basate su Push inviano notifiche al Sottoscrittore tramite segnali distinti:

- Una `onSubscribe()` chiamata indica che il trasferimento dei dati sta per iniziare.
- Una `onError()` chiamata indica che si è verificato un errore, che contrassegna anche la fine del trasferimento dei dati.
- Una `onComplete()` chiamata indica il corretto completamento del trasferimento dei dati.

A differenza dei flussi Java, i flussi reattivi considerano gli errori come eventi di prima classe. I flussi reattivi dispongono di un canale dedicato che consente all'origine di comunicare gli eventuali errori nel Sottoscrittore. Inoltre, i flussi reattivi consentono al Sottoscrittore di negoziare la velocità di trasferimento dei dati per trasformare questi flussi in un modello push-pull.

La specifica [Reactive Streams](https://github.com/reactive-streams/reactive-streams-jvm#reactive-streams) fornisce uno standard per il modo in cui deve essere eseguito il trasferimento dei dati. A un livello elevato, la specifica definisce le quattro interfacce seguenti e specifica le regole per la modalità di implementazione di tali interfacce.

- **Publisher** è l'origine di un flusso di dati.
- Il **Sottoscrittore** è il consumer di un flusso di dati.
- La **sottoscrizione** gestisce lo stato del trasferimento dei dati tra un server di pubblicazione e un Sottoscrittore.
- Il **processore** è sia un server di pubblicazione che un Sottoscrittore.

Sono disponibili alcune librerie Java note che forniscono implementazioni di questa specifica, ad esempio [RxJava](https://github.com/ReactiveX/RxJava), [Akka Streams](https://doc.akka.io/docs/akka/current/stream/stream-introduction.html), [Vert. x](https://vertx.io/docs/#reactive)e [Project Reactor](https://projectreactor.io/docs/core/release/reference/).

Azure SDK per Java ha adottato il Reactor del progetto per offrire le proprie API asincrone. Il fattore principale che determina questa decisione consiste nel fornire un'integrazione uniforme con [Spring webflux](https://docs.spring.io/spring/docs/current/spring-framework-reference/web-reactive.html), che usa anche il reattore del progetto. Un altro fattore che contribuisce a scegliere progetto Reactor rispetto a RxJava era che il Reactor del progetto utilizza Java 8 ma RxJava, al momento, era ancora in Java 7. Il Reactor del progetto offre anche un set completo di operatori componibili e consente di scrivere codice dichiarativo per la creazione di pipeline di elaborazione dati. Un altro aspetto interessante del reattore del progetto è che include adattatori per la conversione dei tipi di reattori di progetto in altri tipi di implementazione comuni

## <a name="comparing-apis-of-synchronous-and-asynchronous-operations"></a>Confronto tra API di operazioni sincrone e asincrone

Sono state illustrate le opzioni e i client sincroni per i client asincroni. La tabella seguente riepiloga le API che hanno un aspetto simile a quelle progettate usando le opzioni seguenti:

| Tipo di API                                           | Nessun valore                   | Valore singolo            | Più valori                         |
|----------------------------------------------------|----------------------------|-------------------------|-------------------------------|
| API Java sincrone standard                   | `void`                     | `T`                     | `Iterable<T>`                 |
| Java standard-API asincrone                  | `CompletableFuture<Void>`  | `CompletableFuture<T>`  | `CompletableFuture<List<T>>`   |
| Interfacce di flussi reattive                        | `Publisher<Void>`          | `Publisher<T>`          | `Publisher<T>`                 |
| Implementazione del Reactor di progetto dei flussi reattivi | `Mono<Void>`               | `Mono<T>`               | `Flux<T>`                      |

Per motivi di completezza, vale la pena ricordare che in Java 9 è stata introdotta la classe [Flow](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/concurrent/Flow.html) che include le quattro interfacce di flussi reattive. Tuttavia, questa classe non include alcuna implementazione.

## <a name="use-async-apis-in-the-azure-sdk-for-java"></a>Usare le API asincrone in Azure SDK per Java

La specifica Reactive Streams non distingue tra i tipi di autori. Nella specifica Reactive Streams, i Publisher producono semplicemente zero o più elementi di dati. In molti casi, esiste una distinzione utile tra un server di pubblicazione che produce al massimo un elemento dati rispetto a uno che produce zero o più elementi. Nelle API basate sul cloud questa distinzione indica se una richiesta restituisce una risposta a valore singolo o una raccolta. Il Reactor del progetto fornisce due tipi per distinguere tra [mono](https://projectreactor.io/docs/core/release/api/reactor/core/publisher/Mono.html) e [Flux](https://projectreactor.io/docs/core/release/api/reactor/core/publisher/Flux.html). Un'API che restituisce un oggetto conterrà `Mono` una risposta che ha al massimo un valore e un'API che restituisce un oggetto conterrà `Flux` una risposta con zero o più valori.

Si supponga, ad esempio, di usare un [ConfigurationAsyncClient](/java/api/com.azure.data.appconfiguration.configurationasyncclient) per recuperare una configurazione archiviata usando il servizio di configurazione app Azure. Per ulteriori informazioni, vedere [che cos'è app Azure configurazione?](/azure/azure-app-configuration/overview).)

Se si crea un oggetto `ConfigurationAsyncClient` e si chiama `getConfigurationSetting()` sul client, viene restituito un oggetto `Mono` , che indica che la risposta contiene un singolo valore. Tuttavia, la chiamata da solo questo metodo non esegue alcuna operazione. Il client non ha ancora effettuato una richiesta al servizio di configurazione app Azure. In questa fase, l'oggetto `Mono<ConfigurationSetting>` restituito da questa API è semplicemente un "assembly" della pipeline di elaborazione dati. Ciò significa che la configurazione necessaria per l'utilizzo dei dati è stata completata. Per attivare effettivamente il trasferimento dei dati, ovvero per eseguire la richiesta al servizio e ottenere la risposta, è necessario sottoscrivere l'oggetto restituito `Mono` . Quindi, quando si gestiscono questi flussi reattivi, è necessario ricordare di chiamare `subscribe()` perché non accade nulla fino a quando non si esegue questa operazione.

Nell'esempio seguente viene illustrato come sottoscrivere `Mono` e stampare il valore di configurazione nella console.

```java
ConfigurationAsyncClient asyncClient = new ConfigurationClientBuilder()
    .connectionString("<your connection string>")
    .buildAsyncClient();

asyncClient.getConfigurationSetting("<your config key>", "<your config value>").subscribe(
    config -> System.out.println("Config value: " + config.getValue()),
    ex -> System.out.println("Error getting configuration: " + ex.getMessage()),
    () -> System.out.println("Successfully retrieved configuration setting"));

System.out.println("Done");
```

Si noti che dopo aver chiamato `getConfigurationSetting()` sul client, il codice di esempio sottoscrive il risultato e fornisce tre espressioni lambda separate. La prima espressione lambda utilizza i dati ricevuti dal servizio, che viene attivato al completamento della risposta. La seconda espressione lambda viene attivata se si verifica un errore durante il recupero della configurazione. La terza espressione lambda viene richiamata quando il flusso di dati è completo, ovvero non sono previsti altri elementi dati da questo flusso.

> [!NOTE]
> Come per tutta la programmazione asincrona, dopo la creazione della sottoscrizione, l'esecuzione continua come di consueto. Se non sono presenti elementi per il programma attivo ed eseguito, può terminare prima del completamento dell'operazione asincrona. Il thread principale che `subscribe()` ha chiamato non attende fino a quando non si effettua la chiamata di rete per app Azure configurazione e si riceve una risposta. Nei sistemi di produzione, è possibile continuare a elaborare altri elementi, ma in questo esempio è possibile aggiungere un piccolo ritardo chiamando `Thread.sleep()` o usare un `CountDownLatch` per fornire all'operazione asincrona la possibilità di essere completati.

Come illustrato nell'esempio seguente, le API che restituiscono un oggetto `Flux` seguono anche un modello simile. La differenza è che il primo callback fornito al `subscribe()` metodo viene chiamato più volte per ogni elemento dati nella risposta. L'errore o i callback di completamento vengono chiamati esattamente una volta e vengono considerati come segnali terminali. Non vengono richiamati altri callback se uno di questi segnali viene ricevuto dal server di pubblicazione.

```java
EventHubConsumerAsyncClient asyncClient = new EventHubClientBuilder()
    .connectionString("<your connection string>")
    .consumerGroup("<your consumer group>")
    .buildAsyncConsumerClient();

asyncClient.receive().subscribe(
    event -> System.out.println("Sequence number of received event: " + event.getData().getSequenceNumber()),
    ex -> System.out.println("Error receiving events: " + ex.getMessage()),
    () -> System.out.println("Successfully completed receiving all events"));
```

### <a name="backpressure"></a>Congestione

Cosa accade quando l'origine sta producendo i dati a una velocità più elevata rispetto a quella che può essere gestita dal Sottoscrittore? Il Sottoscrittore può essere sopraffatto dai dati, che possono causare errori di memoria insufficiente. Il Sottoscrittore necessita di un modo per comunicare di nuovo al server di pubblicazione per rallentare quando non è possibile tenersi aggiornato. Per impostazione predefinita, quando si chiama `subscribe()` su un `Flux` come illustrato nell'esempio precedente, il Sottoscrittore richiede un flusso di dati non associato, che indica al server di pubblicazione di inviare i dati il più rapidamente possibile. Questo comportamento non è sempre auspicabile ed è possibile che il Sottoscrittore debba controllare la frequenza di pubblicazione tramite "backpressure". Il backpressure consente al Sottoscrittore di assumere il controllo del flusso di elementi di dati. Un Sottoscrittore richiederà un numero limitato di elementi di dati che è possibile gestire. Al termine dell'elaborazione di questi elementi da parte del Sottoscrittore, il Sottoscrittore può richiedere altro. Con la sovrapressione è possibile trasformare un modello push per il trasferimento dei dati in un modello push-pull.

Nell'esempio seguente viene illustrato come è possibile controllare la frequenza con cui gli eventi vengono ricevuti dal consumer di hub eventi:

```java
EventHubConsumerAsyncClient asyncClient = new EventHubClientBuilder()
    .connectionString("<your connection string>")
    .consumerGroup("<your consumer group>")
    .buildAsyncConsumerClient();

asyncClient.receive().subscribe(new Subscriber<PartitionEvent>() {
    private Subscription subscription;

    @Override
    public void onSubscribe(Subscription subscription) {
        this.subscription = subscription;
        this.subscription.request(1); // request 1 data element to begin with
    }

    @Override
    public void onNext(PartitionEvent partitionEvent) {
        System.out.println("Sequence number of received event: " + partitionEvent.getData().getSequenceNumber());
        this.subscription.request(1); // request another event when the subscriber is ready
    }

    @Override
    public void onError(Throwable throwable) {
        System.out.println("Error receiving events: " + throwable.getMessage());
    }

    @Override
    public void onComplete() {
        System.out.println("Successfully completed receiving all events")
    }
});
```

Quando il Sottoscrittore si connette per la prima volta al server di pubblicazione, il server di pubblicazione invia al Sottoscrittore un' `Subscription` istanza che gestisce lo stato del trasferimento dei dati. Si `Subscription` tratta del supporto tramite il quale il Sottoscrittore può applicare la sovrapressione chiamando `request()` per specificare il numero di elementi di dati che è in grado di gestire.

Se il Sottoscrittore richiede più di un elemento dati ogni volta che chiama `onNext()` , `request(10)` ad esempio, il server di pubblicazione invierà immediatamente i 10 elementi successivi se sono disponibili o quando diventano disponibili. Questi elementi si accumulano in un buffer nell'entità finale del Sottoscrittore e poiché ogni `onNext()` chiamata richiederà altre 10, il backlog continuerà a crescere fino a quando il server di pubblicazione non avrà più elementi di dati da inviare o l'overflow del buffer del Sottoscrittore, causando errori di memoria insufficiente.

### <a name="cancel-a-subscription"></a>Annullare una sottoscrizione

Una sottoscrizione gestisce lo stato del trasferimento dei dati tra un server di pubblicazione e un Sottoscrittore. La sottoscrizione è attiva fino a quando il server di pubblicazione non ha completato il trasferimento di tutti i dati nel Sottoscrittore o se il Sottoscrittore non è più interessato a ricevere dati. Esistono due modi per annullare una sottoscrizione, come illustrato di seguito.

Nell'esempio seguente la sottoscrizione viene annullata tramite l'eliminazione del Sottoscrittore:

```java
EventHubConsumerAsyncClient asyncClient = new EventHubClientBuilder()
    .connectionString("<your connection string>")
    .consumerGroup("<your consumer group>")
    .buildAsyncConsumerClient();

Disposable disposable = asyncClient.receive().subscribe(
    partitionEvent -> {
        Long num = partitionEvent.getData().getSequenceNumber()
        System.out.println("Sequence number of received event: " + num);
    },
    ex -> System.out.println("Error receiving events: " + ex.getMessage()),
    () -> System.out.println("Successfully completed receiving all events"));

// much later on in your code, when you are ready to cancel the subscription,
// you can call the dispose method, as such:
disposable.dispose();
```

Nell'esempio seguente la sottoscrizione viene annullata chiamando il `cancel()` metodo su `Subscription` :

```java
EventHubConsumerAsyncClient asyncClient = new EventHubClientBuilder()
    .connectionString("<your connection string>")
    .consumerGroup("<your consumer group>")
    .buildAsyncConsumerClient();

asyncClient.receive().subscribe(new Subscriber<PartitionEvent>() {
    private Subscription subscription;

    @Override
    public void onSubscribe(Subscription subscription) {
        this.subscription = subscription;
        this.subscription.request(1); // request 1 data element to begin with
    }

    @Override
    public void onNext(PartitionEvent partitionEvent) {
        System.out.println("Sequence number of received event: " + partitionEvent.getData().getSequenceNumber());
        this.subscription.cancel(); // Cancels the subscription. No further event is received.
    }

    @Override
    public void onError(Throwable throwable) {
        System.out.println("Error receiving events: " + throwable.getMessage());
    }

    @Override
    public void onComplete() {
        System.out.println("Successfully completed receiving all events")
    }
});
```

## <a name="conclusion"></a>Conclusione

I thread sono risorse costose che non devono essere sprecate in attesa di risposte dalle chiamate al servizio remoto. Con l'aumentare dell'adozione delle architetture di microservizi, la necessità di ridimensionare e usare le risorse in modo efficiente diventa essenziale. Le API asincrone sono favorevoli quando sono presenti operazioni associate alla rete. Azure SDK per Java offre un set completo di API per le operazioni asincrone che consentono di ottimizzare le risorse di sistema. Si consiglia vivamente di provare i nostri client asincroni.

Per altre informazioni sugli operatori più adatti alle attività specifiche, vedere [quale operatore è necessario?](https://projectreactor.io/docs/core/release/reference/#which-operator) nella Guida di riferimento a [Reactor 3](https://projectreactor.io/docs/core/release/reference/index.html).

## <a name="next-steps"></a>Passaggi successivi

Ora che è meglio comprendere i diversi concetti di programmazione asincrona, è importante imparare a scorrere i risultati. Per ulteriori informazioni sulle strategie di iterazione migliori e per informazioni dettagliate sul funzionamento della paginazione, vedere [impaginazione e iterazione in Azure SDK per Java](pagination.md).
