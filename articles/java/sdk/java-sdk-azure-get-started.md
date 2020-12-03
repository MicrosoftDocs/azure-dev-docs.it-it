---
title: Introduzione ad Azure SDK per Java
description: Informazioni su come creare risorse cloud di Azure e come connetterle e usarle nelle applicazioni Java.
keywords: Azure, Java, SDK, API, autenticare, introduzione
author: bmitchell287
ms.author: brendm
ms.date: 11/20/2020
ms.topic: article
ms.service: multiple
ms.assetid: b1e10b79-f75e-4605-aecd-eed64873e2d3
ms.custom: seo-java-august2019, devx-track-java, devx-track-azurecli
ms.openlocfilehash: c98e9194d7c9b391206104f249f1356351938d32
ms.sourcegitcommit: 709fa38a137b30184a7397e0bfa348822f3ea0a7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/01/2020
ms.locfileid: "96442287"
---
# <a name="get-started-with-cloud-development-using-java-on-azure"></a>Introduzione allo sviluppo per il cloud con Java in Azure

Questo articolo illustra come configurare un ambiente per lo sviluppo di Azure in Java. Verranno quindi create alcune risorse di Azure a cui ci si connetterà per eseguire alcune attività di base, come il caricamento di un file o la distribuzione di un'applicazione Web. Al termine sarà possibile iniziare a usare i servizi di Azure nelle proprie applicazioni Java.

[!INCLUDE [chrome-note](includes/chrome-note.md)]

## <a name="prerequisites"></a>Prerequisiti

- Un account Azure. Se non è disponibile, [ottenere una versione di valutazione gratuita](https://azure.microsoft.com/free/).
- [Azure Cloud Shell](/azure/cloud-shell/quickstart) o [interfaccia della riga di comando di Azure 2.0](/cli/azure/install-az-cli2).
- [Java 8](https://docs.microsoft.com/azure/developer/java/fundamentals/java-jdk-long-term-support), incluso in Azure Cloud Shell.
- [Maven 3](https://maven.apache.org/download.cgi), incluso in Azure Cloud Shell.

## <a name="set-up-authentication"></a>Configurare l'autenticazione

L'applicazione Java deve leggere e creare le autorizzazioni nella sottoscrizione di Azure per eseguire il codice di esempio in questa esercitazione. Creare un'entità servizio e configurare l'applicazione per l'esecuzione con le rispettive credenziali. Le entità servizio consentono di creare un account non interattivo associato all'identità a cui vengono concessi solo i privilegi necessari per l'esecuzione dell'app.

[Crea un'entità servizio usando l'interfaccia della riga di comando di Azure 2.0](/cli/azure/create-an-azure-service-principal-azure-cli) e acquisire l'output:

```azurecli-interactive
az ad sp create-for-rbac --name AzureJavaTest
```

Restituisce una risposta nel formato seguente:

```json
{
  "appId": "a487e0c1-82af-47d9-9a0b-af184eb87646d",
  "displayName": "AzureJavaTest",
  "name": "http://AzureJavaTest",
  "password": "aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa",
  "tenant": "tttttttt-tttt-tttt-tttt-tttttttttttt"
}
```

Configurare quindi le variabili di ambiente:

- `AZURE_SUBSCRIPTION_ID`: usare il valore di *id* di `az account show` nell'interfaccia della riga di comando di Azure 2.0.
- `AZURE_CLIENT_ID`: usare il valore di *appId* dell'output di un'entità servizio.
- `AZURE_CLIENT_SECRET`: usare il valore di *password* dell'output dell'entità servizio.
- `AZURE_TENANT_ID`: usare il valore di *tenant* dell'output dell'entità servizio.

Per altre opzioni di autenticazione, vedere [Identità di Azure](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/identity/azure-identity#azure-identity-client-library-for-java).

## <a name="tooling"></a>Strumenti

### <a name="create-a-new-maven-project"></a>Creare un nuovo progetto Maven

> [!NOTE]
> Questo articolo usa lo strumento Maven per compilare ed eseguire il codice di esempio. Anche altri strumenti di compilazione, come Gradle, sono compatibili con le librerie di Azure per Java.

Creare un progetto Maven dalla riga di comando in una nuova directory del sistema.

```shell
mkdir java-azure-test
cd java-azure-test
mvn archetype:generate -DgroupId=com.fabrikam -DartifactId=AzureApp \
-DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
```

Questo passaggio crea un progetto Maven di base nella cartella `testAzureApp`. Aggiungere le voci seguenti nel progetto `pom.xml` per importare le librerie usate nel codice di esempio in questa esercitazione.

```XML
<dependency>
    <groupId>com.azure</groupId>
    <artifactId>azure-identity</artifactId>
    <version>1.2.0</version>
</dependency>
<dependency>
    <groupId>com.azure.resourcemanager</groupId>
    <artifactId>azure-resourcemanager</artifactId>
    <version>2.1.0</version>
</dependency>
<dependency>
    <groupId>com.azure</groupId>
    <artifactId>azure-storage-blob</artifactId>
    <version>12.8.0</version>
</dependency>
<dependency>
    <groupId>com.microsoft.sqlserver</groupId>
    <artifactId>mssql-jdbc</artifactId>
    <version>6.2.1.jre8</version>
</dependency>
```

Aggiungere una voce `build` sotto l'elemento `project` di primo livello per usare [maven-exec-plugin](https://www.mojohaus.org/exec-maven-plugin/) per eseguire gli esempi.

```XML
<build>
    <plugins>
        <plugin>
            <groupId>org.codehaus.mojo</groupId>
            <artifactId>exec-maven-plugin</artifactId>
            <configuration>
                <mainClass>com.fabrikam.AzureApp</mainClass>
            </configuration>
        </plugin>
    </plugins>
</build>
 ```

### <a name="install-the-azure-toolkit-for-intellij"></a>Installare Azure Toolkit for IntelliJ

[Azure Toolkit](../toolkit-for-intellij/index.yml) è necessario se si prevede di distribuire app Web o API a livello di codice. Attualmente, non viene usato per altri tipi di sviluppo. La procedura seguente sintetizza il processo di installazione. Per una guida di avvio rapido, vedere [Azure Toolkit for IntelliJ](../toolkit-for-intellij/create-hello-world-web-app.md).

1. Selezionare il menu **File** e quindi selezionare **Settings** (Impostazioni).
1. Selezionare **Browse repositories** (Sfoglia repository), quindi cercare **Azure** e installare **Azure Toolkit for Intellij**.
1. Riavviare IntelliJ.

### <a name="install-the-azure-toolkit-for-eclipse"></a>Installare Azure Toolkit for Eclipse

[Azure Toolkit](../toolkit-for-eclipse/index.yml) è necessario se si prevede di distribuire app Web o API a livello di codice. Attualmente, non viene usato per altri tipi di sviluppo. La procedura seguente sintetizza il processo di installazione. Per una guida di avvio rapido, vedere [Azure Toolkit for Eclipse](../toolkit-for-eclipse/create-hello-world-web-app.md).

1. Dal menu **Help** (Guida) scegliere **Install New Software** (Installa nuovo software).
1. Nella casella **Work with** (Usa con) immettere `http://dl.microsoft.com/eclipse/` e premere **INVIO**.
1. Selezionare la casella di controllo accanto as **Azure Toolkit for Java**. Deselezionare la casella di controllo **Contact all update sites during install to find required software** (Contatta tutti i siti di aggiornamento durante l'installazione per trovare il software necessario). Fare quindi clic su **Avanti**.

## <a name="create-a-linux-virtual-machine"></a>Creare una macchina virtuale Linux

Creare un nuovo file denominato `AzureApp.java` nella directory `src/main/java/com/fabrikam` del progetto e incollare il blocco di codice seguente. Aggiornare le variabili `userName` e `sshKey` con i valori reali del computer in uso. Il codice crea una nuova VM Linux denominata `testLinuxVM` in un gruppo di risorse `sampleResourceGroup` in esecuzione nell'area Stati Uniti orientali di Azure.

```java
package com.fabrikam;

import com.azure.core.credential.TokenCredential;
import com.azure.core.http.policy.HttpLogDetailLevel;
import com.azure.core.management.AzureEnvironment;
import com.azure.core.management.Region;
import com.azure.core.management.profile.AzureProfile;
import com.azure.identity.AzureAuthorityHosts;
import com.azure.identity.EnvironmentCredentialBuilder;
import com.azure.resourcemanager.AzureResourceManager;
import com.azure.resourcemanager.compute.models.KnownLinuxVirtualMachineImage;
import com.azure.resourcemanager.compute.models.VirtualMachine;
import com.azure.resourcemanager.compute.models.VirtualMachineSizeTypes;

public class AzureApp {

    public static void main(String[] args) {

        final String userName = "YOUR_VM_USERNAME";
        final String sshKey = "YOUR_PUBLIC_SSH_KEY";

        try {
            TokenCredential credential = new EnvironmentCredentialBuilder()
                    .authorityHost(AzureAuthorityHosts.AZURE_PUBLIC_CLOUD)
                    .build();

            // If you do not set the tenant ID and subscription ID via environment variables,
            // change to create the Azure profile with tenantId, subscriptionId, and Azure environment.
            AzureProfile profile = new AzureProfile(AzureEnvironment.AZURE);

            AzureResourceManager azureResourceManager = AzureResourceManager.configure()
                    .withLogLevel(HttpLogDetailLevel.BASIC)
                    .authenticate(credential, profile)
                    .withDefaultSubscription();

            // Create an Ubuntu virtual machine in a new resource group.
            VirtualMachine linuxVM = azureResourceManager.virtualMachines().define("testLinuxVM")
                    .withRegion(Region.US_EAST)
                    .withNewResourceGroup("sampleVmResourceGroup")
                    .withNewPrimaryNetwork("10.0.0.0/24")
                    .withPrimaryPrivateIPAddressDynamic()
                    .withoutPrimaryPublicIPAddress()
                    .withPopularLinuxImage(KnownLinuxVirtualMachineImage.UBUNTU_SERVER_16_04_LTS)
                    .withRootUsername(userName)
                    .withSsh(sshKey)
                    .withUnmanagedDisks()
                    .withSize(VirtualMachineSizeTypes.STANDARD_D3_V2)
                    .create();

        } catch (Exception e) {
            System.out.println(e.getMessage());
            e.printStackTrace();
        }
    }
}
```

Eseguire l'esempio dalla riga di comando.

```shell
mvn compile exec:java
```

Nella console verranno visualizzate alcune richieste e risposte REST, mentre l'SDK effettua le chiamate sottostanti all'API REST di Azure per configurare la VM e le relative risorse. Al termine del programma, verificare la VM nella sottoscrizione con l'interfaccia della riga di comando di Azure 2.0.

```azurecli-interactive
az vm list --resource-group sampleVmResourceGroup
```

Dopo aver verificato che il codice funziona, usare l'interfaccia della riga di comando per eliminare la VM e le relative risorse.

```azurecli-interactive
az group delete --name sampleVmResourceGroup
```

## <a name="deploy-a-web-app-from-a-github-repo"></a>Distribuire un'app Web da un repository di GitHub

Sostituire il metodo main in `AzureApp.java` con il codice seguente. Aggiornare la variabile `appName` specificando un valore univoco prima di eseguire il codice. Questo codice sviluppa un'applicazione Web dal ramo `master` di un repository di GitHub pubblico in una nuova [app Web del servizio app di Azure](/azure/app-service-web/app-service-web-overview) in esecuzione nel piano tariffario gratuito.

```java
    public static void main(String[] args) {
        try {

            final String appName = "YOUR_APP_NAME";

            TokenCredential credential = new EnvironmentCredentialBuilder()
                    .authorityHost(AzureAuthorityHosts.AZURE_PUBLIC_CLOUD)
                    .build();

            // If you do not set the tenant ID and subscription ID via environment variables,
            // change to create the Azure profile with tenantId, subscriptionId, and Azure environment.
            AzureProfile profile = new AzureProfile(AzureEnvironment.AZURE);
            
            AzureResourceManager azureResourceManager = AzureResourceManager.configure()
                    .withLogLevel(HttpLogDetailLevel.BASIC)
                    .authenticate(credential, profile)
                    .withDefaultSubscription();

            WebApp app = azureResourceManager.webApps().define(appName)
                    .withRegion(Region.US_WEST2)
                    .withNewResourceGroup("sampleWebResourceGroup")
                    .withNewWindowsPlan(PricingTier.FREE_F1)
                    .defineSourceControl()
                    .withPublicGitRepository(
                            "https://github.com/Azure-Samples/app-service-web-java-get-started")
                    .withBranch("master")
                    .attach()
                    .create();

        } catch (Exception e) {
            System.out.println(e.getMessage());
            e.printStackTrace();
        }
    }
```

Eseguire il codice come prima usando Maven.

```shell
mvn clean compile exec:java
```

Aprire un browser che punta all'applicazione usando l'interfaccia della riga di comando.

```azurecli-interactive
az appservice web browse --resource-group sampleWebResourceGroup --name YOUR_APP_NAME
```

Rimuovere l'app Web e il piano dalla sottoscrizione dopo avere verificato la distribuzione.

```azurecli-interactive
az group delete --name sampleWebResourceGroup
```

## <a name="connect-to-an-azure-sql-database"></a>Connettersi a un database SQL di Azure

Sostituire il metodo main corrente in `AzureApp.java` con il codice seguente. Impostare valori reali per le variabili.
Questo codice crea un nuovo database SQL con una regola del firewall che consente l'accesso remoto. Quindi il codice si connette al database SQL usando il relativo driver JBDC.

```java
    public static void main(String args[])
    {
        // Create the db using the management libraries.
        try {
            TokenCredential credential = new EnvironmentCredentialBuilder()
                    .authorityHost(AzureAuthorityHosts.AZURE_PUBLIC_CLOUD)
                    .build();

            // If you do not set the tenant ID and subscription ID via environment variables,
            // change to create the Azure profile with tenantId, subscriptionId, and Azure environment.
            AzureProfile profile = new AzureProfile(AzureEnvironment.AZURE);

            AzureResourceManager azureResourceManager = AzureResourceManager.configure()
                    .withLogLevel(HttpLogDetailLevel.BASIC)
                    .authenticate(credential, profile);

            final String adminUser = "YOUR_USERNAME_HERE";
            final String sqlServerName = "YOUR_SERVER_NAME_HERE";
            final String sqlDbName = "YOUR_DB_NAME_HERE";
            final String dbPassword = "YOUR_PASSWORD_HERE";
            final String firewallRuleName = "YOUR_RULE_NAME_HERE";

            SqlServer sampleSQLServer = azureResourceManager.sqlServers().define(sqlServerName)
                    .withRegion(Region.US_EAST)
                    .withNewResourceGroup("sampleSqlResourceGroup")
                    .withAdministratorLogin(adminUser)
                    .withAdministratorPassword(dbPassword)
                    .defineFirewallRule(firewallRuleName)
                        .withIpAddressRange("0.0.0.0","255.255.255.255")
                        .attach()
                    .create();

            SqlDatabase sampleSQLDb = sampleSQLServer.databases().define(sqlDbName).create();

            // Assemble the connection string to the database.
            final String domain = sampleSQLServer.fullyQualifiedDomainName();
            String url = "jdbc:sqlserver://"+ domain + ":1433;" +
                    "database=" + sqlDbName +";" +
                    "user=" + adminUser+ "@" + sqlServerName + ";" +
                    "password=" + dbPassword + ";" +
                    "encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;";

            // Connect to the database, create a table, and insert an entry into it.
            Connection conn = DriverManager.getConnection(url);

            String createTable = "CREATE TABLE CLOUD ( name varchar(255), code int);";
            String insertValues = "INSERT INTO CLOUD (name, code ) VALUES ('Azure', 1);";
            String selectValues = "SELECT * FROM CLOUD";
            Statement createStatement = conn.createStatement();
            createStatement.execute(createTable);
            Statement insertStatement = conn.createStatement();
            insertStatement.execute(insertValues);
            Statement selectStatement = conn.createStatement();
            ResultSet rst = selectStatement.executeQuery(selectValues);

            while (rst.next()) {
                System.out.println(rst.getString(1) + " "
                        + rst.getString(2));
            }


        } catch (Exception e) {
            System.out.println(e.getMessage());
            System.out.println(e.getStackTrace().toString());
        }
    }
```

Eseguire l'esempio dalla riga di comando.

```shell
mvn clean compile exec:java
```

Pulire quindi le risorse usando l'interfaccia della riga di comando.

```azurecli-interactive
az group delete --name sampleSqlResourceGroup
```

## <a name="write-a-blob-into-a-new-storage-account"></a>Scrivere un BLOB in un nuovo account di archiviazione

Sostituire il metodo main corrente in `AzureApp.java` con il codice seguente. Questo codice crea un [account di Archiviazione di Azure](/azure/storage/common/storage-introduction). Quindi usa le librerie di archiviazione di Azure per Java per creare un nuovo file di testo nel cloud.

```java
    public static void main(String[] args) {

        try {
            TokenCredential tokenCredential = new EnvironmentCredentialBuilder()
                    .authorityHost(AzureAuthorityHosts.AZURE_PUBLIC_CLOUD)
                    .build();

            // If you do not set the tenant ID and subscription ID via environment variables,
            // change to create the Azure profile with tenantId, subscriptionId, and Azure environment.
            AzureProfile profile = new AzureProfile(AzureEnvironment.AZURE);

            AzureResourceManager azureResourceManager = AzureResourceManager.configure()
                    .withLogLevel(HttpLogDetailLevel.BASIC)
                    .authenticate(tokenCredential, profile)
                    .withDefaultSubscription();

            // Create a new storage account.
            String storageAccountName = "YOUR_STORAGE_ACCOUNT_NAME_HERE";
            StorageAccount storage = azureResourceManager.storageAccounts().define(storageAccountName)
                    .withRegion(Region.US_WEST2)
                    .withNewResourceGroup("sampleStorageResourceGroup")
                    .create();

            // Create a storage container to hold the file.
            List<StorageAccountKey> keys = storage.getKeys();
            PublicEndpoints endpoints = storage.endPoints();
            String accountName = storage.name();
            String accountKey = keys.get(0).value();
            String endpoint = endpoints.primary().blob();

            StorageSharedKeyCredential credential = new StorageSharedKeyCredential(accountName, accountKey);

            BlobServiceClient storageClient =new BlobServiceClientBuilder()
                    .endpoint(endpoint)
                    .credential(credential)
                    .buildClient();

            // Container name must be lowercase.
            BlobContainerClient blobContainerClient = storageClient.getBlobContainerClient("helloazure");
            blobContainerClient.create();

            // Make the container public.
            blobContainerClient.setAccessPolicy(PublicAccessType.CONTAINER, null);

            // Write a blob to the container.
            String fileName = "helloazure.txt";
            String textNew = "Hello Azure";

            BlobClient blobClient = blobContainerClient.getBlobClient(fileName);
            InputStream is = new ByteArrayInputStream(textNew.getBytes());
            blobClient.upload(is, textNew.length());

        } catch (Exception e) {
            System.out.println(e.getMessage());
            e.printStackTrace();
        }
    }
```

Eseguire l'esempio dalla riga di comando.

```shell
mvn clean compile exec:java
```

È possibile cercare il file `helloazure.txt` nell'account di archiviazione tramite il portale di Azure o con [Azure Storage Explorer](/azure/vs-azure-tools-storage-explorer-blobs).

Pulire l'account di archiviazione usando l'interfaccia della riga di comando.

```azurecli-interactive
az group delete --name sampleStorageResourceGroup
```

## <a name="explore-more-samples"></a>Esplorare altri esempi

Per altre informazioni su come usare le librerie di gestione di Azure per Java per gestire risorse e automatizzare attività, vedere il codice di esempio per [macchine virtuali](java-sdk-azure-virtual-machine-samples.md), [app Web](java-sdk-azure-web-apps-samples.md) e [database SQL](java-sdk-azure-sql-database-samples.md).

## <a name="reference-and-release-notes"></a>Informazioni di riferimento e note sulla versione

Le [informazioni di riferimento](/java/api) sono disponibili per tutti i pacchetti.

## <a name="get-help-and-give-feedback"></a>Ottenere supporto e inviare commenti

Pubblicare le domande per la community in [Stack Overflow](https://stackoverflow.com/questions/tagged/azure+java). Segnalare bug e problemi in sospeso relativi alle librerie di Azure per Java nel [progetto GitHub](https://github.com/Azure/azure-sdk-for-java).
