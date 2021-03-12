---
title: "Esercitazione: leggere un segreto da Azure Key Vault in un'applicazione Spring boot"
description: In questa esercitazione si creerà un'app Spring boot che legge un valore da Azure Key Vault e si distribuisce l'app nel servizio app Azure e nel cloud Spring di Azure.
ms.date: 03/11/2021
ms.service: key-vault
ms.topic: tutorial
ms.custom: devx-track-java, devx-track-azurecli
ms.author: edburns
author: edburns
ms.openlocfilehash: 7d951994a0c563460b2086d0138ce710ef494771
ms.sourcegitcommit: 3536f174735cd3bb7da7e4b266fbf43349a22b67
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/12/2021
ms.locfileid: "103193543"
---
# <a name="tutorial-read-a-secret-from-azure-key-vault-in-a-spring-boot-application"></a>Esercitazione: leggere un segreto da Azure Key Vault in un'applicazione Spring boot

Questa esercitazione illustra come creare un'app Spring boot che legge un valore da Azure Key Vault. Dopo aver creato l'app, la si distribuirà nel servizio app Azure e nel cloud Spring di Azure.

Le applicazioni Spring Boot esternalizzano informazioni sensibili come nomi utente e password. L'esternalizzazione delle informazioni sensibili consente una migliore manutenibilità, testabilità e sicurezza. L'archiviazione di segreti al di fuori del codice è preferibile rispetto all'impostazione delle informazioni come hardcoded o alla relativa incorporazione in fase di compilazione.

In questa esercitazione verranno illustrate le procedure per:

> [!div class="checklist"]
> * Creare una Azure Key Vault e archiviare un segreto
> * Creare un'app con Spring Initializr
> * Aggiungere l'integrazione di Key Vault all'app
> * Distribuire nel Servizio app di Azure
> * Ridistribuire il servizio app Azure con identità gestite per le risorse di Azure
> * Distribuire in Azure Spring Cloud

## <a name="prerequisites"></a>Prerequisiti

- [!INCLUDE [free subscription](includes/quickstarts-free-trial-note.md)]
[!INCLUDE [curl](includes/prerequisites-curl.md)]
[!INCLUDE [jq](includes/prerequisites-jq.md)]
[!INCLUDE [Azure CLI](includes/prerequisites-azure-cli.md)]
[!INCLUDE [JDK](includes/prerequisites-java.md)]
[!INCLUDE [Maven](includes/prerequisites-maven.md)]

## <a name="create-a-new-azure-key-vault"></a>Creare una nuova istanza di Azure Key Vault

Le sezioni seguenti illustrano come accedere ad Azure e creare un Azure Key Vault.

### <a name="sign-into-azure-and-set-your-subscription"></a>Accedere ad Azure e impostare la sottoscrizione

Usare prima di tutto la procedura seguente per eseguire l'autenticazione con l'interfaccia della riga di comando di Azure.

1. Facoltativamente, disconnettersi ed eliminare alcuni file di autenticazione per rimuovere eventuali credenziali persistenti:

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

   Azure restituirà un elenco delle sottoscrizioni. Copiare il `id` valore per la sottoscrizione che si vuole usare, ad esempio:

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

### <a name="create-a-service-principal"></a>Creare un'entità servizio

Le *entità servizio* di Azure AD forniscono accesso alle risorse Azure all'interno della sottoscrizione. Un'entità servizio può essere considerata come un'identità utente per un servizio. "Service" è un'applicazione, un servizio o una piattaforma che deve accedere alle risorse di Azure. È possibile configurare un'entità servizio con diritti di accesso limitati solo alle risorse specificate. Configurare quindi l'applicazione o il servizio in modo che usi le credenziali dell'entità servizio per accedere a tali risorse.

Per creare un'entità servizio, usare il comando seguente.

```azurecli
az ad sp create-for-rbac --name contososp
```

Il valore dell'opzione `name` deve essere univoco all'interno della sottoscrizione. Salvare i valori restituiti dal comando per usarli più avanti nell'esercitazione. Il codice JSON restituito sarà simile all'output seguente:

```output
{
  "appId": "sample-app-id",
  "displayName": "ejbcontososp",
  "name": "http://ejbcontososp",
  "password": "sample-password",
  "tenant": "sample-tenant"
}
```

### <a name="create-the-key-vault-instance"></a>Creare l'istanza di Key Vault

Per creare e inizializzare la Azure Key Vault, attenersi alla procedura seguente:

1. Determinare l'area di Azure che conterrà le risorse.
   1. Per visualizzare l'elenco delle aree e delle rispettive località, vedere [geografie di Azure](https://azure.microsoft.com/regions/).
   1. Usare il `az account list-locations` comando per trovare la corretta `Name` per l'area scelta.

      ```azurecli
      az account list-locations --output table
      ```

      In questa esercitazione viene usato `eastus`.

1. Creare un gruppo di risorse per contenere l'istanza di Key Vault e l'app del Servizio app. Il valore deve essere univoco all'interno della sottoscrizione di Azure. In questa esercitazione viene usato `contosorg`.

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
   | `query` | Recupera l'URI di Key Vault dalla risposta. L'URI è necessario per completare questa esercitazione. |
   | `sku` | Specifica l'[opzione SKU di Key Vault](/cli/azure/keyvault). |

   L'interfaccia della riga di comando di Azure visualizzerà l'URI di Key Vault che verrà usato in un secondo momento, ad esempio:

   ```output
   "https://contosokv.vault.azure.net/"
   ```

1. Configurare la Key Vault per consentire `get` `list` le operazioni e da tale identità gestita. Il valore di `object-id` corrisponde al valore `appId` del comando `az ad sp create-for-rbac` precedente.

   ```azurecli
   az keyvault set-policy --name contosokv --spn http://ejbcontososp --secret-permissions get list
   ```

   L'output sarà un oggetto JSON completo di informazioni su Key Vault. Sarà presente una voce `type` con valore `Microsoft.KeyVault/vaults`.

   Questa tabella illustra le proprietà riportate in precedenza.

   | Parametro | Descrizione |
   |---|---|
   | name | Nome dell'istanza Key Vault. |
   | spn | Il valore `name` dell'output del comando `az ad sp create-for-rbac` precedente. |
   | secret-permissions | Elenco di operazioni da consentire dell'entità di sicurezza denominata. |

   > [!NOTE]
   > Anche se il principio dei privilegi minimi suggerisce di concedere a una risorsa il set di privilegi più ridotto possibile, per l'integrazione di Key Vault sono richieste almeno le operazioni `get` e `list`.

1. Archiviare un segreto nella nuova istanza di Key Vault. Un caso d'uso comune consiste nell'archiviare una stringa di connessione JDBC. Ad esempio:

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

   ```output
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
     "id": "https://contosokv.vault.azure.net/secrets/connectionString/sample-id",
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
   * **Progetto**: **progetto Maven**
   * **Lingua**: **Java**
   * **Spring boot**: **2.3.3**
   * **Gruppo**: *com. contoso* . è possibile inserire qui un nome di pacchetto Java valido.
   * **Artifact** (Artefatto): *keyvault* (È possibile inserire qui qualsiasi nome di classe Java valido).
   * Creazione **pacchetto**: **jar**
   * **Java**: **11** (è possibile scegliere 8, ma questa esercitazione è stata convalidata con 11).
1. Selezionare **Add Dependencies...** (Aggiungi dipendenze).
1. Nel campo di testo digitare *Spring Web* e premere CTRL + INVIO.
1. Nel campo di testo digitare *Azure Key Vault* e premere INVIO. Verrà visualizzata una schermata simile a quella riportata di seguito.
   :::image type="content" source="media/configure-spring-boot-starter-java-app-with-azure-key-vault/spring-initializr-choices.png" alt-text="Spring Initializr con le opzioni corrette selezionate.":::
1. Nella parte inferiore della pagina selezionare **Generate** (Genera).
1. Quando richiesto, scaricare il progetto in un percorso nel computer locale. Questa esercitazione *Usa una directory dell'insieme di credenziali* delle credenziali nella home directory dell'utente corrente. I valori precedenti genereranno il file *keyvault.zip* in tale directory.

Usare la procedura seguente per esaminare l'applicazione ed eseguirla localmente.

1. Decomprimere il file *keyvault.zip*. Il layout del file sarà simile a quello riportato di seguito. Questa esercitazione ignora la directory di *test* e il relativo contenuto.

   ```
   ├── HELP.md
   ├── mvnw
   ├── mvnw.cmd
   ├── pom.xml
   └── src
       ├── main
       │   ├── java
       │   │   └── com
       │   │       └── contoso
       │   │           └── keyvault
       │   │               └── KeyvaultApplication.java
       │   └── resources
       │       ├── application.properties
       │       ├── static
       │       └── templates
   ```

1. Aprire il file *KeyvaultApplication.java* in un editor di testo. Modificare il file in modo che includa il contenuto seguente.

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

   Nell'elenco seguente vengono evidenziati alcuni dettagli su questo codice:

   * La classe viene annotata con `@RestController`. `@RestController` indica a Spring Boot che la classe può rispondere alle richieste HTTP RESTful.
   * La classe contiene un metodo annotato con `@GetMapping(get)`. `@GetMapping` indica a Spring Boot di inviare richieste HTTP con il percorso `/get` a tale metodo, consentendo la restituzione della risposta dal metodo al client HTTP.
   * La classe contiene la variabile di istanza privata `connectionString`. Il valore di questa variabile di istanza viene restituito dal metodo `get()`.

1. Aprire una finestra bash e passare *alla directory dell'* insieme di credenziali di primo livello in cui si trova il file di *pom.xml* .

1. Immettere il comando seguente:

   ```bash
   mvn spring-boot:run
   ```

   Output del comando `Completed initialization` , che indica che il server è pronto.

1. In una finestra bash separata immettere il comando seguente:

   ```bash
   curl http://localhost:8080/get
   ```

   L'output mostrerà `defaultValue`.

1. Terminare il processo che esegue da `mvn spring-boot:run` . È possibile digitare CTRL + C oppure è possibile usare il `jps` comando per ottenere il PID del `Launcher` processo e terminarlo.

## <a name="add-key-vault-integration-to-the-app"></a>Aggiungere l'integrazione di Key Vault all'app

Questa sezione illustra come aggiungere Key Vault integrazione all'applicazione in esecuzione localmente modificando l'applicazione Spring boot `KeyvaultApplication` .

Proprio come Key Vault consente di esternalizzare i segreti dal codice dell'applicazione, la configurazione di Spring consente di esternalizzare la configurazione dal codice. Il modo più semplice per configurare Spring è il file *application.properties*. In un progetto Maven questo file si trova in *src/main/resources/application.properties*. Spring Initializr include un file di lunghezza zero in questa posizione. Usare la procedura seguente per aggiungere la configurazione necessaria a questo file.

1. Modificare il file *src/main/Resources/Application. Properties* in modo che includa il contenuto seguente, modificando i valori per la sottoscrizione di Azure.

   ```txt
   azure.keyvault.client-id=<your client ID>
   azure.keyvault.client-key=<your client key>
   azure.keyvault.enabled=true
   azure.keyvault.tenant-id=<your tenant ID>
   azure.keyvault.uri=https://contosokv.vault.azure.net/
   ```

   Questa tabella illustra le proprietà riportate in precedenza.

   | Parametro | Descrizione |
   |---|---|
   | azure.keyvault.client-id | `appId` del codice JSON restituito da `az ad sp create-for-rbac`.|
   | azure.keyvault.client-key | `password` del codice JSON restituito da `az ad sp create-for-rbac`.|
   | azure.keyvault.enabled | Questa configurazione può essere utile quando è necessario impostare `enabled` o `disabled` in fase di distribuzione. Per ulteriori informazioni sulla configurazione di Spring, vedere la pagina relativa alla [configurazione esternalizzata](https://docs.spring.io/spring-boot/docs/2.2.2.RELEASE/reference/htmlsingle/#boot-features-external-config) nella documentazione di Spring.
   | azure.keyvault.tenant-id | `tenant` del codice JSON restituito da `az ad sp create-for-rbac`.|
   | azure.keyvault.uri | Output del comando `az keyvault create` precedente. |

   Per l'elenco completo delle proprietà, vedere la [libreria client Starter di Spring Boot Azure Key Vault Secrets per Java](https://aka.ms/azure-spring-boot-starter-keyvault-secrets).

1. Salvare il file e chiuderlo.

1. Aprire *src/main/Java/com/contoso/Vault/KeyvaultApplication. Java* in un editor.

1. Aggiungere l'istruzione `import` seguente.

   ```java
   import org.springframework.beans.factory.annotation.Value;
   ```

1. Aggiungere l'annotazione seguente alla `connectionString` variabile di istanza.

   ```java
   @Value("${connectionString}")
   private String connectionString;
   ```

   L'integrazione di Key Vault fornisce una molla `PropertySource` popolata dai valori della Key Vault. Per altri dettagli sull'implementazione, vedere [la libreria client Starter di Spring Boot Azure Key Vault Secrets per Java](https://aka.ms/azure-spring-boot-starter-keyvault-secrets).

1. Aprire una finestra bash e passare *alla directory dell'* insieme di credenziali di primo livello in cui si trova il file di *pom.xml* .

1. Immettere il comando seguente:

   ```bash
   mvn clean package spring-boot:run
   ```

   Output del comando `initialization completed` , che indica che il server è pronto.

1. In una finestra bash separata immettere il comando seguente:

   ```bash
   curl http://localhost:8080/get
   ```

   L'output mostrerà `jdbc:sqlserver://SERVER.database.windows.net:1433;database=DATABASE` anziché `defaultValue`.

1. Terminare il processo che esegue da `mvn spring-boot:run` . È possibile digitare CTRL + C oppure è possibile usare il `jps` comando per ottenere il PID del `Launcher` processo e terminarlo.

## <a name="deploy-to-azure-app-service"></a>Distribuire nel Servizio app di Azure

Nei passaggi seguenti viene illustrato come distribuire il `KeyvaultApplication` al servizio app Azure.

1. Nella *directory dell'* insieme di credenziali delle credenziali di primo livello aprire il file di *pom.xml* .
1. Nella `<build><plugins>` sezione aggiungere l'oggetto `azure-webapp-maven-plugin` inserendo il codice XML seguente.

   ```xml
   <plugin>
     <groupId>com.microsoft.azure</groupId>
     <artifactId>azure-webapp-maven-plugin</artifactId>
     <version>1.12.0</version>
   </plugin>
   ```

   > [!NOTE]
   > Non preoccuparsi della formattazione. `azure-webapp-maven-plugin` riformatterà l'intero file POM durante questo processo.

1. Salvare e chiudere il file di *pom.xml*
1. In una riga di comando usare il comando seguente per richiamare l' `config` obiettivo del nuovo plug-in aggiunto.

   ```bash
   mvn azure-webapp:config
   ```

   Il plug-in Maven ti chiederà alcune domande e modificherà il file di *pom.xml* in base alle risposte. Usare i valori seguenti:

   * Per **Subscription (sottoscrizione**), assicurarsi di avere selezionato lo stesso ID sottoscrizione con il Key Vault creato.
   * Per l' **app Web**, è possibile selezionare un'app Web esistente o selezionare `<create>` per crearne una nuova. Se si seleziona un'app Web esistente, passerà direttamente all'ultimo passaggio di **conferma** .
   * Per il **sistema operativo**, assicurarsi che sia selezionato **Linux** .
   * Per **javaVersion**, assicurarsi di selezionare la versione Java scelta in Spring Initializr. Questa esercitazione usa la versione 11.
   * Accettare le impostazioni predefinite per le domande rimanenti.
   * Quando viene richiesto di confermare, rispondere Y per continuare o N per iniziare a rispondere di nuovo alle domande. Al termine dell'esecuzione del plug-in, si è pronti per modificare il file POM.

1. Successivamente, aprire il *pom.xml* modificato in un editor. Il contenuto del file dovrebbe essere simile al codice XML seguente. Sostituire i segnaposto seguenti con i valori specificati se il valore non è già stato fornito nel passaggio precedente.

   * `YOUR_SUBSCRIPTION_ID`: Questo segnaposto indica il percorso dell'ID fornito in precedenza.
   * `YOUR_RESOURCE_GROUP_NAME`: Sostituire il segnaposto con il valore specificato al momento della creazione del Key Vault.
   * `YOUR_APP_NAME`: Sostituire il segnaposto con un valore ragionevole univoco all'interno della sottoscrizione.
   * `YOUR_REGION`: Sostituire il segnaposto con il valore specificato al momento della creazione del Key Vault.
   * `APP_SETTINGS`: Copiare l' `<appSettings>` elemento indicato dall'esempio e incollarlo in tale percorso nel file di *pom.xml* . Questa impostazione fa in modo che il server sia in ascolto sulla porta TCP 80.

   ```xml
   <plugins>
     <plugin>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-maven-plugin</artifactId>
     </plugin>
     <plugin>
       <groupId>com.microsoft.azure</groupId>
       <artifactId>azure-webapp-maven-plugin</artifactId>
       <version>1.12.0</version>
       <configuration>
         <schemaVersion>V2</schemaVersion>
         <subscriptionId>YOUR_SUBSCRIPTION_ID</subscriptionId>
         <resourceGroup>YOUR_RESOURCE_GROUP_NAME</resourceGroup>
         <appName>YOUR_APP_NAME</appName>
         <pricingTier>P1v2</pricingTier>
         <region>YOUR_REGION</region>
         <runtime>
           <os>linux</os>
           <javaVersion>java 11</javaVersion>
           <webContainer>Java SE</webContainer>
         </runtime>
         <!-- start of APP_SETTINGS -->
         <appSettings>
           <property>
             <name>JAVA_OPTS</name>
             <value>-Dserver.port=80</value>
           </property>
         </appSettings>
         <!-- end of APP_SETTINGS -->
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
1. Usare il comando seguente per distribuire l'app nel servizio app Azure.

   ```bash
   mvn -DskipTests clean package azure-webapp:deploy
   ```

   Questo comando può richiedere alcuni minuti, a seconda di molti fattori oltre il controllo. Quando viene visualizzato un output simile all'esempio seguente, si sa che l'app è stata distribuita correttamente.

   ```output
   [INFO] Deploying the zip package contosokeyvault-22b7c1a3-b41b-4082-a9f0-9339723fa36a11893059035499017844.zip...
   [INFO] Successfully deployed the artifact to https://contosokeyvault.azurewebsites.net
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  01:45 min
   [INFO] Finished at: 2020-08-16T22:47:48-04:00
   [INFO] ------------------------------------------------------------------------
   ```

1. Attendere da tre a cinque minuti per consentire il completamento della distribuzione. Quindi, è possibile accedere alla distribuzione con un `curl` comando simile a quello illustrato in precedenza, ma questa volta usando il nome host visualizzato nell' `BUILD SUCCESS` output. Nell'esempio seguente viene usato `contosokeyvault` come illustrato nell'output precedente.

   ```bash
   curl https://contosokeyvault.azurewebsites.net/get
   ```

   L'output seguente indica l'esito positivo.

   ```output
   jdbc:sqlserver://SERVER.database.windows.net:1433;database=DATABASE;
   ```

La distribuzione dell'app in Servizio app di Azure è stata completata.

## <a name="redeploy-to-azure-app-service-and-use-managed-identities-for-azure-resources"></a>Ridistribuire il servizio app Azure e usare le identità gestite per le risorse di Azure

Questa sezione descrive come associare un'identità alla risorsa di Azure per l'app. Questa associazione è necessaria in modo che Azure possa applicare la sicurezza e tenere traccia dell'accesso.

Uno dei principi fondamentali di cloud computing consiste nel pagare solo le risorse usate. Questo rilevamento delle risorse con granularità fine è possibile solo se ogni risorsa è associata a un'identità. App Azure servizio e Azure Key Vault sono due dei molti servizi di Azure che sfruttano le identità gestite per le risorse di Azure. Per altre informazioni su questa importante tecnologia, vedere [che cosa sono le identità gestite per le risorse di Azure?](/azure/active-directory/managed-identities-azure-resources/overview)

> [!NOTE]
> "Identità gestite per le risorse di Azure" è il nuovo nome del servizio precedentemente noto come identità del servizio gestita (MSI).

Usare la procedura seguente per creare l'identità gestita per l'app di servizio app Azure e quindi consentire a tale identità di accedere al Key Vault.

1. Creare un'identità gestita per l'app del servizio app. Sostituire i `<your resource group name>` `<your app name>` segnaposto e con i valori degli `<resourceGroup>` elementi e del `<appName>` file di *pom.xml* .

   ```azurecli
   az webapp identity assign --resource-group <your resource group name> --name <your app name>
   ```

   L'output sarà simile all'esempio seguente. Annotare il valore di `principalId` per il passaggio successivo.

   ```json
   {
     "principalId": "2r7s6r00-92o9-4sq7-n10r-popq294ssr8s",
     "tenantId": "72s988os-86s1-41ns-91no-2q7pq011qo47",
     "type": "SystemAssigned",
     "userAssignedIdentities": null
   }
   ```

1. Modificare le *proprietà dell'applicazione* in modo che denominano l'identità gestita per le risorse di Azure create nel passaggio precedente.

   1. Rimuovere `azure.keyvault.client-key`.
   1. Aggiornare l'oggetto `azure.keyvault.client-id` in modo da avere il valore di nel `principalId` passaggio precedente. Il file completato dovrebbe ora essere simile all'esempio seguente.

   ```properties
   azure.keyvault.client-id=56rqs994-0o66-43o3-9roo-8e3534d0cb23
   azure.keyvault.enabled=true
   azure.keyvault.tenant-id=72s988os-86s1-41ns-91ab-2q7pq011qo47
   azure.keyvault.uri=https://contosokv.vault.azure.net/
   ```

1. Configurare la Key Vault per consentire `get` le `list` operazioni e dall'identità gestita. Il valore di `object-id` è `principalId` dell'output precedente.

   ```azurecli
   az keyvault set-policy \
       --name <your Key Vault name> \
       --object-id <your principal ID> \
       --secret-permissions get list
   ```

   L'output sarà un oggetto JSON completo di informazioni su Key Vault. Sarà presente una voce `type` con valore `Microsoft.KeyVault/vaults`.

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

1. È consigliabile attendere alcuni minuti per consentire il totale completamento della distribuzione. È quindi possibile contattare la distribuzione con un `curl` comando simile a quello illustrato in precedenza, ma questa volta usando il nome host visualizzato nell' `BUILD SUCCESS` output. Nell'esempio seguente viene usato `contosokeyvault` come illustrato nell' `BUILD SUCCESS` output della sezione precedente.

   ```bash
   curl https://contosokeyvault.azurewebsites.net/get
   ```

   L'output seguente indica l'esito positivo.

   ```output
   jdbc:sqlserver://SERVER.database.windows.net:1433;database=DATABASE;
   ```

Anziché restituire `defaultValue` , l'app viene recuperata `connectionString` dal Key Vault.

## <a name="deploy-to-azure-spring-cloud"></a>Distribuire in Azure Spring Cloud

In questa sezione l'app verrà distribuita nel cloud Spring di Azure.

Azure Spring Cloud è una piattaforma completamente gestita per la distribuzione e l'esecuzione delle applicazioni Spring Boot in Azure. Per una panoramica di Azure Spring cloud, vedere [che cos'è Azure Spring cloud?](/azure/spring-cloud/spring-cloud-overview)

Questa sezione userà l'app Spring boot e Key Vault creata in precedenza con una nuova istanza di Azure Spring cloud.

I passaggi seguenti illustrano come creare una risorsa di Azure Spring Cloud e distribuirvi l'app. Assicurarsi di aver installato l'estensione dell'interfaccia della riga di comando di Azure per il cloud Spring di Azure, come illustrato nei [prerequisiti](#prerequisites).

1. Decidere un nome per l'istanza del servizio. Per usare Azure Spring Cloud nella sottoscrizione di Azure, è necessario creare una risorsa di Azure di tipo Azure Spring Cloud. Come per tutte le altre risorse di Azure, l'istanza del servizio deve rimanere all'interno di un gruppo di risorse. Usare il gruppo di risorse già creato per ospitare l'istanza del servizio e scegliere un nome per l'istanza di Azure Spring cloud. Creare l'istanza del servizio con il comando seguente.

   ```bash
   az spring-cloud create --resource-group <your resource group name> --name <your Azure Spring Cloud instance name>
   ```

   Il completamento di questo comando richiede diversi minuti.

1. Creare un'app Spring Cloud all'interno del servizio.

   ```bash
   az spring-cloud app create \
       --resource-group <your resource group name> \
       --service <your Azure Spring Cloud instance name> \
       --name <your app name> \
       --assign-identity \
       --is-public true \
       --runtime-version Java_11 \
   ```

   Questa tabella illustra le opzioni riportate in precedenza.

   | Parametro | Descrizione |
   |---|---|
   | resource-group | Nome del gruppo di risorse in cui è stata creata l'istanza del servizio esistente. |
   | service | Nome del servizio esistente. |
   | name | nome dell'app. |
   | assign-identity | Fa in modo che il servizio crei un'identità per le identità gestite per le risorse di Azure. |
   | is-public | Assegna un nome di dominio DNS pubblico al servizio. |
   | runtime-version | Versione del runtime Java. Il valore deve corrispondere al valore scelto in Spring Initializr in precedenza. |

   Per comprendere la differenza tra *servizio* e *app*, vedere [Informazioni su app e distribuzione in Azure Spring Cloud](/azure/spring-cloud/spring-cloud-concept-understand-app-and-deployment).

1. Usare il comando seguente per ottenere l'identità gestita per la risorsa di Azure e usarla per configurare il Key Vault esistente per consentire l'accesso da questa app.

   ```bash
   SERVICE_IDENTITY=$(az spring-cloud app show --resource-group "contosorg" --name "contosoascsapp" --service "contososvc" | jq -r '.identity.principalId')
   az keyvault set-policy \
       --name <your Key Vault name> \
       --object-id <the value of the environment variable SERVICE_IDENTITY> \
       --secret-permissions set get list
   ```

1. Poiché l'app Spring Boot esistente contiene già un file *application.properties* con la configurazione necessaria, è possibile distribuire questa app direttamente in Spring Cloud usando il comando seguente. Eseguire il comando nella directory contenente il file POM.

   ```bash
   az spring-cloud app deploy \
       --resource-group <your resource group name> \
       --name <your Spring Cloud app name> \
       --jar-path target/keyvault-0.0.1-SNAPSHOT.jar \
       --service <your Azure Spring Cloud instance name>
   ```

   Questo comando crea una *distribuzione* all'interno dell'app all'interno del servizio. Per altri dettagli sui concetti relativi a istanze del servizio, app e distribuzioni, vedere [Informazioni su app e distribuzione in Azure Spring Cloud](/azure/spring-cloud/spring-cloud-concept-understand-app-and-deployment).

   Se la distribuzione non riesce, configurare i log per la risoluzione dei problemi, come descritto in [Configurare i log dell'applicazione](https://aka.ms/azure-spring-cloud-configure-logs). È probabile che i log contengano informazioni utili per diagnosticare e risolvere il problema.

1. Dopo aver distribuito correttamente l'app, è possibile usare `curl` per verificare che l'integrazione di Key Vault funzioni. Poiché è stato specificato `--is-public`, l'URL predefinito per il servizio è `https://<your Azure Spring Cloud instance name>-<your app name>.azuremicroservices.io/`. Il comando seguente mostra un esempio in cui il nome dell'istanza del servizio è `contososvc` e il nome dell'app è `contosoascsapp` . L'URL aggiunge il valore dell' `@GetMapping` annotazione.

   ```bash
   curl https://contososvc-contosoascsapp.azuremicroservices.io/get
   ```

   L'output mostrerà `jdbc:sqlserver://SERVER.database.windows.net:1433;database=DATABASE`.

## <a name="summary"></a>Riepilogo

In questa esercitazione è stata creata una nuova applicazione Web Java con Spring Initializr. È stata creata un'istanza di Azure Key Vault per l'archiviazione di informazioni sensibili e quindi si è configurata l'applicazione per recuperare le informazioni dall'istanza di Key Vault. Dopo aver eseguito il test in locale, l'app è stata distribuita nel Servizio app di Azure e in Azure Spring Cloud.

## <a name="clean-up-resources"></a>Pulire le risorse

Dopo aver terminato le operazioni relative alle risorse di Azure create in questa esercitazione, è possibile eliminarle usando il comando seguente:

```azurecli
az group delete --name <your resource group name>
```

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni su Spring e Azure, passare all'articolo successivo nel centro di documentazione di Spring in Azure.

> [!div class="nextstepaction"]
> [Come usare l'utilità di avvio Spring Boot per il JMS del bus di servizio di Azure](configure-spring-boot-starter-java-app-with-azure-service-bus.md)
