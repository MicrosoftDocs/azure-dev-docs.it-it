---
title: Autenticare le app con le librerie di gestione di Azure per Python
description: Autenticare un'app Python con i servizi di Azure usando le librerie dell'SDK di gestione di Azure
ms.date: 01/16/2020
ms.topic: conceptual
ms.custom: seo-python-october2019
ms.openlocfilehash: 0c1dc8395710eec86f2bcd2b7e8334b987f15917
ms.sourcegitcommit: 20634277152d72a35ad9b35fa1203608740d1145
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/11/2020
ms.locfileid: "77144035"
---
# <a name="authenticate-by-using-the-azure-management-libraries-for-python"></a>Eseguire l'autenticazione con le librerie di gestione di Azure per Python

Questo articolo illustra come usare le librerie di gestione di Python SDK per autenticare un'applicazione con Azure Active Directory (Azure AD) usando un'entità servizio. L'entità servizio è un'identità per un'applicazione registrata con Azure AD e consente all'applicazione di accedere alle risorse o modificarle in base alle relative autorizzazioni.

Per registrare le applicazioni, è innanzitutto necessario creare un'istanza di Active Directory con un tenant appropriato per l'organizzazione. A questo scopo, seguire le istruzioni riportate in [Creare un nuovo tenant in Azure Active Directory](/azure/active-directory/fundamentals/active-directory-access-create-new-tenant). Al termine, seguire l'articolo [Procedura: Usare il portale per creare un'applicazione Azure AD e un'entità servizio in grado di accedere alle risorse](/azure/active-directory/develop/howto-create-service-principal-portal), in cui si registra un'applicazione, si [recuperano gli ID del tenant e dell'applicazione (client) per l'entità servizio](/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in)) e si configura un [segreto dell'applicazione](/azure/active-directory/develop/howto-create-service-principal-portal#create-a-new-application-secret) con cui eseguire l'autenticazione dal codice Python.

Dopo aver ottenuto questi valori, è possibile usare tali credenziali per eseguire l'autenticazione in diversi modi, usando le librerie di Python SDK. Il risultato di ogni metodo è l'oggetto client SDK usato per accedere ad altre risorse dal codice.

È consigliabile archiviare l'ID tenant, l'ID client e il segreto in [Azure KeyVault](/azure/key-vault/), in modo che tali valori non siano presenti nei sistemi o nel controllo del codice sorgente. È possibile recuperare facilmente i valori ogni volta che è necessario.

[!INCLUDE [chrome-note](includes/chrome-note.md)]

## <a name="mgmt-auth-file"></a>Eseguire l'autenticazione con un file JSON

In questo metodo si crea un file JSON che contiene le credenziali necessarie per l'entità servizio, quindi si crea l'oggetto client SDK usando le informazioni contenute nel file.

1. Creare un file JSON (con qualsiasi nome, ad esempio *app_credentials.jsono*) nel formato seguente. Sostituire i quattro segnaposto con l'ID sottoscrizione di Azure, l'ID tenant di Azure AD, l'ID applicazione (client) e il segreto:

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

    > [!TIP]
    > È possibile recuperare un file di credenziali con l'ID sottoscrizione già presente eseguendo l'accesso ad Azure con il comando [az login](/cli/azure/group#az-login) seguito dal comando [az ad sp create-for-rbac](/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac):
    >
    > ```azurecli
    > az login
    > az ad sp create-for-rbac --sdk-auth > credentials.json
    > ```
    >
    > È quindi possibile sostituire i valori di `tenantId`, `clientId` e `clientSecret` per l'applicazione specifica, invece di usare i valori generici.

1. Salvare questo file in una posizione sicura e accessibile dal codice.

1. Usare il metodo [get_client_from_auth_file](/python/api/azure-common/azure.common.client_factory?view=azure-python#get-client-from-auth-file-client-class--auth-path-none----kwargs-) per creare l'oggetto client, sostituendo `<path_to_file>` con il percorso del file JSON:

    ```python
    from azure.common.client_factory import get_client_from_auth_file
    from azure.mgmt.compute import ComputeManagementClient

    client = get_client_from_auth_file(ComputeManagementClient, auth_path=<path_to_file>)
    ```

1. In alternativa, è possibile archiviare il percorso del file in una variabile di ambiente denominata `AZURE_AUTH_LOCATION` e omettere l'argomento `auth_path`.

## <a name="authenticate-with-a-json-dictionary"></a>Eseguire l'autenticazione con un dizionario JSON

Invece di usare un file, come descritto nella sezione precedente, è possibile creare il codice JSON necessario in una variabile e chiamare [get_client_from_json_dict](/python/api/azure-common/azure.common.client_factory?view=azure-python#get-client-from-json-dict-client-class--config-dict----kwargs-). In questo caso, è consigliabile archiviare sempre l'ID tenant, l'ID client e il segreto in una posizione sicura, ad esempio [Azure KeyVault](/azure/key-vault/).

```python
   from azure.common.client_factory import get_client_from_auth_file
   from azure.mgmt.compute import ComputeManagementClient

    # Retrieve tenant_id, client_id, and client_secret from Azure KeyVault

   config_dict = {
       "subscriptionId": "bfc42d3a-65ca-11e7-95cf-ecb1d756380e",
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
   client = get_client_from_json_dict(ComputeManagementClient, config_dict)
```

## <a name="mgmt-auth-token"></a>Eseguire l'autenticazione con le credenziali del token

Supponendo di recuperare le credenziali da una risorsa di archiviazione protetta, ad esempio [Azure KeyVault](/azure/key-vault/), creare prima un oggetto [ServicePrincipalCredentials], quindi creare un'istanza del client usando tali credenziali e l'ID sottoscrizione:

```python
from azure.mgmt.compute import ComputeManagementClient
from azure.common.credentials import ServicePrincipalCredentials

# Retrieve credentials from secure storage. Never hard-code credentials into code.

credentials = ServicePrincipalCredentials(tenant = <tenant_id>,
    client_id = <client_id>, secret = <secret>)

client = ComputeManagementClient(credentials, <subscription_id>)
```

Se è necessario un maggior controllo, usare [Azure Active Directory Authentication Library (ADAL) per Python](https://github.com/AzureAD/azure-activedirectory-library-for-python) e il wrapper ADAL SDK:

```python
from azure.mgmt.compute import ComputeManagementClient
import adal
from msrestazure.azure_active_directory import AdalAuthentication
from msrestazure.azure_cloud import AZURE_PUBLIC_CLOUD

# Retrieve credentials from secure storage. Never hard-code credentials into code.

LOGIN_ENDPOINT = AZURE_PUBLIC_CLOUD.endpoints.active_directory
RESOURCE = AZURE_PUBLIC_CLOUD.endpoints.active_directory_resource_id

context = adal.AuthenticationContext(LOGIN_ENDPOINT + '/' + TENANT_ID)

credentials = AdalAuthentication(context.acquire_token_with_client_credentials,
    RESOURCE, <client_id>, <secret>)

client = ComputeManagementClient(credentials, <subscription_id>)
```

> [!NOTE]
> Se si usa un cloud sovrano di Azure, specificare l'URL di base appropriato, usando una costante in `msrestazure.azure_cloud`, durante la creazione del client di gestione:
>
> ```python
> client = ComputeManagementClient(credentials, subscription_id,
>     base_url=AZURE_CHINA_CLOUD.endpoints.resource_manager)
> ```

### <a name="mgmt-auth-legacy"></a>Eseguire l'autenticazione con le credenziali del token (deprecato)

Quando [Azure Active Directory Authentication Library (ADAL) per Python](https://github.com/AzureAD/azure-activedirectory-library-for-python) non era ancora disponibile, si usava la classe `UserPassCredentials`. L'uso di questa classe viene considerato deprecato e non deve essere più adottato, perché non supporta l'autenticazione a due fattori.

```python
from azure.common.credentials import UserPassCredentials

# DEPRECATED - legacy purposes only - use ADAL instead
credentials = UserPassCredentials(
    'user@domain.com',
    'my_smart_password'
)
```

## <a name="mgmt-auth-msi"></a>Autenticazione con identità gestite di Azure

Le identità gestite di Azure offrono a un modo semplice per autenticare le risorse di Azure senza usare specifiche credenziali.

Per usare le identità gestite, è necessario connettersi ad Azure da un'altra risorsa di Azure, ad esempio una funzione o una macchina virtuale di Azure. Per altre informazioni sulla configurazione di un'identità gestita per una risorsa, vedere [Configurare le identità gestite per le risorse di Azure](/azure/active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vm) e [Come usare le identità gestite per le risorse di Azure](/azure/active-directory/managed-identities-azure-resources/how-to-use-vm-sign-in).

```python
from msrestazure.azure_active_directory import MSIAuthentication
from azure.mgmt.resource import ResourceManagementClient, SubscriptionClient

# Create MSI Authentication
credentials = MSIAuthentication()

# Create a Subscription Client
subscription_client = SubscriptionClient(credentials)
subscription = next(subscription_client.subscriptions.list())
subscription_id = subscription.subscription_id

# Create a Resource Management client
client = ResourceManagementClient(credentials, subscription_id)

# List resource groups as an example. The only limit is what role and policy are assigned to this MSI token.
for resource_group in resource_client.resource_groups.list():
    print(resource_group.name)
```

## <a name="mgmt-auth-cli"></a>Autenticazione basata sull'interfaccia della riga di comando (solo a scopo di sviluppo)

L'SDK consente di creare un client tramite la sottoscrizione attiva dell'interfaccia della riga di comando di Azure, dopo aver eseguito `az login`. L'SDK usa l'ID sottoscrizione predefinito oppure è possibile impostare la sottoscrizione usando [az account](https://docs.microsoft.com/cli/azure/manage-azure-subscriptions-azure-cli)

Questa opzione deve essere usata solo a scopo di sviluppo.

```python
from azure.common.client_factory import get_client_from_cli_profile
from azure.mgmt.compute import ComputeManagementClient

client = get_client_from_cli_profile(ComputeManagementClient)
```
