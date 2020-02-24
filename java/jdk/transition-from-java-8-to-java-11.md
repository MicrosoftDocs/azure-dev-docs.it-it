---
title: Transizione da Java 8 a Java 11
titleSuffix: Azure
description: Guida alla gestione del passaggio da Java 8 a Java 11.
author: dsgrieve
manager: maverbur
tags: java
ms.service: azure
ms.devlang: java
ms.topic: article
ms.date: 11/19/2019
ms.author: dagrieve
ms.openlocfilehash: a5e36d535cba39728d28c1f2aa64985863103ee6
ms.sourcegitcommit: aceed8548ad4529a81d83eb15a095edc8607cac5
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/18/2020
ms.locfileid: "77441088"
---
# <a name="transition-from-java-8-to-java-11"></a>Transizione da Java 8 a Java 11

Per la transizione del codice da Java 8 a Java 11, non esiste un'unica soluzione che soddisfi tutte le esigenze.
Per le applicazioni non semplici, il passaggio da Java 8 a Java 11 può comportare una quantità significativa di lavoro. I potenziali problemi riguardano le API rimosse, i pacchetti deprecati, l'uso di API interne, le modifiche apportate ai caricatori di classi e quelle apportate a Garbage Collection. 

In generale, gli approcci prevedono di provare l'esecuzione in Java 11 senza compilazione oppure di compilare prima con JDK 11. Se l'obiettivo è fare in modo che un'applicazione sia attiva e funzionante il più rapidamente possibile, spesso l'approccio migliore è proprio quello di provare l'esecuzione in Java 11. Per una libreria, l'obiettivo sarà pubblicare un artefatto compilato e testato con JDK 11.

Vale comunque la pena passare a Java 11. Rispetto a Java 8 sono state aggiunte nuove funzionalità e sono state effettuate ottimizzazioni. Queste funzionalità e ottimizzazioni migliorano l'avvio, le prestazioni e l'utilizzo della memoria, oltre a offrire una maggiore integrazione con i contenitori. Sono state inoltre apportate aggiunte e modifiche alle API per migliorare la produttività degli sviluppatori. 

Questo documento riguarda gli strumenti per esaminare il codice. Descrive anche i problemi che potrebbero verificarsi e fornisce suggerimenti per risolverli. È consigliabile consultare anche altre guide, ad esempio la [Guida alla migrazione a Oracle JDK](https://docs.oracle.com/en/java/javase/11/migrate/index.htm). Questo documento non descrive come rendere [modulare](http://openjdk.java.net/projects/jigsaw) il codice esistente.  


## <a name="the-toolbox"></a>L'insieme di strumenti

Java 11 include due strumenti, *jdeprscan* e *jdeps*, che risultano utili per l'analisi di potenziali problemi. Questi strumenti possono essere eseguiti su file di classe o jar esistenti. È possibile valutare la transizione senza dover ricompilare il codice. 

[jdeprscan](https://docs.oracle.com/en/java/javase/11/tools/jdeprscan.html) verifica se sono in uso API deprecate o rimosse.
L'uso di API deprecate non è un problema che causa un blocco, ma va comunque esaminato. È presente un file jar aggiornato? È necessario registrare un problema per risolvere l'uso di API deprecate? L'uso di API rimosse è un problema che causa un blocco e deve essere risolto prima di provare l'esecuzione in Java 11.


[jdeps](https://docs.oracle.com/en/java/javase/11/tools/jdeps.html) è un analizzatore delle dipendenze delle classi Java. Se usato con l'opzione `--jdk-internals`, *jdeps* indica quale classe dipende da quale API interna. È possibile continuare a usare l'API interna in Java 11, ma la sostituzione dell'utilizzo dovrebbe essere prioritaria. La pagina wiki di OpenJDK sullo [strumento di analisi delle dipendenze Java](https://wiki.openjdk.java.net/display/JDK8/Java+Dependency+Analysis+Tool) raccomanda opzioni sostitutive per alcune API interne JDK di uso comune. 

Sono disponibili plug-in *jdeps* e *jdeprscan* per Gradle e Maven. È consigliabile aggiungere questi strumenti agli script di compilazione. 

> [!div class="mx-tdBreakAll"]
> |Strumento|Plug-in per Gradle|Plug-in per Maven|
> |-|-|-|
> |jdeps|[jdeps-gradle-plugin](https://github.com/kordamp/jdeps-gradle-plugin)|[Plug-in JDeps per Apache Maven](https://maven.apache.org/plugins/maven-jdeps-plugin/index.html)|
> |jdeprscan|[jdeprscan-gradle-plugin](https://github.com/kordamp/jdeprscan-gradle-plugin)|[Plug-in JDeprScan per Apache Maven](https://maven.apache.org/plugins/maven-jdeprscan-plugin/index.html)|

Il compilatore Java stesso, *javac*, è un altro strumento disponibile. Gli avvisi e gli errori che si ricevono da *jdeprscan* e *jdeps* provengono dal compilatore.  Il vantaggio di usare *jdeprscan* e *jdeps* è che è possibile eseguire questi strumenti su file di classe e jar esistenti, incluse le librerie di terze parti.

Quello che *jdeprscan* e *jdeps* non sono in grado di fare è avvisare in caso venga usata la reflection per accedere all'API incapsulata. L'accesso reflective viene controllato in fase di esecuzione. In definitiva, è necessario eseguire il codice in Java 11 per saperlo con certezza.

### <a name="using-jdeprscan"></a>Uso di jdeprscan

Il modo più semplice per usare [jdeprscan](https://docs.oracle.com/en/java/javase/11/tools/jdeprscan.html) consiste nel fornire allo strumento un file jar di una compilazione esistente. È anche possibile fornire una directory, ad esempio la directory di output del compilatore, o un nome di classe individuale. Usare l'opzione `--release 11` per ottenere l'elenco più completo di API deprecate. Se si vuole assegnare una priorità all'API deprecata da cercare, ripristinare l'impostazione `--release 8`. È probabile che l'API deprecata in Java 8 venga rimossa prima dell'API deprecata più di recente. 

```console
jdeprscan --release 11 my-application.jar
```

Lo strumento *jdeprscan* genera un messaggio di errore se si è verificato un problema durante la risoluzione di una classe dipendente.
Ad esempio: `error: cannot find class org/apache/logging/log4j/Logger`. È consigliabile aggiungere classi dipendenti a `--class-path` o usare il class-path dell'applicazione, ma lo strumento continuerà l'analisi anche senza.
L'argomento è *&#8209;&#8209;class&#8209;path*. Non è possibile usare altre varianti dell'argomento class-path.

```console
jdeprscan --release 11 --class-path log4j-api-2.13.0.jar my-application.jar
error: cannot find class sun/misc/BASE64Encoder
class com/company/Util uses deprecated method java/lang/Double::<init>(D)V
```
Questo output indica che la classe `com.company.Util` chiama un costruttore deprecato della classe `java.lang.Double`. Lo strumento javadoc consiglierà l'API da usare al posto di quella deprecata. Non sarà possibile risolvere l'errore `error: cannot find class sun/misc/BASE64Encoder`, perché si tratta di un'API che è stata rimossa. Dopo Java 8, è necessario usare `java.util.Base64`. 

Eseguire `jdeprscan --release 11 --list` per avere un'idea di quale API è stata deprecata dopo Java 8. Per ottenere un elenco di API rimosse, eseguire `jdeprscan --release 11 --list --for-removal`.

### <a name="using-jdeps"></a>Uso di jdeps

Usare [jdeps](https://docs.oracle.com/en/java/javase/11/tools/jdeps.html) con l'opzione `--jdk-internals` per trovare le dipendenze dall'API interna JDK. Per questo esempio è necessaria l'opzione della riga di comando `--multi-release 11` perché *log4j-core-2.13.0.jar* è un [file jar multiversione](https://docs.oracle.com/en/java/javase/11/docs/specs/jar/jar.html#multi-release-jar-files). Senza questa opzione, *jdeps* genera un errore se trova un file jar multiversione. L'opzione specifica la versione dei file di classe da ispezionare. 

```console
jdeps --jdk-internals --multi-release 11 --class-path log4j-core-2.13.0.jar my-application.jar
Util.class -> JDK removed internal API
Util.class -> jdk.base
Util.class -> jdk.unsupported
   com.company.Util        -> sun.misc.BASE64Encoder        JDK internal API (JDK removed internal API)
   com.company.Util        -> sun.misc.Unsafe               JDK internal API (jdk.unsupported)
   com.company.Util        -> sun.nio.ch.Util               JDK internal API (java.base)

Warning: JDK internal APIs are unsupported and private to JDK implementation that are
subject to be removed or changed incompatibly and could break your application.
Please modify your code to eliminate dependence on any JDK internal APIs.
For the most recent update on JDK internal API replacements, please check:
https://wiki.openjdk.java.net/display/JDK8/Java+Dependency+Analysis+Tool

JDK Internal API                         Suggested Replacement
----------------                         ---------------------
sun.misc.BASE64Encoder                   Use java.util.Base64 @since 1.8
sun.misc.Unsafe                          See http://openjdk.java.net/jeps/260   

```

L'output fornisce un consiglio utile su come eliminare l'uso dell'API interna JDK. Laddove possibile, viene suggerita l'API sostitutiva. Il nome del modulo in cui è incapsulato il pacchetto viene specificato tra parentesi. Il nome del modulo può essere usato con `--add-exports` o `--add-opens` se è necessario [interrompere l'incapsulamento](https://docs.oracle.com/javase/9/migrate/toc.htm#JSMIG-GUID-2F61F3A9-0979-46A4-8B49-325BA0EE8B66) in modo esplicito. 

L'uso di *sun.misc.BASE64Encoder* o *sun.misc.BASE64Decoder* genererà un errore *java.lang.NoClassDefFoundError* in Java 11. Il codice che usa queste API deve essere modificato per usare *java.util.Base64*. 

Provare a eliminare l'uso di qualsiasi API proveniente dal modulo *jdk.unsupported*. Le API di questo modulo indicheranno [JDK Enhancement proposto (JEP) 260](http://openjdk.java.net/jeps/260) come sostituzione consigliata.
In breve, JEP 260 indica che l'uso dell'API interna sarà supportato fino a quando non sarà disponibile l'API sostitutiva. Anche se il codice potrebbe usare l'API interna JDK, continuerà a essere eseguito, almeno per un breve periodo di tempo. Esaminare JEP 260 poiché fa riferimento alle opzioni sostitutive per alcune API interne. 
È ad esempio possibile usare [handle di variabili](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/invoke/VarHandle.html) al posto di alcune API *sun.misc.Unsafe*. 

*jdeps* offre più funzionalità oltre all'analisi dell'uso di API interne JDK. Si tratta di uno strumento utile per analizzare le dipendenze e generare file module-info. Per altre informazioni, vedere la [documentazione](https://docs.oracle.com/en/java/javase/11/tools/jdeps.html).

### <a name="using-javac"></a>Uso di javac

Per la compilazione con JDK 11, è necessario apportare aggiornamenti agli script di compilazione, agli strumenti, ai framework di test e alle librerie incluse. Usare l'opzione `-Xlint:unchecked` per *javac* per ottenere i dettagli sull'uso dell'API interna JDK e altri avvisi. Può anche essere necessario usare `--add-opens` o `--add-reads` per esporre i pacchetti incapsulati al compilatore (vedere [JEP 261](http://openjdk.java.net/jeps/261)). 

Le librerie possono considerare i pacchetti come [file jar multiversione](https://docs.oracle.com/en/java/javase/11/docs/specs/jar/jar.html#multi-release-jar-files). I file jar multiversione consentono di supportare sia i runtime Java 8 che Java 11 dallo stesso file jar. Aggiungono però complessità alla compilazione. La procedura di compilazione di jar multiversione esula dall'ambito di questo documento. 

## <a name="running-on-java-11"></a>Esecuzione in Java 11

La maggior parte delle applicazioni dovrebbero essere eseguite in Java 11 senza modifiche. Il primo tentativo da fare è eseguire il codice in Java 11 senza ricompilarlo. L'obiettivo è verificare quali avvisi ed errori vengono generati dall'esecuzione. Il risultato di questo approccio è  
un'esecuzione più rapida dell'applicazione in Java 11, perché ci si concentra sul minimo degli interventi da effettuare. 

Nella maggior parte dei casi, gli eventuali problemi riscontrati possono essere risolti senza la necessità di ricompilare il codice.
Se è necessario risolvere un problema nel codice, apportare la correzione ma continuare a compilare con JDK 8. Se possibile, assicurarsi che l'applicazione possa essere *eseguita* con `java` versione 11 prima di procedere alla *compilazione* con JDK 11. 

### <a name="check-command-line-options"></a>Controllare le opzioni della riga di comando

Prima dell'esecuzione in Java 11, analizzare brevemente le opzioni della riga di comando. 
Le [opzioni che sono state rimosse](#unrecognized options) genereranno l'uscita della JVM (Java Virtual Machine). Questo controllo è particolarmente importante se si usano le opzioni di registrazione di GC, perché sono cambiate drasticamente rispetto a Java 8. Lo strumento [JaCoLine](https://jacoline.dev/about) è un'opzione valida da usare per rilevare i problemi con le opzioni della riga di comando. 

### <a name="check-third-party-libraries"></a>Controllare le librerie di terze parti

Una potenziale fonte di problemi è rappresentata dalle librerie di terze parti su cui non si ha controllo. È possibile aggiornare proattivamente queste librerie alle versioni più recenti. In alternativa, verificare il risultato dell'esecuzione dell'applicazione e aggiornare solo le librerie necessarie. Con l'aggiornamento di tutte le librerie a una versione recente, il problema è che risulta più difficile trovare la causa radice di un eventuale errore nell'applicazione. L'errore si è verificato a causa di una libreria aggiornata? Oppure è stato causato da una modifica nel runtime? Se si aggiornano solo le librerie necessarie, il problema è che possono essere necessarie diverse iterazioni per risolvere gli errori.

In questo caso, è consigliabile apportare la quantità minima possibile di modifiche e di aggiornare le librerie di terze parti [separatamente](#next-steps). Se si decide di aggiornare una libreria di terze parti, è molto probabile che si voglia avere la versione più recente e avanzata compatibile con Java 11. A seconda di quanto poco recente è la versione corrente, è consigliabile adottare un approccio più prudente ed eseguire l'aggiornamento alla prima versione compatibile con Java 9 e versioni successive. 

Oltre a esaminare le note sulla versione, è possibile usare gli strumenti *jdeps* e *jdeprscan* per valutare il file jar. Inoltre, l'OpenJDK Quality Group gestisce la pagina wiki [Quality Outreach](https://wiki.openjdk.java.net/display/quality/Quality+Outreach) che riporta lo stato dei test di molti progetti FOSS (Free Open Source Software) rispetto alle versioni di OpenJDK. 

### <a name="explicitly-set-garbage-collection"></a>Impostare esplicitamente Garbage Collection

Parallel Garbage Collector (Parallel GC) è lo strumento predefinito in Java 8. Se l'applicazione usa lo strumento predefinito, è necessario impostare GC esplicitamente con l'opzione della riga di comando `-XX:+UseParallelGC`.
In Java 9 lo strumento predefinito è stato sostituito da Garbage-First Garbage Collector(G1GC). Per un confronto equo tra l'esecuzione di applicazioni in Java 8 rispetto a Java 11, è necessario che le impostazioni di GC siano identiche. La sperimentazione con le impostazioni di GC dovrebbe essere rimandata a dopo la convalida dell'applicazione in Java 11. 

### <a name="explicitly-set-default-options"></a>Impostare esplicitamente le opzioni predefinite

Se si usa la VM HotSpot, l'impostazione dell'opzione della riga di comando `-XX:+PrintCommandLineFlags` eseguirà il dump dei valori delle opzioni impostate dalla VM, in particolare i valori predefiniti impostati da GC.
Applicare questo flag per l'esecuzione in Java 8 e usare le opzioni stampate durante l'esecuzione in Java 11. Nella maggior parte dei casi, le impostazioni predefinite sono le stesse dalla versione 8 alla 11. Ma l'uso delle impostazioni della versione 8 assicura parità.

È consigliabile impostare l'opzione della riga di comando `--illegal-access=warn`.
In Java 11 l'uso della reflection per accedere all'API interna JDK genererà un [avviso di accesso reflective non valido](#warning-an-illegal-reflective-access-operation-has-occurred).
Per impostazione predefinita, l'avviso viene emesso solo per il primo accesso non valido. Con l'impostazione di `--illegal-access=warn`, verrà generato un avviso per *ogni* accesso reflective. Con l'opzione impostata su *warn* verranno riscontrati più casi di accesso non valido. Ma si riceveranno anche molti avvisi ridondanti.  
Quando l'applicazione viene eseguita in Java 11, impostare `--illegal-access=deny` per simulare il comportamento futuro del runtime Java. A partire da Java 16, l'impostazione predefinita sarà `--illegal-access=deny`. 

### <a name="classloader-cautions"></a>Avvisi per ClassLoader

In Java 8 è possibile eseguire il cast del caricatore di classi in `URLClassLoader`. Questa operazione viene in genere eseguita da applicazioni e librerie per inserire le classi nel classpath in fase di esecuzione. La gerarchia dei caricatori di classi è cambiata in Java 11. Il caricatore di classi di sistema, anche detto caricatore di classi delle applicazioni, è ora una classe interna. Il cast in `URLClassLoader` genererà `ClassCastException` in fase di esecuzione. In Java 11 l'API non aumenta dinamicamente il classpath in fase di esecuzione, ma questa operazione può essere eseguita tramite reflection, con gli ovvi rischi associati all'uso dell'API interna. 

In Java 11 il caricatore di classi all'avvio carica solo i moduli principali. Se si crea un caricatore di classi con un elemento padre Null, è possibile che non vengano individuate tutte le classi della piattaforma. In tali casi, in Java 11 è necessario passare `ClassLoader.getPlatformClassLoader()` invece di `null` come caricatore di classi padre. 

### <a name="locale-data-changes"></a>Modifiche ai dati delle impostazioni locali

L'origine predefinita per i dati delle impostazioni locali in Java 11 è stata cambiata con [JEP 252](http://openjdk.java.net/jeps/252) nel Common Locale Data Repository del Consorzio Unicode. Questa modifica potrebbe avere effetti sulla formattazione localizzata. Impostare la proprietà `java.locale.providers=COMPAT,SPI` per ripristinare il comportamento di Java 8 con le impostazioni locali, se necessario. 

### <a name="potential-issues"></a>Potenziali problemi

Ecco alcuni problemi comuni che potrebbero verificarsi. Per altri dettagli su questi problemi, seguire i collegamenti.

- [Opzione della VM non riconosciuta](#unrecognized-options)
- [Opzione non riconosciuta](#unrecognized-options)
- [Avviso per la VM: L'opzione verrà ignorata](#vm-warnings)
- [Avviso per la VM: L'opzione &lt;*opzione*&gt; è stata deprecata](#vm-warnings)
- [AVVISO: Si è verificata un'operazione di accesso reflective non valida](#warning-an-illegal-reflective-access-operation-has-occurred)
- [java.lang.reflect.InaccessibleObjectException](#javalangreflectinaccessibleobjectexception)
- [java.lang.NoClassDefFoundError](#javalangnoclassdeffounderror)
- [L'opzione -Xbootclasspath/p non è più supportata](#-xbootclasspathp-is-no-longer-a-supported-option)
- [java.lang.UnsupportedClassVersionError](#unsupportedclassversionerror)

#### <a name="unrecognized-options"></a>Opzioni non riconosciute

Se un'opzione della riga di comando è stata rimossa, l'applicazione stamperà `Unrecognized option:` o `Unrecognized VM option` seguito dal nome di tale opzione. Un'opzione non riconosciuta causerà l'uscita della VM.
Le opzioni che sono state deprecate, ma non rimosse, genereranno un [avviso per la VM](#vm-warnings).

In generale, le opzioni che sono state rimosse non sono state sostituite e l'unica soluzione è rimuoverle dalla riga di comando. L'eccezione riguarda le opzioni per la registrazione di Garbage Collection. La registrazione di GC è stata [reimplementata](http://openjdk.java.net/jeps/271) in Java 9 per usare il [framework di registrazione unificato per la JVM](http://openjdk.java.net/jeps/158). Vedere la tabella 2-2 sul mapping dei flag di registrazione legacy per Garbage Collection con la configurazione Xlog nella sezione su come [abilitare la registrazione con il framework di registrazione unificato per la JVM](https://docs.oracle.com/en/java/javase/11/tools/java.html#GUID-BE93ABDC-999C-4CB5-A88B-1994AAAC74D5) delle informazioni di riferimento sugli strumenti Java SE 11. 

#### <a name="vm-warnings"></a>Avvisi per la VM

L'uso di opzioni deprecate genererà avvisi. Un'opzione si considera deprecata quando è stata sostituita o non è più utile. Così come per le [opzioni rimosse](#unrecognized-options), è consigliabile rimuovere tali opzioni dalla riga di comando.
L'avviso `VM Warning: Option <option> was deprecated` significa che il supporto per l'opzione è ancora disponibile, ma potrebbe essere rimosso in futuro. Un opzione non più supportata genererà l'avviso `VM Warning: Ignoring option`.
Le opzioni non più supportate non hanno effetti sul runtime.

La pagina Web sulle [opzioni delle VM](https://chriswhocodes.com/hotspot_option_differences.html) include l'elenco completo di opzioni aggiunte o rimosse da Java dopo JDK 7. 

#### <a name="error-could-not-create-the-java-virtual-machine"></a>Errore: Non è stato possibile creare la Java Virtual Machine

Questo messaggio di errore viene stampato se la JVM riscontra un'[opzione non riconosciuta](#unrecognized-options).

#### <a name="warning-an-illegal-reflective-access-operation-has-occurred"></a>AVVISO: Si è verificata un'operazione di accesso reflective non valida

Quando il codice Java usa la reflection per accedere all'API interna JDK, il runtime genera un avviso su accesso reflective non valido.

```console
WARNING: An illegal reflective access operation has occurred
WARNING: Illegal reflective access by my.sample.Main (file:/C:/sample/) to method sun.nio.ch.Util.getTemporaryDirectBuffer(int)
WARNING: Please consider reporting this to the maintainers of com.company.Main
WARNING: Use --illegal-access=warn to enable warnings of further illegal reflective access operations
WARNING: All illegal access operations will be denied in a future release
```

Questo significa che un modulo non ha esportato il pacchetto il cui accesso avviene tramite reflection. Il pacchetto viene *incapsulato* nel modulo e corrisponde, essenzialmente, a un'API interna. L'avviso può essere ignorato come primo tentativo per diventare operativi con Java 11.
Il runtime di Java 11 permette l'accesso reflective per cui il codice legacy può continuare a funzionare.  

Per risolvere questo avviso, cercare codice aggiornato che non usa l'API interna. Se il problema non può essere risolto con il codice aggiornato, è possibile usare l'opzione della riga di comando `--add-exports` o `--add-opens` per aprire l'accesso al pacchetto.
Queste opzioni consentono l'accesso a tipi non esportati di un modulo da un altro modulo.

L'opzione [`--add-exports`](https://docs.oracle.com/javase/9/migrate/toc.htm#JSMIG-GUID-2F61F3A9-0979-46A4-8B49-325BA0EE8B66) consente al modulo di destinazione di accedere ai tipi *pubblici* del pacchetto denominato del modulo di origine. A volte il codice userà `setAccessible(true)` per accedere ad API e membri non pubblici. Questa operazione è detta *deep reflection*. In questo caso, usare [`--add-opens`](https://docs.oracle.com/javase/9/migrate/toc.htm#JSMIG-GUID-12F945EB-71D6-46AF-8C3D-D354FD0B1781) per fornire al codice l'accesso ai membri non pubblici di un pacchetto. In caso di dubbi se usare *--add-exports* o *--add-opens*, iniziare con *--add-exports*. 

L'opzione `--add-exports` o `--add-opens` dovrà essere considerata come soluzione alternativa e non come soluzione a lungo termine.
L'uso di queste opzioni interrompe l'incapsulamento del sistema di moduli, che è destinato a impedire l'uso dell'API interna JDK.  Se l'API interna viene rimossa o modificata, l'applicazione genera un errore.  L'accesso reflective verrà negato in Java 16, tranne nei casi in cui è abilitato da opzioni della riga di comando come `--add-opens`.
Per simulare il comportamento futuro, impostare `--illegal-access=deny` nella riga di comando.

L'avviso nell'esempio precedente viene generato perché il pacchetto `sun.nio.ch` non viene esportato dal modulo `java.base`. In altri termini, non sono presenti `exports sun.nio.ch;` nel file `module-info.java` del modulo `java.base`. Questo problema può essere risolto con `--add-exports=java.base/sun.nio.ch=ALL-UNNAMED`. Le classi non definite in un modulo appartengono implicitamente al modulo *senza nome*, letteralmente denominato `ALL-UNNAMED`.

#### <a name="javalangreflectinaccessibleobjectexception"></a>java.lang.reflect.InaccessibleObjectException

Questa eccezione indica che si sta provando a chiamare `setAccessible(true)` per un campo o un metodo di una classe incapsulata. È anche possibile che si riceva un [avviso di accesso reflective non valido](#warning-an-illegal-reflective-access-operation-has-occurred). Usare l'opzione [`--add-opens`](https://docs.oracle.com/javase/9/migrate/toc.htm#JSMIG-GUID-12F945EB-71D6-46AF-8C3D-D354FD0B1781) per fornire al codice l'accesso ai membri non pubblici di un pacchetto. Il messaggio di eccezione indica che il modulo "non apre" il pacchetto per il modulo che sta provando a chiamare *setAccessible*. Se il modulo è "senza nome", usare `UNNAMED-MODULE` come modulo di destinazione nell'opzione *--add-opens*.

```shell
java.lang.reflect.InaccessibleObjectException: Unable to make field private final java.util.ArrayList jdk.internal.loader.URLClassPath.loaders accessible: 
module java.base does not "opens jdk.internal.loader" to unnamed module @6442b0a6

$ java --add-opens=java.base/jdk.internal.loader=UNNAMED-MODULE example.Main
```

#### <a name="javalangnoclassdeffounderror"></a>java.lang.NoClassDefFoundError

L'errore *NoClassDefFoundError* è molto probabilmente causato da un pacchetto diviso o da un riferimento a moduli rimossi. 

##### <a name="noclassdeffounderror-caused-by-split-packages"></a>NoClassDefFoundError causato da pacchetti divisi

Per pacchetti divisi si intendono quelli disponibili in più di una libreria. Il sintomo di un problema dovuto a pacchetto diviso è che non è possibile trovare una classe anche se è noto che si trova nel class-path. 

Questo problema si verifica solo quando si usa il module-path. Il sistema di moduli Java ottimizza la ricerca delle classi limitando un pacchetto a un solo modulo *denominato*. Il runtime considera prioritario il module-path rispetto al class-path durante la ricerca di una classe. Se un pacchetto è diviso tra un modulo e il class-path, per la ricerca della classe viene usato solo il modulo. Ciò può generare errori `NoClassDefFound`. 

Un modo semplice per verificare la presenza di un pacchetto diviso consiste nell'inserire il percorso del modulo e il percorso della classe in [jdeps](https://docs.oracle.com/en/java/javase/11/tools/jdeps.html) e usare il percorso dei file della classe dell'applicazione come &lt;path&gt;. Se è presente un pacchetto diviso, jdeps stampa l'avviso `Warning: split package: <package-name> <module-path> <split-path>`. 

Questo problema può essere risolto usando `--patch-module <module-name>=<path>[,<path>]` per aggiungere il pacchetto diviso in un modulo denominato. 

##### <a name="noclassdeffounderror-caused-by-using-java-ee-or-corba-modules"></a>NoClassDefFoundError causato da moduli Java EE o CORBA

Se l'applicazione viene eseguita in Java 8 ma genera un errore `java.lang.NoClassDefFoundError` o `java.lang.ClassNotFoundError`, è probabile che usi un pacchetto dei moduli Java EE o CORBA. Questi moduli sono stati deprecati in Java 9 e [rimossi in Java 11](https://openjdk.java.net/jeps/320). 

Per risolvere il problema, aggiungere una dipendenza dal runtime nel progetto.

> [!div class="mx-tdBreakAll"]
> |Modulo rimosso|Pacchetto interessato|Dipendenza suggerita|
> |-|-|-|
> |API Java per servizi Web XML (JAX-WS) |java.xml.ws |[Runtime RI JAX WS](https://mvnrepository.com/artifact/com.sun.xml.ws/jaxws-rt) |
> |Architettura Java per binding XML (JAXB) |java.xml.bind |[Runtime JAXB ](https://mvnrepository.com/artifact/org.glassfish.jaxb/jaxb-runtime)|
> |JavaBeans Activation Framework (JAV) |java.activation |[JavaBeans (TM) Activation Framework](https://mvnrepository.com/artifact/javax.activation/activation) |
> |Annotazioni comuni |java.xml.ws.annotation |[API Javax Annotation](https://mvnrepository.com/artifact/javax.annotation/javax.annotation-api)|
> |Common Object Request Broker Architecture (CORBA) |java.corba | [GlassFish CORBA ORB](https://mvnrepository.com/artifact/org.glassfish.corba/glassfish-corba-orb) |
> |API Java Transaction (JTA) |java.transaction | [API Java Transaction](https://mvnrepository.com/artifact/javax.transaction/jta)|

#### <a name="-xbootclasspathp-is-no-longer-a-supported-option"></a>L'opzione -Xbootclasspath/p non è più supportata

Il supporto per `-Xbootclasspath/p` è stato rimosso. Usare invece `--patch-module`. L'opzione *--patch-module* è descritta in [JEP 261](http://openjdk.java.net/jeps/261). Cercare la sezione relativa al contenuto del modulo patch. L'opzione *--patch-module* può essere usata con *javac* e con *java* per eseguire l'override o aumentare le classi di un modulo. 

In realtà, *--patch-module* inserisce il modulo patch nella ricerca di classi del sistema di moduli. Il sistema di moduli recupera la classe prima dal modulo patch. L'effetto è lo stesso dell'aggiunta del bootclasspath in Java 8. 

#### <a name="unsupportedclassversionerror"></a>UnsupportedClassVersionError

Questa eccezione significa che si sta provando a eseguire codice compilato con una versione successiva di Java in una versione precedente. Ad esempio, si esegue codice in Java 11 con un jar compilato con JDK 13. 

| Versione Java | Versione del formato di file della classe |
|-|-|
| 8  | 52 |
| 9  | 53 |
| 10 | 54 |
| 11 | 55 |
| 12 | 56 |
| 13 | 57 |

## <a name="next-steps"></a>Passaggi successivi

Quando l'applicazione viene eseguita in Java 11, è consigliabile rimuovere le librerie dal class-path e spostarle nel module-path. Cercare le versioni aggiornate delle librerie da cui dipende l'applicazione. Scegliere librerie modulari, se disponibili. Usare il più possibile il module-path, anche se non si prevede di usare moduli nell'applicazione. L'uso del module-path assicura prestazioni più elevate per il caricamento delle classi rispetto al class-path. 
