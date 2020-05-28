---
title: Come usare l'utilità di avvio Spring Boot per Azure Key Vault
description: Informazioni su come configurare un'app Spring Boot Initializer con l'utilità di avvio Azure Key Vault.
services: key-vault
documentationcenter: java
ms.date: 10/29/2019
ms.service: key-vault
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: identity
ms.openlocfilehash: a43b951fddfea01c4678fca3174f15a3b3d714f9
ms.sourcegitcommit: 9330d5af796b4b114466bbe75b8e18a9206f218e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/26/2020
ms.locfileid: "83862814"
---
# <a name="how-to-use-the-spring-boot-starter-for-azure-key-vault"></a>Come usare l'utilità di avvio Spring Boot per Azure Key Vault

Questo articolo illustra la creazione di un'app con **[Spring Initializr]** , che usa l'utilità di avvio Spring Boot per Azure Key Vault per recuperare una stringa di connessione archiviata come segreto in un insieme di credenziali delle chiavi.

## <a name="prerequisites"></a>Prerequisiti

I prerequisiti seguenti sono necessari per completare le procedure disponibili in questo articolo:

* Sottoscrizione di Azure; se non si ha una sottoscrizione di Azure, è possibile attivare i [vantaggi per i sottoscrittori di MSDN] oppure iscriversi per ottenere un [account Azure gratuito].
* Java Development Kit (JDK) supportato. Per altre informazioni sulle versioni di JDK utilizzabili per lo sviluppo in Azure, vedere <https://aka.ms/azure-jdks>.
* [Apache Maven](http://maven.apache.org/), versione 3.0 o versione successiva.

## <a name="create-an-app-using-spring-initializr"></a>Creare un'app con Spring Initializr

La procedura seguente crea l'applicazione con Spring Initializr.

1. Passare a <https://start.spring.io/>.

1. Specificare di voler generare un progetto **Maven** con **Java**.  

1. Immettere i nomi di **Group** (Gruppo) e **Artifact** (Artefatto) per l'applicazione.

1. Nella sezione **Dependencies** (Dipendenze) immettere **Azure Key Vault**.

1. Scorrere alla fine della pagina e fare clic su **Generate** (Genera).

   ![Generare il progetto Spring Boot][secrets-01]

1. Quando richiesto, scaricare il progetto in un percorso nel computer locale.

## <a name="sign-into-azure"></a>Accesso ad Azure

La procedura seguente autentica l'utente nell'interfaccia dell'interfaccia della riga di comando di Azure.

1. Aprire un prompt dei comandi.

1. Accedere all'account Azure con l'interfaccia della riga di comando di Azure:

   ```azurecli
   az login
   ```

Seguire le istruzioni per completare il processo di accesso.

1. Elencare le sottoscrizioni:

   ```azurecli
   az account list
   ```

   Azure restituirà un elenco delle sottoscrizioni e sarà necessario copiare il GUID per la sottoscrizione che si vuole usare, ad esempio:

   ```json
   [
     {
       "cloudName": "AzureCloud",
       "id": "ssssssss-ssss-ssss-ssss-ssssssssssss",
       "isDefault": true,
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

1. Specificare il GUID per l'account che si vuole usare con Azure, ad esempio:

   ```azurecli
   az account set -s ssssssss-ssss-ssss-ssss-ssssssssssss
   ```

## <a name="create-a-new-azure-key-vault"></a>Creare una nuova istanza di Azure Key Vault

La procedura seguente crea e inizializza l'insieme di credenziali delle chiavi.

1. Creare un gruppo di risorse per le risorse di Azure che verranno usate per l'insieme di credenziali delle chiavi, ad esempio:

   ```azurecli
   az group create --name vged-rg2 --location westus
   ```

   Dove:

   | Parametro | Descrizione |
   |---|---|
   | `name` | Specifica un nome univoco per il gruppo di risorse. |
   | `location` | Specifica l'[area di Azure](https://azure.microsoft.com/regions/) in cui verrà ospitato il gruppo di risorse. |

   Nell'interfaccia della riga di comando di Azure verranno visualizzati i risultati della creazione del gruppo di risorse, ad esempio:  

   ```json
   {
     "id": "/subscriptions/ssssssss-ssss-ssss-ssss-ssssssssssss/resourceGroups/vged-rg2",
     "location": "westus",
     "managedBy": null,
     "name": "vged-rg2",
     "properties": {
       "provisioningState": "Succeeded"
     },
     "tags": null
   }
   ```

2. Creare un nuovo insieme di credenziali delle chiavi nel gruppo di risorse, ad esempio:

   ```azurecli
   az keyvault create --resource-group vged-rg2 \
       --name vgedkeyvault \
       --enabled-for-deployment true \
       --enabled-for-disk-encryption true \
       --enabled-for-template-deployment true \
       --sku standard --query properties.vaultUri \
       --location westus
   ```

   Dove:

   | Parametro | Descrizione |
   |---|---|
   | `name` | Specifica un nome univoco per l'insieme di credenziali delle chiavi. |
   | `enabled-for-deployment` | Specifica l'[opzione di distribuzione dell'insieme di credenziali delle chiavi](/cli/azure/keyvault). |
   | `enabled-for-disk-encryption` | Specifica l'[opzione di crittografia dell'insieme di credenziali delle chiavi](/cli/azure/keyvault). |
   | `enabled-for-template-deployment` | Specifica l'[opzione di crittografia dell'insieme di credenziali delle chiavi](/cli/azure/keyvault). |
   | `sku` | Specifica l'[opzione SKU dell'insieme di credenziali delle chiavi](/cli/azure/keyvault). |
   | `query` | Specifica un valore da recuperare dalla risposta, corrispondente all'URI dell'insieme di credenziali delle chiavi che sarà necessario per completare questa esercitazione. |
   | `location` | Specifica l'[area di Azure](https://azure.microsoft.com/regions/) in cui verrà ospitato il gruppo di risorse. |

   L'interfaccia della riga di comando di Azure visualizzerà l'URI per l'insieme di credenziali delle chiavi, che verrà usato in un secondo momento, ad esempio:  

   ```output
   "https://vgedkeyvault.vault.azure.net"
   ```

3. Archiviare un segreto nel nuovo insieme di credenziali delle chiavi, ad esempio:

   ```azurecli
   az keyvault secret set --vault-name "vgedkeyvault" \
       --name "connectionString" \
       --value "jdbc:sqlserver://SERVER.database.windows.net:1433;database=DATABASE;"
   ```

   Dove:

   | Parametro | Descrizione |
   |---|---|
   | `vault-name` | Specifica il nome dell'insieme di credenziali delle chiavi precedente. |
   | `name` | Specifica il nome del segreto. |
   | `value` | Specifica il valore del segreto. |

   Nell'interfaccia della riga di comando di Azure verranno visualizzati i risultati della creazione del segreto, ad esempio:  

   ```json
   {
     "attributes": {
       "created": "2017-12-01T09:00:16+00:00",
       "enabled": true,
       "expires": null,
       "notBefore": null,
       "recoveryLevel": "Purgeable",
       "updated": "2017-12-01T09:00:16+00:00"
     },
     "contentType": null,
     "id": "https://vgedkeyvault.vault.azure.net/secrets/connectionString/123456789abcdef123456789abcdef",
     "kid": null,
     "managed": null,
     "tags": {
       "file-encoding": "utf-8"
     },
     "value": "jdbc:sqlserver://.database.windows.net:1433;database=DATABASE;"
   }
   ```

## <a name="configure-and-compile-your-app"></a>Configurare e compilare l'app

Usare la procedura seguente per configurare e compilare l'applicazione.

1. Estrarre i file dai file di archivio del progetto Spring Boot scaricati prima in una directory.

2. Passare alla cartella *src/main/resources* nel progetto e aprire il file *application.properties* in un editor di testo.

3. Aggiungere i valori per l'insieme di credenziali delle chiavi usando i valori dei passaggi completati prima in questa esercitazione, ad esempio:

   ```yaml
   azure.keyvault.enabled=true
   azure.keyvault.uri=Your-Keyvault-uri
   azure.keyvault.client-id=Your-Client-ID
   azure.keyvault.tenant-id=Your-Tenant-ID
   ```

   Dove:

   |          Parametro          |                                 Descrizione                                 |
   |-----------------------------|-----------------------------------------------------------------------------|
   |    `azure.keyvault.uri`     |           Specifica l'URI di quando è stato creato l'insieme di credenziali delle chiavi.           |
    
    
4. Passare al file di codice sorgente principale del progetto, ad esempio: */src/main/java/com/vged/secrets*.

5. Aprire il file Java principale dell'applicazione in un file in un editor di testo, ad esempio *SecretsApplication.java*, e aggiungere le righe seguenti al file:

   ```java
   package com.vged.secrets;

   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   import org.springframework.beans.factory.annotation.Value;
   import org.springframework.boot.CommandLineRunner;
   import org.springframework.web.bind.annotation.GetMapping;
   import org.springframework.web.bind.annotation.RestController;
   
   @SpringBootApplication
   @RestController
   public class SecretsApplication implements CommandLineRunner {

      @Value("${connectionString}")
      private String connectionString;

      public static void main(String[] args) {
         SpringApplication.run(SecretsApplication.class, args);
      }
   
      @GetMapping("get")
      public String get() {
         return connectionString;
      }
   
      public void run(String... varl) throws Exception {
         System.out.println(String.format("\nConnection String stored in Azure Key Vault:\n%s\n",connectionString));
      }
   }
   ```
   Questo esempio di codice recupera la stringa di connessione dall'insieme di credenziali delle chiavi e lo visualizza all'URL `https://{your-appservice-name}.azurewebsites.net/get`.

6. Salvare e chiudere il file Java.

7. Disabilitare il test e compilare il file JAR usando Maven.
    
   ```bash
   mvn clean package -Dmaven.test.skip=true
   ```

## <a name="configure-maven-plugin-for-azure-app-service"></a>Configurare il plug-in Maven per il servizio app di Azure

Questa sezione consente di configurare il progetto Spring Boot per consentire la distribuzione dell'app nel servizio app di Azure.

1.  Seguire il collegamento all'articolo [Configurare il plug-in Maven per il servizio app di Azure].
    
    La procedura descritta consente di creare un nuovo servizio app di Azure. Se si vuole distribuire l'app in una esistente, è possibile riconfigurare la distribuzione tramite il comando `mvn azure-webapp:config` e scegliere parte dell'applicazione da configurare.
    
    ```bash
    [INFO] Scanning for projects...                                                     
    [INFO]                                                                              
    [INFO] ----------------------< com.wingtiptoys:secrets >-----------------------     
    [INFO] Building secrets 0.0.1-SNAPSHOT                                              
    [INFO] --------------------------------[ jar ]---------------------------------     
    [INFO]                                                                              
    [INFO] --- azure-webapp-maven-plugin:1.9.0:config (default-cli) @ secrets ---       
    Please choose which part to config                                                  
    1. Application                                                                      
    2. Runtime                                                                          
    3. DeploymentSlot                                                                   
    Enter index to use: 1                                                              
    Define value for appName(Default: ********):                                      
    Define value for resourceGroup(Default: ********):                                 
    Define value for region(Default: ********):                                           
    Define value for pricingTier(Default: P1v2):                                        
    1. b1                                                                               
    2. b2                                                                               
    3. b3                                                                               
    4. d1                                                                               
    5. f1                                                                               
    6. p1v2 [*]                                                                         
    7. p2v2                                                                             
    8. p3v2                                                                             
    9. s1                                                                               
    10. s2                                                                              
    11. s3                                                                              
    Enter index to use:                                                                 
    Please confirm webapp properties                                                                                                          
    ```
    
    È anche possibile modificare direttamente la sezione `<configuration>` di `<azure-webapp-maven-plugin>` nel file *pom.xml*. Modificare i valori di `<resourceGroup>`, `<appName>` e `<region>` per il servizio app specifico.

2. Assegnare l'identità al servizio app e prendere nota del valore di `principalId` per il passaggio successivo.

   ```bash
   az webapp identity assign --name your-appservice-name \
      --resource-group vged-rg2
   ```
   
3. Concedere l'autorizzazione a MSI.

   ```bash
   az keyvault set-policy --name vgedkeyvault \
       --object-id your-managed-identity-objectId \
       --secret-permissions get list
   ```

## <a name="deploy-the-app-to-azure-and-run-app-service"></a>Distribuire l'app in Azure ed eseguire il servizio app

A questo punto è possibile distribuire l'app Web in Azure. A tale scopo, seguire questa procedura:

1. Ricompilare il file JAR usando Maven se sono state apportate modifiche al file *pom.xml*.

   ```bash
   mvn clean package
   ```
   
2. Distribuire l'app in Azure usando Maven.

   ```bash
   mvn azure-webapp:deploy
   ```
   
3. Riavviare il servizio app.

4. Passare a questo URL nel browser `https://{your-appservice-name}.azurewebsites.net/get` per ottenere `connectionString`.
   

## <a name="summary"></a>Summary

In questa esercitazione si sono create una nuova applicazione Web Java con **[Spring Initializr]** e un'istanza di Azure Key Vault per l'archiviazione delle informazioni riservate e quindi si è configurata l'applicazione per il recupero delle informazioni dall'insieme di credenziali delle chiavi.

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni su Spring e Azure, passare al centro di documentazione di Spring in Azure.

> [!div class="nextstepaction"]
> [Spring in Azure](/azure/developer/java/spring-framework)

### <a name="additional-resources"></a>Risorse aggiuntive

Per altre informazioni sull'uso di Azure Key Vault, vedere gli articoli seguenti:

* [Documentazione su Key Vault].

* [Introduzione all'insieme di credenziali delle chiavi di Azure]

Per altre informazioni sull'uso delle applicazioni Spring Boot in Azure, vedere gli articoli seguenti:

* [Distribuire un'applicazione Spring Boot nel servizio app di Azure](deploy-spring-boot-java-app-from-container-registry-using-maven-plugin.md)

* [Eseguire un'applicazione Spring Boot in un cluster Kubernetes nel servizio Azure Container](deploy-spring-boot-java-app-on-kubernetes.md)

Per altre informazioni sull'uso di Azure con Java, vedere [Azure per sviluppatori Java] e la documentazione relativa all'[uso di Azure DevOps e Java].

Per altre informazioni sull'uso delle identità gestite per il servizio app, vedere [Uso delle identità gestite per il servizio app].

<!-- URL List -->

[Documentazione su Key Vault]: /azure/key-vault/
[Introduzione all'insieme di credenziali delle chiavi di Azure]: /azure/key-vault/key-vault-get-started
[Azure per sviluppatori Java]: /azure/developer/java/
[Account Azure gratuito]: https://azure.microsoft.com/pricing/free-trial/
[Uso di Azure DevOps e Java]: /azure/devops/
[vantaggi per i sottoscrittori di MSDN]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/
[Uso delle identità gestite per il servizio app]: /azure/app-service/overview-managed-identity
[Configurare il plug-in Maven per il servizio app di Azure]: /azure/developer/java/spring-framework/deploy-spring-boot-java-app-with-maven-plugin#configure-maven-plugin-for-azure-app-service

<!-- IMG List -->

[secrets-01]: media/configure-spring-boot-starter-java-app-with-azure-key-vault/secrets-01.png
[secrets-02]: media/configure-spring-boot-starter-java-app-with-azure-key-vault/secrets-02.png
[secrets-03]: media/configure-spring-boot-starter-java-app-with-azure-key-vault/secrets-03.png

[build-application-01]: media/configure-spring-boot-starter-java-app-with-azure-key-vault/build-application-01.png
[build-application-02]: media/configure-spring-boot-starter-java-app-with-azure-key-vault/build-application-02.png
