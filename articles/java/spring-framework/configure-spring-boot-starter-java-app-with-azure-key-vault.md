---
title: Esercitazione sulla lettura di un segreto da Azure Key Vault in un'applicazione Spring Boot
description: Esercitazione sulla lettura di un segreto da Azure Key Vault in un'applicazione Spring Boot
services: key-vault
documentationcenter: java
ms.date: 08/15/2020
ms.service: key-vault
ms.tgt_pltfrm: multiple
ms.topic: tutorial
ms.workload: identity
ms.custom: devx-track-java
ms.openlocfilehash: e06d09d4f44366ba995ecaa401df901dc6270c6d
ms.sourcegitcommit: f80537193d3e22eb24cce4a0a5464a996d1e63eb
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/28/2020
ms.locfileid: "91409973"
---
# <a name="tutorial-reading-a-secret-from-azure-key-vault-in-a-spring-boot-application"></a>Esercitazione: Lettura di un segreto da Azure Key Vault in un'applicazione Spring Boot

Le applicazioni Spring Boot esternalizzano informazioni sensibili come nomi utente e password.  L'esternalizzazione delle informazioni sensibili consente una migliore manutenibilità, testabilità e sicurezza.  L'archiviazione di segreti al di fuori del codice è preferibile rispetto all'impostazione delle informazioni come hardcoded o alla relativa incorporazione in fase di compilazione.

Questa esercitazione descrive come creare un'app Spring Boot che legge un valore da Azure Key Vault e quindi come distribuire l'app nel Servizio app di Azure e in Azure Spring Cloud.

> [!div class="checklist"]
> * Creare Azure Key Vault e archiviare un segreto
> * Creare l'app con Spring Initializr
> * Aggiungere l'integrazione di Key Vault all'app
> * Distribuire nel Servizio app di Azure
> * Ridistribuire nel Servizio app di Azure con le identità gestite per le risorse di Azure
> * Distribuire in Azure Spring Cloud

## <a name="prerequisites"></a>Prerequisiti

* Una sottoscrizione di Azure attiva.
  * Se non si ha una sottoscrizione di Azure, [creare un account gratuito](https://azure.microsoft.com/free/).
* [Installare l'interfaccia della riga di comando di Azure versione 2.0.67 o successiva](/cli/azure/install-azure-cli?preserve-view=true&view=azure-cli-latest) e l'estensione Azure Spring Cloud con il comando `az extension add --name spring-cloud`
* Java Development Kit (JDK) supportato. Per altre informazioni sulle versioni di JDK utilizzabili per lo sviluppo in Azure, vedere <https://aka.ms/azure-jdks>.
* [Apache Maven](http://maven.apache.org/), versione 3.0 o versione successiva.
* Il comando `curl`.  Nella maggior parte dei sistemi operativi di tipo UNIX questo comando è preinstallato.  I client specifici del sistema operativo sono disponibili nel [sito Web ufficiale di curl](https://curl.haxx.se/).
* Il comando `jq`. Nella maggior parte dei sistemi operativi di tipo UNIX questo comando è preinstallato.  I client specifici del sistema operativo sono disponibili nel [sito Web ufficiale di jq](https://stedolan.github.io/jq/).

## <a name="create-a-new-azure-key-vault"></a>Creare una nuova istanza di Azure Key Vault

Le sezioni seguenti illustrano come accedere ad Azure e creare un'istanza di Azure Key Vault.

### <a name="sign-into-azure-and-set-your-subscription"></a>Accedere ad Azure e impostare la sottoscrizione

Usare prima di tutto la procedura seguente per eseguire l'autenticazione con l'interfaccia della riga di comando di Azure.

1. Facoltativamente, disconnettersi ed eliminare alcuni file di autenticazione per rimuovere eventuali credenziali rimanenti:

   ```azurecli
   az logout
   rm ~/.azure/accessTokens.json
   rm ~/.azure/azureProfile.json
   ```

1. Accedere all'account Azure con l'interfaccia della riga di comando di Azure:

   ```azurecli
   az login
   ```

   Seguire le istruzioni per completare il processo di accesso.

1. Elencare le sottoscrizioni:

   ```azurecli
   az account list
   ```

   Azure restituirà un elenco delle sottoscrizioni. Copiare il valore `id` della sottoscrizione da usare, ad esempio:

   ```json
   [
     {
       "cloudName": "AzureCloud",
       "id": "ssssssss-ssss-ssss-ssss-ssssssssssss",
       "name": "Converted Windows Azure MSDN - Visual Studio Ultimate",
       "state": "Enabled",
       "tenantId": "tttttttt-tttt-tttt-tttt-tttttttttttt",
       "user": {
         "name": "contoso@microsoft.com",
         "type": "user"
       }
     }
   ]
   ```

1. Specificare il GUID per la sottoscrizione che si vuole usare con Azure, ad esempio:

   ```azurecli
   az account set -s ssssssss-ssss-ssss-ssss-ssssssssssss
   ```

### <a name="create-a-service-principal-for-use-in-by-your-app"></a>Creare un'entità servizio da usare nell'app

Le *entità servizio* di Azure AD forniscono accesso alle risorse Azure all'interno della sottoscrizione. Un'entità servizio può essere considerata come un'identità utente per un servizio.  Per "servizio" si intende qualsiasi applicazione, servizio o piattaforma, inclusa l'app di esempio creata in questa esercitazione, che deve accedere alle risorse di Azure. È possibile configurare un'entità servizio con diritti di accesso limitati solo alle risorse specificate. Configurare quindi l'applicazione o il servizio in modo che usi le credenziali dell'entità servizio per accedere a tali risorse.

Creare un'entità servizio con questo comando.

```azurecli
az ad sp create-for-rbac --name contososp
```

Il valore dell'opzione `name` deve essere univoco all'interno della sottoscrizione.  Salvare i valori restituiti dal comando per usarli più avanti nell'esercitazione.  Il codice JSON restituito sarà simile a quello riportato di seguito.

```json
{
  "appId": "8r7o486s-o5q9-450s-8457-pr26p86n0497",
  "displayName": "ejbcontososp",
  "name": "http://ejbcontososp",
  "password": "4bt.lCKJKlbYLn_3XF~wWtUwyHU0jKggu2",
  "tenant": "72s988os-86s1-41ns-91no-2d7cd011db47"
}
```

### <a name="create-the-key-vault-instance"></a>Creare l'istanza di Key Vault

La procedura seguente crea e inizializza l'istanza di Key Vault.

1. Determinare l'area di Azure che conterrà le risorse.
   1. Per conoscerle, è possibile consultare l' [elenco delle aree e le relative posizioni](https://azure.microsoft.com/regions/).
   1. È possibile usare il comando `az account list-locations` per trovare il valore `Name` corretto per l'area scelta.

      ```azurecli
      az account list-locations -o table
      ```

      In questa esercitazione si userà `eastus`.
1. Creare un gruppo di risorse per contenere l'istanza di Key Vault e l'app del Servizio app.  Il valore deve essere univoco all'interno della sottoscrizione di Azure.  In questa esercitazione si userà `contosorg`.

   ```azurecli
   az group create --name contosorg --location eastus
   ```

1. Creare una nuova istanza di Key Vault nel gruppo di risorse.

   ```azurecli
   az keyvault create \
       --resource-group contosorg \
       --name contosokv \
       --enabled-for-deployment true \
       --enabled-for-disk-encryption true \
       --enabled-for-template-deployment true \
       --location eastus
       --query properties.vaultUri \
       --sku standard 
   ```

    > [!NOTE]
    > Il valore dell'opzione `--name` deve essere univoco all'interno della sottoscrizione di Azure.

   Questa tabella illustra le opzioni riportate in precedenza.

   | Parametro | Descrizione |
   |---|---|
   | `enabled-for-deployment` | Specifica l'[opzione di distribuzione di Key Vault](/cli/azure/keyvault). |
   | `enabled-for-disk-encryption` | Specifica l'[opzione di crittografia di Key Vault](/cli/azure/keyvault). |
   | `enabled-for-template-deployment` | Specifica l'[opzione di crittografia di Key Vault](/cli/azure/keyvault). |
   | `location` | Specifica l'[area di Azure](https://azure.microsoft.com/regions/) in cui verrà ospitato il gruppo di risorse. |
   | `name` | Specifica un nome univoco per l'istanza di Key Vault. |
   | `query` | Recupera l'URI di Key Vault dalla risposta.  L'URI è necessario per completare questa esercitazione. |
   | `sku` | Specifica l'[opzione SKU di Key Vault](/cli/azure/keyvault). |

   L'interfaccia della riga di comando di Azure visualizzerà l'URI di Key Vault che verrà usato in un secondo momento, ad esempio:

   ```output
   "https://contosokv.vault.azure.net/"
   ```

1. Configurare Key Vault per consentire le operazioni `get` e `list` da tale identità gestita.  Il valore di `object-id` corrisponde al valore `appId` del comando `az ad sp create-for-rbac` precedente.

   ```azurecli
   az keyvault set-policy --name contosokv --spn http://ejbcontososp --secret-permissions get list
   ```

   L'output sarà un oggetto JSON completo di informazioni su Key Vault.  Sarà presente una voce `type` con valore `Microsoft.KeyVault/vaults`.

   Questa tabella illustra le proprietà riportate in precedenza.

   | Parametro | Descrizione |
   |---|---|
   | name | Nome dell'istanza Key Vault. |
   | spn | Il valore `name` dell'output del comando `az ad sp create-for-rbac` precedente. |
   | secret-permissions | Elenco di operazioni da consentire dell'entità di sicurezza denominata. |

    > [!NOTE]
    > Anche se il principio dei privilegi minimi suggerisce di concedere a una risorsa il set di privilegi più ridotto possibile, per l'integrazione di Key Vault sono richieste almeno le operazioni `get` e `list`.

1. Archiviare un segreto nella nuova istanza di Key Vault.  Un caso d'uso comune consiste nell'archiviare una stringa di connessione JDBC.  Ad esempio:

   ```azurecli
   az keyvault secret set --name "connectionString" \
       --vault-name "contosokv" \
       --value "jdbc:sqlserver://SERVER.database.windows.net:1433;database=DATABASE;"
   ```

   Questa tabella illustra le opzioni riportate in precedenza.

   | Parametro | Descrizione |
   |---|---|
   | `name` | Specifica il nome del segreto. |
   | `value` | Specifica il valore del segreto. |
   | `vault-name` | Specifica il nome dell'istanza di Key Vault precedente. |

   Nell'interfaccia della riga di comando di Azure verranno visualizzati i risultati della creazione del segreto, ad esempio:

   ```json
   {
     "attributes": {
       "created": "2020-08-24T21:48:09+00:00",
       "enabled": true,
       "expires": null,
       "notBefore": null,
       "recoveryLevel": "Purgeable",
       "updated": "2020-08-24T21:48:09+00:00"
     },
     "contentType": null,
     "id": "https://contosokv.vault.azure.net/secrets/connectionString/123456789abcdef123456789abcdef",
     "kid": null,
     "managed": null,
     "tags": {
       "file-encoding": "utf-8"
     },
     "value": "jdbc:sqlserver://.database.windows.net:1433;database=DATABASE;"
   }
   ```

Dopo aver creato un'istanza di Key Vault e archiviato un segreto, nella sezione successiva verrà illustrato come creare un'app con Spring Initializr.

## <a name="create-the-app-with-spring-initializr"></a>Creare l'app con Spring Initializr

Questa sezione illustra come usare Spring Initializr e `RestController` per creare ed eseguire un'applicazione Spring Boot in locale.

1. Passare a <https://start.spring.io/>.
1. Selezionare le opzioni come illustrato nell'immagine riportata dopo questo elenco.
   1. **Project** (Progetto): `Maven Project`
   1. **Language** (Linguaggio): `Java`
   1. **Spring Boot**: `2.3.3`
   1. **Group** (Gruppo): `com.contoso`  (È possibile inserire qui qualsiasi nome di pacchetto Java valido).
   1. **Artifact** (Artefatto): *keyvault* (È possibile inserire qui qualsiasi nome di classe Java valido).
   1. **Packaging** (Creazione pacchetti): `Jar`
   1. **Java**: `11` (È possibile scegliere 8, ma questa esercitazione è stata convalidata con 11).
1. Selezionare **Add Dependencies...** (Aggiungi dipendenze).
1. Nel campo di testo digitare `Spring Web` e premere CTRL+INVIO.
1. Nel campo di testo digitare `Azure Key Vault` e premere INVIO.  Verrà visualizzata una schermata simile a quella riportata di seguito.
   :::image type="content" source="media/configure-spring-boot-starter-java-app-with-azure-key-vault/spring-initializr-choices.png" alt-text="Spring Initializr con le opzioni corrette selezionate.":::
1. Nella parte inferiore della pagina selezionare **Generate** (Genera).
1. Quando richiesto, scaricare il progetto in un percorso nel computer locale.  In questo caso verrà usata la directory *keyvault* nella home directory dell'utente corrente.  I valori precedenti genereranno il file *keyvault.zip* in tale directory.

Attenersi alla procedura seguente per esaminare l'applicazione ed eseguirla localmente.

1. Decomprimere il file *keyvault.zip*.  Il layout del file sarà simile a quello riportato di seguito.  In questa esercitazione verranno ignorati la directory *test* e il relativo contenuto.

   ```bash
   ├── HELP.md
   ├── mvnw
   ├── mvnw.cmd
   ├── pom.xml
   └── src
       ├── main
       │   ├── java
       │   │   └── com
       │   │       └── contoso
       │   │           └── keyvault
       │   │               └── KeyvaultApplication.java
       │   └── resources
       │       ├── application.properties
       │       ├── static
       │       └── templates
   ```

1. Aprire il file *KeyvaultApplication.java* in un editor di testo.  Modificare il file per renderlo simile a quello riportato di seguito.

   * La classe viene annotata con `@RestController`.  `@RestController` indica a Spring Boot che la classe può rispondere alle richieste HTTP RESTful.
   * La classe contiene un metodo annotato con `@GetMapping(get)`.  `@GetMapping` indica a Spring Boot di inviare richieste HTTP con il percorso `/get` a tale metodo, consentendo la restituzione della risposta dal metodo al client HTTP.
   * La classe contiene la variabile di istanza privata `connectionString`.  Il valore di questa variabile di istanza viene restituito dal metodo `get()`.

   ```java
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   import org.springframework.web.bind.annotation.GetMapping;
   import org.springframework.web.bind.annotation.RestController;

   @SpringBootApplication
   @RestController
   public class KeyvaultApplication {

      public static void main(String[] args) {
        SpringApplication.run(KeyvaultApplication.class, args);
      }

     @GetMapping("get")
     public String get() {
       return connectionString;
     }

     private String connectionString = "defaultValue\n";  

     public void run(String... varl) throws Exception {
       System.out.println(String.format("\nConnection String stored in Azure Key Vault:\n%s\n",connectionString));
     }  

   }
   ```

1. Nella directory di livello principale *keyvault*, dove si trova il file *pom.xml*, immettere `mvn spring-boot:run`.  
1. Il messaggio **Inizializzazione completata** nell'output del comando significa che il server è pronto.  In una finestra della shell separata immettere questo comando.

   ```bash
   $ curl http://localhost:8080/get
   ```

   L'output mostrerà `defaultValue`.

1. Terminare il processo eseguito da `mvn spring-boot:run`.  È possibile digitare CTRL+C oppure è possibile usare il comando `jps` per ottenere il PID del processo `Launcher` e terminarlo.

Nella sezione successiva verrà illustrato come aggiungere l'integrazione di Key Vault all'applicazione in esecuzione localmente.

## <a name="add-key-vault-integration-to-the-app"></a>Aggiungere l'integrazione di Key Vault all'app

I passaggi seguenti illustrano le modifiche necessarie per l'applicazione Spring Boot `KeyvaultApplication`.

Proprio come Key Vault consente di esternalizzare i segreti dal codice dell'applicazione, la configurazione di Spring consente di esternalizzare la configurazione dal codice.  Il modo più semplice per configurare Spring è il file *application.properties*.  In un progetto Maven questo file si trova in *src/main/resources/application.properties*.  Spring Initializer include un file di lunghezza zero in questa posizione.

Per aggiungere la configurazione necessaria a questo file, seguire questa procedura.

1. Aprire *src/main/resources/application.properties* in un editor e fare in modo che includa il contenuto seguente, modificando i valori per la sottoscrizione di Azure.

   ```txt
   azure.keyvault.client-id=685on005-ns8q-4o04-8s16-n7os38o2ro5n
   azure.keyvault.client-key=4bt.lCKJKlbYLn_3XF~wWtUwyHU0jKggu2
   azure.keyvault.enabled=true
   azure.keyvault.tenant-id=72s988os-86s1-41ns-91no-2q7pq011qo47
   azure.keyvault.uri=https://contosokv.vault.azure.net/
   ```

   Questa tabella illustra le proprietà riportate in precedenza.

   | Parametro | Descrizione |
   |---|---|
   | azure.keyvault.client-id | `appId` del codice JSON restituito da `az ad sp create-for-rbac`.|
   | azure.keyvault.client-key | `password` del codice JSON restituito da `az ad sp create-for-rbac`.|
   | azure.keyvault.enabled | Questa configurazione può essere utile quando è necessario impostare `enabled` o `disabled` in fase di distribuzione.  Per altre informazioni sulla configurazione di Spring, vedere la [documentazione di Spring](https://docs.spring.io/spring-boot/docs/2.2.2.RELEASE/reference/htmlsingle/#boot-features-external-config).
   | azure.keyvault.tenant-id | `tenant` del codice JSON restituito da `az ad sp create-for-rbac`.|
   | azure.keyvault.uri | Output del comando `az keyvault create` precedente. |

   L'elenco completo delle proprietà disponibili è documentato nelle [informazioni di riferimento sulle proprietà](https://aka.ms/azure-spring-boot-starter-keyvault-secrets).

1. Salvare il file e chiuderlo.

Apportare una semplice modifica al file *KeyvaultApplication.java* (o qualunque sia il nome della classe nel proprio caso).

1. Aprire *src/main/java/com/contoso/keyvault/KeyvaultApplication.java* in un editor di testo.
1. Aggiungere questo comando di importazione.

   ```java
   import org.springframework.beans.factory.annotation.Value;
   ```

1. Aggiungere un'annotazione alla variabile di istanza `connectionString`.

   ```java
   @Value("${connectionString}")
   private String connectionString;  
   ```

   L'integrazione di Key Vault fornisce un oggetto `PropertySource` di Spring popolato con i valori di Key Vault.  Altri dettagli sull'implementazione sono disponibili nella [documentazione di riferimento](https://aka.ms/azure-spring-boot-starter-keyvault-secrets).

1. Nella directory di livello principale *keyvault*, dove si trova il file *pom.xml*, immettere `mvn clean package spring-boot:run`.  
1. Il messaggio **Inizializzazione completata** nell'output del comando significa che il server è pronto.  In una finestra della shell separata immettere questo comando.

   ```bash
   $ curl http://localhost:8080/get
   ```

   L'output mostrerà `jdbc:sqlserver://SERVER.database.windows.net:1433;database=DATABASE` anziché `defaultValue`.

1. Terminare il processo eseguito da `mvn spring-boot:run`.  È possibile digitare CTRL+C oppure è possibile usare il comando `jps` per ottenere il PID del processo `Launcher` e terminarlo.


Nella sezione successiva verrà illustrato come distribuire l'app nel Servizio app di Azure.

## <a name="deploy-to-azure-app-service"></a>Distribuire nel Servizio app di Azure

Eseguire la procedura descritta in questa sezione per distribuire `KeyvaultApplication` nel Servizio app di Azure.

### <a name="use-the-azure-maven-web-app-plugin-to-deploy-the-application-to-azure-app-service"></a>Usare il plug-in per app Web Maven di Azure per distribuire l'applicazione nel Servizio app di Azure

Seguire questa procedura per preparare il file POM per la distribuzione di `KeyvaultApplication` nel Servizio app di Azure.

1. Nella directory di livello principale *keyvault* aprire il file *pom.xml*.
1. Nella sezione `<build><plugins>` aggiungere `azure-webapp-maven-plugin` inserendo questo codice XML.

   ```xml
    <plugin>
     <groupId>com.microsoft.azure</groupId>
     <artifactId>azure-webapp-maven-plugin</artifactId>
     <version>1.11.0</version>
    </plugin>
   ```

   > [!NOTE]
   > Non preoccuparsi della formattazione.  `azure-webapp-maven-plugin` riformatterà l'intero file POM durante questo processo.

1. Salvare e chiudere il file *pom.xml*.
1. Alla riga di comando richiamare l'obiettivo `config` del plug-in appena aggiunto.  Il plug-in Maven farà alcune domande e modificherà il file *pom.xml* in base alle risposte.  Il file POM verrà modificato ulteriormente.

   ```bash
   mvn azure-webapp:config
   ```

1. Per `Subscription`, verificare di aver selezionato lo stesso ID sottoscrizione con l'insieme di credenziali delle chiavi creato.
1. Per `Web App`, è possibile selezionare un'app Web esistente o selezionare `<create>` per crearne una nuova. Se si seleziona un'app Web esistente, si passerà direttamente all'ultimo passaggio **confirm**.
1. Per `OS`, assicurarsi che sia selezionato `linux`.
1. Per `javaVersion` assicurarsi che sia selezionata la versione Java scelta in Spring Initializr.  In precedenza è stato scelto `11`, quindi qui verrà scelto 11.
1. Accettare le impostazioni predefinite per le domande rimanenti.
1. Quando viene richiesto di confermare, rispondere Y per continuare o N per iniziare a rispondere di nuovo alle domande.  Al termine dell'esecuzione del plug-in, si è pronti per modificare il file POM.

Attenersi alla procedura seguente per apportare ulteriori modifiche necessarie al file POM.

1. Nella directory di livello principale *keyvault* aprire il file *pom.xml*.
1. Trovare la voce `azure-webapp-maven-plugin` nella sezione `<plugins>.
1. Modificare il valore di `<resourceGroup>`, `<appName>` e `<region>`.  
   1. Impostare il valore di `<resourceGroup>` in modo che corrisponda a quello specificato durante la creazione di Key Vault.
   1. Scegliere un valore ragionevole per `<appName>` che sia univoco all'interno della sottoscrizione.
   1. Impostare il valore di `<region>` in modo che corrisponda a quello specificato durante la creazione di Key Vault.
1. Includere un elemento `<appSettings>` che fa in modo che il server sia in ascolto sulla porta TCP 80.
1. Di seguito viene visualizzato l'oggetto `<plugin>` completo modificato per `azure-webapp-maven-plugin`.  I valori che è necessario modificare sono indicati con `*`.

   ```xml
   <plugins> 
     <plugin> 
       <groupId>org.springframework.boot</groupId>  
       <artifactId>spring-boot-maven-plugin</artifactId> 
     </plugin>  
     <plugin> 
       <groupId>com.microsoft.azure</groupId>  
       <artifactId>azure-webapp-maven-plugin</artifactId>  
       <version>1.11.0</version>  
       <configuration>
         <schemaVersion>V2</schemaVersion>
         *<subscriptionId>********-****-****-****-************</subscriptionId>
         *<resourceGroup>contosorg</resourceGroup>
         *<appName>contosokeyvault</appName>
         <pricingTier>P1v2</pricingTier>
         *<region>eastus</region>
         <runtime>
           <os>linux</os>
           <javaVersion>java11</javaVersion>
           <webContainer>java11</webContainer>
         </runtime>
         *<!-- Begin of App Settings  -->
         *<appSettings>
         *  <property>
         *    <name>JAVA_OPTS</name>
         *    <value>-Dserver.port=80</value>
         *  </property>
         *</appSettings>
         *<!-- End of App Settings  -->          
         <deployment>
           <resources>
             <resource>
               <directory>${project.basedir}/target</directory>
               <includes>
                 <include>*.jar</include>
               </includes>
             </resource>
           </resources>
         </deployment>
       </configuration>
     </plugin> 
   </plugins>
   ```

1. Salvare e chiudere il file POM.
1. Distribuire l'app nel Servizio app di Azure.

   ```bash
   mvn -DskipTests clean package azure-webapp:deploy
   ```

1. Il comando può richiedere diversi minuti, a seconda di molti fattori che esulano dal proprio controllo.  Quando viene visualizzato un output simile al seguente, vuol dire che l'app è stata distribuita correttamente.

   ```bash
   [INFO] Deploying the zip package contosokeyvault-22b7c1a3-b41b-4082-a9f0-9339723fa36a11893059035499017844.zip...
   [INFO] Successfully deployed the artifact to https://contosokeyvault.azurewebsites.net
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  01:45 min
   [INFO] Finished at: 2020-08-16T22:47:48-04:00
   [INFO] ------------------------------------------------------------------------
   ```

1. Attendere da tre a cinque minuti per consentire il completamento della distribuzione.  Quindi, è possibile accedere alla distribuzione con un comando `curl` come quello mostrato in precedenza, ma questa volta usando il nome host indicato nell'output di `BUILD SUCCESS`.  Ad esempio, questo output del comando `curl` indica l'esito positivo.

   ```bash
   curl https://contosokeyvault.azurewebsites.net/get
   jdbc:sqlserver://SERVER.database.windows.net:1433;database=DATABASE;
   ```

La distribuzione dell'app in Servizio app di Azure è stata completata.

## <a name="redeploy-to-azure-app-service-and-use-managed-identities-for-azure-resources"></a>Ridistribuire nel Servizio app di Azure usando le identità gestite per le risorse di Azure

Questa sezione descrive come associare un'identità alla risorsa di Azure per l'app.  Questa operazione è necessaria in modo che Azure possa applicare la sicurezza e tenere traccia dell'accesso.  Pagare solo per le risorse usate è un principio fondamentale del cloud computing.  Questo rilevamento delle risorse con granularità fine è possibile solo se ogni risorsa è associata a un'identità.  Servizio app di Azure e Azure Key Vault sono due dei tanti servizi di Azure che sfruttano le identità gestite per le risorse di Azure.  Per altre informazioni su questa importante tecnologia, vedere [Informazioni sulle identità gestite per le risorse di Azure](/azure/active-directory/managed-identities-azure-resources/overview).

> [!NOTE]
> Identità gestite per le risorse di Azure è il nuovo nome per il servizio precedentemente noto come identità del servizio gestita.

Seguire questa procedura per creare l'identità gestita per l'app del Servizio app di Azure e quindi consentire a tale identità di accedere a Key Vault.

1. Creare un'identità gestita per l'app del Servizio app.  Sostituire le opzioni con i valori degli elementi `<resourceGroup>` e `<appName>` del file POM.

   ```azurecli
   az webapp identity assign --resource-group contosorg --name contosokeyvault
   ```

   L'output sarà simile a quanto riportato di seguito.  Annotare il valore di `principalId` per il passaggio successivo.

   ```json
   {
     "principalId": "2r7s6r00-92o9-4sq7-n10r-popq294ssr8s",
     "tenantId": "72s988os-86s1-41ns-91no-2q7pq011qo47",
     "type": "SystemAssigned",
     "userAssignedIdentities": null
   }
   ```

1. Modificare il file *application.properties* in modo che definisca il nome dell'identità gestita per le risorse di Azure create nel passaggio precedente.

   1. Rimuovere `azure.keyvault.client-key`.
   1. Aggiornare `azure.keyvault.client-id` in modo da avere un valore da `principalId` del passaggio precedente.  Il file completato avrà l'aspetto simile a quello riportato di seguito.

   ```bash
   azure.keyvault.client-id=56rqs994-0o66-43o3-9roo-8e3534d0cb23
   azure.keyvault.enabled=true
   azure.keyvault.tenant-id=72s988os-86s1-41ns-91ab-2q7pq011qo47
   azure.keyvault.uri=https://contosokv.vault.azure.net/    
   ```

1. Configurare Key Vault per consentire le operazioni `get` e `list` da tale identità gestita.  Il valore di `object-id` è `principalId` dell'output precedente.

   ```azurecli
   az keyvault set-policy --name contosokv \
     --object-id 2r7s6r00-92o9-4sq7-n10r-popq294ssr8s --secret-permissions get list
   ```

   L'output sarà un oggetto JSON completo di informazioni su Key Vault.  Sarà presente una voce `type` con valore `Microsoft.KeyVault/vaults`.

   Questa tabella illustra le proprietà riportate in precedenza.

   | Parametro | Descrizione |
   |---|---|
   | name | Nome dell'istanza Key Vault. | 
   | object-id | `principalId` del comando precedente. |
   | secret-permissions | Elenco di operazioni da consentire dell'entità di sicurezza denominata. |

1. Creare un pacchetto dell'applicazione e ridistribuirla.

   ```bash
   mvn -DskipTests clean package azure-webapp:deploy
   ```

1. È consigliabile attendere alcuni minuti per consentire il totale completamento della distribuzione.  Quindi, è possibile accedere alla distribuzione con un comando `curl` come quello mostrato in precedenza, ma questa volta usando il nome host indicato nell'output di `BUILD SUCCESS`.  Ad esempio, questo output del comando `curl` indica l'esito positivo.

   ```bash
   curl https://contosokeyvault.azurewebsites.net/get
   jdbc:sqlserver://SERVER.database.windows.net:1433;database=DATABASE;
   ```

Anziché restituire `defaultValue`, il valore per `connectionString` viene cercato e restituito da Key Vault.  Nella sezione successiva verrà distribuita la stessa app in Azure Spring Cloud.

## <a name="deploy-to-azure-spring-cloud"></a>Distribuire in Azure Spring Cloud

Azure Spring Cloud è una piattaforma completamente gestita per la distribuzione e l'esecuzione delle applicazioni Spring Boot in Azure.  Per una panoramica di Azure Spring Cloud, vedere [Cos'è Azure Spring Cloud?](/azure/spring-cloud/spring-cloud-overview)

Questa sezione userà l'app Spring Boot esistente e l'istanza di Key Vault creata in precedenza con una nuova istanza di Azure Spring Cloud.

I passaggi seguenti illustrano come creare una risorsa di Azure Spring Cloud e distribuirvi l'app.  Assicurarsi di aver installato l'estensione dell'interfaccia della riga di comando di Azure per Azure Spring Cloud, come illustrato nei [Prerequisiti](#prerequisites).

1. Decidere un nome per l'istanza del servizio.  Per usare Azure Spring Cloud nella sottoscrizione di Azure, è necessario creare una risorsa di Azure di tipo Azure Spring Cloud.  Come per tutte le altre risorse di Azure, l'istanza del servizio deve rimanere all'interno di un gruppo di risorse.  Usare il gruppo di risorse già creato per ospitare l'istanza del servizio.  Creare l'istanza del servizio con questo comando. 

   ```bash
   az spring-cloud create --resource-group "contosorg" --name "contososvc" 
   ```

   Il completamento di questo comando richiede diversi minuti.

1. Creare un'app Spring Cloud all'interno del servizio.

   ```bash
   az spring-cloud app create --resource-group "contosorg" --name "contosoascsapp" --assign-identity --is-public true
     --runtime-version Java_11 --service "contososvc"
   ```

   Questa tabella illustra le opzioni riportate in precedenza.

   | Parametro | Descrizione |
   |---|---|
   | assign-identity | Fa in modo che il servizio crei un'identità per le identità gestite per le risorse di Azure. |
   | is-public | Assegna un nome di dominio DNS pubblico al servizio. |
   | name | nome dell'app. |
   | resource-group | Nome del gruppo di risorse in cui è stata creata l'istanza del servizio esistente. |
   | runtime-version | Versione del runtime Java.  **Il valore deve corrispondere al valore scelto in Spring Initializr in precedenza.** |
   | service | Nome del servizio esistente. |

   Per comprendere la differenza tra *servizio* e *app*, vedere [Informazioni su app e distribuzione in Azure Spring Cloud](/azure/spring-cloud/spring-cloud-concept-understand-app-and-deployment).

1. Ottenere l'identità gestita per la risorsa di Azure.  Usarla per configurare l'istanza di Key Vault esistente per consentire l'accesso da questa app.

   ```bash
   SERVICE_IDENTITY=$(az spring-cloud app show --resource-group "contosorg" --name "contosoascsapp" --service "contososvc" | jq -r '.identity.principalId')
   az keyvault set-policy --name "contosokv" --object-id <the value of the environment variable SERVICE_IDENTITY> --secret-permissions set get list
   ```

1. Poiché l'app Spring Boot esistente contiene già un file *application.properties* con la configurazione necessaria, è possibile distribuire questa app direttamente in Spring Cloud usando il comando seguente.  Eseguire il comando nella directory contenente il file POM.

   ```bash
   az spring-cloud app deploy --resource-group "contosorg" --name "contosoascsapp" --jar-path target/keyvault-0.0.1-SNAPSHOT.jar \
     --service "contososvc"
   ```

   Questo comando crea una *distribuzione* all'interno dell'app all'interno del servizio.  Per altri dettagli sui concetti relativi a istanze del servizio, app e distribuzioni, vedere [Informazioni su app e distribuzione in Azure Spring Cloud](/azure/spring-cloud/spring-cloud-concept-understand-app-and-deployment).

   Se la distribuzione non riesce, configurare i log per la risoluzione dei problemi, come descritto in [Configurare i log dell'applicazione](https://aka.ms/azure-spring-cloud-configure-logs).  È probabile che i log contengano informazioni utili per diagnosticare e risolvere il problema.

1. Dopo aver distribuito correttamente l'app, è possibile usare `curl` per verificare che l'integrazione di Key Vault funzioni.  Poiché è stato specificato `--is-public`, l'URL predefinito per il servizio è `https://<service name>-<app name>.azuremicroservices.io/`.  Se si sostituiscono i valori appropriati e si aggiunge il valore dell'annotazione `@GetMapping`, si otterrà questo comando `curl`.

   ```bash
   curl https://contososvc-contosoascsapp.azuremicroservices.io/get
   ```

   L'output mostrerà `jdbc:sqlserver://SERVER.database.windows.net:1433;database=DATABASE`.

## <a name="summary"></a>Riepilogo

È stata creata una nuova applicazione Web Java con **Spring Initializr**.  È stata creata un'istanza di Azure Key Vault per l'archiviazione di informazioni sensibili e quindi si è configurata l'applicazione per recuperare le informazioni dall'istanza di Key Vault.  Dopo aver eseguito il test in locale, l'app è stata distribuita nel Servizio app di Azure e in Azure Spring Cloud.

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni su Spring e Azure, passare al centro di documentazione di Spring in Azure.

> [!div class="nextstepaction"]
> [Configurare un'app Spring Boot Initializer per l'uso di Application Insights](configure-spring-boot-java-applicationinsights.md)
