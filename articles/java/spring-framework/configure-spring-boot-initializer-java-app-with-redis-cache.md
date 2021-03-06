---
title: Creare un'app Spring Boot Initializer - Cache di Azure per Redis
description: Configurare un'applicazione Spring Boot creata con Spring Initializr per l'uso di Redis sul cloud con la cache Redis di Azure.
services: redis-cache
documentationcenter: java
ms.date: 10/13/2020
ms.service: cache
ms.tgt_pltfrm: cache-redis
ms.topic: conceptual
ms.custom: devx-track-java
ms.openlocfilehash: 7d8ee875339adb741fbddeba6d4328eb22cd3e46
ms.sourcegitcommit: 5c7f5fef798413b1a304cc9ee31c8518b73f27eb
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/30/2020
ms.locfileid: "93066275"
---
# <a name="configure-a-spring-boot-initializer-app-to-use-redis-in-the-cloud-with-azure-redis-cache"></a>Configurare un'app Spring Boot Initializer per l'uso di Redis sul cloud con la cache Redis di Azure

Questo articolo illustra in modo dettagliato come creare una cache Redis sul cloud usando il portale di Azure, quindi usare **[Spring Initializr]** per creare un'applicazione personalizzata e infine creare un'applicazione Web Java che archivia e recupera i dati tramite la cache Redis.

## <a name="prerequisites"></a>Prerequisites

I prerequisiti seguenti sono necessari per completare le procedure disponibili in questo articolo:

* Sottoscrizione di Azure; se non si ha una sottoscrizione di Azure, è possibile attivare i [vantaggi per i sottoscrittori di MSDN] oppure iscriversi per ottenere un [account Azure gratuito].
* Java Development Kit (JDK) supportato. Per altre informazioni sulle versioni di JDK utilizzabili per lo sviluppo in Azure, vedere <https://aka.ms/azure-jdks>.
* [Apache Maven](http://maven.apache.org/), versione 3.0 o versione successiva.

## <a name="create-a-custom-application-using-the-spring-initializr"></a>Creare un'applicazione personalizzata tramite Spring Initializr

1. Passare a <https://start.spring.io/>.

1. Specificare che si vuole generare un progetto **Maven** con **Java** , selezionare la versione **8** e immettere i nomi in **Group** (Gruppo) e **Artifact** (Artefatto) per l'applicazione.

1. Aggiungere le dipendenze per la sezione **Spring Web** e selezionare la casella **Web** , quindi scorrere verso il basso fino alla sezione **NoSQL** e selezionare la casella **Spring Data Reactive Redis**. 
1. Scorrere fino in fondo alla pagina e fare clic sul pulsante **Generate Project** (Genera progetto).

   ![Opzioni di base di Spring Initializr][SI01]

   > [!NOTE]
   >
   > Spring Initializr userà i valori di **Group** (Gruppo) e **Artifact** (Artefatto) per creare il nome del pacchetto, ad esempio *com.contoso.myazuredemo*.
   >

1. Quando richiesto, scaricare il progetto in un percorso nel computer locale.

   ![Scaricare il progetto Spring Boot personalizzato][SI03]

1. Dopo l'estrazione dei file nel sistema locale, l'applicazione Spring Boot personalizzata sarà pronta per la modifica.

   ![File del progetto Spring Boot personalizzato][SI04]

## <a name="create-a-redis-cache-on-azure"></a>Creare una cache Redis in Azure

1. Passare al portale di Azure all'indirizzo <https://portal.azure.com/> e fare clic su **+Nuovo**.

1. Fare clic su **Database** e quindi su **Cache Redis**.

   ![Selezione della Cache Redis nel portale di Azure.][AZ02]

1. Nella pagina **Nuova cache Redis** specificare le informazioni seguenti:

   * Immettere **Nome DNS** per la cache.
   * Specificare **Sottoscrizione** , **Gruppo di risorse** , **Posizione** e **Piano tariffario**.
   * Per questa esercitazione, scegliere **Sblocca la porta 6379**.

   > [!NOTE]
   >
   > È possibile usare SSL con le cache Redis, ma è necessario usare un client Redis diverso, come Jedis. Per altre informazioni, vedere [Come usare Cache Redis di Azure con Java][Redis Cache with Java].
   >

   Dopo avere specificato queste opzioni, fare clic su **Crea** per creare la cache.

   ![Creare la cache nel portale di Azure.][AZ03]

1. Al termine, la cache verrà elencata nel **dashboard** di Azure, nonché nelle pagine **Tutte le risorse** e **Caches Redis**. È possibile fare clic sulla cache in una di queste posizioni per aprire la pagina delle proprietà per la cache.

   ![Risorsa sottoposta a provisioning nel portale di Azure.][AZ04]

1. Quando viene visualizzata la pagina contenente l'elenco delle proprietà della cache, fare clic su **Chiavi di accesso** e copiare le chiavi di accesso per la cache.

   ![Copiare le chiavi di accesso nella sezione Chiavi di accesso.][AZ05]

## <a name="configure-your-custom-spring-boot-to-use-your-redis-cache"></a>Configurare il progetto Spring Boot personalizzato per l'uso della cache Redis

1. Individuare il file *application.properties* nella directory *resources* dell'app o creare il file se non esiste già.

   ![Individuare il file application.properties][RE01]

1. Aprire il file *application.properties* in un editor di testo, quindi aggiungere le righe seguenti al file e sostituire i valori di esempio con le proprietà appropriate dalla cache:

   ```yaml
   # Specify the DNS URI of your Redis cache.
   spring.redis.host=myspringbootcache.redis.cache.windows.net

   # Specify the port for your Redis cache.
   spring.redis.port=6379

   # Specify the access key for your Redis cache.
   spring.redis.password=57686f6120447564652c2049495320526f636b73=
   ```

   ![Modifica del file application.properties][RE02]

   > [!NOTE] 
   > 
   > Se si usa un client Redis diverso, come Jedis che consente l'uso di SSL, specificare che si intende usare SSL e la porta 6380 nel file *application.properties*. Ad esempio:
   > 
   > ```yaml
   > # Specify the DNS URI of your Redis cache.
   > spring.redis.host=myspringbootcache.redis.cache.windows.net
   > # Specify the access key for your Redis cache.
   > spring.redis.password=57686f6120447564652c2049495320526f636b73=
   > # Specify that you want to use SSL.
   > spring.redis.ssl=true
   > # Specify the SSL port for your Redis cache.
   > spring.redis.port=6380
   > ```
   > 
   > Per altre informazioni, vedere [Come usare Cache Redis di Azure con Java][Redis Cache with Java]. 
   > 

1. Salvare e chiudere il file *application.properties*.

1. Creare una cartella denominata *controller* nella cartella di origine del pacchetto, ad esempio:

   `C:\SpringBoot\myazuredemo\src\main\java\com\contoso\myazuredemo\controller`

   -oppure-

   `/users/example/home/myazuredemo/src/main/java/com/contoso/myazuredemo/controller`

1. Creare un nuovo file denominato *HelloController.java* nella cartella *controller*. Aprire il file in un editor di testo e aggiungere il codice seguente:

   ```java
   package com.contoso.myazuredemo;

   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.bind.annotation.RestController;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   import org.springframework.data.redis.core.StringRedisTemplate;
   import org.springframework.data.redis.core.ValueOperations;

   @RestController
   public class HelloController {
   
      @Autowired
      private StringRedisTemplate template;

      @RequestMapping("/")
      // Define the Hello World controller.
      public String hello() {
      
         ValueOperations<String, String> ops = this.template.opsForValue();

         // Add a Hello World string to your cache.
         String key = "greeting";
         if (!this.template.hasKey(key)) {
             ops.set(key, "Hello World!");
         }

         // Return the string from your cache.
         return ops.get(key);
      }
   }
   ```
   
   Sarà necessario sostituire `com.contoso.myazuredemo` con il nome del pacchetto per il progetto.

1. Salvare e chiudere il file *HelloController.java*.

1. Compilare l'applicazione Spring Boot con Maven ed eseguirla, ad esempio:

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

1. Testare l'app Web passando a `http://localhost:8080` con un Web browser oppure usare una sintassi simile all'esempio seguente, se è disponibile curl:

   ```shell
   curl http://localhost:8080
   ```

   Dovrebbe essere visualizzato il messaggio "Hello World!" dal controller di esempio, recuperato dinamicamente dalla cache Redis.

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni su Spring e Azure, passare al centro di documentazione di Spring in Azure.

> [!div class="nextstepaction"]
> [Spring in Azure](./index.yml)

### <a name="additional-resources"></a>Risorse aggiuntive

Per altre informazioni sull'uso delle applicazioni Spring Boot in Azure, vedere gli articoli seguenti:

* [Distribuire un'applicazione Spring Boot in Linux nel Servizio app di Azure](deploy-spring-boot-java-app-on-linux.md)

* [Eseguire un'applicazione Spring Boot in un cluster Kubernetes nel servizio Azure Container](deploy-spring-boot-java-app-on-kubernetes.md)

Per altre informazioni sull'uso di Azure con Java, vedere [Azure per sviluppatori Java] e la documentazione relativa all'[uso di Azure DevOps e Java].

Per altre informazioni sulle operazioni iniziali nella cache Redis con Java in Azure, vedere [Come usare Cache Redis di Azure con Java][Redis Cache with Java].

**[Spring Framework]** è una soluzione open source che consente agli sviluppatori Java di creare applicazioni di livello enterprise. Uno dei progetti più comuni che si basa su questa piattaforma è [Spring Boot], che fornisce un approccio semplificato per la creazione di applicazioni Java autonome. Per semplificare le operazioni iniziali con Spring Boot per gli sviluppatori, alcuni pacchetti Spring Boot di esempio sono disponibili all'indirizzo <https://github.com/spring-guides/>. Oltre a consentire di scegliere dall'elenco di progetti Spring Boot di base, **[Spring Initializr]** semplifica le operazioni iniziali degli sviluppatori per la creazione di applicazioni Spring Boot personalizzate.

<!-- URL List -->

[Azure per sviluppatori Java]: ../index.yml
[Account Azure gratuito]: https://azure.microsoft.com/pricing/free-trial/
[Uso di Azure DevOps e Java]: /azure/devops/java/
[vantaggi per i sottoscrittori di MSDN]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/ (Framework di Spring)
[Redis Cache with Java]: /azure/redis-cache/cache-java-get-started

<!-- IMG List -->

[AZ01]: media/configure-spring-boot-initializer-java-app-with-redis-cache/AZ01.png
[AZ02]: media/configure-spring-boot-initializer-java-app-with-redis-cache/AZ02.png
[AZ03]: media/configure-spring-boot-initializer-java-app-with-redis-cache/AZ03.png
[AZ04]: media/configure-spring-boot-initializer-java-app-with-redis-cache/AZ04.png
[AZ05]: media/configure-spring-boot-initializer-java-app-with-redis-cache/AZ05.png

[SI01]: media/configure-spring-boot-initializer-java-app-with-redis-cache/SI01.png
[SI02]: media/configure-spring-boot-initializer-java-app-with-redis-cache/SI02.png
[SI03]: media/configure-spring-boot-initializer-java-app-with-redis-cache/SI03.png
[SI04]: media/configure-spring-boot-initializer-java-app-with-redis-cache/SI04.png

[RE01]: media/configure-spring-boot-initializer-java-app-with-redis-cache/RE01.png
[RE02]: media/configure-spring-boot-initializer-java-app-with-redis-cache/RE02.png
