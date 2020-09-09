---
title: 'Procedura dettagliata, parte 6: Autenticare le app Python con i servizi di Azure'
description: Informazioni sul codice di avvio dell'app principale, che configura l'oggetto DefaultAzureCredential e gli oggetti client necessari per l'endpoint dell'API.
ms.date: 08/24/2020
ms.topic: conceptual
ms.custom: devx-track-python
ms.openlocfilehash: 5fc344fcf00655bbac7b2a20a2401ffb52482447
ms.sourcegitcommit: 324da872a9dfd4c55b34739824fc6a6598f2ae12
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/02/2020
ms.locfileid: "89379531"
---
# <a name="part-6-main-app-startup-code"></a>Parte 6: Codice di avvio dell'app principale

[Parte precedente: Dipendenze, istruzioni import e variabili di ambiente](walkthrough-tutorial-authentication-05.md)

Il codice di avvio dell'app, che segue le istruzioni `import`, inizializza le diverse variabili usate nelle funzioni che gestiscono le richieste HTTP.

Creare prima di tutto l'oggetto app Flask e recuperare l'URL dell'endpoint dell'API di terze parti dalla variabile di ambiente:

```python
app = Flask(__name__)

number_url = os.environ["THIRD_PARTY_API_ENDPOINT"]
```

Si otterrà quindi l'oggetto [`DefaultAzureCredential`](/api/azure-identity/azure.identity.defaultazurecredential?view=azure-python), che corrisponde alle credenziali consigliate per l'uso quando si esegue l'autenticazione con i servizi di Azure. Vedere [Come autenticare le app Python](azure-sdk-authenticate.md#authenticate-with-defaultazurecredential).

```python
credential = DefaultAzureCredential()
```

Per l'esecuzione in locale, `DefaultAzureCredential` cerca le variabili di ambiente `AZURE_TENANT_ID`, `AZURE_CLIENT_ID` e `AZURE_CLIENT_SECRET` che contengono informazioni per l'entità servizio locale. Per l'esecuzione nel cloud, `DefaultAzureCredential` usa automaticamente l'entità servizio registrata per l'app, che in genere è contenuta nell'identità gestita.

Il codice recupera quindi la terza chiave di accesso dell'API di terze parti da Azure Key Vault. Nello script di provisioning, l'istanza di Key Vault viene creata con [`az keyvault create`](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-create) e il segreto viene archiviato con [`az keyvault secret set`](/cli/azure/keyvault/secret?view=azure-cli-latest#az-keyvault-secret-set).

È possibile accedere alla risorsa Key Vault stessa tramite un URL, che viene caricato dalla variabile di ambiente `KEY_VAULT_URL`.

```python
key_vault_url = os.environ["KEY_VAULT_URL"]
```

Per connettersi all'insieme di credenziali delle chiavi, è necessario creare un oggetto client adatto. Poiché si vuole recuperare un segreto, si usa [`SecretClient`](/python/api/azure-keyvault-secrets/azure.keyvault.secrets.secretclient?view=azure-python), che richiede l'URL dell'insieme di credenziali delle chiavi e l'oggetto credenziali che rappresenta l'identità con cui l'app è in esecuzione.

```python
keyvault_client = SecretClient(vault_url=key_vault_url, credential=credential)
```

La creazione dell'oggetto `SecretClient` non autentica in alcun modo le credenziali. `SecretClient` è semplicemente un costrutto lato client che gestisce internamente l'URL della risorsa e le credenziali. L'autenticazione e l'autorizzazione avvengono solo quando si richiama un'operazione tramite il client, ad esempio [`get_secret`](/python/api/azure-keyvault-secrets/azure.keyvault.secrets.secretclient?view=azure-python#get-secret-name--version-none----kwargs-), che genera una chiamata API REST alla risorsa di Azure.

```python
api_secret_name = os.environ["THIRD_PARTY_API_SECRET_NAME"]
vault_secret = keyvault_client.get_secret(api_secret_name)

# The "secret" from Key Vault is an object with multiple properties. The key we
# want for the third-party API is in the value property. 
access_key = vault_secret.value
```

Anche se l'identità dell'app è autorizzata ad accedere all'insieme di credenziali delle chiavi, deve essere comunque specificamente autorizzata ad accedere ai segreti.  In caso contrario, la chiamata `get_secret` non riesce. Per questo motivo, lo script di provisioning imposta un criterio di accesso "get secrets" per l'app usando il comando [`az keyvault set-policy`](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-set-policy) dell'interfaccia della riga di comando di Azure. Per altre informazioni, vedere [Autenticazione di Key Vault](/azure/key-vault/general/authentication) e [Concedere all'app l'accesso a Key Vault](/azure/key-vault/general/managed-identity#grant-your-app-access-to-key-vault). Quest'ultimo articolo illustra come impostare un criterio di accesso con il portale di Azure. L'articolo è scritto per l'identità gestita, ma si applica ugualmente a un'entità servizio locale usata nello sviluppo.

Infine, il codice dell'app configura l'oggetto client tramite il quale è possibile scrivere i messaggi in una coda di archiviazione di Azure. L'URL della coda si trova nella variabile di ambiente `STORAGE_QUEUE_URL`.

```python
queue_url = os.environ["STORAGE_QUEUE_URL"]
queue_client = QueueClient.from_queue_url(queue_url=queue_url, credential=credential)
```

Come per Key Vault, si usa un oggetto client specifico dalle librerie di Azure, [`QueueClient`](/python/api/azure-storage-queue/azure.storage.queue.queueclient?view=azure-python), e il relativo metodo [`from_queue_url`](/python/api/azure-storage-queue/azure.storage.queue.queueclient?view=azure-python#from-queue-url-queue-url--credential-none----kwargs-) per connettersi alla risorsa che si trova all'URL in questione. Ancora una volta, il tentativo di creare questo oggetto client verifica che l'identità dell'app rappresentata dalle credenziali sia autorizzata ad accedere alla coda. Come indicato in precedenza, l'autorizzazione è stata concessa assegnando il ruolo "Collaboratore ai dati della coda di archiviazione" all'app principale.

Supponendo che tutto il codice di avvio abbia esito positivo, l'app include tutte le variabili interne per supportare l'endpoint dell'API */api/v1/getcode*.

> [!div class="nextstepaction"]
> [Parte 7: Endpoint dell'API dell'app principale >>>](walkthrough-tutorial-authentication-07.md)
