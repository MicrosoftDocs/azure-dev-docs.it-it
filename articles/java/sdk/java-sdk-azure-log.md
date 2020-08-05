---
title: Configurare la registrazione con Azure SDK per Java
description: Informazioni su come configurare i framework di registrazione per le librerie client di Azure SDK per Java
keywords: Azure, Java, SDK, registrazione
author: bmitchell287
ms.author: brendm
ms.date: 03/25/2020
ms.topic: article
ms.service: multiple
ms.custom: devx-track-java
ms.openlocfilehash: 5bb7f711eae230a08893d2f94c242a06af809f88
ms.sourcegitcommit: cf23d382eee2431a3958b1c87c897b270587bde0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/29/2020
ms.locfileid: "87400619"
---
# <a name="configure-logging-with-the-azure-sdk-for-java"></a>Configurare la registrazione con Azure SDK per Java

Questo articolo fornisce le configurazioni di registrazione di esempio per [Azure SDK](https://azure.microsoft.com/downloads/) per Java. Per informazioni più dettagliate sulle opzioni di configurazione, ad esempio l'impostazione dei livelli di log o la registrazione personalizzata per classe, vedere la documentazione relativa al framework di registrazione scelto.

Le librerie client di Azure SDK per Java usano [Simple Logging Facade for Java](https://www.slf4j.org/) (SLF4J). SLF4J consente di usare il framework di registrazione preferito, che viene chiamato al momento della distribuzione dell'applicazione.

> [!NOTE]
> Questo articolo si applica alle versioni più recenti delle librerie client di Azure SDK. Per verificare se una libreria è supportata, vedere l'elenco delle versioni più recenti di [Azure SDK](https://azure.github.io/azure-sdk/releases/latest/java.html). Se l'applicazione usa una versione precedente delle librerie client di Azure SDK, fare riferimento a istruzioni specifiche nella documentazione del servizio applicabile.

## <a name="declare-a-logging-framework"></a>Dichiarare un framework di registrazione

Prima di implementare questi logger, è necessario dichiarare il framework pertinente come dipendenza nel progetto. Per altre informazioni, vedere il [manuale dell'utente di SLF4J](https://www.slf4j.org/manual.html#projectDep).

Le sezioni seguenti includono esempi di configurazione per i framework di registrazione più comuni.

## <a name="use-log4j"></a>Usare Log4j

Gli esempi seguenti illustrano le configurazioni per il framework di registrazione log4j. Per altre informazioni, vedere la [documentazione di Log4j](https://logging.apache.org/log4j/1.2/).

**Abilitare log4j aggiungendo una dipendenza Maven**

Aggiungere quanto segue al file *pom.xml* del progetto:

```xml
<!-- https://mvnrepository.com/artifact/org.slf4j/slf4j-log4j12 -->
<dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>slf4j-log4j12</artifactId>
    <version>[1.0,)</version> <!-- Version number 1.0 and above -->
</dependency>
```

**Abilitare Log4j usando un file delle proprietà**

Creare un file *log4j.properties* nella directory *./src/main/resource* del progetto e aggiungere il contenuto seguente:

```properties
log4j.rootLogger=INFO, A1
log4j.appender.A1=org.apache.log4j.ConsoleAppender
log4j.appender.A1.layout=org.apache.log4j.PatternLayout
log4j.appender.A1.layout.ConversionPattern=%m%n
log4j.logger.com.azure.core=ERROR
```

**Abilitare log4j usando un file XML**

Creare un file *log4j.xml* nella directory *./src/main/resource* del progetto e aggiungere il contenuto seguente:

```xml
<!DOCTYPE log4j:configuration SYSTEM "log4j.dtd">
<log4j:configuration debug="true" xmlns:log4j='http://jakarta.apache.org/log4j/'>

    <appender name="console" class="org.apache.log4j.ConsoleAppender">
        <param name="Target" value="System.out"/>
        <layout class="org.apache.log4j.PatternLayout">
            <param name="ConversionPattern" value="%m%n" />
        </layout>
    </appender>
    <logger name="com.azure.core">
        <level value="ERROR" />
        <appender-ref ref="console" />
    </logger>

    <root>
        <level value="info" />
        <appender-ref ref="console" />
    </root>

</log4j:configuration>
```

## <a name="use-log4j-2"></a>Usare Log4j 2

Gli esempi seguenti illustrano le configurazioni per il framework di registrazione log4j 2. Per altre informazioni, vedere la [documentazione di Log4j 2](https://logging.apache.org/log4j/2.x/manual/configuration.html).

**Abilitare log4j 2 aggiungendo una dipendenza Maven**

Aggiungere quanto segue al file *pom.xml* del progetto:

```
<!-- https://mvnrepository.com/artifact/org.apache.logging.log4j/log4j-slf4j-impl -->
<dependency>
    <groupId>org.apache.logging.log4j</groupId>
    <artifactId>log4j-slf4j-impl</artifactId>
    <version>[2.0,)</version> <!-- Version number 2.0 and above -->
</dependency>
```

**Abilitare Log4j 2 usando un file delle proprietà**

Creare un file *log4j2.properties* nella directory *./src/main/resource* del progetto e aggiungere il contenuto seguente:

```properties
appender.console.type = Console
appender.console.name = STDOUT
appender.console.layout.type = PatternLayout
appender.console.layout.pattern = %msg%n
logger.app.name=com.azure.core
logger.app.level=ERROR

rootLogger.level = info
rootLogger.appenderRefs = stdout
rootLogger.appenderRef.stdout.ref = STDOUT
```

**Abilitare log4j 2 usando un file XML**

Creare un file *log4j2.xml* nella directory *./src/main/resource* del progetto e aggiungere il contenuto seguente:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Configuration status="INFO">
    <Appenders>
        <Console name="console" target="SYSTEM_OUT">
            <PatternLayout
                pattern="%msg%n" />
        </Console>
    </Appenders>
    <Loggers>
        <Logger name="com.azure.core" level="error" additivity="true">
            <appender-ref ref="console" />
        </Logger>
        <Root level="info" additivity="false">
            <appender-ref ref="console" />
        </Root>
     </Loggers>
</Configuration>
```

## <a name="use-logback"></a>Usare Logback

Gli esempi seguenti illustrano le configurazioni per il framework di registrazione Logback. Per altre informazioni, vedere la [documentazione di Logback](https://logback.qos.ch/manual/configuration.html).

**Abilitare Logback aggiungendo una dipendenza Maven**

Aggiungere quanto segue al file *pom.xml* del progetto:

```
<!-- https://mvnrepository.com/artifact/ch.qos.logback/logback-classic -->
<dependency>
    <groupId>ch.qos.logback</groupId>
    <artifactId>logback-classic</artifactId>
    <version>[0.2.5,)</version> <!-- Version number 0.2.5 and above -->
</dependency>
```

**Abilitare Logback usando un file XML**

Creare un file *logback.xml* nella directory *./src/main/resource* del progetto e aggiungere il contenuto seguente:

```xml
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

## <a name="use-logback-in-a-spring-boot-application"></a>Usare Logback in un'applicazione Spring Boot

Gli esempi seguenti illustrano alcune configurazioni per l'uso di Logback con Spring. In genere le configurazioni della registrazione si aggiungono a un file *logback.xml* nella directory *./src/main/resources* del progetto. Spring esamina questo file per trovare varie configurazioni, inclusa quella della registrazione. Per altre informazioni, vedere la [documentazione di Logback](https://logback.qos.ch/manual/configuration.html).

È possibile configurare l'applicazione in modo da leggere le configurazioni di Logback da qualsiasi file. Per collegare il file *logback.xml* all'applicazione Spring, creare un file *application.properties* nella directory *./src/main/resources* del progetto e aggiungere il contenuto seguente:

```properties
logging.config=classpath:logback.xml
```

Per creare una configurazione di Logback per la registrazione nella console, aggiungere quanto segue al file *logback.xml*:

```xml 
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <appender name="Console"
    class="ch.qos.logback.core.ConsoleAppender">
    <layout class="ch.qos.logback.classic.PatternLayout">
      <Pattern>
        %black(%d{ISO8601}) %highlight(%-5level) [%blue(%t)] %blue(%logger{100}): %msg%n%throwable
      </Pattern>
    </layout>
  </appender>

  <root level="INFO">
    <appender-ref ref="Console" />
  </root>
</configuration>
```

Per configurare la registrazione in un file di cui viene eseguito il rollover dopo ogni ora e che viene archiviato in formato gzip, aggiungere quanto segue al file *logback.xml*:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <property name="LOGS" value="./logs" />
  <appender name="RollingFile" class="ch.qos.logback.core.rolling.RollingFileAppender">
    <file>${LOGS}/spring-boot-logger.log</file>
    <encoder
      class="ch.qos.logback.classic.encoder.PatternLayoutEncoder">
      <Pattern>%d %p %C{1.} [%t] %m%n</Pattern>
    </encoder>

    <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
      <!-- rollover hourly and gzip logs -->
      <fileNamePattern>${LOGS}/archived/spring-boot-logger-%d{yyyy-MM-dd-HH}.log.gz</fileNamePattern>
    </rollingPolicy>
  </appender>

  <!-- LOG everything at INFO level -->
  <root level="info">
    <appender-ref ref="RollingFile" />
  </root>
</configuration>
```

## <a name="configure-fallback-logging-for-temporary-debugging"></a>Configurare la registrazione di fallback per il debug temporaneo

Negli scenari in cui non è possibile ridistribuire l'applicazione con un logger SLF4J, è possibile usare il logger di fallback incorporato nelle librerie client di Azure per Java, in Azure Core 1.3.0 o versione successiva. Per abilitare questo logger, è necessario prima verificare che non esista un logger SLF4J (poiché avrà la precedenza), quindi impostare la variabile di ambiente `AZURE_LOG_LEVEL`. Dopo aver impostato la variabile di ambiente, è necessario riavviare l'applicazione per avviare la generazione dei log.

La tabella seguente mostra i valori consentiti per questa variabile di ambiente.

|Livello di log   |Valori consentiti della variabile di ambiente   |
|----------|-----------|
|DETTAGLIATO   |"verbose", "debug"     |
|INFORMATIVO|"info", "information", "informational"  |
|WARNING     |"warn", "warning"       |
|ERRORE    |"err", "error"  |

## <a name="next-steps"></a>Passaggi successivi

- [Abilitare la registrazione diagnostica per le app nel Servizio app di Azure](/azure/app-service/troubleshoot-diagnostic-logs) 
- [Esaminare le opzioni di registrazione e controllo di sicurezza di Azure](/azure/security/fundamentals/log-audit)
- [Informazioni su come usare i log della piattaforma Azure](/azure/azure-monitor/platform/platform-logs-overview)
