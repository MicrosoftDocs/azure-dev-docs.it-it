---
title: Connettersi a tutte le aree con le librerie di Azure per Python multi-cloud
description: Informazioni su come usare il modulo azure_cloud di msrestazure per connettersi ad Azure in diverse aree sovrane
ms.date: 11/18/2020
ms.topic: conceptual
ms.custom: devx-track-python
ms.openlocfilehash: ba751604351bd7038a7696b6c8e93d663aa9263f
ms.sourcegitcommit: 29930f1593563c5e968b86117945c3452bdefac1
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/23/2020
ms.locfileid: "95485670"
---
# <a name="multi-cloud-connect-to-all-regions-with-the-azure-libraries-for-python"></a>Multi-cloud: connettersi a tutte le aree con le librerie di Azure per Python

È possibile usare le librerie di Azure per Python per connettersi a tutte le aree in cui Azure è [disponibile](https://azure.microsoft.com/regions/services).

Per impostazione predefinita, le librerie di Azure sono configurate per connettersi al cloud globale di Azure.

## <a name="using-pre-defined-sovereign-cloud-constants"></a>Uso di costanti predefinite per il cloud sovrano

Le costanti predefinite per il cloud sovrano sono fornite dal modulo `azure_cloud` della libreria `msrestazure` (0.4.11 e versioni successive):

- `AZURE_PUBLIC_CLOUD`
- `AZURE_CHINA_CLOUD`
- `AZURE_US_GOV_CLOUD`
- `AZURE_GERMAN_CLOUD`

Per usare una definizione, importare la costante appropriata da `msrestazure.azure_cloud` e applicarla durante la creazione di oggetti client. 

Quando si usa `DefaultAzureCredential`, come illustrato nell'esempio seguente, è anche necessario usare il valore appropriato di `azure.identity.AzureAuthorityHosts`.

```python
import os
from msrestazure.azure_cloud import AZURE_CHINA_CLOUD as cloud
from azure.mgmt.resource import ResourceManagementClient, SubscriptionClient
from azure.identity import DefaultAzureCredential

# Assumes the subscription ID to use is in the AZURE_SUBSCRIPTION_ID environment variable
subscription_id = os.environ["AZURE_SUBSCRIPTION_ID"]

# When using sovereign domains (that is, any cloud other than AZURE_PUBLIC_CLOUD),
# you must use an authority with DefaultAzureCredential.
credential = DefaultAzureCredential(authority=cloud.endpoints.active_directory)

resource_client = ResourceManagementClient(credential,
    subscription_id, base_url=cloud.endpoints.resource_manager,
    credential_scopes=[cloud.endpoints.resource_manager + ".default'"])

subscription_client = SubscriptionClient(credential,
    base_url=stack_cloud.endpoints.resource_manager,
    credential_scopes=[cloud.endpoints.resource_manager + ".default'"])
```
  
## <a name="using-your-own-cloud-definition"></a>Uso di una definizione di cloud personalizzata

Nel codice seguente si usa `get_cloud_from_metadata_endpoint` con l'endpoint Azure Resource Manager per un cloud privato, ad esempio uno basato su Azure Stack:

```python
import os
from msrestazure.azure_cloud import get_cloud_from_metadata_endpoint
from azure.mgmt.resource import ResourceManagementClient, SubscriptionClient
from azure.identity import DefaultAzureCredential

# Assumes the subscription ID to use is in the AZURE_SUBSCRIPTION_ID environment variable
subscription_id = os.environ["AZURE_SUBSCRIPTION_ID"]

stack_cloud = get_cloud_from_metadata_endpoint("https://contoso-azurestack-arm-endpoint.com")

# When using a private, you must use an authority with DefaultAzureCredential.
# The active_directory endpoint should be a URL like https://login.microsoftonline.com.
# https:// is optional in the URL but required on the endpoint.
credential = DefaultAzureCredential(authority=stack_cloud.endpoints.active_directory)

resource_client = ResourceManagementClient(credential, subscription_id,
    base_url=stack_cloud.endpoints.resource_manager,
    credential_scopes=[cloud.endpoints.resource_manager + ".default'"])

subscription_client = SubscriptionClient(credential,
    base_url=stack_cloud.endpoints.resource_manager,
    credential_scopes=[stack_cloud.endpoints.resource_manager + ".default'"])
```
