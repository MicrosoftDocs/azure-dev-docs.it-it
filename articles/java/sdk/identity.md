---
title: Autenticazione di Azure con Java e identità di Azure
description: Panoramica della funzionalità di autenticazione e identità di Azure SDK
author: g2vinay
ms.date: 02/02/2021
ms.topic: conceptual
ms.custom: devx-track-java
ms.author: vigera
ms.openlocfilehash: 2a09b4ccaf39564c1cd2547417d722472ed9017c
ms.sourcegitcommit: 71847ee0a1fee3f3320503629d9a8c82319a1f6a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/03/2021
ms.locfileid: "99522120"
---
# <a name="azure-authentication-with-java-and-azure-identity"></a>Autenticazione di Azure con Java e identità di Azure

Questo articolo fornisce una panoramica di Java Azure Identity Library, che fornisce Azure Active Directory supporto per l'autenticazione basata su token in Azure SDK per Java. Questa libreria fornisce un set di `TokenCredential` implementazioni che è possibile usare per creare client SDK di Azure che supportano l'autenticazione del token AAD.

Azure Identity Library attualmente supporta:

* [Autenticazione di Azure negli ambienti di sviluppo Java](identity-dev-env-auth.md), che consente di:
  * IDEA di autenticazione IntelliJ con le informazioni di accesso recuperate dal [Azure Toolkit for IntelliJ](/azure/developer/java/toolkit-for-intellij/).
  * Visual Studio Code l'autenticazione, con le informazioni di accesso salvate nel plug-in [di Azure per Visual Studio Code](https://code.visualstudio.com/docs/azure/extensions).
  * Autenticazione dell'interfaccia della riga di comando di Azure con le informazioni di accesso salvate nell' [interfaccia](/cli/azure/what-is-azure-cli) della riga di comando
* [Autenticazione delle applicazioni ospitate in Azure](identity-azure-hosted-auth.md), che consente di:
  * Autenticazione delle credenziali di Azure predefinita
  * Autenticazione identità gestita
* [Autenticazione con entità servizio](identity-service-principal-auth.md), che consente di:
  * Autenticazione del segreto client
  * Autenticazione con certificati client
* [Autenticazione con credenziali utente](identity-user-auth.md), che consente di:
  * Autenticazione interattiva del browser
  * Autenticazione del codice del dispositivo
  * Autenticazione con nome utente/password

Per ulteriori informazioni sulle specifiche di ognuno di questi approcci di autenticazione, seguire i collegamenti riportati sopra. Nella parte restante di questo articolo verranno introdotti gli argomenti di uso comune `DefaultAzureCredential` e correlati.

## <a name="add-the-maven-dependencies"></a>Aggiungere le dipendenze maven

Per aggiungere la dipendenza Maven, includere il codice XML seguente nel file di *pom.xml* del progetto. Sostituire il numero di versione *1.2.1* con il numero di versione rilasciata più recente visualizzato nella [pagina libreria client Microsoft Azure per identità](https://mvnrepository.com/artifact/com.azure/azure-identity).

```xml
<dependency>
    <groupId>com.azure</groupId>
    <artifactId>azure-identity</artifactId>
    <version>1.2.1</version>
</dependency>
```

## <a name="key-concepts"></a>Concetti chiave

Sono disponibili due concetti chiave per comprendere la libreria di identità di Azure, ovvero il concetto di credenziale e l'implementazione più comune di tali credenziali, il `DefaultAzureCredential` .

Una credenziale è una classe che contiene o può ottenere i dati necessari per l'autenticazione delle richieste da parte di un client del servizio. I client del servizio in Azure SDK accettano le credenziali quando vengono costruiti e i client del servizio usano tali credenziali per autenticare le richieste al servizio.

La libreria di identità di Azure è incentrata sull'autenticazione OAuth con Azure Active Directory e offre varie classi di credenziali che possono acquisire un token AAD per autenticare le richieste di servizio. Tutte le classi di credenziali in questa libreria sono implementazioni della `TokenCredential` classe astratta in [Azure-Core][azure_core_library]ed è possibile usarle per creare client di servizio in grado di eseguire l'autenticazione con un `TokenCredential` .

Il `DefaultAzureCredential` è adatto per la maggior parte degli scenari in cui l'applicazione deve essere eseguita in ultima analisi nel cloud di Azure. `DefaultAzureCredential` combina le credenziali utilizzate comunemente per l'autenticazione durante la distribuzione, con le credenziali utilizzate per l'autenticazione in un ambiente di sviluppo. Per altre informazioni, inclusi gli esempi `DefaultAzureCredential` di utilizzo di, vedere la sezione relativa alle [credenziali di Azure predefinite](identity-azure-hosted-auth.md#default-azure-credential) relativa all' [autenticazione delle applicazioni Java ospitate in Azure](identity-azure-hosted-auth.md).

## <a name="examples"></a>Esempio

Come indicato in [usare Azure SDK per Java](overview.md#provision-and-manage-azure-resources-with-management-libraries), le librerie di gestione differiscono leggermente. Uno dei modi in cui si differenzia è che sono disponibili librerie per l' *utilizzo* di servizi di Azure, denominate librerie client e librerie per la *gestione* dei servizi di Azure, denominate librerie di gestione. Nelle sezioni seguenti è disponibile una rapida panoramica dell'autenticazione nelle librerie client e di gestione.

### <a name="authenticate-azure-client-libraries"></a>Autenticare le librerie client di Azure

Nell'esempio seguente viene illustrata l'autenticazione `SecretClient` di dalla libreria client [Azure-Security-Vault-Secrets][secrets_client_library] utilizzando `DefaultAzureCredential` .

```java
// Azure SDK client builders accept the credential as a parameter.
SecretClient client = new SecretClientBuilder()
  .vaultUrl("https://<your Key Vault name>.vault.azure.net")
  .credential(new DefaultAzureCredentialBuilder().build())
  .buildClient();
```

### <a name="authenticate-azure-management-libraries"></a>Autenticare le librerie di gestione di Azure

Le librerie di gestione di Azure usano le stesse API delle credenziali delle librerie client di Azure, ma richiedono anche un [ID sottoscrizione di Azure](/learn/modules/create-an-azure-account/4-multiple-subscriptions) per gestire le risorse di Azure nella sottoscrizione.

È possibile trovare gli ID sottoscrizione [nella pagina sottoscrizioni del portale di Azure](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade). In alternativa, usare il comando dell'interfaccia della riga di comando di [Azure][azure_cli] seguente per ottenere gli ID sottoscrizione:

```azurecli
az account list --output table
```

È possibile impostare l'ID sottoscrizione nella `AZURE_SUBSCRIPTION_ID` variabile di ambiente. Questo ID viene prelevato da `AzureProfile` come ID sottoscrizione predefinito durante la creazione di un' `Manager` istanza di, come illustrato nell'esempio seguente:

```java
AzureResourceManager azureResourceManager = AzureResourceManager.authenticate(
        new DefaultAzureCredentialBuilder().build(),
        new AzureProfile(AzureEnvironment.AZURE))
    .withDefaultSubscription();
```

L'oggetto `DefaultAzureCredential` utilizzato in questo esempio autentica un' `AzureResourceManager` istanza di utilizzando l'oggetto `DefaultAzureCredential` . È anche possibile usare altre implementazioni di credenziali di token offerte nella libreria di identità di Azure al posto di `DefaultAzureCredential` .

## <a name="troubleshooting"></a>Risoluzione dei problemi

Le credenziali generano eccezioni in caso di esito negativo dell'autenticazione o non possono eseguire l'autenticazione. Quando le credenziali non vengono autenticate, `ClientAuthenticationException` viene generato l'oggetto e ha un `message` attributo che descrive il motivo per cui l'autenticazione non è riuscita. Quando `ChainedTokenCredential` genera questa eccezione, l'esecuzione concatenata dell'elenco sottostante di credenziali viene arrestata.

Quando le credenziali non sono in grado di eseguire l'autenticazione perché una delle risorse sottostanti richieste dalla credenziale non è disponibile nel computer, `CredentialUnavailableException` viene generato e dispone di un `message` attributo che descrive il motivo per cui le credenziali non sono disponibili per l'esecuzione dell'autenticazione. Quando `ChainedTokenCredential` genera questa eccezione, il messaggio raccoglie i messaggi di errore da ogni credenziale nella catena.

## <a name="next-steps"></a>Passaggi successivi

Questo articolo ha introdotto la funzionalità di identità di Azure disponibile in Azure SDK per Java. In molti casi, è stato descritto `DefaultAzureCredential` come comune e appropriato. Gli articoli seguenti descrivono altri modi per eseguire l'autenticazione usando la libreria di identità di Azure e forniscono ulteriori informazioni su `DefaultAzureCredential` :

* [Autenticazione di Azure negli ambienti di sviluppo](identity-dev-env-auth.md)
* [Autenticazione di applicazioni ospitate in Azure](identity-azure-hosted-auth.md)
* [Autenticazione con entità servizio](identity-service-principal-auth.md)
* [Autenticazione con credenziali utente](identity-user-auth.md)

<!-- LINKS -->
[azure_cli]: /cli/azure
[azure_sub]: https://azure.microsoft.com/free/
[source]: https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/identity/azure-identity
[aad_doc]: /azure/active-directory/
[code_of_conduct]: https://opensource.microsoft.com/codeofconduct/
[keys_client_library]: https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/keyvault/azure-security-keyvault-keys
[logging]: https://github.com/Azure/azure-sdk-for-java/wiki/Logging-with-Azure-SDK
[secrets_client_library]: https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/keyvault/azure-security-keyvault-secrets
[eventhubs_client_library]: https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/eventhubs/azure-messaging-eventhubs
[azure_core_library]: https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/core
[javadoc]: https://azure.github.io/azure-sdk-for-java
[jdk_link]: /java/azure/jdk
