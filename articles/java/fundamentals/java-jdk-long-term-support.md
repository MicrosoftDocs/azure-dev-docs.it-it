---
title: Java JDK e supporto a lungo termine per lo sviluppo di Azure
description: Download e indicazione del supporto di Azure per lo sviluppo e l'esecuzione di applicazioni Java.
ms.date: 04/09/2019
ms.topic: conceptual
ms.custom: seo-java-september2019
ms.openlocfilehash: 4722c64e3b328cb5b63b6b976bd25e5b91341117
ms.sourcegitcommit: 367217792f3b16c769e2c39372358bc6b9c9c044
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/02/2020
ms.locfileid: "84292793"
---
# <a name="java-long-term-support-and-medium-term-support-for-azure-and-azure-stack"></a>Supporto a lungo e medio termine di Java per Azure e Azure Stack

Gli sviluppatori Java in Azure e Azure Stack possono compilare ed eseguire applicazioni Java di produzione usando le build JDK [Azul Zulu per Azure - Enterprise Edition](https://www.azul.com/downloads/azure-only/zulu/) senza incorrere in costi di supporto aggiuntivi. È possibile usare qualsiasi runtime Java in Azure, ma se si sceglie Zulu si ottengono aggiornamenti di manutenzione gratuiti ed è possibile richiedere assistenza a Microsoft.

Una versione designata per il supporto a lungo termine (LTS) corrisponde alle versioni LTS designate da Oracle e dalla community di OpenJDK. Per le versioni LTS sono offerti almeno otto anni di accesso alle correzioni di bug, agli aggiornamenti della sicurezza e ad eventuali altre correzioni necessarie ("supporto di produzione"), oltre a due anni di supporto aggiuntivo per consigliare e aiutare gli utenti a eseguire la migrazione a una versione più recente del JDK ("supporto esteso").

Per le versioni progettate per il supporto a medio termine (MTS), il supporto per la produzione viene fornito per almeno un anno e mezzo dopo la disponibilità a livello generale della versione LTS successiva, oltre a un ulteriore anno di supporto esteso.

> [!div class="nextstepaction"]
> [Scaricare e installare Java](java-jdk-install.md)

## <a name="long-term-support-lts"></a>Supporto a lungo termine (LTS)

* [Java 11](https://www.azul.com/downloads/azure-only/zulu/?&version=java-11-lts)
* [Java 8](https://www.azul.com/downloads/azure-only/zulu/?&version=java-8-lts)
* [Java 7](https://www.azul.com/downloads/azure-only/zulu/?&version=java-7-lts)

## <a name="medium-term-support-mts"></a>Supporto a medio termine (MTS)

* [Java 13](https://www.azul.com/downloads/azure-only/zulu/?&version=java-13)

## <a name="technical-preview"></a>Anteprima tecnica

* [Java 14](https://www.azul.com/downloads/azure-only/zulu/?version=java-14)

## <a name="what-is-the-zulu-openjdk-for-azure"></a>Cos'è Zulu OpenJDK per Azure?

Le build Azul Zulu per Azure - Enterprise Edition di OpenJDK sono distribuzioni di OpenJDK gratuite, multipiattaforma e pronte per la produzione per Azure e Azure Stack, supportate da Microsoft e Azul Systems. Queste distribuzioni:

* Sono build di OpenJDK completamente open source fornite come pacchetti JDK (Java Development Kit), JRE (Java Runtime Environment) e JRE headless. Questi binari sono build commerciali di Java Standard Edition (SE) pienamente compatibili e conformi che è possibile usare con applicazioni o componenti Java in Azure e Azure Stack.
* Vengono fornite con supporto a lungo termine, tra cui correzioni di bug, miglioramenti delle prestazioni e patch di sicurezza.
* Sono disponibili per lo sviluppo e l'esecuzione di applicazioni Java in Windows, Linux e macOS.
* Sono disponibili come immagini di contenitori in Docker Hub e come macchine virtuali Windows e Linux in Azure Marketplace.
* Vengono usate in Microsoft Azure per supportare molti servizi di Azure, ad esempio:
  * Servizio app in Windows
  * Servizio app in Linux
  * Funzioni
  * Service Fabric
  * HDInsight
  * Ricerca
  * Azure DevOps
  * Cloud Shell  

## <a name="supported-java-versions-and-update-schedule"></a>Versioni Java supportate e piano degli aggiornamenti

Azul Systems fornisce build [Azul Zulu per Azure - Enterprise Edition](https://www.azul.com/downloads/azure-only/zulu/) completamente supportate per tutte le versioni di Java con supporto a lungo termine (LTS), incluse Java SE 7, 8, 11 e 13. Per altre informazioni, vedere il [comunicato stampa di Azul](https://www.azul.com/press_release/free-java-production-support-for-microsoft-azure-azure-stack) e la roadmap sul [ciclo di vita del supporto per i prodotti Azul](https://www.azul.com/products/azul_support_roadmap/).

|Versione di Java SE  |Data di fine supporto  |
|---------|----------|
|[![Versione Java supportata - LTS - Java 7](media/supported-java-versions-java-7.png)](https://www.azul.com/downloads/azure-only/zulu/?&version=java-7-lts) |Luglio 2023 - LTS|
|[![Versione Java supportata - LTS - Java 8](media/supported-java-versions-java-8.png)](https://www.azul.com/downloads/azure-only/zulu/?&version=java-8-lts) |Dicembre 2030 - LTS|
|[![Versione Java supportata - LTS - Java 11](media/supported-java-versions-java-11.png)](https://www.azul.com/downloads/azure-only/zulu/?&version=java-11-lts) |Settembre 2027 - LTS|
|[![Versione Java supportata - MTS - Java 13](media/supported-java-versions-java-13.png)](https://www.azul.com/downloads/azure-only/zulu/?&version=java-13) |Marzo 2023 - MTS|
|[![Versione Java supportata - Anteprima - Java 14](media/supported-java-versions-java-14.png)](https://www.azul.com/downloads/azure-only/zulu/?version=java-14) |**ANTEPRIMA**|

Queste versioni di JDK con supporto a medio e lungo termine includono aggiornamenti trimestrali di sicurezza e correzioni di bug, oltre a eventuali patch e aggiornamenti critici straordinari necessari.  Il supporto include il backporting a Java 7 e 8 degli aggiornamenti di sicurezza e delle correzioni di bug applicate alle versioni più recenti di Java, come Java 11, garantendo la continua stabilità e la sicurezza delle versioni precedenti di Java.  I clienti di Azure possono ottenere questi aggiornamenti di sicurezza e le correzioni di bug per la piattaforma senza incorrere in costi di sottoscrizione non pianificati per Java SE.

## <a name="benefits-for-developers"></a>Vantaggi per gli sviluppatori

Le versioni degli SDK Azul Zulu per Azure - Enterprise Edition sono:

1. Supporto assicurato sia da Microsoft che da Azul Systems

   * I binari Zulu sono pronti per la produzione e supportati da Microsoft e Azul Systems
   * Zulu viene fornito con supporto a lungo termine (LTS) gratuito per Java 7, 8 e 11 e supporto a medio termine (MTS) per Java 13. Il supporto a lungo termine (LTS) verrà fornito anche per Java 17. È possibile aggiornare le versioni di Java solo quando è necessario.
   * Java 7 è supportato fino a luglio 2023. Java 8 e 11 sono supportati fino a settembre 2027. Java 13 è supportato fino a marzo 2023.
   * Microsoft è impegnata a eseguire Zulu internamente nei computer usati per fornire molti servizi di Azure.

2. Pronte per la produzione

   * Build di OpenJDK completamente open source.
   * Soluzioni sostitutive per molte distribuzioni di Java SE senza richiedere modifiche.
   * JDK, JRE e JRE-headless
   * Java 7, 8, 11 e 13.
   * Conformità verificata alle specifiche di Java SE tramite Technology Compatibility Kit (TCK) della community OpenJDK.
   * Gli sviluppatori continueranno a ricevere aggiornamenti di produzione per Java SE, tra cui correzioni di bug, miglioramenti delle prestazioni e patch di sicurezza, per Java SE 7, 8, 11 e 13.

3. Supporto multipiattaforma. Zulu supporta binari per più piattaforme e versioni, tra cui:

   * Client Windows
     * 10
     * 8.1
     * 8, 7
   * Windows Server
     * 2016R2
     * 2016
     * 2012 R2
     * 2012
     * 2008 R2
   * Linux, tra cui
     * RHEL
     * CentOS
     * Ubuntu
     * SLES
     * Debian
     * Oracle Linux
   * Mac OS X
   * Disponibili in più tipi di pacchetti:
     * MSI, ZIP, TAR, DEB, RPM e DMG

    Le immagini di contenitori Docker certificate per Zulu JDK, JRE e JRE headless con più immagini del sistema operativo di base sono disponibili in Docker.

    Hub:

    * [JDK](https://hub.docker.com/_/microsoft-java-jdk)
    * [JRE](https://hub.docker.com/_/microsoft-java-jre)
    * [JRE headless](https://hub.docker.com/_/microsoft-java-jre-headless)

4. Nessun costo

   * Microsoft fornisce tutto il necessario per creare e ridimensionare le app Java in Azure senza costi aggiuntivi. Tramite Zulu si riceveranno gratuitamente aggiornamenti di sicurezza e correzioni di bug della piattaforma per le app Java.
   * [Java Flight Recorder e Mission Control](java-jdk-flight-recorder-and-mission-control.md) sono disponibili in Zulu Java 8, 11 e versioni successive.

5. Anteprima tecnica di versioni senza supporto a lungo/medio termine

   * Le anteprime tecniche offrono la possibilità di testare progressivamente le nuove funzionalità non appena vengono distribuite in versioni a breve termine, che eventualmente passeranno al supporto a lungo termine di Java 17.

6. Modifiche in upstream di OpenJDK

   * I committer di Azul Systems eseguono il push delle modifiche di Zulu in OpenJDK, per cui il repository upstream è completo e inclusivo.

Come sempre, gli sviluppatori Java possono usare i loro runtime Java, tra cui JDK Oracle e Red Hat, in Azure e sfruttarne l'infrastruttura sicura e i servizi ricchi di funzionalità. Per gli sviluppatori Java è anche disponibile l'edizione di produzione di Oracle Java SE per l'esecuzione di carichi di lavoro Java in macchine virtuali Windows o Linux in Azure.

## <a name="use-for-local-development"></a>Usare per lo sviluppo locale

Gli sviluppatori possono [scaricare](https://www.azul.com/downloads/azure-only/zulu/) i JDK Java per Azure e Azure Stack per l'uso in ambienti di sviluppo locali. I download sono disponibili per Windows, Linux e macOS. Gli sviluppatori che usano Linux possono ottenere i pacchetti anche tramite i gestori di pacchetti [yum](https://www.azul.com/downloads/azure-only/zulu/#yum-repo) e [apt](https://www.azul.com/downloads/azure-only/zulu/#apt-repo).

Per altre istruzioni, vedere l'articolo sulle [immagini Docker per Azure](java-jdk-docker-images.md).
