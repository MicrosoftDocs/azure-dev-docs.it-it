---
title: Utilità di avvio Spring Boot per Azure
description: Questo articolo descrive le diverse utilità di avvio di Spring Boot disponibili per Azure.
documentationcenter: java
ms.date: 09/29/2020
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.custom: devx-track-java
ms.openlocfilehash: 89df7fe719cd8c8301c729bb33cb1d3632a80b0d
ms.sourcegitcommit: 09b4a2dbe13601fdf16fcc4082a5075b46ad3459
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/03/2020
ms.locfileid: "96559270"
---
# <a name="spring-boot-starters-for-azure"></a>Utilità di avvio Spring Boot per Azure

Questo articolo descrive le diverse utilità di avvio Spring Boot per [Spring Initializr] che offrono agli sviluppatori Java l'integrazione di funzionalità per l'uso di Microsoft Azure.

>[!div class="mx-imgBorder"]
![Configurare le utilità di avvio di Azure Spring Boot con Initializr][configure-azure-spring-boot-starters-with-initializr]


Sono attualmente disponibili le utilità di avvio Spring Boot seguenti per Azure:

* **[Supporto di Azure](#azure-support)**

   Fornisce il supporto per la configurazione automatica di servizi di Azure, ad esempio bus di servizio, archiviazione, Active Directory e così via.

* **[Azure Active Directory](#azure-active-directory)**

   Fornisce il supporto di integrazione per Spring Security con Azure Active Directory per l'autenticazione.

* **[Azure Key Vault](#azure-key-vault)**

   Fornisce il supporto dell'annotazione di valori Spring per l'integrazione con i segreti di Azure Key Vault.

* **[Archiviazione di Azure](#azure-storage)**

   Fornisce il supporto di Spring Boot per i servizi di archiviazione di Azure.
   
   > [!NOTE]
   >
   > La nuova versione dell'utilità di avvio di Spring Boot per Archiviazione di Azure non supporta attualmente l'aggiunta di una dipendenza di archiviazione di Azure da Spring Initializr. È tuttavia possibile aggiungere la dipendenza modificando il file *pom.xml* dopo la generazione del progetto.
   > 

<a name="azure-support"></a>
## <a name="azure-support"></a>Supporto di Azure

Questa utilità di avvio Spring Boot fornisce il supporto per la configurazione automatica di servizi di Azure, ad esempio bus di servizio, archiviazione, Active Directory, Cosmos DB, Key Vault e così via.

Per esempi dell'uso delle diverse funzionalità di Azure fornite con questa utilità di avvio, vedere:

* <https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/spring/azure-spring-boot-samples>

Quando si aggiunge questa utilità di avvio a un progetto Spring Boot, vengono apportate le modifiche seguenti al file *pom.xml*:

* Viene aggiunta la proprietà seguente all'elemento `<properties>`:

   ```xml
   <properties>
      <!-- Other properties will be listed here -->
      <java.version>1.8</java.version>
      <azure.version>2.3.5</azure.version>
   </properties>
   ```

* La dipendenza `spring-boot-starter` predefinita viene sostituita con la dipendenza seguente:

    ```xml
    <dependencies>
        <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>azure-spring-boot-starter</artifactId>
        </dependency>
    
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
            <exclusions>
                <exclusion>
                    <groupId>org.junit.vintage</groupId>
                    <artifactId>junit-vintage-engine</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
    </dependencies>
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

* <https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/spring/azure-spring-boot-samples/>

Quando si aggiunge questa utilità di avvio a un progetto Spring Boot, vengono apportate le modifiche seguenti al file *pom.xml*:

* Viene aggiunta la proprietà seguente all'elemento `<properties>`:

   ```xml
   <properties>
      <!-- Other properties will be listed here -->
      <java.version>1.8</java.version>
      <azure.version>2.3.5</azure.version>
   </properties>
   ```

* La dipendenza `spring-boot-starter` predefinita viene sostituita con la dipendenza seguente:

    ```xml
    <dependencies>
        <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>azure-active-directory-spring-boot-starter</artifactId>
        </dependency>
    
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
            <exclusions>
                <exclusion>
                    <groupId>org.junit.vintage</groupId>
                    <artifactId>junit-vintage-engine</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
    </dependencies>
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
## <a name="azure-key-vault"></a>Insieme di credenziali chiave di Azure

Questa utilità di avvio Spring Boot fornisce il supporto dell'annotazione di valori Spring per l'integrazione con i segreti di Azure Key Vault.

Per esempi dell'uso delle funzionalità di Azure Key Vault fornite con questa utilità di avvio, vedere:

* <https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/spring/azure-spring-boot-samples/azure-spring-boot-sample-keyvault-secrets>

Quando si aggiunge questa utilità di avvio a un progetto Spring Boot, vengono apportate le modifiche seguenti al file *pom.xml*:

* Viene aggiunta la proprietà seguente all'elemento `<properties>`:

   ```xml
   <properties>
      <!-- Other properties will be listed here -->
      <java.version>1.8</java.version>
      <azure.version>2.3.5</azure.version>
   </properties>
   ```

* La dipendenza `spring-boot-starter` predefinita viene sostituita con la dipendenza seguente:

    ```xml
    <dependencies>
        <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>azure-keyvault-secrets-spring-boot-starter</artifactId>
        </dependency>
    
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
            <exclusions>
                <exclusion>
                    <groupId>org.junit.vintage</groupId>
                    <artifactId>junit-vintage-engine</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
    </dependencies>
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
* <https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/spring/azure-spring-boot-samples/azure-spring-cloud-sample-storage-queue-operation>

Quando si aggiunge questa utilità di avvio a un progetto Spring Boot, vengono apportate le modifiche seguenti al file *pom.xml*:

* Viene aggiunta la proprietà seguente all'elemento `<properties>`:

   ```xml
   <properties>
      <!-- Other properties will be listed here -->
      <java.version>1.8</java.version>
      <azure.version>2.3.5</azure.version>
   </properties>
   ```

* La dipendenza `spring-boot-starter` predefinita viene sostituita con la dipendenza seguente:

    ```xml
    <dependencies>
        <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>spring-starter-azure-storage</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
            <exclusions>
                <exclusion>
                    <groupId>org.junit.vintage</groupId>
                    <artifactId>junit-vintage-engine</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
    </dependencies>
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
> [Spring in Azure](./index.yml)

### <a name="additional-resources"></a>Risorse aggiuntive

Per altre informazioni sull'uso delle applicazioni [Spring Boot] in Azure, vedere [Spring in Azure].

Per altre informazioni sull'uso di Azure con Java, vedere [Azure per sviluppatori Java] e la documentazione relativa all'[uso di Azure DevOps e Java].

Per iniziare a usare proprie applicazioni Spring Boot, vedere **Spring Initializr** all'indirizzo https://start.spring.io/.

<!-- URL List -->

[Azure per sviluppatori Java]: ../index.yml
[Uso di Azure DevOps e Java]: /azure/devops/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring in Azure]: ./index.yml
[Spring Framework]: https://spring.io/
[Spring Initializr]: https://start.spring.io/

<!-- IMG List -->

[configure-azure-spring-boot-starters-with-initializr]: media/spring-boot-starters-for-azure/configure-azure-spring-boot-starters-with-initializr.png
