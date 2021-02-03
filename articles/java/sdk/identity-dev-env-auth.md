---
title: Autenticazione di Azure in ambienti di sviluppo Java
description: Panoramica dei concetti di Azure SDK per Java correlati all'autenticazione in ambienti di sviluppo
author: g2vinay
ms.date: 02/02/2021
ms.topic: conceptual
ms.custom: devx-track-java
ms.author: vigera
ms.openlocfilehash: 7913430a81a0fe223d6eb23a48541e0f752323b2
ms.sourcegitcommit: 71847ee0a1fee3f3320503629d9a8c82319a1f6a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/03/2021
ms.locfileid: "99522123"
---
# <a name="azure-authentication-in-java-development-environments"></a>Autenticazione di Azure in ambienti di sviluppo Java

Questo articolo fornisce una panoramica del supporto della libreria di identità di Azure per l'autenticazione Azure Active Directory token. Questo supporto consente l'autenticazione per le applicazioni eseguite localmente sui computer degli sviluppatori tramite un set di `TokenCredential` implementazioni.

Gli argomenti trattati in questo articolo includono:

* [Credenziale del codice del dispositivo](#device-code-credential)
* [Credenziali del browser interattivo](#interactive-browser-credential)
* [Credenziali CLI di Azure](#azure-cli-credential)
* [Credenziali IntelliJ](#intellij-credential)
* [Credenziali Visual Studio Code](#visual-studio-code-credential)

## <a name="device-code-credential"></a>Credenziale del codice del dispositivo

La credenziale del codice del dispositivo autentica in modo interattivo un utente nei dispositivi con interfaccia utente limitata. Funziona richiedendo all'utente di visitare un URL di accesso in un computer abilitato per il browser quando l'applicazione tenta di eseguire l'autenticazione. L'utente immette quindi il codice del dispositivo indicato nelle istruzioni insieme alle relative credenziali di accesso. Al termine dell'autenticazione, l'applicazione che ha richiesto l'autenticazione viene autenticata correttamente sul dispositivo in cui è in esecuzione.

Per altre informazioni, vedere [piattaforma Microsoft Identity e il flusso di concessione dell'autorizzazione del dispositivo OAuth 2,0](/azure/active-directory/develop/v2-oauth2-device-code).

### <a name="enable-applications-for-device-code-flow"></a>Abilitare le applicazioni per il flusso di codice del dispositivo

Per autenticare un utente tramite il flusso del codice del dispositivo, seguire questa procedura:

1. Passare a Azure Active Directory nel portale di Azure e trovare la registrazione dell'app.
2. Passare alla sezione **autenticazione** .
3. In **URI reindirizzati suggeriti**, controllare l'URI che termina con `/common/oauth2/nativeclient` .
4. In **tipo di client predefinito** selezionare *Sì* per **considera applicazione come client pubblico**.

Questi passaggi consentiranno all'applicazione di eseguire l'autenticazione, ma non avranno ancora le autorizzazioni per accedere a Active Directory o accedere alle risorse per conto dell'utente. Per risolvere questo problema, passare a **autorizzazioni** per le API e abilitare Microsoft Graph e le risorse a cui si vuole accedere.

È anche necessario essere l'amministratore del tenant per concedere il consenso all'applicazione quando si accede per la prima volta.

Se non è possibile configurare l'opzione flusso di codice del dispositivo nel Active Directory, potrebbe essere necessario che l'app sia multi-tenant. Per rendere l'app multi-tenant, passare al pannello **autenticazione** , quindi selezionare **account in qualsiasi directory dell'organizzazione**. Quindi selezionare *Sì* per **considera applicazione come client pubblico**.

### <a name="authenticate-a-user-account-with-device-code-flow"></a>Autenticare un account utente con il flusso del codice del dispositivo

Nell'esempio seguente viene illustrata l'autenticazione `SecretClient` di dalla libreria client [Azure-Security-Vault-Secrets][secrets_client_library] usando il `DeviceCodeCredential` in un dispositivo Internet delle cose.

```java
DeviceCodeCredential deviceCodeCredential = new DeviceCodeCredentialBuilder()
  .challengeConsumer(challenge -> {
    // lets user know of the challenge
    System.out.println(challenge.getMessage());
  }).build();

// Azure SDK client builders accept the credential as a parameter
SecretClient client = new SecretClientBuilder()
  .vaultUrl("https://<your Key Vault name>.vault.azure.net")
  .credential(deviceCodeCredential)
  .buildClient();
```

## <a name="interactive-browser-credential"></a>Credenziali del browser interattivo

Questa credenziale autentica in modo interattivo un utente con il browser di sistema predefinito e offre un'esperienza di autenticazione uniforme, consentendo di usare le proprie credenziali per autenticare l'applicazione.

### <a name="enable-applications-for-interactive-browser-oauth-2-flow"></a>Abilitare le applicazioni per il flusso OAuth 2 del browser interattivo

Per utilizzare `InteractiveBrowserCredential` , è necessario registrare un'applicazione in Azure Active Directory con le autorizzazioni per accedere per conto di un utente. Per registrare l'applicazione, seguire la procedura descritta in precedenza per il flusso del codice del dispositivo. Come indicato in precedenza, un amministratore del tenant deve concedere il consenso all'applicazione prima che qualsiasi account utente possa eseguire l'accesso.

È possibile notare che in `InteractiveBrowserCredentialBuilder` è necessario un URL di reindirizzamento. Aggiungere l'URL di reindirizzamento alla sottosezione **URI di reindirizzamento** nella sezione **autenticazione** dell'applicazione AAD registrata.

### <a name="authenticate-a-user-account-interactively-in-the-browser"></a>Autenticare un account utente in modo interattivo nel browser

Nell'esempio seguente viene illustrata l'autenticazione `SecretClient` di dalla libreria client [Azure-Security-Vault-Secrets][secrets_client_library] utilizzando `InteractiveBrowserCredential` .

```java
InteractiveBrowserCredential interactiveBrowserCredential = new InteractiveBrowserCredentialBuilder()
  .clientId("<your client ID>")
  .redirectUrl("http://localhost:8765")
  .build();

// Azure SDK client builders accept the credential as a parameter
SecretClient client = new SecretClientBuilder()
  .vaultUrl("https://<your Key Vault name>.vault.azure.net")
  .credential(interactiveBrowserCredential)
  .buildClient();
```

## <a name="azure-cli-credential"></a>Credenziali CLI di Azure

La credenziale Visual Studio Code esegue l'autenticazione in un ambiente di sviluppo con l'utente o l'entità servizio abilitata nell'interfaccia della riga di comando di Azure. Usa l'interfaccia della riga di comando di Azure dato un utente che è già connesso e usa l'interfaccia della riga di comando per autenticare l'applicazione rispetto a Azure Active Directory.

### <a name="sign-in-azure-cli-for-azureclicredential"></a>Accedere all'interfaccia della riga di comando di Azure per AzureCliCredential

Accedere come utente con il comando dell'interfaccia della riga di comando di [Azure][azure_cli] seguente:

```bash
az login
```

Accedere come entità servizio usando il comando seguente:

```bash
az login --service-principal --username <client ID> --password <client secret> --tenant <tenant ID>
```

Se l'account o l'entità servizio ha accesso a più tenant, verificare che il tenant o la sottoscrizione desiderata sia nello stato "Enabled" nell'output del comando seguente:

```bash
az account list
```

Prima di usare `AzureCliCredential` nel codice, eseguire il comando seguente per verificare che l'account sia stato configurato correttamente.

```bash
az account get-access-token
```

Potrebbe essere necessario ripetere questo processo dopo un determinato periodo di tempo, a seconda della validità del token di aggiornamento nell'organizzazione. In genere, il periodo di validità del token di aggiornamento è di alcune settimane per alcuni mesi. `AzureCliCredential` verrà richiesto di eseguire di nuovo l'accesso.

### <a name="authenticate-a-user-account-with-azure-cli"></a>Autenticare un account utente con l'interfaccia della riga di comando di Azure

Nell'esempio seguente viene illustrata l'autenticazione `SecretClient` di dalla libreria client [Azure-Security-Vault-Secrets][secrets_client_library] usando il su una workstation con l'interfaccia della riga di comando di `AzureCliCredential` Azure installata e con accesso.

```java
AzureCliCredential cliCredential = new AzureCliCredentialBuilder().build();

// Azure SDK client builders accept the credential as a parameter.
SecretClient client = new SecretClientBuilder()
  .vaultUrl("https://<your Key Vault name>.vault.azure.net")
  .credential(cliCredential)
  .buildClient();
```

## <a name="intellij-credential"></a>Credenziali IntelliJ

Il Visual Studio Code credenziale esegue l'autenticazione in un ambiente di sviluppo con l'account di Azure Toolkit for IntelliJ. Usa le informazioni sull'utente connesso nell'IDE di IntelliJ e le usa per autenticare l'applicazione rispetto a Azure Active Directory.

## <a name="sign-in-azure-toolkit-for-intellij-for-intellijcredential"></a>Azure Toolkit for IntelliJ di accesso per IntelliJCredential

Seguire i passaggi descritti di seguito:

1. Nella finestra di IntelliJ aprire **File > impostazioni > plug**-in.
1. Cercare "Azure Toolkit for IntelliJ" nel Marketplace. Installare e riavviare l'IDE.
1. Trovare i nuovi strumenti per la voce di menu **> azure > Azure Sign in...**
1. L' **accesso al dispositivo** consentirà di accedere come account utente. Seguire le istruzioni per accedere al `login.microsoftonline.com` sito Web con il codice del dispositivo. IntelliJ chiederà di selezionare le sottoscrizioni. Selezionare la sottoscrizione con le risorse a cui si vuole accedere.

In Windows è necessario anche il percorso del database KeePass per leggere le credenziali di IntelliJ. È possibile trovare il percorso in impostazioni di IntelliJ in **File > impostazioni > aspetto & comportamento > impostazioni di sistema > le password**. Prendere nota della posizione del percorso KeePassDatabase.

## <a name="authenticate-a-user-account-with-intellij-idea"></a>Autenticare un account utente con IntelliJ IDEA

Nell'esempio seguente viene illustrata l'autenticazione `SecretClient` di dalla libreria client [Azure-Security-Vault-Secrets][secrets_client_library] usando il `IntelliJCredential` in una workstation in cui è installato IntelliJ IDEA e l'utente ha eseguito l'accesso con un account Azure.

```java
IntelliJCredential intelliJCredential = new IntelliJCredentialBuilder()
  // KeePass configuration isrequired only for Windows. No configuration needed for Linux / Mac.
  .keePassDatabasePath("C:\\Users\\user\\AppData\\Roaming\\JetBrains\\IdeaIC2020.1\\c.kdbx")
  .build();

// Azure SDK client builders accept the credential as a parameter
SecretClient client = new SecretClientBuilder()
  .vaultUrl("https://<your Key Vault name>.vault.azure.net")
  .credential(intelliJCredential)
  .buildClient();
```

## <a name="visual-studio-code-credential"></a>Credenziali Visual Studio Code

Il Visual Studio Code credenziale consente l'autenticazione negli ambienti di sviluppo in cui viene installato VS Code con l' [estensione Account vs code Azure](https://github.com/Microsoft/vscode-azure-account). Usa le informazioni sull'utente connesso nell'IDE di VS Code e le usa per autenticare l'applicazione rispetto a Azure Active Directory.

### <a name="sign-in-visual-studio-code-azure-account-extension-for-visualstudiocodecredential"></a>Accedere Visual Studio Code estensione dell'account Azure per VisualStudioCodeCredential

L'autenticazione Visual Studio Code viene gestita da un'integrazione con l' [estensione dell'account Azure](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account). Per usare questa modalità di autenticazione, installare l'estensione dell'account Azure e quindi usare **visualizza > riquadro comandi** per eseguire il comando **Azure: Sign in** . Questo comando apre una finestra del browser e visualizza una pagina che consente di accedere ad Azure. Dopo aver completato il processo di accesso, è possibile chiudere il browser come indicato. L'esecuzione dell'applicazione (nel debugger o in qualsiasi punto del computer di sviluppo) userà le credenziali dell'accesso.

### <a name="authenticate-a-user-account-with-visual-studio-code"></a>Autenticare un account utente con Visual Studio Code

Nell'esempio seguente viene illustrata l'autenticazione `SecretClient` di dalla libreria client [Azure-Security-Vault-Secrets][secrets_client_library] usando il `VisualStudioCodeCredential` in una workstation in cui è installato Visual Studio Code e l'utente ha eseguito l'accesso con un account Azure.

```java
VisualStudioCodeCredential visualStudioCodeCredential = new VisualStudioCodeCredentialBuilder().build();

// Azure SDK client builders accept the credential as a parameter.
SecretClient client = new SecretClientBuilder()
  .vaultUrl("https://<your Key Vault name>.vault.azure.net")
  .credential(visualStudioCodeCredential)
  .buildClient();
```

## <a name="next-steps"></a>Passaggi successivi

Questo articolo ha analizzato l'autenticazione durante lo sviluppo usando le credenziali disponibili nel computer. Questa forma di autenticazione è uno dei diversi modi in cui è possibile eseguire l'autenticazione in Azure SDK per Java. Gli articoli seguenti descrivono altri modi:

* [Autenticazione di applicazioni ospitate in Azure](identity-azure-hosted-auth.md)
* [Autenticazione con entità servizio](identity-service-principal-auth.md)
* [Autenticazione con credenziali utente](identity-user-auth.md)

Dopo aver eseguito l'autenticazione, vedere [configurare la registrazione in Azure SDK per Java](logging-overview.md) per informazioni sulla funzionalità di registrazione fornita dall'SDK.

<!-- LINKS -->
[azure_cli]: /cli/azure
[secrets_client_library]: https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/keyvault/azure-security-keyvault-secrets
