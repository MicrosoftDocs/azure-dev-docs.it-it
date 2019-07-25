---
title: Utilità di avvio Spring Boot per Azure
description: Questo articolo descrive le diverse utilità di avvio di Spring Boot disponibili per Azure.
services: ''
documentationcenter: java
author: bmitchell287
manager: douge
editor: ''
ms.assetid: ''
ms.author: brendm
ms.date: 12/19/2018
ms.devlang: java
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: na
ms.openlocfilehash: 2e9d2ecd22d6f276629cb8b0e519959837346693
ms.sourcegitcommit: 2efdb9d8a8f8a2c1914bd545a8c22ae6fe0f463b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/15/2019
ms.locfileid: "68285312"
---
# <a name="spring-boot-starters-for-azure"></a>Utilità di avvio Spring Boot per Azure

Questo articolo descrive le diverse utilità di avvio Spring Boot per [Spring Initializr] che offrono agli sviluppatori Java l'integrazione di funzionalità per l'uso di Microsoft Azure.

![Utilità di avvio Spring Boot per Azure][spring-boot-starters]

Sono attualmente disponibili le utilità di avvio Spring Boot seguenti per Azure:

* **[Supporto di Azure](#azure-support)**

   Fornisce il supporto per la configurazione automatica di servizi di Azure, ad esempio bus di servizio, archiviazione, Active Directory e così via.

* **[Azure Active Directory](#azure-active-directory)**

   Fornisce il supporto di integrazione per Spring Security con Azure Active Directory per l'autenticazione.

* **[Azure Key Vault](#azure-key-vault)**

   Fornisce il supporto dell'annotazione di valori Spring per l'integrazione con i segreti di Azure Key Vault.

* **[Archiviazione di Azure](#azure-storage)**

   Fornisce il supporto di Spring Boot per i servizi di archiviazione di Azure.

<a name="azure-support"></a>
## <a name="azure-support"></a>Supporto di Azure

Questa utilità di avvio Spring Boot offre il supporto della configurazione automatica per i servizi di Azure, ad esempio per il bus di servizio, Archiviazione, Active Directory, Cosmos DB, Key Vault e così via.

Per esempi dell'uso delle diverse funzionalità di Azure fornite con questa utilità di avvio, vedere:

* <https://github.com/Microsoft/azure-spring-boot/tree/master/azure-spring-boot-samples>

Quando si aggiunge questa utilità di avvio a un progetto Spring Boot, vengono apportate le modifiche seguenti al file *pom.xml*:

* Viene aggiunta la proprietà seguente all'elemento `<properties>`:

   ```xml
   <properties>
      <!-- Other properties will be listed here -->
      <azure.version>0.2.0</azure.version>
   </properties>
   ```

* La dipendenza `spring-boot-starter` predefinita viene sostituita con la dipendenza seguente:

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-spring-boot</artifactId>
   </dependency>
   ```

* Viene aggiunta la sezione seguente al file:

   ```xml
   <dependencyManagement>
      <dependencies>
         <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>azure-spring-boot-bom</artifactId>
            <version>${azure.version}</version>
            <type>pom</type>
            <scope>import</scope>
         </dependency>
      </dependencies>
   </dependencyManagement>
   ```

<a name="azure-active-directory"></a>
## <a name="azure-active-directory"></a>Azure Active Directory

Questa utilità di avvio Spring Boot fornisce il supporto per la configurazione automatica di Spring Security per l'integrazione con Azure Active Directory per l'autenticazione.

Per esempi dell'uso delle funzionalità di Azure Active Directory fornite con questa utilità di avvio, vedere:

* <https://github.com/Microsoft/azure-spring-boot/tree/master/azure-spring-boot-samples/azure-active-directory-spring-boot-sample>

Quando si aggiunge questa utilità di avvio a un progetto Spring Boot, vengono apportate le modifiche seguenti al file *pom.xml*:

* Viene aggiunta la proprietà seguente all'elemento `<properties>`:

   ```xml
   <properties>
      <!-- Other properties will be listed here -->
      <azure.version>0.2.0</azure.version>
   </properties>
   ```

* La dipendenza `spring-boot-starter` predefinita viene sostituita con la dipendenza seguente:

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-active-directory-spring-boot-starter</artifactId>
   </dependency>
   ```

* Viene aggiunta la sezione seguente al file:

   ```xml
   <dependencyManagement>
      <dependencies>
         <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>azure-spring-boot-bom</artifactId>
            <version>${azure.version}</version>
            <type>pom</type>
            <scope>import</scope>
         </dependency>
      </dependencies>
   </dependencyManagement>
   ```

<a name="azure-key-vault"></a>
## <a name="azure-key-vault"></a>Azure Key Vault

Questa utilità di avvio Spring Boot fornisce il supporto dell'annotazione di valori Spring per l'integrazione con i segreti di Azure Key Vault.

Per esempi dell'uso delle funzionalità di Azure Key Vault fornite con questa utilità di avvio, vedere:

* <https://github.com/Microsoft/azure-spring-boot/tree/master/azure-spring-boot-samples/azure-keyvault-secrets-spring-boot-sample>

Quando si aggiunge questa utilità di avvio a un progetto Spring Boot, vengono apportate le modifiche seguenti al file *pom.xml*:

* Viene aggiunta la proprietà seguente all'elemento `<properties>`:

   ```xml
   <properties>
      <!-- Other properties will be listed here -->
      <azure.version>0.2.0</azure.version>
   </properties>
   ```

* La dipendenza `spring-boot-starter` predefinita viene sostituita con la dipendenza seguente:

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-keyvault-secrets-spring-boot-starter</artifactId>
   </dependency>
   ```

* Viene aggiunta la sezione seguente al file:

   ```xml
   <dependencyManagement>
      <dependencies>
         <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>azure-spring-boot-bom</artifactId>
            <version>${azure.version}</version>
            <type>pom</type>
            <scope>import</scope>
         </dependency>
      </dependencies>
   </dependencyManagement>
   ```

<a name="azure-storage"></a>
## <a name="azure-storage"></a>Archiviazione di Azure

Questa utilità di avvio Spring Boot fornisce il supporto di integrazione di Spring Boot per i servizi di archiviazione di Azure.

Per esempi dell'uso delle funzionalità di Archiviazione di Azure fornite con questa utilità di avvio, vedere:

* [Come usare l'utilità di avvio Spring Boot per Archiviazione di Azure](configure-spring-boot-starter-java-app-with-azure-storage.md)

* <https://github.com/Microsoft/azure-spring-boot/tree/master/azure-spring-boot-samples/azure-storage-spring-boot-sample>

Quando si aggiunge questa utilità di avvio a un progetto Spring Boot, vengono apportate le modifiche seguenti al file *pom.xml*:

* Viene aggiunta la proprietà seguente all'elemento `<properties>`:

   ```xml
   <properties>
      <!-- Other properties will be listed here -->
      <azure.version>0.2.0</azure.version>
   </properties>
   ```

* La dipendenza `spring-boot-starter` predefinita viene sostituita con la dipendenza seguente:

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-storage-spring-boot-starter</artifactId>
   </dependency>
   ```

* Viene aggiunta la sezione seguente al file:

   ```xml
   <dependencyManagement>
      <dependencies>
         <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>azure-spring-boot-bom</artifactId>
            <version>${azure.version}</version>
            <type>pom</type>
            <scope>import</scope>
         </dependency>
      </dependencies>
   </dependencyManagement>
   ```

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni su Spring e Azure, passare al centro di documentazione di Spring in Azure.

> [!div class="nextstepaction"]
> [Spring in Azure](/azure/java/spring-framework)

### <a name="additional-resources"></a>Risorse aggiuntive

Per altre informazioni sull'uso delle applicazioni [Spring Boot] in Azure, vedere [Spring in Azure].

Per altre informazioni sull'uso di Azure con Java, vedere [Azure per sviluppatori Java] e la documentazione relativa all'[uso di Azure DevOps e Java].

Per iniziare a usare proprie applicazioni Spring Boot, vedere **Spring Initializr** all'indirizzo https://start.spring.io/.

<!-- URL List -->

[Azure per sviluppatori Java]: /azure/java/
[Uso di Azure DevOps e Java]: /azure/devops/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring in Azure]: /azure/java/spring-framework/
[Spring Framework]: https://spring.io/
[Spring Initializr]: https://start.spring.io/

<!-- IMG List -->

[spring-boot-starters]: media/spring-boot-starters-for-azure/spring-boot-starters-cropped.png
