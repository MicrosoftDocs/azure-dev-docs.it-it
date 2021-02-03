---
title: Eseguire la registrazione con Azure SDK per Java e Logback
description: Panoramica dell'integrazione di Azure SDK per Java con logback
author: srnagar
ms.date: 02/02/2021
ms.topic: conceptual
ms.custom: devx-track-java
ms.author: srnagar
ms.openlocfilehash: ca59b7dc9f861dd833788c88c18ce8b14a016e70
ms.sourcegitcommit: 71847ee0a1fee3f3320503629d9a8c82319a1f6a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/03/2021
ms.locfileid: "99522109"
---
# <a name="log-with-the-azure-sdk-for-java-and-logback"></a>Eseguire la registrazione con Azure SDK per Java e Logback

Questo articolo fornisce una panoramica su come aggiungere la registrazione con Logback alle applicazioni che usano Azure SDK per Java. Come indicato in [configurare la registrazione in Azure SDK per Java](logging-overview.md), tutte le librerie client di Azure accedono a [SLF4J](http://www.slf4j.org/), in modo da poter usare i Framework di registrazione, ad esempio [Logback](http://logback.qos.ch/).

Per abilitare la registrazione Logback, è necessario eseguire due operazioni:

1. Includere la libreria Logback come dipendenza,
2. Creare un file denominato *logback.xml* nella directory del progetto */src/main/Resources* .

Per ulteriori informazioni sulla configurazione di Logback, vedere la pagina relativa alla [configurazione di Logback](http://logback.qos.ch/manual/configuration.html) nella documentazione di Logback.

## <a name="add-the-maven-dependency"></a>Aggiungere la dipendenza Maven

Per aggiungere la dipendenza Maven, includere il codice XML seguente nel file di *pom.xml* del progetto. Sostituire il numero di versione *1.2.3* con il numero di versione rilasciata più recente visualizzato nella [pagina del modulo Logback classico](https://mvnrepository.com/artifact/ch.qos.logback/logback-classic).

```xml
<dependency>
    <groupId>ch.qos.logback</groupId>
    <artifactId>logback-classic</artifactId>
    <version>1.2.3</version>
</dependency>
```

## <a name="add-logbackxml-to-your-project"></a>Aggiungere logback.xml al progetto

[Logback](https://logback.qos.ch/manual/introduction.html) è uno dei framework di registrazione più diffusi. Per abilitare la registrazione Logback, creare un file denominato *logback.xml* nella directory *./src/main/Resources* del progetto. Questo file conterrà le configurazioni di registrazione per personalizzare le esigenze di registrazione. Per ulteriori informazioni sulla configurazione di *logback.xml*, vedere la pagina relativa alla [configurazione di Logback](https://logback.qos.ch/manual/configuration.html) nella documentazione di Logback.

### <a name="console-logging"></a>Registrazione console

È possibile creare una configurazione Logback per accedere alla console, come illustrato nell'esempio seguente. Questo esempio è configurato per registrare tutti gli eventi di registrazione di livello INFO o superiore, ovunque si trovino.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
    <layout class="ch.qos.logback.classic.PatternLayout">
      <Pattern>
        %black(%d{ISO8601}) %highlight(%-5level) [%blue(%t)] %blue(%logger{100}): %msg%n%throwable
      </Pattern>
    </layout>
  </appender>

  <root level="INFO">
    <appender-ref ref="STDOUT" />
  </root>
</configuration>
```

### <a name="log-azure-core-errors"></a>Registrare gli errori di Azure Core

La configurazione di esempio seguente è simile a quella precedente, ma riduce il livello di registrazione proveniente da tutte le `com.azure.core` classi in pacchetto (inclusi i pacchetti sottopacchetti). In questo modo, viene registrato tutto il livello di informazioni e superiore, ad eccezione di `com.azure.core` , in cui vengono registrati solo i livelli di errore e superiori. Ad esempio, è possibile usare questo approccio se il codice è `com.azure.core` troppo rumoroso. Questo tipo di configurazione può anche andare in entrambe le direzioni. Se ad esempio si desidera ottenere più informazioni di debug dalle classi in `com.azure.core` , è possibile modificare questa impostazione in debug.

È possibile avere un controllo accurato sulla registrazione di classi specifiche o di pacchetti specifici. Come illustrato di seguito, `com.azure.core` controllerà l'output di tutte le classi principali, ma è possibile usare `com.azure.security.keyvault` o equivalente per controllare l'output in modo appropriato per le circostanze che risultano più informative nel contesto dell'applicazione in esecuzione.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
    <encoder>
      <pattern>%message%n</pattern>
    </encoder>
  </appender>

  <logger name="com.azure.core" level="ERROR" />

  <root level="INFO">
    <appender-ref ref="STDOUT" />
  </root>
</configuration>
```

### <a name="log-to-a-file-with-log-rotation-enabled"></a>Registra in un file con rotazione del log abilitata

Gli esempi precedenti registrano nella console, che in genere non è il percorso preferito per i log. Usare la configurazione seguente per accedere a un file, con il rollup orario e l'archiviazione in formato gzip:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <property name="LOGS" value="./logs" />
  <appender name="RollingFile" class="ch.qos.logback.core.rolling.RollingFileAppender">
    <file>${LOGS}/spring-boot-logger.log</file>
    <encoder class="ch.qos.logback.classic.encoder.PatternLayoutEncoder">
      <Pattern>%d %p %C{1.} [%t] %m%n</Pattern>
    </encoder>

    <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
      <!-- rollover hourly and gzip logs -->
      <fileNamePattern>${LOGS}/archived/spring-boot-logger-%d{yyyy-MM-dd-HH}.log.gz</fileNamePattern>
    </rollingPolicy>
  </appender>

  <!-- LOG everything at INFO level -->
  <root level="INFO">
    <appender-ref ref="RollingFile" />
  </root>
</configuration>
```

### <a name="spring-applications"></a>Applicazioni Spring

Spring Framework funziona leggendo il file Spring *Application. Properties* per diverse configurazioni, inclusa la configurazione della registrazione. Tuttavia, è possibile configurare l'applicazione Spring per leggere le configurazioni Logback da qualsiasi file. A tale scopo, configurare la `logging.config` Proprietà in modo che punti al file di configurazione *logback.xml* aggiungendo la riga seguente nel file Spring */src/main/Resources/Application.Properties* :

```properties
logging.config=classpath:logback.xml
```

## <a name="next-steps"></a>Passaggi successivi

Questo articolo ha illustrato la configurazione di Logback e come fare in modo che Azure SDK per Java lo usi per la registrazione. Poiché Azure SDK per Java funziona con tutti i Framework di registrazione SLF4J, vedere la pagina relativa al [manuale dell'utente di SLF4J](http://www.slf4j.org/manual.html) per altri dettagli. Se si usa Logback, è disponibile anche una grande quantità di indicazioni sulla configurazione nel sito Web. Per ulteriori informazioni, vedere la pagina relativa alla [configurazione di Logback](http://logback.qos.ch/manual/configuration.html) nella documentazione di Logback.

Una volta completata la registrazione, provare a esaminare le integrazioni offerte da Azure in Framework come [Spring](/azure/developer/java/spring-framework/spring-boot-starters-for-azure) e [microprofile](/azure/developer/java/eclipse-microprofile/).
