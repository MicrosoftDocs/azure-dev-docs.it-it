---
title: Motivi per passare a Java 11
titleSuffix: Azure
description: Documento riepilogativo destinato ai decision maker che valutano i vantaggi derivanti dal passaggio da Java 8 a Java 11.
author: dsgrieve
manager: maverbur
tags: java
ms.topic: article
ms.date: 11/19/2019
ms.author: dagrieve
ms.openlocfilehash: c0a2f46f8a3249f6c9580e823e102a86291e15e7
ms.sourcegitcommit: 0af39ee9ff27c37ceeeb28ea9d51e32995989591
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/21/2020
ms.locfileid: "81670637"
---
# <a name="reasons-to-move-to-java-11"></a>Motivi per passare a Java 11

La domanda da porsi non è *se* è necessario passare a Java 11, ma *quando*. Nei prossimi anni, Java 8 non sarà più supportato e gli utenti dovranno passare a Java 11. Il passaggio a Java 11 offre diversi vantaggi ed è auspicabile completarlo il prima possibile.

Rispetto a Java 8 sono state aggiunte nuove funzionalità e sono state effettuate ottimizzazioni. Sono state apportate aggiunte e modifiche di rilievo all'API e sono state introdotte ottimizzazioni per migliorare l'avvio, le prestazioni e l'utilizzo della memoria.

## <a name="transitioning-to-java-11"></a>Transizione a Java 11

La transizione a Java 11 può essere eseguita in maniera graduale. Per l'esecuzione in Java 11, *non* è necessario che il codice usi i moduli Java. È possibile usare Java 11 per eseguire codice sviluppato e compilato con JDK 8.
Esistono tuttavia alcuni potenziali problemi, soprattutto per quanto riguarda l'API deprecata, i caricatori di classe e la reflection.

Il Microsoft Java Engineering Group ha definito una guida per la [transizione da Java 8 a Java 11](./transition-from-java-8-to-java-11.md). [Java Platform, Standard Edition Oracle JDK 9 Migration Guide](https://docs.oracle.com/javase/9/migrate/toc.htm) e [The State of the Module System: Compatibility and Migration](http://openjdk.java.net/projects/jigsaw/spec/sotms/#compatibility--migration) sono altre guide utili. 

## <a name="high-level-changes-between-java-8-and-11"></a>Modifiche di rilievo tra Java 8 e 11

In questa sezione non vengono enumerate tutte le modifiche apportate nelle versioni Java 9 \[[1](#ref1)\], 10 \[[2](#ref2)\] e 11 \[[3](#ref3)\]. Vengono evidenziate solo le modifiche che incidono su prestazioni, diagnostica e produttività.

### <a name="modules-4"></a>Moduli \[[4](#ref4)\]

I moduli risolvono i problemi di configurazione e incapsulamento difficili da gestire nelle applicazioni su larga scala eseguite in *classpath*. Un *modulo* è una raccolta autodescrittiva di classi e interfacce Java e di risorse correlate.

I moduli consentono di personalizzare le configurazioni di runtime che contengono solo i componenti richiesti da un'applicazione. Questa personalizzazione crea un footprint ridotto e consente di collegare un'applicazione in modo statico, tramite [jlink](https://docs.oracle.com/en/java/javase/11/tools/jlink.html), a un runtime personalizzato per la distribuzione. Questo footprint ridotto può essere particolarmente utile in un'architettura di microservizi.

Internamente, la JVM è in grado di sfruttare i vantaggi dei moduli in modo da rendere più efficiente il caricamento delle classi. Il risultato è un runtime più piccolo, più leggero e più veloce da avviare. Le tecniche di ottimizzazione usate dalla JVM per migliorare le prestazioni delle applicazioni possono risultare più efficaci perché i moduli codificano i componenti richiesti da una classe.

Per i programmatori, i moduli consentono di applicare un incapsulamento avanzato richiedendo la dichiarazione esplicita dei pacchetti esportati da un modulo e dei componenti necessari, oltre a limitare l'accesso riflettente.
Questo livello di incapsulamento rende un'applicazione più sicura e più semplice da gestire.

L'applicazione può continuare a usare *classpath* e non è necessario eseguire la transizione ai moduli come requisito per l'esecuzione in Java 11.

### <a name="profiling-and-diagnostics"></a>Profilatura e diagnostica

#### <a name="java-flight-recorder-5"></a>Java Flight Recorder \[[5](#ref5)\]

Java Flight Recorder (JFR) raccoglie dati di diagnostica e profilatura da un'applicazione Java in esecuzione. JFR ha un effetto minimo su un'applicazione Java in esecuzione. I dati raccolti possono quindi essere analizzati con Java Mission Control (JMC) e altri strumenti. Mentre in Java 8 JFR e JMC sono funzionalità commerciali, in Java 11 sono open source.

#### <a name="java-mission-control-6"></a>Java Mission Control \[[6](#ref6)\]

Java Mission Control (JMC) fornisce una visualizzazione grafica dei dati raccolti da Java Flight Recorder (JFR) ed è open source in Java
11. Oltre alle informazioni generali sull'applicazione in esecuzione, JMC consente all'utente di eseguire il drill-down dei dati. JFR e JMC si possono usare per diagnosticare problemi di runtime, ad esempio perdite di memoria, sovraccarico di GC, metodi caldi, colli di bottiglia dei thread e I/O bloccanti.

#### <a name="unified-logging-7"></a>Registrazione unificata \[[7](#ref7)\]

Java 11 include un sistema di registrazione comune per tutti i componenti della JVM.
Questo sistema di registrazione unificata consente all'utente di definire quali componenti registrare e a quale livello. La registrazione con granularità fine è utile per eseguire l'analisi della causa radice negli arresti anomali della JVM e per diagnosticare i problemi di prestazioni in un ambiente di produzione.

#### <a name="low-overhead-heap-profiling-8"></a>Low-Overhead Heap Profiling \[[8](#ref8)\]

In Java Virtual Machine Tool Interface (JVMTI) è stata aggiunta una nuova API per il campionamento delle allocazioni dell'heap Java. Il campionamento presenta un sovraccarico ridotto e può essere abilitato in modo continuo. Mentre l'allocazione dell'heap può essere monitorata con Java Flight Recorder (JFR), il metodo di campionamento in JFR funziona solo con le allocazioni. L'implementazione di JFR potrebbe anche perdere allocazioni. Al contrario, il campionamento dell'heap in Java 11 può fornire informazioni su oggetti attivi e inattivi.

I fornitori di Application Performance Monitoring (APM) stanno iniziando a usare questa nuova piattaforma e il Java Engineering Group ne sta esaminando il potenziale uso con gli strumenti di monitoraggio delle prestazioni di Azure.

#### <a name="stackwalker-9"></a>StackWalker \[[9](#ref9)\]

Durante la registrazione viene spesso usata l'acquisizione di uno snapshot dello stack per il thread corrente. Il problema riguarda la quantità di dati dell'analisi dello stack da registrare e la decisione se eseguire effettivamente questa registrazione o meno. Ad esempio, è possibile che si voglia visualizzare l'analisi dello stack solo per una determinata eccezione di un metodo. La classe StackWalker (aggiunta in Java 9) fornisce uno snapshot dello stack e prevede metodi che consentono al programmatore di controllare con granularità fine il modo in cui utilizzare l'analisi dello stack.

### <a name="garbage-collection-10"></a>Garbage Collection \[[10](#ref10)\]

In Java 11 sono disponibili i Garbage Collector seguenti: seriale, parallelo, Garbage-First ed epsilon. L'impostazione predefinita in Java 11 è Garbage-First Garbage Collector(G1GC).

Gli altri tre collector vengono citati solo per completezza. Il Garbage Collector Z (ZGC) è un collector simultaneo a bassa latenza che tenta di mantenere periodi di pausa inferiori a 10 ms. ZGC è disponibile come funzionalità sperimentale in Java 11. Shenandoah è un collector che riduce i tempi di pausa di GC eseguendo più operazioni di Garbage Collection contemporaneamente al programma Java in esecuzione.
Shenandoah è una funzionalità sperimentale di Java 12, ma sono presenti backport in Java 11. Concurrent Mark and Sweep (CMS) è disponibile ma è stato deprecato dopo Java 9.

La JVM definisce le impostazioni predefinite di GC per il caso d'uso tipico. Spesso queste impostazioni predefinite e altre impostazioni di GC devono essere regolate per ottimizzare la velocità effettiva o la latenza, in base ai requisiti dell'applicazione.
Per regolare correttamente la Garbage Collection, è necessaria una conoscenza approfondita della tecnologia, competenze che vengono offerte dal [Microsoft Java Engineering Group](mailto:javaplatformgroup@microsoft.com).

#### <a name="g1gc"></a>G1GC

L'impostazione predefinita in Java 11 è G1 Garbage Collector (G1GC). Lo scopo di G1GC è quello di raggiungere un equilibrio tra latenza e velocità effettiva. Il Garbage Collector G1 prova a raggiungere una velocità effettiva elevata rispettando gli obiettivi del tempo di pausa con un'alta probabilità. G1GC è progettato per evitare raccolte complete, ma quando le raccolte simultanee non possono recuperare memoria con la velocità sufficiente, si verificherà un fallback della Garbage Collection completa. La Garbage Collection completa usa lo stesso numero di thread di lavoro paralleli delle raccolte di nuova generazione e miste.

#### <a name="parallel-gc"></a>GC parallelo

Il collector parallelo è l'impostazione predefinita in Java 8. Si tratta di un collector per la velocità effettiva che usa più thread per velocizzare l'operazione di Garbage Collection.

#### <a name="epsilon-11"></a>Epsilon \[[11](#ref11)\]

Il Garbage Collector epsilon gestisce le allocazioni, ma non recupera alcuna memoria. Quando l'heap viene esaurito, la JVM verrà arrestata.
Epsilon è utile per i servizi di breve durata e per le applicazioni note per essere prive di garbage.

#### <a name="improvements-for-docker-containers-12"></a>Miglioramenti per i contenitori Docker \[[12](#ref12)\]

Prima di Java 10, i vincoli di memoria e CPU impostati in un contenitore non venivano riconosciuti dalla JVM. In Java 8, ad esempio, la JVM predefinita imposterà come predefinite le dimensioni massime dell'heap su ¼ della memoria fisica dell'host sottostante. A partire da Java 10, la JVM usa i vincoli impostati dai gruppi di controllo contenitore (cgroups) per impostare i limiti di memoria e CPU (vedere la nota di seguito).
Ad esempio, le dimensioni massime predefinite dell'heap sono ¼ del limite di memoria del contenitore (ad esempio, 500 MB per -m2G).

Sono state inoltre aggiunte opzioni della JVM per offrire agli utenti di contenitori Docker un controllo granulare sulla quantità di memoria di sistema che verrà usata per l'heap Java.

Questo supporto è abilitato per impostazione predefinita ed è disponibile solo nelle piattaforme basate su Linux.

> [!NOTE]
> Per la maggior parte del supporto di cgroup è stato eseguito il backporting a Java 8 a partire da jdk8u191. Ulteriori miglioramenti potrebbero non essere necessariamente sottoposti a backporting alla versione 8.

#### <a name="multi-release-jar-files-13"></a>File JAR con più versioni \[[13](#ref13)\]

In Java 11 è possibile creare un file JAR contenente più versioni dei file di classe specifiche della versione Java. I file JAR con più versioni consentono agli sviluppatori di librerie di supportare più versioni di Java senza dover distribuire più versioni dei file JAR. Per il consumer di queste librerie, i file JAR con più versioni risolvono il problema di dover abbinare file JAR specifici a destinazioni di runtime specifiche.

## <a name="miscellaneous-performance-improvements"></a>Miglioramenti vari delle prestazioni

Le modifiche seguenti apportate alla JVM hanno un impatto diretto sulle prestazioni.

-   **JEP 197: Segmented Code Cache**\[[14](#ref14)\]: divide la cache di codice in segmenti distinti. Questa segmentazione assicura un maggior controllo del footprint di memoria della JVM, riduce il tempo di analisi dei metodi compilati, riduce significativamente la frammentazione della cache del codice e migliora le prestazioni.

-   **JEP 254: Compact Strings** \[[15](#ref15)\]: cambia la rappresentazione interna di una stringa da due byte per carattere a uno o due byte per carattere, a seconda della codifica dei caratteri. Poiché la maggior parte delle stringhe contiene caratteri ISO-8859-1/Latin-1, questa modifica dimezza di fatto la quantità di spazio necessaria per archiviare una stringa.

-   **JEP 310: Application Class-Data Sharing** \[[16](#ref16)\]: la condivisione di dati di classe riduce il tempo di avvio consentendo il mapping in memoria delle classi archiviate in fase di esecuzione. Questa funzionalità estende la condivisione di dati delle classi consentendo di inserire le classi dell'applicazione nell'archivio CDS. Quando più JVM condividono lo stesso file di archivio, viene risparmiata memoria i tempi di risposta generali del sistema migliorano.

-   **JEP 312: Thread-Local Handshakes** \[[17](#ref17)\]: consente di eseguire un callback nei thread senza eseguire un safepoint globale della VM, per cui la VM può raggiungere una latenza più bassa senza ridurre il numero di safepoint globali.

-   **Lazy Allocation of Compiler Threads** \[[18](#ref18)\]: in modalità di compilazione a più livelli, la macchina virtuale avvia un numero elevato di thread del compilatore.
    Questa modalità è l'impostazione predefinita nei sistemi con molte CPU. Questi thread vengono creati indipendentemente dalla memoria disponibile o dal numero di richieste di compilazione. I thread consumano memoria anche quando sono inattivi (ovvero quasi sempre), il che comporta un uso inefficiente delle risorse. Per risolvere questo problema, l'implementazione è stata cambiata per cui durante l'avvio del sistema viene avviato solo un thread del compilatore di ogni tipo. L'avvio di thread aggiuntivi e l'arresto di quelli inutilizzati vengono gestiti dinamicamente. 

Le modifiche seguenti apportate alle librerie principali hanno un impatto sulle prestazioni del codice nuovo o modificato.

-   **JEP 193: Variable Handles** \[[19](#ref19)\]: definisce uno strumento standard per richiamare gli equivalenti di varie operazioni java.util.concurrent.atomic e sun.misc.Unsafe sui campi degli oggetti e sugli elementi di matrice, un set standard di operazioni di limitazione per il controllo con granularità fine dell'ordinamento della memoria e un'operazione standard di limitazione della raggiungibilità standard per assicurare che un oggetto a cui viene fatto riferimento rimanga ampiamente raggiungibile.

-   **JEP 269: Convenience Factory Methods for Collections** \[[20](#ref20)\]: definisce le API della libreria per semplificare la creazione di istanze di raccolte e mappe con un numero ridotto di elementi. Si stratta di metodi factory statici per le interfacce di raccolte che creano istanze di raccolte compatte e non modificabili. Queste istanze sono intrinsecamente più efficienti. Le API creano raccolte rappresentate in maniera compatta e prive di una classe wrapper.

-   **JEP 285: Spin-Wait Hints** \[[21](#ref21)\]: fornisce un'API che consente a Java di suggerire al sistema di runtime che si trova in un ciclo di rotazione. Determinate piattaforme hardware traggono vantaggio dal software che indica che un thread è in uno stato di occupato in attesa.

-   **JEP 321: HTTP Client (Standard)** \[[22](#ref22)\]: fornisce una nuova API client HTTP che implementa HTTP/2 e WebSocket e può sostituire l'API HttpURLConnection legacy.

## <a name="references"></a>Riferimenti

<a id="ref1">\[1\]</a> Oracle Corporation, \"Note sulla versione di Java Development Kit 9\", (online). Disponibile: https://www.oracle.com/technetwork/java/javase/9u-relnotes-3704429.html.
(Ultimo accesso: 13 novembre 2019).

<a id="ref2">\[2\]</a> Oracle Corporation, \"Note sulla versione di Java Development Kit 10\", (online). Disponibile: https://www.oracle.com/technetwork/java/javase/10u-relnotes-4108739.html.
(Ultimo accesso: 13 novembre 2019).

<a id="ref3">\[3\]</a> Oracle Corporation, \"Note sulla versione di Java Development Kit 11\", (online). Disponibile: https://www.oracle.com/technetwork/java/javase/11u-relnotes-5093844.html.
(Ultimo accesso: 13 novembre 2019).

<a id="ref4">\[4\]</a> Oracle Corporation, \"Project Jigsaw\", 22 settembre 2017, 
2017. (online). Disponibile: http://openjdk.java.net/projects/jigsaw/.
(Ultimo accesso: 13 novembre 2019).

<a id="ref5">\[5\]</a> Oracle Corporation, \"JEP 328: Flight Recorder\", 9 settembre 2018, (online). Disponibile: http://openjdk.java.net/jeps/328. (Ultimo accesso: 13 novembre 2019).

<a id="ref6">\[6\]</a> Oracle Corporation, \"Mission Control\", 25 aprile 2019, (online). Disponibile: https://wiki.openjdk.java.net/display/jmc/Main. (Ultimo accesso: 13 novembre 2019).

<a id="ref7">\[7\]</a> Oracle Corporation, \"JEP 158: Unified JVM Logging\", 14 febbraio 2019, (online). Disponibile: http://openjdk.java.net/jeps/158.
(Ultimo accesso: 13 novembre 2019).

<a id="ref8">\[8\]</a> Oracle Corporation, \"JEP 331: Low-Overhead Heap Profiling\", 5 settembre 2018, (online). Disponibile: http://openjdk.java.net/jeps/331. (Ultimo accesso: 13 novembre 2019).

<a id="ref9">\[9\]</a> Oracle Corporation, \"JEP 259: Stack-Walking API\", 18 luglio 2017, (online).
Disponibile: http://openjdk.java.net/jeps/259. (Ultimo accesso: 13 novembre 2019).

<a id="ref10">\[10\]</a> Oracle Corporation, \"JEP 248: Make G1 the Default Garbage Collector\", 12 settembre 2017, (online). Disponibile: http://openjdk.java.net/jeps/248. (Ultimo accesso: 13 novembre 2019).

<a id="ref11">\[11\]</a> Oracle Corporation, \"JEP 318: Epsilon: A No-Op Garbage Collector\", 24 settembre 2018,
(online). Disponibile: http://openjdk.java.net/jeps/318. (Ultimo accesso: 13 novembre 2019).

<a id="ref12">\[12\]</a> Oracle Corporation, \"JDK-8146115: Improve docker container detection and resource configuration usage\", 16 settembre 2019,
(online). Disponibile: https://bugs.java.com/bugdatabase/view\_bug.do?bug\_id=JDK-8146115.
(Ultimo accesso: 13 novembre 2019).

<a id="ref13">\[13\]</a> Oracle Corporation, \"JEP 238: Multi-Release JAR Files\", 22 giugno 2017, (online). Disponibile: http://openjdk.java.net/jeps/238. (Ultimo accesso: 13 novembre 2019).

<a id="ref14">\[14\]</a> Oracle Corporation, \"JEP 197: Segmented Code Cache\", 28 aprile 2017, (online).
Disponibile: http://openjdk.java.net/jeps/197. (Ultimo accesso: 13 novembre 2019).

<a id="ref15">\[15\]</a> Oracle Corporation, \"JEP 254: Compact Strings\" 18 maggio 2019, (online). Disponibile: http://openjdk.java.net/jeps/254.
(Ultimo accesso: 13 novembre 2019).

<a id="ref16">\[16\]</a> Oracle Corporation, \"JEP 310: Application Class-Data Sharing\", 17 agosto 2018, (online). Disponibile: https://openjdk.java.net/jeps/310. (Ultimo accesso: 13 novembre 2019).

<a id="ref17">\[17\]</a> Oracle Corporation, \"JEP 312: Thread-Local Handshakes\", 21 agosto 2019,
(online). Disponibile: https://openjdk.java.net/jeps/312. (Ultimo accesso: 13 novembre 2019).

<a id="ref18">\[18\]</a> Oracle Corporation, \"JDK-8198756: Lazy allocation of compiler threads\", 29 ottobre 2018, (online). Disponibile: https://bugs.java.com/bugdatabase/view\_bug.do?bug\_id=8198756.
(Ultimo accesso: 13 novembre 2019).

<a id="ref19">\[19\]</a> Oracle Corporation, \"JEP 193: Variable Handles\" 17 agosto 2017, (online). Disponibile: https://openjdk.java.net/jeps/193. (Ultimo accesso: 13 novembre 2019).

<a id="ref20">\[20\]</a> Oracle Corporation, \"JEP 269: Convenience Factory Methods for Collections\", 26 giugno 2017, (online). Disponibile: https://openjdk.java.net/jeps/269.
(Ultimo accesso: 13 novembre 2019).

<a id="ref21">\[21\]</a> Oracle Corporation, \"JEP 285: Spin-Wait Hints\", 20 agosto 2017, (online). Disponibile: https://openjdk.java.net/jeps/285. (Ultimo accesso: 13 novembre 2019).

<a id="ref22">\[22\]</a> Oracle Corporation, \"JEP 321: HTTP Client (Standard)\", 27 settembre 2018, (online).
Disponibile: https://openjdk.java.net/jeps/321. (Ultimo accesso: 13 novembre 2019).
