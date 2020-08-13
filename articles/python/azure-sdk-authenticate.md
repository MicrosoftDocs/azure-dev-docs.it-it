---
title: Come autenticare le applicazioni Python con i servizi di Azure
description: Come acquisire gli oggetti credenziali necessari per autenticare un'app Python con i servizi di Azure usando le librerie di Azure
ms.date: 05/12/2020
ms.topic: conceptual
ms.custom: devx-track-python
ms.openlocfilehash: 08636d4a9b8b0b93b6e448b919a14cbfc3ae2a96
ms.sourcegitcommit: 980efe813d1f86e7e00929a0a3e1de83514ad7eb
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/07/2020
ms.locfileid: "87982673"
---
# <a name="how-to-authenticate-python-apps-with-azure-services"></a>Come autenticare le app Python con i servizi di Azure

Per scrivere codice dell'app con le librerie di Azure per Python, usare il modello seguente per accedere alle risorse di Azure:

1. Acquisire le credenziali (in genere un'operazione una tantum).
1. Usare le credenziali per acquisire l'oggetto cliente appropriato per una risorsa.
1. Provare ad accedere o a modificare la risorsa tramite l'oggetto client. Viene generata una richiesta HTTP all'API REST della risorsa.

La richiesta all'API REST è il punto in cui Azure autentica l'identità dell'app come descritto dall'oggetto credenziali. Azure controlla quindi se tale identità è autorizzata a eseguire l'azione richiesta. Se l'identità non ha l'autorizzazione, l'operazione non riesce. La concessione delle autorizzazioni dipende dal tipo di risorsa, ad esempio Azure Key Vault, Archiviazione di Azure e così via. Per altre informazioni, vedere la documentazione relativa al tipo di risorsa specifico.

L'identità coinvolta in questi processi, ovvero quella descritta dall'oggetto credenziali, è in genere definita da un'*entità di sicurezza* che rappresenta un utente, un gruppo, un servizio o un'app. Diversi metodi di autenticazione descritti in questo articolo usano un'entità esplicita, che in genere viene definita *entità servizio*.

Per la maggior parte delle applicazioni cloud, tuttavia, è consigliabile usare l'oggetto `DefaultAzureCredential`, come illustrato nella prima sezione, perché elimina completamente la necessità di gestire un'entità servizio per l'applicazione.

[!INCLUDE [chrome-note](includes/chrome-note.md)]

## <a name="authenticate-with-defaultazurecredential"></a>Eseguire l'autenticazione con DefaultAzureCredential

```python
import os
from azure.identity import DefaultAzureCredential
from azure.keyvault.secrets import SecretClient

# Obtain the credential object. When run locally, DefaultAzureCredential relies
# on environment variables named AZURE_CLIENT_ID, AZURE_CLIENT_SECRET, and AZURE_TENANT_ID.
credential = DefaultAzureCredential()

# Create the client object using the credential
#
# **NOTE**: SecretClient here is only an example; the same process
# applies to all other Azure client libraries.

vault_url = os.environ["KEY_VAULT_URL"]
secret_client = SecretClient(vault_url=vault_url, credential=credential)

# Attempt to retrieve a secret value. The operation fails if the principal
# cannot be authenticated or is not authorized for the operation in question.
retrieved_secret = client.get_secret("secret-name-01")
```

La classe [`DefaultAzureCredential`](/python/api/azure-identity/azure.identity.defaultazurecredential?view=azure-python) della libreria [`azure-identity`](/python/api/azure-identity/azure.identity?view=azure-python) fornisce il metodo di autenticazione più semplice e consigliato.

Il codice precedente usa `DefaultAzureCredential` per l'accesso a un'istanza di Azure Key Vault il cui URL è disponibile in una variabile di ambiente denominata `KEY_VAULT_URL`. Il codice implementa chiaramente il modello descritto all'inizio dell'articolo: acquisire un oggetto credenziali, creare un oggetto client SDK, quindi provare a eseguire un'operazione con tale oggetto client.

Anche in questo caso, l'autenticazione e l'autorizzazione non avvengono fino al passaggio finale. La creazione dell'oggetto [`SecretClient`](/python/api/azure-keyvault-secrets/azure.keyvault.secrets.secretclient?view=azure-python) SDK non implica alcuna comunicazione con la risorsa in questione. L'oggetto `SecretClient` è semplicemente un wrapper intorno all'API REST di Azure sottostante ed esiste solo nella memoria di runtime dell'app. È solo quando si chiama il metodo [`get_secret`](/python/api/azure-keyvault-secrets/azure.keyvault.secrets.secretclient?view=azure-python#get-secret-name--version-none----kwargs-) che l'oggetto client genera la chiamata API REST appropriata ad Azure. L'endpoint di Azure per `get_secret` autentica quindi l'identità del chiamante e controlla l'autorizzazione.

Quando il codice viene distribuito ed è in esecuzione in Azure, `DefaultAzureCredential` usa automaticamente l'identità gestita assegnata dal sistema che è possibile abilitare per l'app all'interno di qualsiasi servizio la ospiti. Per un'app Web distribuita nel servizio app di Azure, ad esempio, l'identità gestita si abilita tramite l'opzione **Identità** > **Assegnata dal sistema** nel portale di Azure oppure usando il comando `az webapp identity assign` dell'interfaccia della riga di comando di Azure. Anche le autorizzazioni per risorse specifiche, ad esempio Archiviazione di Azure o Azure Key Vault, vengono assegnate a tale identità tramite il portale di Azure o l'interfaccia della riga di comando di Azure. In questi casi, questa identità gestita da Azure ottimizza la sicurezza, perché non si dovrà mai gestire un'entità servizio esplicita nel codice.

Quando si esegue il codice in locale, `DefaultAzureCredential` usa automaticamente l'entità servizio descritta dalle variabili di ambiente denominate `AZURE_TENANT_ID`, `AZURE_CLIENT_ID` e `AZURE_CLIENT_SECRET`. L'oggetto client SDK include quindi questi valori (in modo sicuro) nell'intestazione della richiesta HTTP quando chiama l'endpoint API. Non è necessario apportare modifiche al codice. Per informazioni dettagliate sulla creazione dell'entità servizio e sulla configurazione delle variabili di ambiente, vedere [Configurare l'ambiente di sviluppo Python locale per Azure - Configurare l'autenticazione](configure-local-development-environment.md#configure-authentication).

In entrambi i casi, all'identità coinvolta è necessario assegnare le autorizzazioni per la risorsa appropriata, come descritto nella documentazione relativa ai singoli servizi. Per informazioni dettagliate sulle autorizzazioni di Key Vault, che sarebbero necessarie per il codice precedente, vedere [Fornire un'autenticazione di Key Vault con un criterio di controllo di accesso](/azure/key-vault/general/group-permissions-for-apps).

<a name="cli-auth-note"></a>
> [!IMPORTANT]
> In futuro, `DefaultAzureCredential` userà l'identità che ha eseguito l'accesso all'interfaccia della riga di comando di Azure tramite `az login`, se le variabili di ambiente dell'entità servizio non sono disponibili. Se si è il proprietario o l'amministratore della sottoscrizione, il risultato pratico di questa funzionalità è che il codice ha accesso intrinseco alla maggior parte delle risorse nella sottoscrizione senza la necessità di assegnare autorizzazioni specifiche. Questo comportamento è utile per la sperimentazione. Tuttavia, è consigliabile usare specifiche entità servizio e assegnare specifiche autorizzazioni quando si inizia a scrivere codice di produzione, perché si saprà come assegnare autorizzazioni esatte a identità diverse e sarà possibile convalidare accuratamente tali autorizzazioni negli ambienti di test prima della distribuzione in produzione.

### <a name="using-defaultazurecredential-with-sdk-management-libraries"></a>Uso di DefaultAzureCredential con le librerie di gestione dell'SDK

```python
# WARNING: this code presently fails with current release libraries!

from azure.identity import DefaultAzureCredential

# azure.mgmt.resource is an Azure SDK management library
from azure.mgmt.resource import SubscriptionClient

# Attempt to retrieve the subscription ID
credential = DefaultAzureCredential()
subscription_client = SubscriptionClient(credential)

# The following line produces a "no attribute 'signed_session'" error:
subscription = next(subscription_client.subscriptions.list())

print(subscription.subscription_id)
```

Attualmente, `DefaultAzureCredential` è compatibile solo con le librerie client di Azure SDK ("piano dati") e non funziona con le librerie di gestione di Azure SDK, ovvero l'ultima versione di anteprima delle librerie i cui nomi iniziano con `azure-mgmt`, come illustrato in questo esempio di codice. Ovvero, con le librerie della versione corrente, la chiamata a `subscription_client.subscriptions.list()` non riesce con un messaggio di errore piuttosto vago, analogo a "All'oggetto 'DefaultAzureCredential' non è associato alcun attributo 'signed_session'". Questo errore si verifica perché le librerie di gestione correnti dell'SDK presuppongono che l'oggetto credenziali contenga una proprietà `signed_session`, che risulta mancante in `DefaultAzureCredential`.

È possibile risolvere l'errore usando le librerie di gestione di anteprima, come descritto nel post di blog [Introduzione alle nuove anteprime per le librerie di gestione di Azure](https://devblogs.microsoft.com/azure-sdk/introducing-new-previews-for-azure-management-libraries/).

In alternativa, è possibile usare i metodi seguenti:

1. Usare uno degli altri metodi di autenticazione descritti nelle sezioni successive di questo articolo, che può funzionare correttamente per il codice che usa *solo* le librerie di gestione dell'SDK e che non verrà distribuito nel cloud, nel qual caso è possibile fare affidamento esclusivamente sulle entità servizio locali.

1. Invece di `DefaultAzureCredential`, usare la [classe CredentialWrapper (cred_wrapper.py)](https://gist.github.com/lmazuel/cc683d82ea1d7b40208de7c9fc8de59d) fornita da un membro del team di progettazione di Azure SDK. Una volta terminata la fase di anteprima delle librerie di gestione aggiornate, sarà possibile riprendere semplicemente a usare `DefaultAzureCredential`. Il vantaggio di questo metodo è che è possibile usare le stesse credenziali sia per il client che per le librerie di gestione dell'SDK e che funziona sia in locale che nel cloud.

    Supponendo che sia stata scaricata una copia di *cred_wrapper.py* nella cartella del progetto, il codice precedente sarà come segue:

    ```python
    from cred_wrapper import CredentialWrapper
    from azure.mgmt.resource import SubscriptionClient

    credential = CredentialWrapper()
    subscription_client = SubscriptionClient(credential)
    subscription = next(subscription_client.subscriptions.list())
    print(subscription.subscription_id)
    ```

    Anche in questo caso, una volta terminata la fase di anteprima delle librerie di gestione aggiornate, sarà possibile usare direttamente `DefaultAzureCredential`.

## <a name="other-authentication-methods"></a>Altri metodi di autenticazione

Anche se `DefaultAzureCredential` è il metodo di autenticazione consigliato per la maggior parte degli scenari, sono disponibili altri metodi con le limitazioni seguenti:

- La maggior parte dei metodi funziona con entità servizio esplicite e non sfrutta i vantaggi dell'identità gestita per il codice distribuito nel cloud. Se usati con il codice di produzione, quindi, è necessario gestire e mantenere entità servizio distinte per le applicazioni cloud.

- Alcuni metodi, come l'autenticazione basata su interfaccia della riga di comando, funzionano solo con script locali e non possono essere usati con il codice di produzione.

Le entità servizio per le applicazioni distribuite nel cloud vengono gestite nelle istanze di Active Directory delle sottoscrizioni. Per altre informazioni, vedere [Come gestire le entità servizio](how-to-manage-service-principals.md).

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
    > Come illustrato in [Configurare l'ambiente di sviluppo locale](configure-local-development-environment.md#create-a-service-principal-and-environment-variables-for-development), è possibile usare il comando [az ad sp create-for-rbac](/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac) con il parametro `--sdk-auth` per generare direttamente questo formato JSON.

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


1. Usare il metodo [get_client_from_auth_file](/python/api/azure-common/azure.common.client_factory?view=azure-python#get-client-from-auth-file-client-class--auth-path-none----kwargs-) per creare l'oggetto client:

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

Invece di usare un file, come descritto nella sezione precedente, è possibile creare i dati JSON necessari in una variabile e chiamare [get_client_from_json_dict](/python/api/azure-common/azure.common.client_factory?view=azure-python#get-client-from-json-dict-client-class--config-dict----kwargs-). Questo codice presuppone che siano state create le variabili di ambiente descritte in [Configurare l'ambiente di sviluppo locale](configure-local-development-environment.md#create-a-service-principal-and-environment-variables-for-development). Per il codice distribuito nel cloud, è possibile creare queste variabili di ambiente nella macchina virtuale server o tra le impostazioni dell'applicazione quando si usa un servizio della piattaforma come il servizio app di Azure e Funzioni di Azure.

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

Con questo metodo si crea un oggetto [`ServicePrincipalCredentials`](/python/api/msrestazure/msrestazure.azure_active_directory.serviceprincipalcredentials?view=azure-python) usando le credenziali ottenute da una risorsa di archiviazione sicura, come Azure Key Vault o le variabili di ambiente. Il codice precedente presuppone che siano state create le variabili di ambiente descritte in [Configurare l'ambiente di sviluppo locale](configure-local-development-environment.md#create-a-service-principal-and-environment-variables-for-development).

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

Con questo metodo si crea un oggetto client usando le credenziali dell'utente che ha eseguito l'accesso con il comando `az login` dell'interfaccia della riga di comando di Azure.

L'SDK usa l'ID sottoscrizione predefinito oppure è possibile impostare la sottoscrizione usando [`az account`](https://docs.microsoft.com/cli/azure/manage-azure-subscriptions-azure-cli)

Questo metodo dovrebbe essere usato solo per le fasi iniziali di sperimentazione e sviluppo, perché un utente che ha eseguito l'accesso ha in genere privilegi di proprietario o amministratore e può accedere alla maggior parte delle risorse senza autorizzazioni aggiuntive. Per altre informazioni, vedere la nota precedente sull'[uso delle credenziali dell'interfaccia della riga di comando con `DefaultAzureCredential`](#cli-auth-note).

### <a name="deprecated-authenticate-with-userpasscredentials"></a>Deprecato: Eseguire l'autenticazione con UserPassCredentials

Quando [Azure Active Directory Authentication Library (ADAL) per Python](https://github.com/AzureAD/azure-activedirectory-library-for-python) non era ancora disponibile, era necessario usare la classe [`UserPassCredentials`](/python/api/msrestazure/msrestazure.azure_active_directory.userpasscredentials?view=azure-python) ora deprecata. Questa classe non supporta l'autenticazione a due fattori e non deve più essere usata.

## <a name="see-also"></a>Vedere anche

- [Configurare l'ambiente di sviluppo Python locale per Azure](configure-local-development-environment.md)
- [Esempio: Effettuare il provisioning di un gruppo di risorse](azure-sdk-example-resource-group.md)
- [Esempio: Effettuare il provisioning e usare Archiviazione di Azure](azure-sdk-example-storage.md)
- [Esempio: Effettuare il provisioning di un'app Web e distribuire il codice](azure-sdk-example-web-app.md)
- [Esempio: Effettuare il provisioning e usare un database MySQL](azure-sdk-example-database.md)
- [Esempio: Effettuare il provisioning di una macchina virtuale](azure-sdk-example-virtual-machines.md)
