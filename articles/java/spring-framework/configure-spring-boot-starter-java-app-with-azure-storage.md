---
title: Come usare l'utilità di avvio Spring Boot per Archiviazione di Azure
description: Informazioni su come configurare un'app Spring Boot Initializer con l'utilità di avvio Archiviazione di Azure.
services: storage
documentationcenter: java
ms.date: 10/14/2020
ms.service: storage
ms.topic: article
ms.workload: storage
ms.custom: devx-track-java, devx-track-azurecli
ms.openlocfilehash: a459f9eba2661cefddf5c90ae4764fade415ac4d
ms.sourcegitcommit: e1175aa94709b14b283645986a34a385999fb3f7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/03/2020
ms.locfileid: "93192433"
---
# <a name="how-to-use-the-spring-boot-starter-for-azure-storage"></a>Come usare l'utilità di avvio Spring Boot per Archiviazione di Azure

Questo articolo illustra la creazione di un'applicazione personalizzata con **Spring Initializr** , quindi l'aggiunta dell'utilità di avvio per Archiviazione di Azure e infine l'uso dell'applicazione per caricare un BLOB nell'account di archiviazione di Azure.

## <a name="prerequisites"></a>Prerequisiti

I prerequisiti seguenti sono necessari per seguire le procedure disponibili in questo articolo:

* Sottoscrizione di Azure; se non si ha una sottoscrizione di Azure, è possibile attivare i [vantaggi per i sottoscrittori di MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) oppure iscriversi per ottenere un [account Azure gratuito](https://azure.microsoft.com/pricing/free-trial/).
* [Interfaccia della riga di comando di Azure](/cli/azure/index).
* Java Development Kit (JDK) supportato. Per altre informazioni sulle versioni di JDK utilizzabili per lo sviluppo in Azure, vedere <https://aka.ms/azure-jdks>.
* [Apache Maven](http://maven.apache.org/), versione 3.0 o versione successiva.

> [!IMPORTANT]
>
> Per completare i passaggi descritti in questo articolo è necessario Spring Boot versione 2.0 o successiva.
>

## <a name="create-an-azure-storage-account-and-blob-container-for-your-application"></a>Creare un account di archiviazione e un contenitore BLOB di Azure per l'applicazione

La procedura seguente crea un account di archiviazione e un contenitore di Azure nel portale.

1. Passare al portale di Azure all'indirizzo <https://portal.azure.com/> ed eseguire l'accesso.

1. Selezionare **Crea una risorsa** , quindi **Attività iniziali** e quindi **Account di archiviazione**.

   ![Creare un account di archiviazione di Azure][IMG01]

1. Nella pagina **Crea account di archiviazione** immettere le informazioni seguenti:

   * Selezionare **Sottoscrizione**.
   * Selezionare un **gruppo di risorse** oppure crearne uno nuovo.
   * Immettere un nome univoco per **Nome dell'account di archiviazione** , che diventerà parte dell'URI dell'account di archiviazione. Se si immette **wingtiptoysstorage** in **Nome** , ad esempio, l'URI sarà *wingtiptoysstorage.core.windows.net*.
   * Specificare la **località** per l'account di archiviazione.
1. Dopo aver specificato le opzioni elencate sopra, selezionare **Rivedi e crea**. 
1. Verificare le specifiche, quindi selezionare **Crea** per creare l'account di archiviazione.
1. Una volta completata la distribuzione, selezionare **Vai alla risorsa**.
1. Selezionare **Contenitori**.
1. Selezionare **Contenitore**.
   * Assegnare un nome al contenitore.
   * Selezionare *BLOB* nell'elenco a discesa.

   ![Creare un contenitore BLOB][IMG02]

1. Al termine della creazione, il contenitore BLOB sarà incluso nell'elenco nel portale di Azure.

È anche possibile usare l'interfaccia della riga di comando di Azure per creare un account e un contenitore di Archiviazione di Azure seguendo questa procedura. È necessario ricordare di sostituire i valori segnaposto tra parentesi acute con i valori personalizzati.

1. Aprire un prompt dei comandi.
1. Accedere all'account Azure:

   ```azurecli
   az login
   ```
   
1. Se non è disponibile, creare un gruppo di risorse usando il comando seguente:
   
   ```azurecli
   az group create \
      --name <resource-group> \
      --location <location>
   ```
   
1. Creare un account di archiviazione usando il comando seguente:
  
   ```azurecli
    az storage account create \
      --name <storage-account> \
      --resource-group <resource-group> \
      --location <location> 
   ```

1. Per creare un contenitore, usare il comando seguente:
   
   ```azurecli
    az storage container create \
      --account-name <storage-account-name> \
      --name <container-name> \
      --auth-mode login
   ```
## <a name="create-a-simple-spring-boot-application-with-the-spring-initializr"></a>Creare un'applicazione Spring Boot semplice con Spring Initializr

La procedura seguente crea l'applicazione Spring Boot.

1. Passare a <https://start.spring.io/>.

1. Specificare le opzioni seguenti:

   * Generare un progetto **Maven**.
   * Specificare **Java 8**.
   * Specificare **Spring Boot** versione 2.3 o successiva.
   * Specificare i nomi di **Group** (Gruppo) e **Artifact** (Artefatto) per l'applicazione.
   * Aggiungere la dipendenza **Spring Web**.

      ![Opzioni di base di Spring Initializr][SI01]

   > [!NOTE]
   > 1. Spring Initializr usa i nomi in **Group** (Gruppo) e **Artifact** (Artefatto) per creare il nome del pacchetto, ad esempio *com.wingtiptoys.storage*.
   > 2. Spring Initializr usa Java 11 come versione predefinita. Per usare le utilità di avvio di Spring Boot descritte in questo argomento, è necessario selezionare invece Java 8.

1. Dopo aver specificato le opzioni elencate sopra, selezionare **GENERA**.

1. Quando richiesto, scaricare il progetto in un percorso nel computer locale.

1. Dopo aver estratto i file nel sistema locale, la semplice applicazione Spring Boot sarà pronta per la modifica.

## <a name="configure-your-spring-boot-app-to-use-the-azure-storage-starter"></a>Configurare l'app Spring Boot per l'uso dell'utilità di avvio per Archiviazione di Azure

La procedura seguente configura l'applicazione Spring Boot per l'uso dell'archiviazione di Azure.

1. Individuare il file *pom.xml* nella directory radice dell'app, ad esempio:

   `C:\SpringBoot\storage\pom.xml`

   -oppure-

   `/users/example/home/storage/pom.xml`

1. Aprire il file *pom.xml* in un editor di testo e aggiungere l'utilità di avvio Spring Cloud per Archiviazione di Azure all'elenco in `<dependencies>`:

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>spring-starter-azure-storage</artifactId>
      <version>1.2.8</version>
   </dependency>
   ```

1. Se si usa JDK versione 9 o successiva, aggiungere le dipendenze seguenti:

   ```xml
   <dependency>
       <groupId>javax.xml.bind</groupId>
       <artifactId>jaxb-api</artifactId>
       <version>2.3.1</version>
   </dependency>
   <dependency>
       <groupId>org.glassfish.jaxb</groupId>
       <artifactId>jaxb-runtime</artifactId>
       <version>2.3.1</version>
       <scope>runtime</scope>
   </dependency>
   ```

1. Salvare e chiudere il file *pom.xml*.

## <a name="create-an-azure-credential-file"></a>Creare un file di credenziali di Azure

La procedura seguente crea il file delle credenziali di Azure.

1. Aprire un prompt dei comandi.

1. Passare alla directory *resources* dell'app Spring Boot, ad esempio:

   ```cmd
   cd C:\SpringBoot\storage\src\main\resources
   ```

   -oppure-

   ```bash
   cd /users/example/home/storage/src/main/resources
   ```

1. Accedere all'account Azure:

   ```azurecli
   az login
   ```

1. Elencare le sottoscrizioni:

   ```azurecli
   az account list
   ```
   Azure restituirà un elenco delle sottoscrizioni e sarà necessario copiare il GUID per la sottoscrizione che si vuole usare, ad esempio:

   ```json
   [
     {
       "cloudName": "AzureCloud",
       "id": "11111111-1111-1111-1111-111111111111",
       "isDefault": true,
       "name": "Converted Windows Azure MSDN - Visual Studio Ultimate",
       "state": "Enabled",
       "tenantId": "22222222-2222-2222-2222-222222222222",
       "user": {
         "name": "gena.soto@wingtiptoys.com",
         "type": "user"
       }
     }
   ]
   ```

1. Specificare il GUID per la sottoscrizione che si vuole usare con Azure, ad esempio:

   ```azurecli
   az account set -s 11111111-1111-1111-1111-111111111111
   ```

1. Creare il file di credenziali di Azure:

   ```azurecli
   az ad sp create-for-rbac --sdk-auth > my.azureauth
   ```

   Questo comando creerà un file *my.azureauth* nella directory *resources* con contenuto simile all'esempio seguente:

   ```json
   {
     "clientId": "33333333-3333-3333-3333-333333333333",
     "clientSecret": "44444444-4444-4444-4444-444444444444",
     "subscriptionId": "11111111-1111-1111-1111-111111111111",
     "tenantId": "22222222-2222-2222-2222-222222222222",
     "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
     "resourceManagerEndpointUrl": "https://management.azure.com/",
     "activeDirectoryGraphResourceId": "https://graph.windows.net/",
     "sqlManagementEndpointUrl": "https://management.core.windows.net:8443/",
     "galleryEndpointUrl": "https://gallery.azure.com/",
     "managementEndpointUrl": "https://management.core.windows.net/"
   }
   ```

## <a name="configure-your-spring-boot-app-to-use-your-azure-storage-account"></a>Configurare l'app Spring Boot per l'uso dell'account di archiviazione di Azure

La procedura seguente configura l'applicazione Spring Boot per l'uso dell'account di archiviazione di Azure.

1. Individuare *application.properties* nella directory *resources* dell'app, ad esempio:

   `C:\SpringBoot\storage\src\main\resources\application.properties`

   -oppure-

   `/users/example/home/storage/src/main/resources/application.properties`

2. Aprire il file *application.properties* in un editor di testo, aggiungere le righe seguenti e quindi sostituire i valori di esempio con le proprietà appropriate per l'account di archiviazione:

   ```yaml
   spring.cloud.azure.credential-file-path=my.azureauth
   spring.cloud.azure.resource-group=wingtiptoysresources
   spring.cloud.azure.region=westUS
   spring.cloud.azure.storage.account=wingtiptoysstorage
   blob=azure-blob://containerName/blobName
   ```
   Dove:

   |                   Campo                   |                                            Descrizione                                            |
   |-------------------------------------------|---------------------------------------------------------------------------------------------------|
   | `spring.cloud.azure.credential-file-path` |            Specifica il file di credenziali di Azure creato in precedenza in questa esercitazione.             |
   |    `spring.cloud.azure.resource-group`    |           Specifica il gruppo di risorse di Azure contenente l'account di archiviazione di Azure.            |
   |        `spring.cloud.azure.region`        | Specifica l'area geografica indicata al momento della creazione dell'account di archiviazione di Azure. |
   |   `spring.cloud.azure.storage.account`    |            Specifica l'account di archiviazione di Azure creato in precedenza in questa esercitazione.             |
   |                   `blob`                  |           Specifica i nomi del contenitore e del BLOB in cui archiviare i dati.         |
    
3. Salvare e chiudere il file *application.properties*.

## <a name="add-sample-code-to-implement-basic-azure-storage-functionality"></a>Aggiungere codice di esempio per implementare le funzionalità di archiviazione di Azure di base

In questa sezione si creano le classi Java necessarie per archiviare un BLOB nell'account di archiviazione di Azure.

### <a name="modify-the-main-application-class"></a>Modificare la classe dell'applicazione main

1. Individuare il file Java dell'applicazione main nella directory del pacchetto dell'app, ad esempio:

   `C:\SpringBoot\storage\src\main\java\com\wingtiptoys\storage\StorageApplication.java`

   -oppure-

   `/users/example/home/storage/src/main/java/com/wingtiptoys/storage/StorageApplication.java`

1. Aprire il file Java dell'applicazione main in un editor di testo e aggiungervi le righe seguenti: Sostituire wingtiptoys con i propri valori:

   ```java
   package com.wingtiptoys.storage;

   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;

   @SpringBootApplication
   public class StorageApplication {
      public static void main(String[] args) {
         SpringApplication.run(StorageApplication.class, args);
      }
   }
   ```

1. Salvare e chiudere il file Java dell'applicazione main.

### <a name="add-a-blob-controller-class"></a>Aggiungere una classe controller BLOB

1. Creare un nuovo file Java denominato *BLObController.java* nella directory del pacchetto dell'app, ad esempio:

   `C:\SpringBoot\storage\src\main\java\com\wingtiptoys\storage\BlobController.java`

   -oppure-

   `/users/example/home/storage/src/main/java/com/wingtiptoys/storage/BlobController.java`

1. Aprire il file Java del controller BLOB in un editor di testo e aggiungervi le righe seguenti.  Sostituire *wingtiptoys* con il proprio gruppo di risorse e *storage* con il nome dell'artefatto.

   ```java
   package com.wingtiptoys.storage;

   import org.springframework.beans.factory.annotation.Value;
   import org.springframework.core.io.Resource;
   import org.springframework.core.io.WritableResource;
   import org.springframework.util.StreamUtils;
   import org.springframework.web.bind.annotation.*;

   import java.io.IOException;
   import java.io.OutputStream;
   import java.nio.charset.Charset;

   @RestController
   @RequestMapping("blob")
   public class BlobController {
   
       @Value("${blob}")
       private Resource blobFile;
   
       @GetMapping
       public String readBlobFile() throws IOException {
           return StreamUtils.copyToString(
                   this.blobFile.getInputStream(),
                   Charset.defaultCharset());
       }
   
       @PostMapping
       public String writeBlobFile(@RequestBody String data) throws IOException {
           try (OutputStream os = ((WritableResource) this.blobFile).getOutputStream()) {
               os.write(data.getBytes());
           }
           return "file was updated";
       }
   }
   ```

1. Salvare e chiudere il file Java del controller BLOB.

1. Aprire un prompt dei comandi e cambiare la directory passando alla cartella in cui si trova il file *pom.xml* , ad esempio:

   ```cmd
   cd C:\SpringBoot\storage
   ```

   -oppure-
   
   ```bash
   cd /users/example/home/storage
   ```

1. Compilare l'applicazione Spring Boot con Maven ed eseguirla, ad esempio:

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

1. Quando l'applicazione è in esecuzione, è possibile testarla usando *curl* , ad esempio:

   a. Inviare una richiesta POST per aggiornare il contenuto di un file:

      ```shell
      curl -d 'new message' -H 'Content-Type: text/plain' localhost:8080/blob
      ```

      Verrà visualizzata una risposta che indica che il file è stato aggiornato.

   b. Inviare una richiesta GET per verificare il contenuto del file:

      ```shell
      curl -X GET http://localhost:8080/
      ```

     Verrà visualizzato il testo "Hello World" che è stato inserito.

## <a name="summary"></a>Summary

In questa esercitazione si è creata una nuova applicazione Java con **[Spring Initializr]** , si è aggiunta l'utilità di avvio per Archiviazione di Azure all'applicazione e quindi si è configurata l'applicazione per il caricamento di un BLOB nell'account di archiviazione di Azure.


## <a name="clean-up-resources"></a>Pulizia delle risorse

Quando le risorse create in questo articolo non sono più necessarie, usare il [portale di Azure](https://portal.azure.com/) per eliminarle in modo da evitare addebiti imprevisti.

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni su Spring e Azure, passare al centro di documentazione di Spring in Azure.

> [!div class="nextstepaction"]
> [Spring in Azure](index.yml)

### <a name="additional-resources"></a>Risorse aggiuntive

Per altre informazioni sulle utilità di avvio Spring Boot aggiuntive disponibili per Microsoft Azure, vedere [Spring Boot Starters for Azure (Utilità di avvio Spring Boot per Azure)](spring-boot-starters-for-azure.md).

Per informazioni dettagliate sulle altre API di archiviazione di Azure che è possibile chiamare dalle applicazioni Spring Boot, vedere gli articoli seguenti:
* [Come usare l'archivio BLOB di Azure da Java](/azure/storage/blobs/storage-java-how-to-use-blob-storage)
* [Come usare l'archivio code di Azure da Java](/azure/storage/queues/storage-java-how-to-use-queue-storage)
* [Come usare l'archivio tabelle di Azure da Java](/azure/cosmos-db/table-storage-how-to-use-java)
* [Come usare Archiviazione file di Azure da Java](/azure/storage/files/storage-java-how-to-use-file-storage)

<!-- IMG List -->

[IMG01]: media/configure-spring-boot-starter-java-app-with-azure-storage/create-storage-account-01.png
[IMG02]: media/configure-spring-boot-starter-java-app-with-azure-storage/create-storage-account-02.png

[SI01]: media/configure-spring-boot-starter-java-app-with-azure-storage/create-project-01.png
