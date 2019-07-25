---
title: Usare immagini Docker con un JDK per lo sviluppo Java in Azure
description: ''
author: bmitchell287
manager: douge
ms.author: brendm
ms.date: 04/09/2019
ms.devlang: java
ms.topic: conceptual
ms.service: Azure
ms.openlocfilehash: 5de2866f1f303deccf40158ac7451dbd35de92f1
ms.sourcegitcommit: 4cc7f5e1e4601065bfcb4c2eeb7d47ad0bec61f8
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/24/2019
ms.locfileid: "68430954"
---
# <a name="use-docker-with-a-jdk-for-azure"></a>Usare Docker con JDK per Azure 

Le immagini Docker predefinite per Java 7, 8 e 11 sono disponibili tramite [Docker Hub](https://hub.docker.com/_/microsoft-java-se).

Le immagini di contenitori Docker certificate per Zulu JDK, JRE e JRE headless con più immagini del sistema operativo di base sono disponibili in Docker Hub:

* [JDK](https://hub.docker.com/_/microsoft-java-jdk)
* [JRE](https://hub.docker.com/_/microsoft-java-jre)
* [JRE headless](https://hub.docker.com/_/microsoft-java-jre-headless)

## <a name="running-a-docker-image"></a>Esecuzione di un'immagine Docker

Le immagini Docker possono essere eseguite con la sintassi `$ docker run mcr.microsoft.com/java/jdk:tag java`, come illustrato nell'esempio seguente.

```cli
docker run mcr.microsoft.com/java/jdk:8u212-zulu-alpine java -version 
```

## <a name="creating-a-docker-image"></a>Creazione di un'immagine Docker

È possibile creare un'immagine usando le immagini di Docker Hub ufficiali di Microsoft, come illustrato negli esempi seguenti.

### <a name="create-a-docker-file"></a>Creare un file Docker

```cli
FROM mcr.microsoft.com/java/jdk:8u212-zulu-alpine 
  
RUN echo $' \
  
public class HelloWorld { \
   public static void main(String[] args) { \
      // Prints "Hello, World" in the terminal window. \
      System.out.println("Hello, World - From Microsoft Azure !!!"); \
   } \
}' > HelloWorld.java
  
RUN javac HelloWorld.java
  
CMD ["java", "HelloWorld"]
```

### <a name="build-a-docker-image"></a>Creare un'immagine Docker

```cli
docker build -t hello-world
```

### <a name="run-the-new-image"></a>Eseguire la nuova immagine

```cli
docker run hello-world
```

Viene visualizzato l'output seguente:

```output
Hello World - From Microsoft Azure !!!
```
