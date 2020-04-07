---
title: Configurare la registrazione con Azure SDK per Java
description: Informazioni su come configurare i framework di registrazione per le librerie client di Azure SDK per Java
keywords: Azure, Java, SDK, registrazione
author: bmitchell287
ms.author: brendm
ms.date: 03/25/2020
ms.topic: article
ms.service: multiple
ms.openlocfilehash: 9f7ad8d772b7de1335ebc3ba77cdea8aa1e8b683
ms.sourcegitcommit: 31f6d047f244f1e447faed6d503afcbc529bd28c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80319901"
---
# <a name="configure-logging-with-the-azure-sdk-for-java"></a>Configurare la registrazione con Azure SDK per Java

Questo articolo fornisce le configurazioni di registrazione di esempio per [Azure SDK](https://azure.microsoft.com/downloads/) per Java. Per informazioni più dettagliate sulle opzioni di configurazione, ad esempio l'impostazione dei livelli di log o la registrazione personalizzata per classe, vedere la documentazione relativa al framework di registrazione scelto.

Le librerie client di Azure SDK per Java usano [Simple Logging Facade for Java](https://www.slf4j.org/) (SLF4J). SLF4J consente di usare il framework di registrazione preferito, che viene chiamato al momento della distribuzione dell'applicazione.

> [!NOTE]
> Questo articolo si applica alle versioni più recenti delle librerie client di Azure SDK. Per verificare se una libreria è supportata, vedere l'elenco delle versioni più recenti di [Azure SDK](https://azure.github.io/azure-sdk/releases/latest/java.html). Se l'applicazione usa una versione precedente delle librerie client di Azure SDK, fare riferimento a istruzioni specifiche nella documentazione del servizio applicabile.

## <a name="declare-a-logging-framework"></a>Dichiarare un framework di registrazione

Prima di implementare questi logger, è necessario dichiarare il framework pertinente come dipendenza nel progetto. Per altre informazioni, vedere il [manuale dell'utente di SLF4J](http://www.slf4j.org/manual.html#projectDep).

## <a name="configure-log4j-or-log4j-2"></a>Configurare Log4j o Log4j 2

È possibile configurare la registrazione di Log4j e log4j 2 in un file di proprietà o in un file XML. Per informazioni dettagliate sulla registrazione di Log4j e log4j 2, vedere il [manuale di Apache Log4j 2](https://logging.apache.org/log4j/2.x/manual/configuration.html).

### <a name="use-a-properties-file"></a>Usare un file di proprietà

Nella directory *./src/main/resource* del progetto creare un nuovo file denominato *log4j.properties* o *log4j2.properties* (quest'ultimo per Logj4 2). Usare questi esempi per iniziare.

Esempio di Log4j:

```properties
log4j.rootLogger=INFO, A1
log4j.appender.A1=org.apache.log4j.ConsoleAppender
log4j.appender.A1.layout=org.apache.log4j.PatternLayout
log4j.appender.A1.layout.ConversionPattern=%m%n
log4j.logger.com.azure.core=ERROR
```

Esempio di Log4j 2:

```properties
appender.console.type = Console
appender.console.name = LogToConsole
appender.console.layout.type = PatternLayout
appender.console.layout.pattern = %msg%n
logger.app.name=com.azure.core
logger.app.level=ERROR
```

### <a name="use-an-xml-file"></a>Usare un file XML

In alternativa, è possibile usare un file XML per configurare Log4j e Log4j 2. Nella directory *./src/main/resource* del progetto creare un nuovo file denominato *log4j.xml* o *log4j2.xml* (quest'ultimo per Logj4 2). Usare questi esempi per iniziare.

Esempio di Log4j:

```xml
<!DOCTYPE log4j:configuration SYSTEM "log4j.dtd">
<log4j:configuration debug="true" xmlns:log4j='http://jakarta.apache.org/log4j/'>

  <appender name="console" class="org.apache.log4j.ConsoleAppender">
    <param name="Target" value="System.out"/>
    <layout class="org.apache.log4j.PatternLayout">
    <param name="ConversionPattern" value="%m%n" />
    </layout>
  </appender>
  <logger name="com.azure.core" additivity="true">
    <level value="ERROR" />
    <appender-ref ref="console" />
  </logger>

  <root>
    <priority value ="info"></priority>
    <appender-ref ref="console"></appender>
  </root>

</log4j:configuration>
```

Esempio di Log4j 2:

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

## <a name="configure-logback"></a>Configurare Logback

[Logback](https://logback.qos.ch/manual/introduction.html) è uno dei framework di registrazione più diffusi e un'implementazione nativa di SLF4J. Per configurare Logback, creare un nuovo file XML denominato *logback.xml* nella directory *./src/main/resources* del progetto. Per altre informazioni sulle opzioni di configurazione, vedere il [sito Web del progetto Logback](https://logback.qos.ch/manual/configuration.html).

Ecco un esempio di configurazione di Logback:

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

Ecco una semplice configurazione di Logback per la registrazione nella console:

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

Ecco una configurazione per la registrazione in un file di cui viene eseguito il rollback dopo ogni ora e che viene archiviato in formato di file GZIP:

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

### <a name="configure-logback-for-a-spring-boot-application"></a>Configurare Logback per un'applicazione Spring Boot

Spring cerca le configurazioni del progetto, inclusa la registrazione, nel file *application.properties*, che si trova nella directory *./src/main/resources*. Nel file *application.properties* aggiungere la riga seguente per collegare il file *logback.xml* all'applicazione Spring Boot:

```properties
logging.config=classpath:logback.xml
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
