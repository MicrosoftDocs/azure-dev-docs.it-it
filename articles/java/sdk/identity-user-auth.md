---
title: Autenticazione di Azure con le credenziali utente
description: Panoramica dei concetti di Azure SDK per Java correlati all'autenticazione delle applicazioni con le credenziali utente
author: g2vinay
ms.date: 02/02/2021
ms.topic: conceptual
ms.custom: devx-track-java
ms.author: vigera
ms.openlocfilehash: d2b5b24b0f39d56ca2235ec56a4ea56c4be6b185
ms.sourcegitcommit: 71847ee0a1fee3f3320503629d9a8c82319a1f6a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/03/2021
ms.locfileid: "99522121"
---
# <a name="azure-authentication-with-user-credentials"></a>Autenticazione di Azure con le credenziali utente

Questo articolo illustra il modo in cui la libreria di identità di Azure supporta l'autenticazione Azure Active Directory token con le credenziali fornite dall'utente. Questo supporto è reso possibile tramite un set di implementazioni di TokenCredential descritte di seguito.

Questo articolo include gli argomenti seguenti:

* [Credenziale del codice del dispositivo](#device-code-credential)
* [Credenziali del browser interattivo](#interactive-browser-credential)
* [Credenziali password nome utente](#username-password-credential)

## <a name="device-code-credential"></a>Credenziale del codice del dispositivo

La credenziale del codice del dispositivo autentica in modo interattivo un utente nei dispositivi con interfaccia utente limitata. Funziona richiedendo all'utente di visitare un URL di accesso in un computer abilitato per il browser quando l'applicazione tenta di eseguire l'autenticazione. L'utente immette quindi il codice del dispositivo indicato nelle istruzioni insieme alle relative credenziali di accesso. Al termine dell'autenticazione, l'applicazione che ha richiesto l'autenticazione viene autenticata correttamente sul dispositivo in cui è in esecuzione.

Per altre informazioni, vedere [piattaforma Microsoft Identity e il flusso di concessione dell'autorizzazione del dispositivo OAuth 2,0](/azure/active-directory/develop/v2-oauth2-device-code).

### <a name="enable-applications-for-device-code-flow"></a>Abilitare le applicazioni per il flusso di codice del dispositivo

Per autenticare un utente tramite il flusso del codice del dispositivo, seguire questa procedura:

1. Passare a Azure Active Directory in portale di Azure e trovare la registrazione dell'app.
2. Passare alla sezione **autenticazione** .
3. In **URI reindirizzati suggeriti**, controllare l'URI che termina con `/common/oauth2/nativeclient` .
4. In **tipo di client predefinito** selezionare `yes` per `Treat application as a public client` .

Questi passaggi consentiranno all'applicazione di eseguire l'autenticazione, ma non avranno ancora le autorizzazioni per accedere a Active Directory o accedere alle risorse per conto dell'utente. Per risolvere questo problema, passare a **autorizzazioni** per le API e abilitare Microsoft Graph e le risorse a cui si vuole accedere, ad esempio gestione dei servizi di Azure, Key Vault e così via.

È anche necessario essere l'amministratore del tenant per concedere il consenso all'applicazione quando si accede per la prima volta.

Se non è possibile configurare l'opzione flusso di codice del dispositivo nel Active Directory, potrebbe essere necessario che l'app sia multi-tenant. Per rendere l'app multi-tenant, passare al pannello **autenticazione** , quindi selezionare **account in qualsiasi directory dell'organizzazione**. Quindi selezionare *Sì* per **considera applicazione come client pubblico**.

### <a name="authenticate-a-user-account-with-device-code-flow"></a>Autenticare un account utente con il flusso del codice del dispositivo

Nell'esempio seguente viene illustrata l'autenticazione `SecretClient` di dalla [libreria client segreta Azure Key Vault per Java][secrets_client_library] usando il `DeviceCodeCredential` su un dispositivo Internet.

```java
/**
 * Authenticate with device code credential.
 */
DeviceCodeCredential deviceCodeCredential = new DeviceCodeCredentialBuilder()
    .challengeConsumer(challenge -> {
    // Lets the user know about the challenge.
    System.out.println(challenge.getMessage());
    }).build();

// Azure SDK client builders accept the credential as a parameter.
SecretClient client = new SecretClientBuilder()
    .vaultUrl("https://<your Key Vault name>.vault.azure.net")
    .credential(deviceCodeCredential)
    .buildClient();
```

## <a name="interactive-browser-credential"></a>Credenziali del browser interattivo

Questa credenziale autentica in modo interattivo un utente con il browser di sistema predefinito e offre un'esperienza di autenticazione uniforme consentendo di usare le proprie credenziali per autenticare l'applicazione.

### <a name="enable-applications-for-interactive-browser-oauth-2-flow"></a>Abilitare le applicazioni per il flusso OAuth 2 del browser interattivo

Per utilizzare `InteractiveBrowserCredential` , è necessario registrare un'applicazione in Azure Active Directory con le autorizzazioni per accedere per conto di un utente. Per registrare l'applicazione, seguire la procedura descritta in precedenza per il flusso del codice del dispositivo. Come indicato in precedenza, un amministratore del tenant deve concedere il consenso all'applicazione prima che qualsiasi account utente possa eseguire l'accesso.

Si può notare che `InteractiveBrowserCredentialBuilder` è necessario un URL di reindirizzamento. Aggiungere l'URL di reindirizzamento alla sottosezione **URI di reindirizzamento** nella sezione **autenticazione** dell'applicazione AAD registrata.

### <a name="authenticate-a-user-account-interactively-in-the-browser"></a>Autenticare un account utente in modo interattivo nel browser

Nell'esempio seguente viene illustrata l'autenticazione `SecretClient` di dalla libreria client [Azure-Security-Vault-Secrets][secrets_client_library] utilizzando `InteractiveBrowserCredential` .

```java
/**
 * Authenticate interactively in the browser.
 */
InteractiveBrowserCredential interactiveBrowserCredential = new InteractiveBrowserCredentialBuilder()
    .clientId("<your app client ID>")
    .redirectUrl("YOUR_APP_REGISTERED_REDIRECT_URL")
    .build();

// Azure SDK client builders accept the credential as a parameter.
SecretClient client = new SecretClientBuilder()
    .vaultUrl("https://<your Key Vault name>.vault.azure.net")
    .credential(interactiveBrowserCredential)
    .buildClient();
```

## <a name="username-password-credential"></a>Credenziali password nome utente

`UsernamePasswordCredential`Consente di autenticare un'applicazione client pubblica usando le credenziali utente che non richiedono l'autenticazione a più fattori. Nell'esempio seguente viene illustrata l'autenticazione `SecretClient` di dalla libreria client [Azure-Security-Vault-Secrets][secrets_client_library] utilizzando `UsernamePasswordCredential` . L'utente non deve avere attivato la funzionalità di autenticazione a più fattori.

```java
/**
 * Authenticate with username, password.
 */
UsernamePasswordCredential usernamePasswordCredential = new UsernamePasswordCredentialBuilder()
    .clientId("<your app client ID>")
    .username("<your username>")
    .password("<your password>")
    .build();

// Azure SDK client builders accept the credential as a parameter.
SecretClient client = new SecretClientBuilder()
    .vaultUrl("https://<your Key Vault name>.vault.azure.net")
    .credential(usernamePasswordCredential)
    .buildClient();
```

Per ulteriori informazioni, vedere la pagina relativa alle [credenziali della password del proprietario delle risorse OAuth 2,0 e della piattaforma Microsoft Identity](/azure/active-directory/develop/v2-oauth-ropc).

## <a name="next-steps"></a>Passaggi successivi

Questo articolo ha analizzato l'autenticazione con le credenziali utente. Questa forma di autenticazione è uno dei diversi modi in cui è possibile eseguire l'autenticazione in Azure SDK per Java. Gli articoli seguenti descrivono altri modi:

* [Autenticazione di Azure negli ambienti di sviluppo](identity-dev-env-auth.md)
* [Autenticazione di applicazioni ospitate in Azure](identity-azure-hosted-auth.md)
* [Autenticazione con entità servizio](identity-service-principal-auth.md)

Dopo aver eseguito l'autenticazione, vedere [configurare la registrazione in Azure SDK per Java](logging-overview.md) per informazioni sulla funzionalità di registrazione fornita dall'SDK.

<!-- LINKS -->
[secrets_client_library]: https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/keyvault/azure-security-keyvault-secrets
