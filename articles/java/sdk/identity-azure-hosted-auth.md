---
title: Autenticare le applicazioni Java ospitate in Azure
description: Panoramica dei concetti relativi a Azure SDK per Java correlati all'autenticazione di applicazioni ospitate in Azure
author: g2vinay
ms.date: 02/02/2021
ms.topic: conceptual
ms.custom: devx-track-java
ms.author: vigera
ms.openlocfilehash: e9097a432abf5957c9983ec2846bfb18ba581db4
ms.sourcegitcommit: 71847ee0a1fee3f3320503629d9a8c82319a1f6a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/03/2021
ms.locfileid: "99522124"
---
# <a name="authenticate-azure-hosted-java-applications"></a>Autenticare le applicazioni Java ospitate in Azure

Questo articolo illustra il modo in cui la libreria di identità di Azure supporta l'autenticazione del token Azure Active Directory per le applicazioni ospitate in Azure. Questo supporto è reso possibile tramite un set di `TokenCredential` implementazioni, descritte di seguito.

Questo articolo include gli argomenti seguenti:

* [Credenziali di Azure predefinite](#default-azure-credential)
* [Credenziali di identità gestite](#managed-identity-credential)

## <a name="default-azure-credential"></a>Credenziali di Azure predefinite

`DefaultAzureCredential`È adatto per la maggior parte degli scenari in cui l'applicazione viene infine eseguita nel cloud di Azure. `DefaultAzureCredential` combina le credenziali utilizzate comunemente per l'autenticazione durante la distribuzione, con le credenziali utilizzate per l'autenticazione in un ambiente di sviluppo. Il `DefaultAzureCredential` tenterà di eseguire l'autenticazione tramite i meccanismi seguenti nell'ordine.

![Flusso di autenticazione di DefaultAzureCredential](./media/defaultazurecredential.svg)

* Ambiente: il `DefaultAzureCredential` leggerà le informazioni sull'account specificate tramite le [variabili di ambiente](#environment-variables) e le userà per l'autenticazione.
* Identità gestita: se l'applicazione viene distribuita in un host di Azure con identità gestita abilitata, `DefaultAzureCredential` eseguirà l'autenticazione con tale account.
* IntelliJ-se è stata eseguita l'autenticazione tramite Azure Toolkit for IntelliJ, `DefaultAzureCredential` eseguirà l'autenticazione con l'account.
* Visual Studio Code: se è stata eseguita l'autenticazione tramite il plug-in account Azure Visual Studio Code, il `DefaultAzureCredential` eseguirà l'autenticazione con l'account.
* INTERFACCIA della riga di comando di Azure: se è stato autenticato un account tramite il comando dell'interfaccia della `az login` riga di comando di Azure, l' `DefaultAzureCredential` autenticazione con tale account.

### <a name="configure-defaultazurecredential"></a>Configurare DefaultAzureCredential

`DefaultAzureCredential` supporta un set di configurazioni tramite Setter nelle variabili di `DefaultAzureCredentialBuilder` ambiente o.

* L'impostazione delle variabili `AZURE_CLIENT_ID` di ambiente, `AZURE_CLIENT_SECRET` e `AZURE_TENANT_ID` come definito nelle [variabili di ambiente](#environment-variables) `DefaultAzureCredential` consente di configurare per l'autenticazione come entità servizio specificata dai valori.
* L'impostazione del `.managedIdentityClientId(String)` generatore o della variabile `AZURE_CLIENT_ID` di ambiente `DefaultAzureCredential` consente di configurare per l'autenticazione come identità gestita definita dall'utente, lasciando vuota la configurazione per l'autenticazione come identità gestita assegnata dal sistema.
* L'impostazione del `.tenantId(String)` generatore o della variabile `AZURE_TENANT_ID` di ambiente `DefaultAzureCredential` consente di configurare per l'autenticazione in un tenant specifico per la cache di token condivisa, Visual Studio Code e IntelliJ idea.
* L'impostazione della variabile `AZURE_USERNAME` di ambiente configura la `DefaultAzureCredential` per selezionare il token memorizzato nella cache corrispondente dalla cache dei token condivisi.
* L'impostazione del `.intelliJKeePassDatabasePath(String)` Generatore configura la `DefaultAzureCredential` per la lettura di un file KeePass specifico quando si esegue l'autenticazione con le credenziali di IntelliJ.

### <a name="authenticate-with-defaultazurecredential"></a>Eseguire l'autenticazione con DefaultAzureCredential

Nell'esempio seguente viene illustrata l'autenticazione `SecretClient` di dalla libreria client [Azure-Security-Vault-Secrets][secrets_client_library] utilizzando `DefaultAzureCredential` .

```java
// Azure SDK client builders accept the credential as a parameter.
SecretClient client = new SecretClientBuilder()
  .vaultUrl("https://<your Key Vault name>.vault.azure.net")
  .credential(new DefaultAzureCredentialBuilder().build())
  .buildClient();
```

### <a name="authenticate-a-user-assigned-managed-identity-with-defaultazurecredential"></a>Autenticare un'identità gestita assegnata dall'utente con DefaultAzureCredential

Nell'esempio seguente viene illustrata l'autenticazione `SecretClient` di dalla libreria client [Azure-Security-Vault-Secrets][secrets_client_library] usando la `DefaultAzureCredential` distribuita in una risorsa di Azure con un'identità gestita assegnata dall'utente configurata.

```java
/**
 * The default credential will use the user-assigned managed identity with the specified client ID.
 */
DefaultAzureCredential defaultCredential = new DefaultAzureCredentialBuilder()
  .managedIdentityClientId("<managed identity client ID>")
  .build();

// Azure SDK client builders accept the credential as a parameter.
SecretClient client = new SecretClientBuilder()
  .vaultUrl("https://<your Key Vault name>.vault.azure.net")
  .credential(defaultCredential)
  .buildClient();
```

### <a name="authenticate-a-user-in-azure-toolkit-for-intellij-with-defaultazurecredential"></a>Autenticare un utente in Azure Toolkit for IntelliJ con DefaultAzureCredential

Nell'esempio seguente viene illustrata l'autenticazione `SecretClient` di dalla libreria client [Azure-Security-Vault-Secrets][secrets_client_library] usando `DefaultAzureCredential` , in una workstation in cui è installato IntelliJ IDEA e l'utente ha eseguito l'accesso con un account Azure al Azure Toolkit for IntelliJ.

Per altre informazioni sulla configurazione di IntelliJ IDEA, vedere [Azure Toolkit for IntelliJ di accesso per IntelliJCredential](identity-dev-env-auth.md#sign-in-azure-toolkit-for-intellij-for-intellijcredential).

```java
/**
 * The default credential will use the KeePass database path to find the user account in IntelliJ on Windows.
 */
// KeePass configuration is required only for Windows. No configuration needed for Linux / Mac.
DefaultAzureCredential defaultCredential = new DefaultAzureCredentialBuilder()
  .intelliJKeePassDatabasePath("C:\\Users\\user\\AppData\\Roaming\\JetBrains\\IdeaIC2020.1\\c.kdbx")
  .build();

// Azure SDK client builders accept the credential as a parameter.
SecretClient client = new SecretClientBuilder()
  .vaultUrl("https://<your Key Vault name>.vault.azure.net")
  .credential(defaultCredential)
  .buildClient();
```

## <a name="managed-identity-credential"></a>Credenziali di identità gestite

L'identità gestita autentica l'identità gestita (sistema o utente assegnato) di una risorsa di Azure. Quindi, se l'applicazione è in esecuzione all'interno di una risorsa di Azure che supporta l'identità gestita tramite `IDENTITY/MSI` , `IMDS` gli endpoint o entrambi, questa credenziale riceverà l'autenticazione dell'applicazione e offrirà un'esperienza di autenticazione segreta.

Per altre informazioni, vedere [Informazioni sulle identità gestite per le risorse di Azure](/azure/active-directory/managed-identities-azure-resources/overview).

### <a name="authenticate-in-azure-with-managed-identity"></a>Eseguire l'autenticazione in Azure con identità gestita

Nell'esempio seguente viene illustrata l'autenticazione `SecretClient` di dalla libreria client [Azure-Security-Vault-Secrets][secrets_client_library] usando il `ManagedIdentityCredential` in una macchina virtuale, il servizio app, l'app per le funzioni, cloud Shell, Service Fabric, Arc o l'ambiente AKS in Azure, con l'identità gestita assegnata dal sistema o gestita dall'utente abilitata.

```java
/**
 * Authenticate with a managed identity.
 */
ManagedIdentityCredential managedIdentityCredential = new ManagedIdentityCredentialBuilder()
  .clientId("<user-assigned managed identity client ID>") // required only for user-assigned
  .build();

// Azure SDK client builders accept the credential as a parameter.
SecretClient client = new SecretClientBuilder()
  .vaultUrl("https://<your Key Vault name>.vault.azure.net")
  .credential(managedIdentityCredential)
  .buildClient();
```

## <a name="environment-variables"></a>Variabili di ambiente

È possibile configurare `DefaultAzureCredential` e `EnvironmentCredential` con le variabili di ambiente. Ogni tipo di autenticazione richiede valori per variabili specifiche:

### <a name="service-principal-with-secret"></a>Entità servizio con segreto

| Nome variabile         | Valore                                                  |
| --------------------- | ------------------------------------------------------ |
| `AZURE_CLIENT_ID`     | ID di un'applicazione Azure Active Directory.           |
| `AZURE_TENANT_ID`     | ID del tenant di Azure Active Directory dell'applicazione. |
| `AZURE_CLIENT_SECRET` | Uno dei segreti client dell'applicazione.               |

### <a name="service-principal-with-certificate"></a>Entità servizio con certificato

| Nome variabile         | Valore                                                                                                |
| --------------------- | ---------------------------------------------------------------------------------------------------- |
| `AZURE_CLIENT_ID`     | ID di un'applicazione Azure Active Directory.                                                         |
| `AZURE_TENANT_ID`     | ID del tenant di Azure Active Directory dell'applicazione.                                               |
| `AZURE_CLIENT_CERTIFICATE_PATH` | Percorso di un file di certificato con codifica PEM, inclusa la chiave privata (senza password di protezione).|

### <a name="username-and-password"></a>Nome utente e password

| Nome variabile         | Valore                                            |
| --------------------- | ------------------------------------------------ |
| `AZURE_CLIENT_ID`     | ID di un'applicazione Azure Active Directory.     |
| `AZURE_USERNAME`      | Un nome utente (in genere un indirizzo di posta elettronica).           |
| `AZURE_PASSWORD`      | Password associata per il nome utente specificato.  |

Viene eseguito un tentativo di configurazione nell'ordine precedente. Se, ad esempio, sono presenti valori per un segreto client e un certificato, viene utilizzato il segreto client.

## <a name="next-steps"></a>Passaggi successivi

Questo articolo ha analizzato l'autenticazione per le applicazioni ospitate in Azure. Questa forma di autenticazione è uno dei diversi modi in cui è possibile eseguire l'autenticazione in Azure SDK per Java. Gli articoli seguenti descrivono altri modi:

* [Autenticazione di Azure negli ambienti di sviluppo](identity-dev-env-auth.md)
* [Autenticazione con entità servizio](identity-service-principal-auth.md)
* [Autenticazione con credenziali utente](identity-user-auth.md)

Dopo aver eseguito l'autenticazione, vedere [configurare la registrazione in Azure SDK per Java](logging-overview.md) per informazioni sulla funzionalità di registrazione fornita dall'SDK.

<!-- LINKS -->
[secrets_client_library]: https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/keyvault/azure-security-keyvault-secrets
