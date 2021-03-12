---
title: 'Esercitazione: proteggere le app Spring boot con Azure Key Vault certificati'
description: In questa esercitazione si proteggeranno le app Spring boot (incluse le risorse di Azure Spring cloud) con certificati TLS/SSL usando Azure Key Vault e identità gestite per le risorse di Azure.
ms.date: 03/11/2021
ms.service: key-vault
ms.topic: tutorial
ms.custom: devx-track-java, devx-track-azurecli
ms.author: edburns
author: edburns
ms.openlocfilehash: 57fea456490ed53e76984524744a5d54b6deb31e
ms.sourcegitcommit: 3536f174735cd3bb7da7e4b266fbf43349a22b67
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/12/2021
ms.locfileid: "103196615"
---
# <a name="tutorial-secure-spring-boot-apps-using-azure-key-vault-certificates"></a>Esercitazione: proteggere le app Spring boot con Azure Key Vault certificati

Questa esercitazione illustra come proteggere le app Spring boot (incluse le risorse di Azure Spring cloud) con certificati TLS/SSL usando Azure Key Vault e identità gestite per le risorse di Azure.

Le applicazioni Spring boot di livello produzione, sia nel cloud che in locale, richiedono la crittografia end-to-end per il traffico di rete usando protocolli TLS standard. La maggior parte dei certificati TLS/SSL disponibili è individuabile da un'autorità di certificazione radice pubblica (CA). In alcuni casi, tuttavia, questa individuazione non è possibile. Quando i certificati non sono individuabili, l'app deve disporre di un modo per caricare tali certificati, presentarli alle connessioni di rete in ingresso e accettarli dalle connessioni di rete in uscita.

Le app Spring boot abilitano in genere TLS installando i certificati. I certificati vengono installati nell'archivio chiavi locale del JVM che esegue l'app Spring boot. Con Spring in Azure, i certificati non vengono installati localmente. L'integrazione di Spring per Microsoft Azure fornisce invece un metodo sicuro e senza attrito per abilitare TLS con supporto da Azure Key Vault e identità gestita per le risorse di Azure.

<!-- https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/spring/azure-spring-doc-resource/spring-to-azure-keyvault-certificates.ai -->
:::image type="content" source="media/configure-spring-boot-starter-java-app-with-azure-key-vault-certificates/spring-to-azure-key-vault-certificates.svg" alt-text="Diagramma che illustra l'interazione degli elementi in questa esercitazione." border="false":::

In questa esercitazione verranno illustrate le procedure per:

> [!div class="checklist"]
> * Creare una VM GNU/Linux con identità gestita assegnata dal sistema
> * Creare un Azure Key Vault
> * Creare un certificato TLS/SSL autofirmato
> * Archiviare il certificato TLS/SSL autofirmato nel Azure Key Vault
> * Eseguire un'applicazione Spring boot in cui il certificato TLS/SSL per le connessioni in ingresso deriva da Azure Key Vault
> * Eseguire un'applicazione Spring boot in cui il certificato TLS/SSL per le connessioni in uscita deriva da Azure Key Vault

## <a name="prerequisites"></a>Prerequisiti

- [!INCLUDE [free subscription](includes/quickstarts-free-trial-note.md)]
[!INCLUDE [curl](includes/prerequisites-curl.md)]
[!INCLUDE [jq](includes/prerequisites-jq.md)]
[!INCLUDE [Azure CLI](includes/prerequisites-azure-cli.md)]

- Un Java Development Kit (JDK) supportato, versione 8. Per altre informazioni, vedere [supporto a lungo termine Java e supporto a medio termine in Azure e Azure stack](../fundamentals/java-jdk-long-term-support.md).

- [Apache Maven](http://maven.apache.org/), versione 3,0.

## <a name="create-a-gnulinux-vm-with-system-assigned-managed-identity"></a>Creare una VM GNU/Linux con identità gestita assegnata dal sistema

Usare la procedura seguente per creare una macchina virtuale di Azure con un'identità gestita assegnata dal sistema e prepararla per l'esecuzione dell'applicazione Spring boot. Per una panoramica delle identità gestite per le risorse di Azure, vedere informazioni sulle [identità gestite per le risorse di Azure](/azure/active-directory/managed-identities-azure-resources/overview).

1. Aprire una shell bash.

1. Disconnettersi ed eliminare alcuni file di autenticazione per rimuovere eventuali credenziali persistenti.

   ```azurecli
   az logout
   rm ~/.azure/accessTokens.json
   rm ~/.azure/azureProfile.json
   ```

1. Accedere all'interfaccia della riga di comando di Azure.

   ```azurecli
   az login
   ```

1. Impostare l'ID sottoscrizione. Assicurarsi di sostituire il segnaposto con il valore appropriato.

   ```azurecli
   az account set -s <your subscription ID>
   ```

1. Creare un gruppo di risorse di Azure. Prendere nota del nome del gruppo di risorse per usarlo in seguito.

   ```azurecli
   az group create \
       --name <your resource group name> \
       --location <your resource group region>
   ```

1. Ottenere l'URN (Uniform Resource Name) per la VM che si vuole creare. Questo esempio usa l'immagine di VM certificate Azul Zulu for Azure-Enterprise Edition. Per informazioni complete su Azul Zulu per Azure, vedere [scaricare Java per Azure](https://www.azul.com/downloads/azure-only/zulu/).

   ```azurecli
   az vm image list \
       --offer Zulu \
       --location <your region> \
       --all | grep urn
   ```

   Il completamento di questo comando può richiedere alcuni minuti. Quando il comando viene completato, genera un output simile alle righe seguenti. Selezionare il valore per JDK 11 in Ubuntu.

   ```output
   "urn": "azul:azul-zulu11-ubuntu-2004:zulu-jdk11-ubtu2004:20.11.0",
   ...
   "urn": "azul:azul-zulu8-ubuntu-2004:zulu-jdk8-ubtu2004:20.11.0",
   "urn": "azul:azul-zulu8-windows-2019:azul-zulu8-windows2019:20.11.0",
   ```

1. Accettare i termini dell'immagine per consentire la creazione della macchina virtuale.

   ```azurecli
   az vm image terms accept --urn azul:azul-zulu11-ubuntu-2004:zulu-jdk11-ubtu2004:20.11.0
   ```

1. Creare l'istanza di macchina virtuale con identità gestita assegnata dal sistema abilitata, assegnando il ruolo di **proprietario** nell'ambito del gruppo di risorse.

   ```azurecli
   az vm create \
       --resource-group <your resource group name> \
       --name <your VM name> \
       --debug \
       --generate-ssh-keys \
       --assign-identity \
       --image azul:azul-zulu11-ubuntu-2004:zulu-jdk11-ubtu2004:20.11.0 \
       --scope "/subscriptions/<your subscription ID>/resourcegroups/<your resource group name>" \
       --admin-username azureuser \
       --role owner
   ```

   Nell'output JSON prendere nota del valore delle `publicIpAddress` `systemAssignedIdentity` proprietà e. Questi valori verranno usati più avanti nell'esercitazione.

## <a name="create-and-configure-an-azure-key-vault"></a>Creare e configurare un Azure Key Vault

Usare la procedura seguente per creare un Azure Key Vault e per concedere l'autorizzazione per l'identità gestita assegnata dal sistema della macchina virtuale per accedere al Key Vault per i certificati.

1. Creare un Azure Key Vault all'interno del gruppo di risorse.

   ```azurecli
   az keyvault create \
       --resource-group <your resource group name> \
       --name <your Key Vault name> \
       --location <your resource group region>
   export KEY_VAULT_URI=$(az keyvault show --name ${KEY_VAULT} | jq -r '.properties.vaultUri')
   ```

   Prendere nota del `KEY_VAULT_URI` valore. Verrà usato più avanti.

1. Concedere alla macchina virtuale l'autorizzazione per usare la Key Vault per i certificati.

   ```azurecli
   az keyvault set-policy \
       --name <your Key Vault name> \
       --object-id <your system-assigned identity> \
       --secret-permissions get list \
       --certificate-permissions get list import
   ```

## <a name="create-and-store-a-self-signed-tlsssl-certificate"></a>Creare e archiviare un certificato TLS/SSL autofirmato

I passaggi di questa esercitazione si applicano a qualsiasi certificato TLS/SSL (incluso autofirmato) archiviato direttamente in Azure Key Vault. I certificati autofirmati non sono adatti per l'uso nell'ambiente di produzione, ma sono utili per le applicazioni di sviluppo e test. Questa esercitazione usa un certificato autofirmato. Per creare il certificato, usare il comando seguente.

```azurecli
az keyvault certificate create \
    --vault-name <your Key Vault name> \
    --name mycert \
    --policy "$(az keyvault certificate get-default-policy)"
```

## <a name="run-a-spring-boot-application-with-secure-inbound-connections"></a>Eseguire un'applicazione Spring boot con connessioni in ingresso sicure

In questa sezione verrà creata un'applicazione Spring boot Starter in cui il certificato TLS/SSL per le connessioni in ingresso deriva da Azure Key Vault.

Per creare l'applicazione, attenersi alla procedura seguente:

1. Passare a <https://start.spring.io/>.
1. Selezionare le opzioni come illustrato nell'immagine riportata dopo questo elenco.
   * **Progetto**: **progetto Maven**
   * **Lingua**: **Java**
   * **Spring boot**: **2.3.7**
   * **Gruppo**: *com. contoso* . è possibile inserire qui un nome di pacchetto Java valido.
   * **Artefatto**: *SSLTEST* (è possibile inserire qui qualsiasi nome di classe Java valido).
   * Creazione **pacchetto**: **jar**
   * **Java**: **11**
1. Selezionare **Add Dependencies...** (Aggiungi dipendenze).
1. Nel campo di testo digitare *Spring Web* e premere CTRL + INVIO.
1. Nel campo di testo digitare *supporto di Azure* e premere INVIO. Verrà visualizzata una schermata simile a quella riportata di seguito.
   :::image type="content" source="media/configure-spring-boot-starter-java-app-with-azure-key-vault-certificates/spring-initializr-choices.png" alt-text="Spring Initializr con le opzioni corrette selezionate.":::
1. Nella parte inferiore della pagina selezionare **Generate** (Genera).
1. Quando richiesto, scaricare il progetto in un percorso nel computer locale. Questa esercitazione usa una directory *SSLTEST* nella home directory dell'utente corrente. I valori precedenti forniranno un file di *ssltest.zip* in tale directory.

### <a name="enable-the-spring-boot-app-to-load-the-tlsssl-certificate"></a>Abilitare l'app Spring boot per caricare il certificato TLS/SSL

Per consentire all'app di caricare il certificato, seguire questa procedura:

1. Decomprimere il file di *ssltest.zip* .

1. Rimuovere la directory di *test* e le relative sottodirectory. Questa esercitazione ignora il test, quindi è possibile eliminare la directory in modo sicuro.

1. Il layout del file sarà simile a quello riportato di seguito.

   ```
   ├── HELP.md
   ├── mvnw
   ├── mvnw.cmd
   ├── pom.xml
   └── src
       └── main
           ├── java
           │   └── com
           │       └── contoso
           │           └── ssltest
           │               └── SsltestApplication.java
           └── resources
               ├── application.properties
               ├── static
               └── templates
   ```

1. Modificare il POM per aggiungere una dipendenza `azure-spring-boot-starter-keyvault-certificates` . Aggiungere il codice seguente alla `<dependencies>` sezione del file di *pom.xml* .

   ```xml
   <dependency>
      <groupId>com.azure.spring</groupId>
      <artifactId>azure-spring-boot-starter-keyvault-certificates</artifactId>
      <version>3.0.0-beta.2</version>
   </dependency>
   ```

1. Modificare il file *src/main/Resources/Application. Properties* in modo che includa il contenuto seguente.

   ```properties
   server.port=8443
   server.ssl.key-alias=mycert
   server.ssl.key-store-type=AzureKeyVault
   server.ssl.trust-store-type=AzureKeyVault
   azure.keyvault.uri=https://<your Key Vault name>.vault.azure.net/
   ```

   Questi valori consentono all'app Spring boot di eseguire l'azione di *caricamento* per il certificato TLS/SSL, come indicato all'inizio dell'esercitazione. Nella tabella seguente vengono descritti i valori delle proprietà.

   | Nome della proprietà | Spiegazione |
   |---------------|-------------|
   |Server. Port|Porta TCP locale su cui restare in ascolto delle connessioni HTTPS.|
   |Server. SSL. Key-alias|Valore dell' `--name` argomento passato a `az keyvault certificate create` .|
   |Server. SSL. Key-Store-Type|Deve essere `AzureKeyVault`.|
   |Server. SSL. trust-Store-Type|Deve essere `AzureKeyVault`.|
   |azure.keyvault.uri|`vaultUri`Proprietà nell'oggetto restituito JSON da `az keyvault create` . Questo valore è stato salvato in una variabile di ambiente.|

   L'unica proprietà specifica di Key Vault è `azure.keyvault.uri` . L'app è in esecuzione in una VM la cui identità gestita assegnata dal sistema dispone dell'autorizzazione di accesso al Key Vault. Quindi, all'app è stato concesso anche l'accesso.

Queste modifiche consentono all'app Spring boot di caricare il certificato TLS/SSL. Nella sezione successiva si consentirà all'app di eseguire l'azione di *accettazione* per il certificato TLS/SSL, come indicato all'inizio dell'esercitazione.

### <a name="create-a-spring-boot-rest-controller"></a>Creare un controller REST di Spring boot

Per creare il controller REST, attenersi alla procedura seguente:

1. Modificare il file *src/main/Java/com/contoso/SSLTEST/SsltestApplication. Java* in modo che includa il contenuto seguente.

   ```java
   package com.contoso.ssltest;

   import org.springframework.boot.autoconfigure.SpringBootApplication;
   import org.springframework.web.bind.annotation.GetMapping;
   import org.springframework.web.bind.annotation.RestController;

   @SpringBootApplication
   @RestController
   public class SsltestApplication {

       public static void main(String[] args) {
           SpringApplication.run(SsltestApplication.class, args);
       }

       @GetMapping(value = "/ssl-test")
       public String inbound(){
           return "Inbound TLS is working!!";
       }

       @GetMapping(value = "/exit")
       public void exit() {
           System.exit(0);
       }

   }
   ```

   Si noti che la chiamata `System.exit(0)` dall'interno di una chiamata Get REST non autenticata è solo a scopo dimostrativo. Non eseguire questa operazione in un'applicazione reale.

   Questo codice illustra la *presente* azione citata all'inizio di questa esercitazione. Nell'elenco seguente vengono evidenziati alcuni dettagli su questo codice:

   * È ora presente un' `@RestController` annotazione sulla `SsltestApplication` classe generata da Spring Initializr.
   * È disponibile un metodo con annotazioni con `@GetMapping` un oggetto `value` per la chiamata http che verrà fatta.
   * Il `inbound` metodo restituisce semplicemente un messaggio di saluto quando un browser esegue una richiesta HTTPS al `/ssl-test` percorso. Il `inbound` metodo illustra il modo in cui il server presenta il certificato TLS/SSL al browser.
   * Il `exit` metodo provocherà la chiusura di JVM quando viene richiamato. Questo metodo consente di semplificare l'esecuzione dell'esempio nel contesto di questa esercitazione.

1. Aprire una nuova shell bash e passare alla directory *SSLTEST* Eseguire il comando seguente.

   ```bash
   mvn clean package
   ```

   Maven compila il codice e lo inserisce in un file JAR eseguibile

1. Verificare che il gruppo di sicurezza di rete creato in `<your resource group name>` consenta il traffico in ingresso sulle porte 22 e 8443 dall'indirizzo IP. Per informazioni sulla configurazione delle regole del gruppo di sicurezza di rete per consentire il traffico in ingresso, vedere la sezione [lavorare con le regole di sicurezza](/azure/virtual-network/manage-network-security-group#work-with-security-rules) in [creare, modificare o eliminare un gruppo di sicurezza di rete](/azure/virtual-network/manage-network-security-group).

1. Inserire il file JAR eseguibile nella macchina virtuale.

   ```bash
   cd target
   sftp azureuser@<your VM public IP address>
   put *.jar
   ```

### <a name="run-the-app-on-the-server"></a>Eseguire l'app nel server

Ora che l'app Spring boot è stata compilata e caricata nella macchina virtuale, usare la procedura seguente per eseguirla nella macchina virtuale e chiamare l'endpoint REST con curl.

1. Usare SSH per connettersi alla VM, quindi eseguire il file jar eseguibile.

   ```bash
   set -o noglob
   ssh azureuser@<your VM public IP address> "java -jar *.jar"
   ```

1. Aprire una nuova shell bash ed eseguire il comando seguente per verificare che il server presenti il certificato TLS/SSL.

   ```bash
   curl https://<your VM public IP address>:8443/ssl-test
   ```

   Verrà visualizzato un messaggio sulla legittimità del server non riuscita. Poiché il certificato è autofirmato, è previsto il messaggio. Aggiungere l' `--insecure` opzione al `curl` comando per visualizzare il messaggio `Inbound TLS is working!!` .

1. Richiamare il `exit` percorso per terminare il server e chiudere i socket di rete.

   ```bash
   curl https://<your VM public IP address>:8443/exit
   ```

Ora che sono state visualizzate le azioni *Load* e *present* con un certificato TLS/SSL autofirmato, verranno apportate alcune modifiche semplici all'app per vedere anche l'azione *Accept* .

## <a name="run-a-spring-boot-application-with-secure-outbound-connections"></a>Eseguire un'applicazione Spring boot con connessioni in uscita sicure

In questa sezione si modificherà il codice nella sezione precedente in modo che il certificato TLS/SSL per le connessioni in uscita provenga da Azure Key Vault. Pertanto, le azioni *Load*, *present* e *accept* sono soddisfatte dal Azure Key Vault.

### <a name="modify-the-ssltestapplication-to-illustrate-outbound-tls-connections"></a>Modificare il SsltestApplication per illustrare le connessioni TLS in uscita

In primo luogo, si aggiunge un nuovo endpoint REST denominato `ssl-test-outbound` . Questo endpoint apre un Socket TLS a se stesso e verifica che la connessione TLS accetti il certificato TLS/SSL.

Sostituire il contenuto di *SsltestApplication. Java* con il codice seguente.

```java
   package com.contoso.ssltest;


   import java.security.GeneralSecurityException;
   import java.security.KeyStore;
   import javax.net.ssl.HostnameVerifier;
   import javax.net.ssl.SSLContext;
   import javax.net.ssl.SSLSession;

   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   import org.springframework.http.HttpStatus;
   import org.springframework.http.ResponseEntity;
   import org.springframework.http.client.HttpComponentsClientHttpRequestFactory;
   import org.springframework.web.bind.annotation.GetMapping;
   import org.springframework.web.bind.annotation.RestController;
   import org.springframework.web.client.RestTemplate;

   import org.apache.http.conn.ssl.SSLConnectionSocketFactory;
   import org.apache.http.conn.ssl.TrustSelfSignedStrategy;
   import org.apache.http.impl.client.CloseableHttpClient;
   import org.apache.http.impl.client.HttpClients;
   import org.apache.http.ssl.SSLContexts;

   @SpringBootApplication
   @RestController
   public class SsltestApplication {

       public static void main(String[] args) {
           SpringApplication.run(SsltestApplication.class, args);
       }

       @GetMapping(value = "/ssl-test")
       public String inbound(){
           return "Inbound TLS is working!!";
       }

       @GetMapping(value = "/ssl-test-outbound")
       public String outbound() throws GeneralSecurityException {
           KeyStore ks = KeyStore.getInstance(KeyStore.getDefaultType());
           SSLContext sslContext = SSLContexts.custom()
               .loadTrustMaterial(ks, new TrustSelfSignedStrategy())
               .build();

       HostnameVerifier allowAll = (String hostName, SSLSession session) -> true;
       SSLConnectionSocketFactory csf = new SSLConnectionSocketFactory(sslContext, allowAll);

       CloseableHttpClient httpClient = HttpClients.custom()
           .setSSLSocketFactory(csf)
           .build();

       HttpComponentsClientHttpRequestFactory requestFactory =
           new HttpComponentsClientHttpRequestFactory();

       requestFactory.setHttpClient(httpClient);
       RestTemplate restTemplate = new RestTemplate(requestFactory);
       String sslTest = "https://localhost:8443/ssl-test";

       ResponseEntity<String> response
           = restTemplate.getForEntity(sslTest, String.class);

       return "Outbound TLS " +
           (response.getStatusCode() == HttpStatus.OK ? "is" : "is not")  + " Working!!";
       }

       @GetMapping(value = "/exit")
       public void exit() {
           System.exit(0);
       }

   }
```

Successivamente, ricompilare l'app, caricarla nella macchina virtuale ed eseguirla di nuovo.

1. Aggiungere la dipendenza da Apache HTTP client aggiungendo il codice seguente alla `<dependencies>` sezione del file di *pom.xml* .

   ```xml
   <dependency>
      <groupId>org.apache.httpcomponents</groupId>
      <artifactId>httpclient</artifactId>
      <version>4.5.13</version>
   </dependency>
   ```

1. Compilare l'app.

   ```bash
   cd ssltest
   mvn clean package
   ```

1. Caricare di nuovo l'app usando lo stesso `sftp` comando riportato in precedenza in questo articolo.

   ```bash
   cd target
   sftp <your VM public IP address>
   put *.jar
   ```

1. Eseguire l'app nella macchina virtuale.

   ```bash
   set -o noglob
   ssh azureuser@<your VM public IP address> "java -jar *.jar"
   ```

1. Quando il server è in esecuzione, verificare che il server accetti il certificato TLS/SSL. Nella stessa shell bash in cui è stato emesso il `curl` comando precedente, eseguire il comando seguente.

   ```bash
   curl --insecure https://<your VM public IP address>:8443/ssl-test-outbound
   ```

   Dovrebbe essere visualizzato il messaggio `Outbound TLS is working!!`.

1. Richiamare il `exit` percorso per terminare il server e chiudere i socket di rete.

   ```bash
   curl https://<your VM public IP address>:8443/exit
   ```

A questo punto è stata osservata una semplice illustrazione delle azioni *Load*, *present* e *Accept* con un certificato TLS/SSL autofirmato archiviato in Azure Key Vault.

## <a name="clean-up-resources"></a>Pulire le risorse

Dopo aver terminato le operazioni relative alle risorse di Azure create in questa esercitazione, è possibile eliminarle usando il comando seguente:

```azurecli
az group delete --name <your resource group name>
```

## <a name="next-steps"></a>Passaggi successivi

Scopri le altre cose che puoi fare con Spring e Azure.

> [!div class="nextstepaction"]
> [Altri avvii di Spring boot](spring-boot-starters-for-azure.md)
