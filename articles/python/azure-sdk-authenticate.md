---
title: Come autenticare le applicazioni Python con i servizi di Azure
description: Come acquisire gli oggetti credenziali necessari per autenticare un'app Python con i servizi di Azure usando le librerie di Azure
ms.date: 09/18/2020
ms.topic: conceptual
ms.custom: devx-track-python
ms.openlocfilehash: e842e7530cc475e8431fbadfb3767ea56102c33e
ms.sourcegitcommit: 39f3f69e3be39e30df28421a30747f6711c37a7b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/21/2020
ms.locfileid: "90831907"
---
# <a name="how-to-authenticate-and-authorize-python-apps-on-azure"></a>Come autenticare e autorizzare le app Python in Azure

Quasi tutte le applicazione cloud distribuite in Azure devono accedere ad altre risorse di Azure, ad esempio le risorse di archiviazione, i database, i segreti archiviati e così via. Per accedere a tali risorse, l'applicazione deve essere autenticata e autorizzata:

- L'**autenticazione** verifica l'identità dell'app con Azure Active Directory.

- L'**autorizzazione** determina quali operazioni possono essere eseguite dall'app autenticata su una determinata risorsa. Le operazioni autorizzate sono definite dai **ruoli** assegnati all'identità dell'app per tale risorsa. In alcuni casi, ad esempio per Azure Key Vault, l'autorizzazione è determinata anche da **criteri di accesso** aggiuntivi assegnati all'identità dell'app.

Questo articolo illustra i dettagli di autenticazione e autorizzazione:

- Come assegnare un'identità all'app
- Come concedere autorizzazioni a un'identità
- Come e quando si verificano l'autenticazione e l'autorizzazione
- I diversi strumenti attraverso i quali un'app viene autenticata con Azure tramite le librerie di Azure. L'uso di `DefaultAzureCredential` non è obbligatorio, ma è consigliato.

## <a name="how-to-assign-an-app-identity"></a>Come assegnare un'identità all'app

In Azure un'identità dell'app è definita da un'**entità servizio**. Un'entità servizio è un tipo specifico di "entità di sicurezza" usato per identificare un'app o un servizio, ovvero una porzione di codice, anziché un utente o un gruppo di utenti.

L'entità servizio interessata dipende dalla posizione in cui è viene eseguita l'app, come descritto nelle sezioni seguenti.

### <a name="identity-when-running-the-app-on-azure"></a>Identità quando l'app viene eseguita in Azure

Quando è in esecuzione nel cloud, ad esempio nell'ambiente di produzione, un'app in genere usa un'**identità gestita assegnata dal sistema**. Con un'[identità gestita](/azure/active-directory/managed-identities-azure-resources/overview), quando si assegnano ruoli e autorizzazioni per le risorse viene usato il nome dell'app. Azure gestisce automaticamente l'entità servizio sottostante ed esegue automaticamente l'autenticazione dell'app con le altre risorse di Azure. Di conseguenza, non è necessario gestire direttamente l'entità servizio. Inoltre, dal momento che il codice dell'app non deve mai gestire i token di accesso, i segreti o le stringhe di connessione per le risorse di Azure, il rischio che tali informazioni possano essere divulgate o compromesse in altro modo risulta ridotto.

La configurazione dell'identità gestita dipende dal servizio usato per ospitare l'app. Per i collegamenti alle istruzioni relative a ogni servizio, vedere l'articolo [Servizi che supportano identità gestite](/azure/active-directory/managed-identities-azure-resources/services-support-managed-identities). Per le app Web distribuite nel Servizio app di Azure, ad esempio, l'identità gestita viene abilitata tramite l'opzione **Identità** > **Assegnata dal sistema** nel portale di Azure oppure usando il comando `az webapp identity assign` nell'interfaccia della riga di comando di Azure.

Se non è possibile usare l'identità gestita, l'applicazione può essere registrata manualmente con Azure Active Directory. La registrazione assegna all'app un'entità servizio, che viene usata per l'assegnazione di ruoli e autorizzazioni. Per altre informazioni, vedere [Registrare un'applicazione](/azure/active-directory/develop/quickstart-register-app).

### <a name="identity-when-running-the-app-locally"></a>Identità quando l'app viene eseguita in locale

Durante le attività di sviluppo, spesso si vuole eseguire il codice dell'app e il relativo debug in una workstation per sviluppatori, pur continuando a usare il codice per accedere alle risorse di Azure nel cloud. In questo caso, si crea un'entità servizio separata tramite Azure Active Directory specificamente per lo sviluppo in locale. Si assegnano nuovamente i ruoli e le autorizzazioni a questa entità servizio per le risorse in questione. In genere, questa identità di sviluppo viene autorizzata ad accedere solo alle risorse non di produzione.

Per informazioni dettagliate su come creare l'entità servizio locale e renderla disponibile per le librerie di Azure, vedere [Configurare l'ambiente di sviluppo locale](configure-local-development-environment.md). Dopo aver completato questa configurazione una tantum, è possibile eseguire lo stesso codice dell'app in locale e nel cloud senza apportare modifiche specifiche per l'ambiente.

Ogni sviluppatore deve avere la propria entità servizio che è protetta all'interno del proprio account utente nella propria workstation e che non deve mai essere archiviata in un repository del controllo del codice sorgente. Se un'entità servizio viene rubata o compromessa, è possibile eliminarla facilmente per revocare tutte le relative autorizzazioni e quindi crearla nuovamente per lo specifico sviluppatore. Per altre informazioni, vedere [Come gestire le entità servizio](how-to-manage-service-principals.md).

> [!NOTE]
> Sebbene sia possibile eseguire un'app usando le proprie credenziali utente di Azure, questa operazione non consente di stabilire le autorizzazioni specifiche per le risorse, necessarie per l'app quando viene distribuita nel cloud. È preferibile configurare un'entità servizio per lo sviluppo e assegnare a tale entità i ruoli e le autorizzazioni necessari, di cui è successivamente possibile replicare l'uso con l'identità gestita o l'entità servizio dell'app distribuita.

## <a name="assign-roles-and-permissions-to-an-identity"></a>Assegnare ruoli e autorizzazioni a un'identità

Quando si conoscono le identità dell'app sia in Azure sia quando viene eseguita in locale, si usa il controllo degli accessi in base al ruolo per concedere le autorizzazioni tramite il portale di Azure o l'interfaccia della riga di comando di Azure. Per i dettagli completi, vedere [Come assegnare le autorizzazioni per i ruoli a un'identità dell'app o a un'entità servizio](how-to-assign-role-permissions.md)

## <a name="when-does-authentication-and-authorization-occur"></a>Quando si verificano l'autenticazione e l'autorizzazione?

Per scrivere il codice dell'app con le librerie di Azure (SDK) per Python, usare il criterio seguente per accedere alle risorse di Azure:

1. Acquisire le credenziali, che descrivono l'identità dell'app, usando uno dei metodi descritti più avanti nell'articolo.

1. Usare le credenziali per acquisire un oggetto client per la risorsa di interesse. Ogni tipo di risorsa ha uno specifico oggetto client nelle librerie di Azure, in cui viene specificato l'URL della risorsa.

1. Provare ad accedere o a modificare la risorsa tramite l'oggetto client. Viene generata una richiesta HTTP all'API REST della risorsa. La chiamata API è il punto in cui Azure esegue l'autenticazione dell'identità dell'app e controlla l'autorizzazione.

Il codice seguente illustra questi passaggi nel tentativo di accedere ad Azure Key Vault.

```python
import os
from azure.identity import DefaultAzureCredential
from azure.keyvault.secrets import SecretClient

# Acquire the resource URL. In this code we assume the resource URL is in an
# environment variable, KEY_VAULT_URL in this case.

vault_url = os.environ["KEY_VAULT_URL"]


# Acquire a credential object for the app identity. When running in the cloud,
# DefaultAzureCredential uses the app's managed identity or user-assigned service principal.
# When run locally, DefaultAzureCredential relies on environment variables named
# AZURE_CLIENT_ID, AZURE_CLIENT_SECRET, and AZURE_TENANT_ID.

credential = DefaultAzureCredential()


# Acquire an appropriate client object for the resource identified by the URL. The
# client object only stores the given credential at this point but does not attempt
# to authenticate it.
#
# **NOTE**: SecretClient here is only an example; the same process applies to all
# other Azure client libraries.

secret_client = SecretClient(vault_url=vault_url, credential=credential)

# Attempt to perform an operation on the resource using the client object (in
# this case, retrieve a secret from Key Vault). The operation fails for any of
# the following reasons:
#
# 1. The information in the credential object is invalid (for example, the AZURE_CLIENT_ID
#    environment variable cannot be found).
# 2. The app identity cannot be authenticated using the information in the credential object.
# 3. The app identity is not authorized to perform the requested operation on the
#    resource (identified in this case by the vault_url.

retrieved_secret = secret_client.get_secret("secret-name-01")
```

Anche in questo caso non viene eseguita alcuna autenticazione o autorizzazione finché il codice non invia una richiesta specifica all'API REST di Azure tramite un oggetto client. L'istruzione per creare `DefaultAzureCredential` (vedere la sezione successiva) crea solo un oggetto lato client in memoria, ma non esegue altri controlli. 

La creazione dell'oggetto [`SecretClient`](/python/api/azure-keyvault-secrets/azure.keyvault.secrets.secretclient) dell'SDK non prevede nemmeno comunicazioni con la risorsa in questione. L'oggetto `SecretClient` è semplicemente un wrapper per l'API REST di Azure sottostante ed esiste solo nella memoria di runtime dell'app. 

È solo quando il codice chiama il metodo [`get_secret`](/python/api/azure-keyvault-secrets/azure.keyvault.secrets.secretclient#get-secret-name--version-none----kwargs-) che l'oggetto client genera la chiamata API REST appropriata per Azure. L'endpoint di Azure per `get_secret` autentica quindi l'identità del chiamante e controlla l'autorizzazione.

## <a name="authenticate-with-defaultazurecredential"></a>Eseguire l'autenticazione con DefaultAzureCredential

Per la maggior parte delle applicazioni, la classe [`DefaultAzureCredential`](/python/api/azure-identity/azure.identity.defaultazurecredential) della libreria [`azure-identity`](/python/api/azure-identity/azure.identity) fornisce il metodo di autenticazione più semplice e consigliato. `DefaultAzureCredential` usa automaticamente l'identità gestita dell'app nel cloud e carica automaticamente un'entità servizio locale dalle variabili di ambiente durante l'esecuzione in locale.

```python
import os
from azure.identity import DefaultAzureCredential
from azure.keyvault.secrets import SecretClient

# Acquire the resource URL
vault_url = os.environ["KEY_VAULT_URL"]

# Aquire a credential object
credential = DefaultAzureCredential()

# Acquire a client object
secret_client = SecretClient(vault_url=vault_url, credential=credential)

# Attempt to perform an operation
retrieved_secret = secret_client.get_secret("secret-name-01")
```

Il codice precedente usa un oggetto `DefaultAzureCredential` per l'accesso ad Azure Key Vault, il cui URL è disponibile in una variabile di ambiente denominata `KEY_VAULT_URL`. Il codice implementa chiaramente il criterio di utilizzo tipico della libreria: acquisire un oggetto credenziali, creare un oggetto client appropriato per la risorsa di Azure e quindi tentare di eseguire un'operazione su tale risorsa usando l'oggetto client. Anche in questo caso, l'autenticazione e l'autorizzazione non avvengono fino al passaggio finale.

Quando il codice viene distribuito ed eseguito in Azure, `DefaultAzureCredential` usa automaticamente l'identità gestita assegnata dal sistema che è possibile abilitare per l'app all'interno di qualsiasi servizio la ospiti. Le autorizzazioni per risorse specifiche, ad esempio Archiviazione di Azure o Azure Key Vault, vengono assegnate a tale identità tramite il portale di Azure o l'interfaccia della riga di comando di Azure. In questi casi, questa identità gestita da Azure ottimizza la sicurezza, perché non si dovrà mai gestire un'entità servizio esplicita nel codice.

Quando si esegue il codice in locale, `DefaultAzureCredential` usa automaticamente l'entità servizio descritta dalle variabili di ambiente denominate `AZURE_TENANT_ID`, `AZURE_CLIENT_ID` e `AZURE_CLIENT_SECRET`. L'oggetto client include quindi questi valori (in modo sicuro) nell'intestazione della richiesta HTTP quando chiama l'endpoint API. Non è necessaria alcuna modifica al codice durante l'esecuzione in locale o nel cloud. Per informazioni dettagliate sulla creazione dell'entità servizio e sulla configurazione delle variabili di ambiente, vedere [Configurare l'ambiente di sviluppo Python locale per Azure - Configurare l'autenticazione](configure-local-development-environment.md#configure-authentication).

In entrambi i casi, all'identità interessata è necessario assegnare le autorizzazioni per la risorsa appropriata. Il processo generale è descritto nella sezione [Come assegnare le autorizzazioni per i ruoli](how-to-assign-role-permissions.md). Le specifiche sono disponibili nella documentazione relativa ai singoli servizi. Per informazioni dettagliate sulle autorizzazioni di Key Vault, ad esempio quelle necessarie per il codice precedente, vedere [Fornire un'autenticazione di Key Vault con un criterio di controllo di accesso](/azure/key-vault/general/group-permissions-for-apps).

<a name="cli-auth-note"></a>
> [!IMPORTANT]
> In futuro, `DefaultAzureCredential` userà l'identità che ha eseguito l'accesso all'interfaccia della riga di comando di Azure tramite `az login`, se le variabili di ambiente dell'entità servizio non sono disponibili. Se si è il proprietario o l'amministratore della sottoscrizione, il risultato pratico di questa funzionalità è che il codice ha accesso intrinseco alla maggior parte delle risorse nella sottoscrizione senza la necessità di assegnare autorizzazioni specifiche. Questo comportamento è utile per la sperimentazione. Tuttavia, è consigliabile usare specifiche entità servizio e assegnare specifiche autorizzazioni quando si inizia a scrivere codice di produzione, perché si saprà come assegnare autorizzazioni esatte a identità diverse e sarà possibile convalidare accuratamente tali autorizzazioni negli ambienti di test prima della distribuzione in produzione.

### <a name="using-defaultazurecredential-with-sdk-management-libraries"></a>Uso di DefaultAzureCredential con le librerie di gestione dell'SDK

```python
# WARNING: this code fails with azure-mgmt-resource versions < 15

from azure.identity import DefaultAzureCredential

# azure.mgmt.resource is an Azure SDK management library
from azure.mgmt.resource import SubscriptionClient

# Attempt to retrieve the subscription ID
credential = DefaultAzureCredential()
subscription_client = SubscriptionClient(credential)

# If using azure-mgmt-resource < version 15 the following line produces
# a "no attribute 'signed_session'" error:
subscription = next(subscription_client.subscriptions.list())

print(subscription.subscription_id)
```

`DefaultAzureCredential` è compatibile solo con le librerie client di Azure SDK ("piano dati") e con le versioni aggiornate delle librerie di gestione di Azure SDK incluse nell'elenco [Librerie che usano azure.core](azure-sdk-library-package-index.md#libraries-using-azurecore).

Se si esegue il codice precedente con la versione 15.0.0 o successiva di Azure-Mgmt-Resource, la chiamata a `subscription_client.subscriptions.list()` riesce. Se si usa una versione precedente della libreria, la chiamata non riesce e viene restituito un messaggio di errore vago, simile a "All'oggetto 'DefaultAzureCredential' non è associato alcun attributo 'signed_session'". Questo errore si verifica perché le versioni precedenti delle librerie di gestione dell'SDK presuppongono che l'oggetto credenziali contenga una proprietà `signed_session`, che risulta mancante in `DefaultAzureCredential`.

Per ovviare all'errore, usare le versioni più recenti delle librerie di gestione incluse nell'elenco [Librerie che usano azure.core](azure-sdk-library-package-index.md#libraries-using-azurecore). Quando sono elencate due librerie, usare quella con il numero di versione maggiore. Le pagine pypi per le librerie aggiornate includono inoltre una riga simile a "Il sistema di credenziali è stato completamente rinnovato" per indicare la modifica.

Se la libreria di gestione che si vuole usare non è ancora stata aggiornata, è possibile usare i metodi alternativi seguenti:

1. Usare uno degli altri metodi di autenticazione descritti nelle sezioni successive di questo articolo, che può funzionare correttamente per il codice che usa *solo* le librerie di gestione dell'SDK e che non verrà distribuito nel cloud, nel qual caso è possibile fare affidamento esclusivamente sulle entità servizio locali.

1. Invece di `DefaultAzureCredential`, usare la [classe CredentialWrapper (cred_wrapper.py)](https://gist.github.com/lmazuel/cc683d82ea1d7b40208de7c9fc8de59d) fornita da un membro del team di progettazione di Azure SDK. Quando la libreria di gestione desiderata risulta disponibile, tornare a `DefaultAzureCredential`. Il vantaggio di questo metodo è che è possibile usare le stesse credenziali sia per il client che per le librerie di gestione dell'SDK e che funziona sia in locale che nel cloud.

    Supponendo che sia stata scaricata una copia di *cred_wrapper.py* nella cartella del progetto, il codice precedente sarà come segue:

    ```python
    from cred_wrapper import CredentialWrapper
    from azure.mgmt.resource import SubscriptionClient

    credential = CredentialWrapper()
    subscription_client = SubscriptionClient(credential)
    subscription = next(subscription_client.subscriptions.list())
    print(subscription.subscription_id)
    ```

    Anche in questo caso, quando le librerie di gestione aggiornate saranno disponibili, è possibile usare direttamente `DefaultAzureCredential`, come illustrato nell'esempio di codice originale.

## <a name="other-authentication-methods"></a>Altri metodi di autenticazione

Anche se `DefaultAzureCredential` è il metodo di autenticazione consigliato per la maggior parte degli scenari, sono disponibili altri metodi con le limitazioni seguenti:

- La maggior parte dei metodi funziona con entità servizio esplicite e non sfrutta i vantaggi dell'identità gestita per il codice distribuito nel cloud. Se usati con il codice di produzione, quindi, è necessario gestire e mantenere entità servizio distinte per le applicazioni cloud.

- Alcuni metodi, come l'autenticazione basata su interfaccia della riga di comando, funzionano solo con script locali e non possono essere usati con il codice di produzione.

Le entità servizio per le applicazioni distribuite nel cloud vengono gestite nelle istanze di Active Directory delle sottoscrizioni. Per altre informazioni, vedere [Come gestire le entità servizio](how-to-manage-service-principals.md).

In tutti i casi, l'entità servizio o l'utente appropriato deve avere le autorizzazioni adatte per le risorse e l'operazione in questione.

### <a name="authenticate-with-a-json-file"></a>Eseguire l'autenticazione con un file JSON

Con questo metodo si crea un file JSON che contiene le credenziali necessarie per l'entità servizio. Si crea quindi un oggetto client SDK usando tale file. Questo metodo può essere usato sia in locale che nel cloud.

1. Creare un file JSON con il formato seguente:

    ```json
    {
        "subscriptionId": "<azure_aubscription_id>",
        "tenantId": "<tenant_id>",
        "clientId": "<application_id>",
        "clientSecret": "<application_secret>",
        "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
        "resourceManagerEndpointUrl": "https://management.azure.com/",
        "activeDirectoryGraphResourceId": "https://graph.windows.net/",
        "sqlManagementEndpointUrl": "https://management.core.windows.net:8443/",
        "galleryEndpointUrl": "https://gallery.azure.com/",
        "managementEndpointUrl": "https://management.core.windows.net/"
    }
    ```

    Sostituire i quattro segnaposto con l'ID sottoscrizione di Azure, l'ID tenant, l'ID client e il segreto client.

    > [!TIP]
    > Come illustrato in [Configurare l'ambiente di sviluppo locale](configure-local-development-environment.md#create-a-service-principal-and-environment-variables-for-development), è possibile usare il comando [az ad sp create-for-rbac](/cli/azure/ad/sp#az-ad-sp-create-for-rbac) con il parametro `--sdk-auth` per generare direttamente questo formato JSON.

1. Salvare il file con un nome come *credentials.json* in una posizione sicura accessibile al codice. Per proteggere le credenziali, assicurarsi di omettere questo file dal controllo del codice sorgente e di non condividerlo con altri sviluppatori. Ovvero l'ID tenant, l'ID client e il segreto client di un'entità servizio devono rimanere sempre isolati nella workstation di sviluppo.

1. Creare una variabile di ambiente denominata `AZURE_AUTH_LOCATION` con il percorso del file JSON come valore:

    # <a name="cmd"></a>[cmd](#tab/cmd)

    ```cmd
    set AZURE_AUTH_LOCATION=../credentials.json
    ```

    # <a name="bash"></a>[Bash](#tab/bash)

    ```bash
    AZURE_AUTH_LOCATION="../credentials.json"
    ```

    ---

    Questi esempi presuppongono che il file JSON sia denominato *credentials.json* e che si trovi nella cartella padre del progetto.

1. Usare il metodo [get_client_from_auth_file](/python/api/azure-common/azure.common.client_factory#get-client-from-auth-file-client-class--auth-path-none----kwargs-) per creare l'oggetto client:

    ```python
    from azure.common.client_factory import get_client_from_auth_file
    from azure.mgmt.resource import SubscriptionClient

    # This form of get_client_from_auth_file relies on the AZURE_AUTH_LOCATION
    # environment variable.
    subscription_client = get_client_from_auth_file(SubscriptionClient)

    subscription = next(subscription_client.subscriptions.list())
    print(subscription.subscription_id)
    ```

In alternativa, è possibile specificare il percorso direttamente nel codice usando l'argomento `auth_path`, nel qual caso la variabile di ambiente non è necessaria:

```python
subscription_client = get_client_from_auth_file(SubscriptionClient, auth_path="../credentials.json")
```

### <a name="authenticate-with-a-json-dictionary"></a>Eseguire l'autenticazione con un dizionario JSON

```python
import os
from azure.common.client_factory import get_client_from_json_dict
from azure.mgmt.resource import SubscriptionClient

# Retrieve the IDs and secret to use in the JSON dictionary
subscription_id = os.environ["AZURE_SUBSCRIPTION_ID"]
tenant_id = os.environ["AZURE_TENANT_ID"]
client_id = os.environ["AZURE_CLIENT_ID"]
client_secret = os.environ["AZURE_CLIENT_SECRET"]

config_dict = {
   "subscriptionId": subscription_id,
    "tenantId": tenant_id,
   "clientId": client_id,
   "clientSecret": client_secret,
   "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
   "resourceManagerEndpointUrl": "https://management.azure.com/",
   "activeDirectoryGraphResourceId": "https://graph.windows.net/",
   "sqlManagementEndpointUrl": "https://management.core.windows.net:8443/",
   "galleryEndpointUrl": "https://gallery.azure.com/",
   "managementEndpointUrl": "https://management.core.windows.net/"
}

subscription_client = get_client_from_json_dict(SubscriptionClient, config_dict)

subscription = next(subscription_client.subscriptions.list())
print(subscription.subscription_id)
```

Invece di usare un file, come descritto nella sezione precedente, è possibile creare i dati JSON necessari in una variabile e chiamare [get_client_from_json_dict](/python/api/azure-common/azure.common.client_factory#get-client-from-json-dict-client-class--config-dict----kwargs-). Questo codice presuppone che siano state create le variabili di ambiente descritte in [Configurare l'ambiente di sviluppo locale](configure-local-development-environment.md#create-a-service-principal-and-environment-variables-for-development). Per il codice distribuito nel cloud, è possibile creare queste variabili di ambiente nella macchina virtuale server o tra le impostazioni dell'applicazione quando si usa un servizio della piattaforma come il servizio app di Azure e Funzioni di Azure.

È anche possibile archiviare i valori in Azure Key Vault e recuperarli in fase di esecuzione invece di usare variabili di ambiente.

### <a name="authenticate-with-token-credentials"></a>Eseguire l'autenticazione con le credenziali del token

```python
import os
from azure.mgmt.resource import SubscriptionClient
from azure.common.credentials import ServicePrincipalCredentials

# Retrieve the IDs and secret to use with ServicePrincipalCredentials
subscription_id = os.environ["AZURE_SUBSCRIPTION_ID"]
tenant_id = os.environ["AZURE_TENANT_ID"]
client_id = os.environ["AZURE_CLIENT_ID"]
client_secret = os.environ["AZURE_CLIENT_SECRET"]

credential = ServicePrincipalCredentials(tenant=tenant_id, client_id=client_id, secret=client_secret)

subscription_client = SubscriptionClient(credential)

subscription = next(subscription_client.subscriptions.list())
print(subscription.subscription_id)
```

Con questo metodo si crea un oggetto [`ServicePrincipalCredentials`](/python/api/msrestazure/msrestazure.azure_active_directory.serviceprincipalcredentials) usando le credenziali ottenute da una risorsa di archiviazione sicura, come Azure Key Vault o le variabili di ambiente. Il codice precedente presuppone che siano state create le variabili di ambiente descritte in [Configurare l'ambiente di sviluppo locale](configure-local-development-environment.md#create-a-service-principal-and-environment-variables-for-development).

Con questo metodo, è possibile usare un [cloud sovrano o nazionale di Azure](/azure/active-directory/develop/authentication-national-cloud) invece del cloud pubblico di Azure, specificando un argomento `base_url` per l'oggetto client:

```python
from msrestazure.azure_cloud import AZURE_CHINA_CLOUD

#...

subscription_client = SubscriptionClient(credentials, base_url=AZURE_CHINA_CLOUD.endpoints.resource_manager)
```

Le costanti del cloud sovrano sono disponibili nella [libreria msrestazure.azure_cloud](https://github.com/Azure/msrestazure-for-python/blob/master/msrestazure/azure_cloud.py).

### <a name="authenticate-with-token-credentials-and-an-adal-context"></a>Eseguire l'autenticazione con credenziali token e un contesto ADAL

Se è necessario un maggior controllo quando si usano credenziali di tipo token, usare [Azure Active Directory Authentication Library (ADAL) per Python](https://github.com/AzureAD/azure-activedirectory-library-for-python) e il wrapper ADAL SDK:

```python
import os, adal
from azure.mgmt.resource import SubscriptionClient
from msrestazure.azure_active_directory import AdalAuthentication
from msrestazure.azure_cloud import AZURE_PUBLIC_CLOUD

# Retrieve the IDs and secret to use with ServicePrincipalCredentials
subscription_id = os.environ["AZURE_SUBSCRIPTION_ID"]
tenant_id = os.environ["AZURE_TENANT_ID"]
client_id = os.environ["AZURE_CLIENT_ID"]
client_secret = os.environ["AZURE_CLIENT_SECRET"]

LOGIN_ENDPOINT = AZURE_PUBLIC_CLOUD.endpoints.active_directory
RESOURCE = AZURE_PUBLIC_CLOUD.endpoints.active_directory_resource_id

context = adal.AuthenticationContext(LOGIN_ENDPOINT + '/' + tenant_id)

credential = AdalAuthentication(context.acquire_token_with_client_credentials,
    RESOURCE, client_id, client_secret)

subscription_client = SubscriptionClient(credential)

subscription = next(subscription_client.subscriptions.list())
print(subscription.subscription_id)
```

Se è necessaria la libreria ADAL, eseguire `pip install adal`.

Con questo metodo, è possibile usare un [cloud sovrano o nazionale di Azure](/azure/active-directory/develop/authentication-national-cloud) invece del cloud pubblico di Azure.

```python
from msrestazure.azure_cloud import AZURE_CHINA_CLOUD

# ...

LOGIN_ENDPOINT = AZURE_CHINA_CLOUD.endpoints.active_directory
RESOURCE = AZURE_CHINA_CLOUD.endpoints.active_directory_resource_id
```

È sufficiente sostituire `AZURE_PUBLIC_CLOUD` con la costante del cloud sovrano appropriata della [libreria msrestazure.azure_cloud](https://github.com/Azure/msrestazure-for-python/blob/master/msrestazure/azure_cloud.py).

### <a name="cli-based-authentication-development-purposes-only"></a>Autenticazione basata sull'interfaccia della riga di comando (solo a scopo di sviluppo)

```python
from azure.common.client_factory import get_client_from_cli_profile
from azure.mgmt.resource import SubscriptionClient

subscription_client = get_client_from_cli_profile(SubscriptionClient)

subscription = next(subscription_client.subscriptions.list())
print(subscription.subscription_id)
```

Con questo metodo si crea un oggetto client usando le credenziali dell'utente che ha eseguito l'accesso con il comando `az login` dell'interfaccia della riga di comando di Azure. L'applicazione verrà autorizzata per tutte le operazioni dell'utente.

L'SDK usa l'ID sottoscrizione predefinito oppure è possibile impostare la sottoscrizione prima di eseguire il codice usando [`az account`](/cli/azure/manage-azure-subscriptions-azure-cli). Se è necessario fare riferimento a sottoscrizioni diverse nello stesso script, usare i metodi ['get_client_from_auth_file'](#authenticate-with-a-json-file) o [`get_client_from_json_dict`](#authenticate-with-a-json-dictionary) descritti in precedenza in questo articolo.

La funzione `get_client_from_cli_profile` deve essere usata solo per le fasi iniziali di sperimentazione e sviluppo, perché un utente che ha eseguito l'accesso ha in genere privilegi di proprietario o amministratore e può accedere alla maggior parte delle risorse senza autorizzazioni aggiuntive. Per altre informazioni, vedere la nota precedente sull'[uso delle credenziali dell'interfaccia della riga di comando con `DefaultAzureCredential`](#cli-auth-note).

### <a name="deprecated-authenticate-with-userpasscredentials"></a>Deprecato: Eseguire l'autenticazione con UserPassCredentials

Quando [Azure Active Directory Authentication Library (ADAL) per Python](https://github.com/AzureAD/azure-activedirectory-library-for-python) non era ancora disponibile, era necessario usare la classe [`UserPassCredentials`](/python/api/msrestazure/msrestazure.azure_active_directory.userpasscredentials) ora deprecata. Questa classe non supporta l'autenticazione a due fattori e non deve più essere usata.

## <a name="see-also"></a>Vedere anche

- [Configurare l'ambiente di sviluppo Python locale per Azure](configure-local-development-environment.md)
- [Come assegnare le autorizzazioni per i ruoli](how-to-assign-role-permissions.md)
- [Esempio: Effettuare il provisioning di un gruppo di risorse](azure-sdk-example-resource-group.md)
- [Esempio: Effettuare il provisioning e usare Archiviazione di Azure](azure-sdk-example-storage.md)
- [Esempio: Effettuare il provisioning di un'app Web e distribuire il codice](azure-sdk-example-web-app.md)
- [Esempio: Effettuare il provisioning e usare un database MySQL](azure-sdk-example-database.md)
- [Esempio: Effettuare il provisioning di una macchina virtuale](azure-sdk-example-virtual-machines.md)