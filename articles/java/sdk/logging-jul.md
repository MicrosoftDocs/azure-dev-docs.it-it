---
title: Log con Azure SDK per Java e Java. util. Logging
description: Panoramica dell'integrazione di Azure SDK per Java con Java. util. Logging
author: srnagar
ms.date: 02/02/2021
ms.topic: conceptual
ms.custom: devx-track-java
ms.author: srnagar
ms.openlocfilehash: 48fa4dac679e8b39139e03ae65f331072a063710
ms.sourcegitcommit: 71847ee0a1fee3f3320503629d9a8c82319a1f6a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/03/2021
ms.locfileid: "99522114"
---
# <a name="log-with-the-azure-sdk-for-java-and-javautillogging"></a>Log con Azure SDK per Java e Java. util. Logging

Questo articolo fornisce una panoramica su come aggiungere la registrazione usando Java. util. Logging per le applicazioni che usano Azure SDK per Java. Il framework Java. util. Logging fa parte del JDK. Come indicato in [configurare la registrazione in Azure SDK per Java](logging-overview.md), tutte le librerie client di Azure accedono a [SLF4J](http://www.slf4j.org/), in modo da poter usare i Framework di registrazione, ad esempio [java. util. Logging](https://docs.oracle.com/javase/8/docs/api/java/util/logging/Logger.html).

Per abilitare Java. util. Logging, è necessario eseguire due operazioni:

1. Includere l'adattatore SLF4J per Java. util. Logging come dipendenza.
2. Creare un file denominato *Logging. Properties* nella directory del progetto */src/main/Resources* .

Per ulteriori informazioni relative alla configurazione del logger, vedere [configurazione dell'output di registrazione](https://docs.oracle.com/cd/E23549_01/doc.1111/e14568/handler.htm) nella documentazione di Oracle.

## <a name="add-the-maven-dependency"></a>Aggiungere la dipendenza Maven

Per aggiungere la dipendenza Maven, includere il codice XML seguente nel file di *pom.xml* del progetto. Sostituire il numero di versione di *1.7.30* con il numero di versione rilasciata più recente visualizzato nella [pagina Binding JDK14 SLF4J](https://mvnrepository.com/artifact/org.slf4j/slf4j-jdk14).

```xml
<dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>slf4j-jdk14</artifactId>
    <version>1.7.30</version> <!-- replace this version with the latest available version on Maven central -->
</dependency>
```

## <a name="add-loggingproperties-to-your-project"></a>Aggiungere Logging. Properties al progetto

Per eseguire la registrazione usando `java.util.logging` , creare un file denominato *Logging. Properties* nella directory *./src/main/Resources* del progetto. Questo file conterrà le configurazioni di registrazione per personalizzare le esigenze di registrazione. Per ulteriori informazioni, vedere [Java Logging: Configuration](http://tutorials.jenkov.com/java-logging/configuration.html).

Se si desidera utilizzare un nome di file diverso da *Logging. Properties*, è possibile eseguire questa operazione impostando la `java.util.logging.config.file` proprietà di sistema. Questa proprietà deve essere impostata prima della creazione dell'istanza del logger.

### <a name="console-logging"></a>Registrazione console

È possibile creare una configurazione per accedere alla console, come illustrato nell'esempio seguente. Questo esempio è configurato per registrare tutti gli eventi di registrazione di livello INFO o superiore, ovunque si trovino.

```properties
handlers = java.util.logging.ConsoleHandler
.level = INFO

java.util.logging.ConsoleHandler.level = INFO
java.util.logging.ConsoleHandler.formatter = java.util.logging.SimpleFormatter
java.util.logging.SimpleFormatter.format=[%1$tF %1$tT] [%4$s] %5$s %n
```

### <a name="log-to-a-file"></a>Registra in un file

Nell'esempio precedente viene registrato nella console, che in genere non è il percorso preferito per i log. Per configurare la registrazione in un file, usare la configurazione seguente:

```properties
handlers = java.util.logging.FileHandler
.level = INFO

java.util.logging.FileHandler.pattern = %h/myapplication.log
java.util.logging.FileHandler.formatter = java.util.logging.SimpleFormatter
java.util.logging.FileHandler.level = INFO
```

Questo codice creerà un file denominato *MyApplication. log* nella Home Directory ( `%h` ). Questo logger non supporta la rotazione automatica del file dopo un determinato periodo. Se questa funzionalità è necessaria, è necessario scrivere un'utilità di pianificazione per gestire la rotazione del file di log.

## <a name="next-steps"></a>Passaggi successivi

Questo articolo ha illustrato la configurazione di `java.util.logging` e come fare in modo che Azure SDK per Java lo usi per la registrazione. Poiché Azure SDK per Java funziona con tutti i Framework di registrazione SLF4J, vedere la pagina relativa al [manuale dell'utente di SLF4J](http://www.slf4j.org/manual.html) per altri dettagli.

Una volta completata la registrazione, provare a esaminare le integrazioni offerte da Azure in Framework come [Spring](/azure/developer/java/spring-framework/spring-boot-starters-for-azure) e [microprofile](/azure/developer/java/eclipse-microprofile/).
