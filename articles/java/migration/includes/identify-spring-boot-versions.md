---
author: yevster
ms.author: yebronsh
ms.date: 4/15/2020
ms.openlocfilehash: 34701a7d3fd85fea412be859dc21823df2dde053
ms.sourcegitcommit: 226ebca0d0e3b918928f58a3a7127be49e4aca87
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/08/2020
ms.locfileid: "82990193"
---
#### <a name="identify-spring-boot-versions"></a>Identificare le versioni di Spring Boot

Esaminare le dipendenze di ogni applicazione di cui viene eseguita la migrazione per determinare la versione di Spring Boot.

##### <a name="maven"></a>Maven

Nei progetti Maven la versione di Spring Boot è in genere disponibile nell'elemento `<parent>` del file POM:

```xml
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.2.6.RELEASE</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
```

##### <a name="gradle"></a>Gradle

Nei progetti Gradle la versione di Spring Boot è in genere disponibile nella sezione `plugins`, come versione del plug-in `org.springframework.boot`:

```gradle
plugins {
  id 'org.springframework.boot' version '2.2.6.RELEASE'
  id 'io.spring.dependency-management' version '1.0.9.RELEASE'
  id 'java'
}
```
