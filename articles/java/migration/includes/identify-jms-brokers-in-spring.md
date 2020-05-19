---
author: yevster
ms.author: yebronsh
ms.date: 2/12/2020
ms.openlocfilehash: d17b69ea5299c24406a63fc6239c350b6cbcbdd4
ms.sourcegitcommit: 226ebca0d0e3b918928f58a3a7127be49e4aca87
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/08/2020
ms.locfileid: "82990143"
---
#### <a name="jms-message-brokers"></a>Broker di messaggi JMS

Identificare i broker in uso cercando nel manifesto di compilazione, in genere, un file *pom.xml* o *build.gradle*, per le dipendenze pertinenti.

Ad esempio, un'applicazione Spring Boot che usa ActiveMQ in genere contiene questa dipendenza nel relativo file *pom.xml*:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-activemq</artifactId>
</dependency>
```

Le applicazioni Spring Boot che usano broker proprietari contengono in genere le dipendenze direttamente nelle librerie di driver JMS dei broker. Ecco un esempio di un file *build.gradle*:

```json
    dependencies {
      ...
      compile("com.ibm.mq:com.ibm.mq.allclient:9.0.4.0")
      ...
    }
```
