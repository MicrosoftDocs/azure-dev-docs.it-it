---
title: Effettuare il provisioning e la distribuzione di un'app Web con le librerie di Azure SDK
description: Usare le librerie di gestione di Azure SDK per Python per effettuare il provisioning di un'app Web e quindi distribuirne il codice da un repository GitHub.
ms.date: 05/29/2020
ms.topic: conceptual
ms.openlocfilehash: 8196e86b4a4311b48b47975fd47bb04f11a1fe23
ms.sourcegitcommit: db56786f046a3bde1bd9b0169b4f62f0c1970899
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/03/2020
ms.locfileid: "84329639"
---
# <a name="example-use-the-azure-libraries-to-provision-and-deploy-a-web-app"></a>Esempio: Usare le librerie di Azure per il provisioning e la distribuzione di un'app Web

Questo esempio illustra come usare le librerie di gestione di Azure SDK in uno script Python per effettuare il provisioning di un'app Web nel servizio app Azure e distribuirne il codice da un repository GitHub. I [comandi equivalenti dell'interfaccia della riga di comando di Azure](#for-reference-equivalent-azure-cli-commands) vengono descritti più avanti in questo articolo.

Tutti i comandi di questo articolo funzionano allo stesso modo nella shell Bash Linux/Mac OS e nella shell dei comandi di Windows, se non diversamente specificato.

## <a name="1-set-up-your-local-development-environment"></a>1: Configurare un ambiente di sviluppo locale

Se non è già stato fatto, seguire tutte le istruzioni riportate in [Configurare l'ambiente di sviluppo Python locale per Azure](configure-local-development-environment.md).

Assicurarsi di creare un'entità servizio per lo sviluppo locale e di creare e attivare un ambiente virtuale per questo progetto.

## <a name="2-install-the-needed-azure-library-packages"></a>2: Installare i pacchetti delle librerie di Azure necessari

Creare un file denominato *requirements.txt* con il contenuto seguente:

```text
azure-mgmt-resource
azure-mgmt-web
azure-cli-core
```

In un terminale o da un prompt dei comandi con l'ambiente virtuale attivato, installare i requisiti:

```cmd
pip install -r requirements.txt
```

## <a name="3-fork-the-sample-repository"></a>3: Creare una copia tramite fork del repository di esempi

Visitare [https://github.com/Azure-Samples/python-docs-hello-world](https://github.com/Azure-Samples/python-docs-hello-world) e creare una copia tramite fork del repository nel proprio account GitHub. Si usa un fork per assicurarsi di avere le autorizzazioni per distribuire il repository in Azure.

![Creazione di una copia tramite fork del repository di esempi in GitHub.](media/azure-sdk-example-web-app/fork-github-repository.png)

Quindi creare una variabile di ambiente denominata `REPO_URL` con l'URL del fork. Il codice di esempio della sezione successiva dipende da questa variabile di ambiente:

# <a name="cmd"></a>[cmd](#tab/cmd)

```cmd
set REPO_URL=<url_of_your_fork>
```

# <a name="bash"></a>[Bash](#tab/bash)

```bash
REPO_URL=<url_of_your_fork>
```

---

## <a name="4-write-code-to-provision-and-deploy-a-web-app"></a>4: Scrivere codice per effettuare il provisioning e la distribuzione di un'app Web

Creare un file Python denominato *provision_deploy_webapp.py* con il codice seguente. I commenti spiegano i dettagli:

```python
import random, os
from azure.common.client_factory import get_client_from_cli_profile
from azure.mgmt.resource import ResourceManagementClient
from azure.mgmt.web import WebSiteManagementClient

# Retrieve subscription ID from environment variable
subscription_id = os.environ["AZURE_SUBSCRIPTION_ID"]

# Constants we need in multiple places: the resource group name and the region
# in which we provision resources. You can change these values however you want.
RESOURCE_GROUP_NAME = 'PythonAzureExample-WebApp-rg'
LOCATION = "centralus"

# Step 1: Provision the resource group.
resource_client = get_client_from_cli_profile(ResourceManagementClient)

rg_result = resource_client.resource_groups.create_or_update(RESOURCE_GROUP_NAME,
    { "location": LOCATION })

print(f"Provisioned resource group {rg_result.name}")

# For details on the previous code, see Example: Provision a resource group
# at https://docs.microsoft.com/azure/developer/python/azure-sdk-example-resource-group


#Step 2: Provision the App Service plan, which defines the underlying VM for the web app.

# Names for the App Service plan and App Service. We use a random number with the
# latter to create a reasonably unique name. If you've already provisioned a
# web app and need to re-run the script, set the WEB_APP_NAME environment 
# variable to that name instead.
SERVICE_PLAN_NAME = 'PythonAzureExample-WebApp-plan'
WEB_APP_NAME = os.environ.get("WEB_APP_NAME", f"PythonAzureExample-WebApp-{random.randint(1,100000):05}")

# Obtain the client object
app_service_client = get_client_from_cli_profile(WebSiteManagementClient)

# Provision the plan; Linux is the default
poller = app_service_client.app_service_plans.create_or_update(RESOURCE_GROUP_NAME,
    SERVICE_PLAN_NAME,
    {
        "location": LOCATION,
        "reserved": True,
        "sku" : {"name" : "B1"}
    }
)

plan_result = poller.result()

print(f"Provisioned App Service plan {plan_result.name}")


# Step 3: With the plan in place, provision the web app itself, which is the process that can host
# whatever code we want to deploy to it.

poller = app_service_client.web_apps.create_or_update(RESOURCE_GROUP_NAME,
    WEB_APP_NAME,
    {
        "location": LOCATION,
        "server_farm_id": plan_result.id,
        "site_config": {
            "linux_fx_version": "python|3.8"
        }
    }
)

web_app_result = poller.result()

print(f"Provisioned web app {web_app_result.name} at {web_app_result.default_host_name}")

# Step 4: deploy code from a GitHub repository. For Python code, App Service on Linux runs
# the code inside a container that makes certain assumptions about the structure of the code.
# For more information, see How to configure Python apps,
# https://docs.microsoft.com/azure/app-service/containers/how-to-configure-python.
#
# The create_or_update_source_control method doesn't provision a web app. It only sets the
# source control configuration for the app. In this case we're simply pointing to
# a GitHub repository.
#
# You can call this method again to change the repo.

REPO_URL = 'https://github.com/kraigb/python-docs-hello-world'

poller = app_service_client.web_apps.create_or_update_source_control(RESOURCE_GROUP_NAME,
    WEB_APP_NAME,
    {
        "location": "GitHub",
        "repo_url": REPO_URL,
        "branch": "master"
    }
)

sc_result = poller.result()

print(f"Set source control on web app to {sc_result.branch} branch of {sc_result.repo_url}")
```

Questo codice usa i metodi di autenticazione basati sull'interfaccia della riga di comando (`get_client_from_cli_profile`) perché illustra azioni che altrimenti si potrebbero eseguire direttamente con l'interfaccia della riga di comando di Azure. In entrambi i casi si usa la stessa identità per l'autenticazione.

Per usare tale codice in uno script di produzione, è preferibile usare invece `DefaultAzureCredential` (scelta consigliata) o un metodo basato su entità servizio, come descritto in [Come autenticare le app Python con i servizi di Azure](azure-sdk-authenticate.md).

### <a name="reference-links-for-classes-used-in-the-code"></a>Collegamenti di riferimento per le classi usate nel codice

- [ResourceManagementClient (azure.mgmt.resource)](/python/api/azure-mgmt-resource/azure.mgmt.resource.resourcemanagementclient?view=azure-python)
- [WebSiteManagementClient (azure.mgmt.web import)](/python/api/azure-mgmt-web/azure.mgmt.web.websitemanagementclient?view=azure-python)

## <a name="5-run-the-script"></a>5: Eseguire lo script

```cmd
python provision_deploy_web_app.py
```

## <a name="6-verify-the-web-app-deployment"></a>6: Verificare la distribuzione dell'app Web

1. Visitare il sito Web distribuito eseguendo il comando seguente:

    ```azurecli
    az webapp browse -n PythonAzureExample-WebApp-12345
    ```

    Sostituire "PythonAzureExample-WebApp-12345" con il nome specifico dell'app Web.

    Dovrebbe essere visualizzato "Hello World!" nel browser.

1. Visitare il [portale di Azure](https://portal.azure.com), selezionare **Gruppi di risorse** e verificare che sia "PythonAzureExample-WebApp-rg" sia presente nell'elenco. Passare quindi a tale elenco per verificare che includa le risorse previste, ovvero il piano di servizio app e il servizio app.

## <a name="7-clean-up-resources"></a>7: Pulire le risorse

```azurecli
az group delete -n PythonAzureExample-WebApp-rg
```

Se non è necessario mantenere le risorse di cui è stato effettuato il provisioning in questo esempio, eseguire questo comando per evitare addebiti ricorrenti nella sottoscrizione.

Per eliminare un gruppo di risorse dal codice, è anche possibile usare il metodo [`ResourceManagementClient.resource_groups.delete`](/python/api/azure-mgmt-resource/azure.mgmt.resource.resources.v2019_10_01.operations.resourcegroupsoperations?view=azure-python#delete-resource-group-name--custom-headers-none--raw-false--polling-true----operation-config-).

### <a name="for-reference-equivalent-azure-cli-commands"></a>Per riferimento: comandi equivalenti dell'interfaccia della riga di comando di Azure

I seguenti comandi dell'interfaccia della riga di comando di Azure completano gli stessi passaggi di provisioning dello script Python:

# <a name="cmd"></a>[cmd](#tab/cmd)

```azurecli
az group create -l centralus -n PythonAzureExample-WebApp-rg

az appservice plan create -n PythonAzureExample-WebApp-plan --is-linux --sku F1

az webapp create -g PythonAzureExample-WebApp-rg -n PythonAzureExample-WebApp-12345 ^
    --plan PythonAzureExample-WebApp-plan --runtime "python|3.8"

# You can use --deployment-source-url with the first create command. It's shown here
# to match the sequence of the Python code.

az webapp create -n PythonAzureExample-WebApp-12345 --plan PythonAzureExample-WebApp-plan ^
    --deployment-source-url https://github.com/<your_fork>/python-docs-hello-world

# Replace <your_fork> with the specific URL of your forked repository.
```

# <a name="bash"></a>[Bash](#tab/bash)

```azurecli
az group create -l centralus -n PythonAzureExample-WebApp-rg

az appservice plan create -n PythonAzureExample-WebApp-plan --is-linux --sku F1

az webapp create -g PythonAzureExample-WebApp-rg -n PythonAzureExample-WebApp-12345 \
    --plan PythonAzureExample-WebApp-plan --runtime "python|3.8"

# You can use --deployment-source-url with the first create command. It's shown here
# to match the sequence of the Python code.

az webapp create -n PythonAzureExample-WebApp-12345 --plan PythonAzureExample-WebApp-plan \
    --deployment-source-url https://github.com/<your_fork>/python-docs-hello-world

# Replace <your_fork> with the specific URL of your forked repository.
```

---

## <a name="see-also"></a>Vedere anche

- [Esempio: Effettuare il provisioning di un gruppo di risorse](azure-sdk-example-resource-group.md)
- [Esempio: Effettuare il provisioning di Archiviazione di Azure](azure-sdk-example-storage.md)
- [Esempio: Usare Archiviazione di Azure](azure-sdk-example-storage-use.md)
- [Esempio: Effettuare il provisioning e usare un database MySQL](azure-sdk-example-database.md)
- [Esempio: Effettuare il provisioning di una macchina virtuale](azure-sdk-example-virtual-machines.md)
