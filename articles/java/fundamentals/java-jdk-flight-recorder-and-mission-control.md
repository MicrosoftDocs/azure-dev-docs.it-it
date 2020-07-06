---
title: Esaminare i dati con Zulu Flight Recorder e Mission Control
description: Istruzioni per l'uso di Zulu Flight Recorder e Mission Control per raccogliere ed esaminare i dati delle app.
ms.date: 04/09/2019
ms.topic: conceptual
ms.custom: seo-java-july2019, seo-java-august2019, seo-java-september2019
ms.openlocfilehash: afd95e7f39fb9abdfe2261c8ef0f5ac961347ffb
ms.sourcegitcommit: bbfa6e0dfb3c8e66e5f47b080590105787a6e74b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/26/2020
ms.locfileid: "85418199"
---
# <a name="monitor-and-manage-java-workloads-with-zulu-flight-recorder-and-zulu-mission-control"></a>Monitorare e gestire i carichi di lavoro Java con Zulu Flight Recorder e Zulu Mission Control

Questo articolo illustra come monitorare e gestire i carichi di lavoro Java con Zulu Flight Recorder e Zulu Mission Control.

Zulu Mission Control è una build completamente testata di JDK Mission Control. La soluzione Mission Control è stata resa open source da Oracle nel 2018 e viene gestita come progetto nella famiglia OpenJDK. Insieme a Flight Recorder, Mission Control offre funzionalità interattive di monitoraggio e gestione con sovraccarico ridotto per i carichi di lavoro Java.

Zulu Mission Control è compatibile con i seguenti JDK/JRE:

* Zulu 12.1 e versioni successive
* Zulu 11.0 e versioni successive
* Zulu 8u202 (8.36) e versioni successive
* Oracle OpenJDK 11+15 e versioni successive
* Oracle Java 11.0 e versioni successive
* Oracle Java 8.0 e versioni successive

Seguire la procedura seguente per installare Zulu Mission Control, connettersi a una Java Virtual Machine (JVM) e acquisire visibilità in tempo reale su tutti gli aspetti di un'applicazione in esecuzione.

1. [Installare un JDK/JRE compatibile con Zulu Mission Control](java-jdk-install.md).

2. Scaricare Zulu Mission Control dal [sito di download Azul](https://www.azul.com/products/zulu-mission-control/), scegliere la versione appropriata per il sistema, salvarlo in locale e passare a questa directory.

3. Espandere il file scaricato.

    **Linux:**

    ```cli
    tar -xzvf zmc7.0.0-EA-linux_x64.tar.gz
    ```

    **Windows:**

    ```cli
    unzip -zxvf zmc7.0.0-EA-win_x64.zip
    ```

    **macOS:**

    ```cli
    tar -xzvf zmc7.0.0-EA-macosx_x64.tar.gz
    ```

4. Avviare l'applicazione Java con uno dei JDK compatibili. Ad esempio:

    ```cli
    $JAVA_HOME/bin/java -jar MyApplication.jar
    ```

5. Avviare Zulu Mission Control

    **Linux:**

    ```cli
    zmc7.0.0-EA-linux_x64/zmc
    ```

    **Windows:**

    ```cli
    zmc7.0.0-EA-win_x64\zmc.exe
    ```

    **macOS:**

    ```cli
    zmc7.0.0-EA-macosx_x64/Zulu\ Mission\ Control.app/Contents/MacOS/zmc
    ```

6. Cambiare l'installazione di JVM per Mission Control (facoltativo).

    In Windows *zmc.exe* userà l'installazione di JVM predefinita configurata nel Registro di sistema. È necessario avviare Zulu Mission Control da un JDK completo per rilevare automaticamente le istanze locali di JVM. Se si usa un ambiente JRE, verrà visualizzato l'avviso seguente:

    > [!div class="mx-imgBorder"]
    ![Messaggio di avviso visualizzato se l'installazione di JDK è solo per JRE](media/jfr-jre-warning-message.png)

    Per cambiare la JVM usata da Mission Control, procedere come segue:

    1. Aprire il file di configurazione *zmc.ini* situato nella stessa directory di *zmc.exe*.

    2. Aggiungere due righe prima della riga `-vmargs`:

        * Nella prima riga scrivere `–vm`.
        * Nella seconda riga scrivere il percorso dell'installazione di JDK. Ad esempio `C:\Program Files\Java\jdk1.8.0_212\bin\javaw.exe`.

7. Individuare la JVM che esegue l'applicazione.

    1. Nel riquadro in alto a sinistra della finestra Zulu Mission Control fare clic sulla scheda **JVM Browser**.

    2. Selezionare ed espandere la voce dell'elenco in alto a sinistra relativa all'istanza della JVM che esegue l'applicazione.

    > [!div class="mx-imgBorder"]
    ![Espandere la voce dell'elenco in alto a sinistra per l'istanza della JVM](media/jfr-jvm-instance-dashboard.png)

8. Avviare una registrazione dei dati, se necessario.

    1. Se Flight Recorder indica che non sono presenti registrazioni, avviarne una. Per avviare una registrazione, fare clic con il pulsante destro del mouse sulla riga Flight Recorder nella scheda JVM Browser e quindi selezionare **Start Flight Recording** (Avvia la registrazione di dati).

    2. Selezionare una registrazione a durata fissa oppure continua e una configurazione di profilatura (dettagliata) oppure continua (sovraccarico inferiore), quindi selezionare **Finish** (Fine).

    > [!div class="mx-imgBorder"]
    ![Avviare la registrazione dei dati](media/jfr-start-flight-recording.png)

9. Eseguire il dump della registrazione dei dati.

    1. Sotto la riga Flight Recorder nella scheda JVM Browser dovrebbe essere visualizzata una registrazione. Fare clic con il pulsante destro del mouse sulla riga corrispondente e scegliere **Dump whole recording** (Dump dell'intera registrazione).

    2. Nel riquadro grande a destra della finestra Zulu Mission Control verrà visualizzata una nuova scheda. Questo riquadro rappresenta la registrazione di cui è stato appena eseguito il dump dalla JVM che esegue l'applicazione.

10. Esaminare la registrazione usando Zulu Mission Control
    1. Se non è già attivata, selezionare la scheda **Outline** (Struttura) nel riquadro sinistro della finestra Zulu Mission Control. Questa scheda contiene diverse visualizzazioni dei dati raccolti con la registrazione.

    > [!div class="mx-imgBorder"]
    ![Esaminare la registrazione dei dati](media/jfr-zulu-mission-control-data.png)

## <a name="resources"></a>Risorse

È anche disponibile un [video dimostrativo](https://www.azul.com/presentation/azul-webinar-open-source-flight-recorder-and-mission-control-managing-and-measuring-openjdk-8-performance/) presentato dal vice CTO di Azul Systems, Simon Ritter. Questo video illustra le procedure di configurazione e impostazione di Flight Recorder e Zulu Mission Control. La discussione su Flight Recorder inizia al minuto 31:30.
