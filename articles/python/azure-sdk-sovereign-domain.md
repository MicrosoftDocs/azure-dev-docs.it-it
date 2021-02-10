---
title: Connettersi a tutte le aree con le librerie di Azure per Python multi-cloud
description: Informazioni su come usare il modulo azure_cloud di msrestazure per connettersi ad Azure in diverse aree sovrane
ms.date: 11/18/2020
ms.topic: conceptual
ms.custom: devx-track-python
ms.openlocfilehash: c5d074a8e775fc1766df1ab8c4ed73aabc333fcc
ms.sourcegitcommit: 98a7e855206ff463c1d95f93c23dd665b26a0aa1
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/09/2021
ms.locfileid: "100004755"
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

Quando si usa `DefaultAzureCredential`, come illustrato nell'esempio seguente, è anche necessario usare il valore appropriato di `CLOUD.endpoints.active_directory`.

```python
import os
from msrestazure.azure_cloud import AZURE_CHINA_CLOUD as CLOUD
from azure.mgmt.resource import ResourceManagementClient, SubscriptionClient
from azure.identity import DefaultAzureCredential

# Assumes the subscription ID and tenant ID to use are in the AZURE_SUBSCRIPTION_ID and
# AZURE_TENANT_ID environment variables
subscription_id = os.environ["AZURE_SUBSCRIPTION_ID"]
tenant_id = os.environ["AZURE_TENANT_ID"]

# When using sovereign domains (that is, any cloud other than AZURE_PUBLIC_CLOUD),
# you must use an authority with DefaultAzureCredential.
credential = DefaultAzureCredential(authority=CLOUD.endpoints.active_directory, tenant_id=tenant_id)

resource_client = ResourceManagementClient(
    credential, subscription_id,
    base_url=CLOUD.endpoints.resource_manager,
    credential_scopes=[CLOUD.endpoints.resource_manager + "/.default"])

subscription_client = SubscriptionClient(
    credential,
    base_url=CLOUD.endpoints.resource_manager,
    credential_scopes=[CLOUD.endpoints.resource_manager + "/.default"])
```
  
## <a name="using-your-own-cloud-definition"></a>Uso di una definizione di cloud personalizzata

Nel codice seguente si usa `get_cloud_from_metadata_endpoint` con l'endpoint Azure Resource Manager per un cloud privato, ad esempio uno basato su Azure Stack:

```python
import os
from msrestazure.azure_cloud import get_cloud_from_metadata_endpoint
from azure.mgmt.resource import ResourceManagementClient, SubscriptionClient
from azure.identity import DefaultAzureCredential
from azure.profiles import KnownProfiles

# Assumes the subscription ID and tenant ID to use are in the AZURE_SUBSCRIPTION_ID and
# AZURE_TENANT_ID environment variables
subscription_id = os.environ["AZURE_SUBSCRIPTION_ID"]
tenant_id = os.environ["AZURE_TENANT_ID"]

stack_cloud = get_cloud_from_metadata_endpoint("https://contoso-azurestack-arm-endpoint.com")

# When using a private cloud, you must use an authority with DefaultAzureCredential.
# The active_directory endpoint should be a URL like https://login.microsoftonline.com.
credential = DefaultAzureCredential(authority=stack_cloud.endpoints.active_directory, tenant_id=tenant_id)

resource_client = ResourceManagementClient(
    credential, subscription_id,
    base_url=stack_cloud.endpoints.resource_manager,
    profile=KnownProfiles.v2019_03_01_hybrid,
    credential_scopes=[stack_cloud.endpoints.active_directory_resource_id + "/.default"])

subscription_client = SubscriptionClient(
    credential,
    base_url=stack_cloud.endpoints.resource_manager,
    profile=KnownProfiles.v2019_03_01_hybrid,
    credential_scopes=[stack_cloud.endpoints.active_directory_resource_id + "/.default"])
```
