---
title: Eseguire la registrazione con Azure SDK per Java e log4j
description: Panoramica dell'integrazione di Azure SDK per Java con log4j
author: srnagar
ms.date: 02/02/2021
ms.topic: conceptual
ms.custom: devx-track-java
ms.author: srnagar
ms.openlocfilehash: 537c030b81913c660b873ac028a7bed90c464b67
ms.sourcegitcommit: 71847ee0a1fee3f3320503629d9a8c82319a1f6a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/03/2021
ms.locfileid: "99522111"
---
# <a name="log-with-the-azure-sdk-for-java-and-log4j"></a>Eseguire la registrazione con Azure SDK per Java e log4j

Questo articolo fornisce una panoramica su come aggiungere la registrazione con log4j alle applicazioni che usano Azure SDK per Java. Come indicato in [configurare la registrazione in Azure SDK per Java](logging-overview.md), tutte le librerie client di Azure accedono a [SLF4J](http://www.slf4j.org/), in modo da poter usare i Framework di registrazione, ad esempio [log4j](https://logging.apache.org/log4j/2.x/).

Questo articolo fornisce indicazioni per l'uso delle versioni Log4J 2. x, ma Log4J 1. x è ugualmente supportato da Azure SDK per Java. Per abilitare la registrazione log4j, è necessario eseguire due operazioni:

1. Includere la libreria log4j come dipendenza,
2. Creare un file di configurazione ( *log4j2. Properties* o *log4j2.xml*) nella directory del progetto */src/main/Resources* .

Per altre informazioni relative alla configurazione di log4j, vedere [Benvenuti in log4j 2](https://logging.apache.org/log4j/2.x/manual/index.html).

## <a name="add-the-maven-dependency"></a>Aggiungere la dipendenza Maven

Per aggiungere la dipendenza Maven, includere il codice XML seguente nel file di *pom.xml* del progetto. Sostituire il numero di versione di *2.14.0* con il numero di versione rilasciata più recente visualizzato nella [pagina Binding di Apache log4j SLF4J](https://mvnrepository.com/artifact/org.apache.logging.log4j/log4j-slf4j-impl).

```xml
<dependency>
    <groupId>org.apache.logging.log4j</groupId>
    <artifactId>log4j-slf4j-impl</artifactId>
    <version>2.14.0</version>
</dependency>
```

## <a name="configuring-log4j"></a>Configurazione di log4j

Esistono due modi comuni per configurare Log4j: tramite un file di proprietà esterne o tramite un file XML esterno. Questi approcci sono descritti di seguito.

### <a name="using-a-property-file"></a>Uso di un file di proprietà

È possibile inserire un file flat properties denominato *log4j2. Properties* nella directory */src/main/Resource* del progetto. Il file deve avere il formato seguente:

```properties
appender.console.type = Console
appender.console.name = STDOUT
appender.console.layout.type = PatternLayout
appender.console.layout.pattern = %msg%n

logger.app.name = com.azure.core
logger.app.level = ERROR

rootLogger.level = info
rootLogger.appenderRefs = stdout
rootLogger.appenderRef.stdout.ref = STDOUT
```

### <a name="using-an-xml-file"></a>Uso di un file XML

È possibile inserire un file XML denominato *log4j2.xml* nella directory */src/main/Resource* del progetto. Il file deve avere il formato seguente:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Configuration status="INFO">
    <Appenders>
        <Console name="console" target="SYSTEM_OUT">
            <PatternLayout pattern="%msg%n" />
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

## <a name="next-steps"></a>Passaggi successivi

Questo articolo ha illustrato la configurazione di Log4j e come fare in modo che Azure SDK per Java lo usi per la registrazione. Poiché Azure SDK per Java funziona con tutti i Framework di registrazione SLF4J, vedere la pagina relativa al [manuale dell'utente di SLF4J](http://www.slf4j.org/manual.html) per altri dettagli. Se si usa log4j, è disponibile anche una grande quantità di indicazioni sulla configurazione nel sito Web. Per altre informazioni, vedere [Welcome to log4j 2!](https://logging.apache.org/log4j/2.x/manual/index.html)

Una volta completata la registrazione, provare a esaminare le integrazioni offerte da Azure in Framework come [Spring](/azure/developer/java/spring-framework/spring-boot-starters-for-azure) e [microprofile](/azure/developer/java/eclipse-microprofile/).
