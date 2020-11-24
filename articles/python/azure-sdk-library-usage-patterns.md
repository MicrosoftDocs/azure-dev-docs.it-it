---
title: Modelli di utilizzo delle librerie di Azure per Python
description: Panoramica dei modelli di utilizzo comuni delle librerie di Azure SDK per Python
ms.date: 11/12/2020
ms.topic: conceptual
ms.custom: devx-track-python
ms.openlocfilehash: 6f1a2c07bbda4ebe409722d2381e046ee45f7902
ms.sourcegitcommit: 6514a061ba5b8003ce29d67c81a9f0795c3e3e09
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/13/2020
ms.locfileid: "94601393"
---
# <a name="azure-libraries-for-python-usage-patterns"></a>Modelli di utilizzo delle librerie di Azure per Python

Azure SDK per Python è costituito esclusivamente da numerose librerie indipendenti, elencate nell'[indice dei pacchetti delle librerie di Azure SDK per Python](azure-sdk-library-package-index.md).

Tutte le librerie condividono alcune caratteristiche e modelli di utilizzo comuni, ad esempio l'installazione e l'uso di JSON inline per gli argomenti oggetto.

## <a name="library-installation"></a>Installazione di una libreria

Per installare un pacchetto di libreria specifico, è sufficiente usare `pip install`:

```cmd
# Install the management library for Azure Storage
pip install azure-mgmt-storage
```

```cmd
# Install the client library for Azure Storage
pip install azure-storage-blob
```

`pip install` recupera l'ultima versione di una libreria nell'ambiente Python corrente.

È anche possibile usare `pip` per disinstallare le librerie e installare versioni specifiche, incluse quelle di anteprima. Per altre informazioni, vedere [Come installare i pacchetti di librerie di Azure per Python](azure-sdk-install.md).

## <a name="asynchronous-operations"></a>Operazioni asincrone

Molte operazioni richiamate tramite oggetti client e di gestione client (ad esempio [`ComputeManagementClient.virtual_machines.begin_create_or_update`](/python/api/azure-mgmt-compute/azure.mgmt.compute.v2020_06_01.operations.virtualmachinesoperations#begin-create-or-update-resource-group-name--vm-name--parameters----kwargs-) e [`WebSiteManagementClient.web_apps.create_or_update`](/python/api/azure-mgmt-web/azure.mgmt.web.v2019_08_01.operations.webappsoperations#create-or-update-resource-group-name--name--site-envelope--custom-headers-none--raw-false--polling-true----operation-config-)) restituiscono un oggetto di tipo `AzureOperationPoller[<type>]`, dove `<type>` è specifico dell'operazione in questione.

Entrambi questi metodi sono asincroni. I nomi dei metodi sono diversi a causa delle differenze di versione. Le librerie precedenti che non sono basate su azure.core in genere usano un nome simile a `create_or_update`. Le librerie basate su azure.core aggiungono il prefisso `begin_` ai nomi dei metodi per precisare che sono asincroni. La migrazione del codice precedente a una libreria basata su azure.core più recente indica in genere l'aggiunta del prefisso `begin_` ai nomi dei metodi, perché la maggior parte delle firme del metodo rimane invariata.

In entrambi i casi, un tipo restituito [`AzureOperationPoller`](/python/api/msrestazure/msrestazure.azure_operation.azureoperationpoller) indica decisamente che l'operazione è asincrona. Di conseguenza, è necessario chiamare il metodo `result` dello strumento per il polling per attendere la conclusione dell'operazione e l'ottenimento del risultato.

Il codice seguente, tratto da [Esempio: Effettuare il provisioning e la distribuzione di un'app Web](azure-sdk-example-web-app.md), illustra un esempio dell'utilizzo dello strumento per il polling per attendere un risultato:

```python
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
```

In questo caso, il valore restituito di `create_or_update` è di tipo `AzureOperationPoller[Site]`, che indica che il valore restituito di `poller.result()` è un oggetto [Site](/python/api/azure-mgmt-web/azure.mgmt.web.v2019_08_01.models.site).

## <a name="exceptions"></a>Eccezioni

In generale, le librerie di Azure generano eccezioni quando le operazioni non vengono eseguite come previsto, incluse le richieste HTTP non riuscite all'API REST di Azure. Per il codice dell'app, quindi, è possibile usare blocchi `try...except` per le operazioni della libreria.

Per altre informazioni sul tipo di eccezioni che possono essere generate, consultare la documentazione relativa all'operazione in questione.

## <a name="logging"></a>Registrazione

Le librerie di Azure più recenti usano la libreria `logging` standard Python per generare l'output di registrazione. È possibile impostare il livello di registrazione per singole librerie, per gruppi di librerie o per tutte le librerie. Dopo aver registrato un gestore del flusso di registrazione, è possibile abilitare la registrazione per un oggetto client specifico o per un'operazione specifica. Per altre informazioni, vedere [Configurare la registrazione nelle librerie di Azure per Python](azure-sdk-logging.md).

## <a name="proxy-configuration"></a>Configurazione proxy

Per specificare un proxy è possibile usare variabili di ambiente o argomenti facoltativi. Per altre informazioni, vedere [Come configurare i proxy per le librerie di Azure](azure-sdk-configure-proxy.md).

## <a name="optional-arguments-for-client-objects-and-methods"></a>Argomenti facoltativi per oggetti client e metodi

Nella documentazione di riferimento delle librerie viene spesso visualizzato un argomento `**kwargs` o `**operation_config**` nella firma di un costruttore di oggetti client o di un metodo di operazione specifico. Questi segnaposto indicano che l'oggetto o il metodo in questione può supportare altri argomenti denominati. In genere la documentazione di riferimento indica gli argomenti specifici che è possibile usare. Sono inoltre spesso supportati alcuni argomenti generali descritti nelle sezioni seguenti.

### <a name="arguments-for-libraries-based-on-azurecore"></a>Argomenti per le librerie basate su azure.core

Questi argomenti si applicano alle librerie elencate in [Python - Nuove librerie](https://azure.github.io/azure-sdk/releases/latest/#python).

| Nome                       | Type | Predefinito     | Descrizione |
| ---                        | ---  | ---         | ---         |
| logging_enable             | bool | False       | Abilita la registrazione. Per altre informazioni, vedere [Configurare la registrazione nelle librerie di Azure per Python](azure-sdk-logging.md). |
| proxies                    | dict | {}          | URL del server proxy. Per altre informazioni, vedere [Come configurare i proxy per le librerie di Azure](azure-sdk-configure-proxy.md). |
| use_env_settings           | bool | True        | Se True, consente l'uso delle variabili di ambiente `HTTP_PROXY` e `HTTPS_PROXY` per i proxy. Se False, le variabili di ambiente vengono ignorate. Per altre informazioni, vedere [Come configurare i proxy per le librerie di Azure](azure-sdk-configure-proxy.md). |
| connection_timeout         | INT  | 300         | Timeout in secondi per stabilire una connessione agli endpoint dell'API REST di Azure. |
| read_timeout               | INT  | 300         | Timeout in secondi per il completamento di un'operazione dell'API REST di Azure (ovvero attesa di una risposta). |
| retry_total                | INT  | 10          | Numero di nuovi tentativi consentiti per le chiamate all'API REST. Usare `retry_total=0` per disabilitare i tentativi. |
| retry_mode                 | enum | exponential | Applica l'intervallo dei tentativi in un modo lineare o esponenziale. Se "single", i tentativi vengono eseguiti a intervalli regolari. Se "exponential", ogni ripetizione attende il doppio del tempo del tentativo precedente. |

Le singole librerie non devono obbligatoriamente supportare questi argomenti, quindi consultare sempre la documentazione di riferimento per ogni libreria per i dettagli esatti.

### <a name="arguments-for-non-core-libraries"></a>Argomenti per le librerie non core

| Nome               | Type | Predefinito | Descrizione |
| ---                | ---  | ---     | ---         |
| verify             | bool | True    | Verifica il certificato SSL. |
| cert               | str  | nessuno    | Percorso del certificato locale per la verifica lato client. |
| timeout            | INT  | 30      | Timeout in secondi per stabilire una connessione al server. |
| allow_redirects    | bool | False   | Abilita i reindirizzamenti. |
| max_redirects      | INT  | 30      | Numero massimo di reindirizzamenti consentiti. |
| proxies            | dict | {}      | URL del server proxy. Per altre informazioni, vedere [Come configurare i proxy per le librerie di Azure](azure-sdk-configure-proxy.md). |
| use_env_proxies    | bool | False   | Abilita la lettura delle impostazioni del proxy dalle variabili di ambiente locali. |
| retries            | INT  | 10      | Numero totale di nuovi tentativi consentiti. |
| enable_http_logger | bool | False   | Abilita i log di HTTP in modalità di debug. |

## <a name="inline-json-pattern-for-object-arguments"></a>Modello JSON inline per argomenti oggetto

Molte operazioni all'interno di Azure SDK consentono di esprimere gli argomenti oggetto come oggetti discreti o come JSON inline.

Si supponga, ad esempio, di avere un oggetto [`ResourceManagementClient`](/python/api/azure-mgmt-resource/azure.mgmt.resource.resources.v2019_10_01.resourcemanagementclient) tramite il quale si crea un gruppo di risorse con il relativo metodo [`create_or_update`](/python/api/azure-mgmt-resource/azure.mgmt.resource.resources.v2019_10_01.operations.resourcegroupsoperations#create-or-update-resource-group-name--parameters--custom-headers-none--raw-false----operation-config-). Il secondo argomento di questo metodo è di tipo [`ResourceGroup`](/python/api/azure-mgmt-resource/azure.mgmt.resource.resources.v2019_10_01.models.resourcegroup).

Per chiamare `create_or_update`, è possibile creare direttamente un'istanza discreta di `ResourceGroup` con gli argomenti obbligatori (in questo caso `location`):

```python
rg_result = resource_client.resource_groups.create_or_update(
    "PythonSDKExample-rg",
    ResourceGroup(location="centralus")
)
```

In alternativa, è possibile passare gli stessi parametri come JSON inline:

```python
rg_result = resource_client.resource_groups.create_or_update(
    "PythonSDKExample-rg",
    {
      "location": "centralus"
    }
)
```

Quando si usa JSON, le librerie di Azure convertono automaticamente il file JSON inline nel tipo di oggetto appropriato per l'argomento in questione.

Gli oggetti possono anche avere argomenti oggetto annidati, nel qual caso si può anche usare codice JSON annidato.

Si supponga, ad esempio, di avere un'istanza dell'oggetto [`KeyVaultManagementClient`](/python/api/azure-mgmt-keyvault/azure.mgmt.keyvault.v2019_09_01.keyvaultmanagementclient) e di chiamare il relativo metodo [`create_or_update`](/python/api/azure-mgmt-keyvault/azure.mgmt.keyvault.v2019_09_01.operations.vaultsoperations#create-or-update-resource-group-name--vault-name--parameters--custom-headers-none--raw-false--polling-true----operation-config-). In questo caso, il terzo argomento è di tipo [`VaultCreateOrUpdateParameters`](/python/api/azure-mgmt-keyvault/azure.mgmt.keyvault.v2019_09_01.models.vaultcreateorupdateparameters), che contiene un argomento di tipo [`VaultProperties`](/python/api/azure-mgmt-keyvault/azure.mgmt.keyvault.v2019_09_01.models.vaultproperties). `VaultProperties`, a sua volta, contiene gli argomenti oggetto di tipo [`Sku`](/python/api/azure-mgmt-keyvault/azure.mgmt.keyvault.v2019_09_01.models.sku) e [`list[AccessPolicyEntry]`](/python/api/azure-mgmt-keyvault/azure.mgmt.keyvault.v2019_09_01.models.accesspolicyentry). `Sku` contiene un oggetto [`SkuName`](/python/api/azure-mgmt-keyvault/azure.mgmt.keyvault.v2019_09_01.models.skuname) e ogni oggetto `AccessPolicyEntry` contiene un oggetto [`Permissions`](/python/api/azure-mgmt-keyvault/azure.mgmt.keyvault.v2019_09_01.models.permissions).

Per chiamare `create_or_update` con gli oggetti incorporati, usare codice simile al seguente (presupponendo che `tenant_id` e `object_id` siano già definiti). È anche possibile creare gli oggetti necessari prima della chiamata di funzione.

```python
operation = keyvault_client.vaults.create_or_update(
    "PythonSDKExample-rg",
    "keyvault01",
    VaultCreateOrUpdateParameters(
        location="centralus",
        properties=VaultProperties(
            tenant_id=tenant_id,
            sku=Sku(name="standard"),
            access_policies=[
                AccessPolicyEntry(
                    tenant_id=tenant_id,
                    object_id=object_id,
                    permissions=Permissions(keys=['all'], secrets=['all'])
                )
            ]
        )
    )
)
```

La stessa chiamata usando JSON inline sarà come segue:

```python
operation = keyvault_client.vaults.create_or_update(
    "PythonSDKExample-rg",
    "keyvault01",
    {
        'location': 'centralus',
        'properties': {
            'sku': {
                'name': 'standard'
            },
            'tenant_id': tenant_id,
            'access_policies': [{
                'object_id': object_id,
                'tenant_id': tenant_id,
                'permissions': {
                    'keys': ['all'],
                    'secrets': ['all']
                }
            }]
        }
    }
)
```

Poiché i due formati sono equivalenti, è possibile scegliere quello che si preferisce usare e persino combinarli.

Se il formato JSON non è corretto, si riceve in genere un messaggio di errore analogo a "DeserializationError: Non è possibile deserializzare l'oggetto: tipo, AttributeError: all'oggetto 'str' non è associato un attributo 'Get'". Una causa comune di questo errore è che si specifica una singola stringa di una proprietà quando la libreria prevede un oggetto JSON annidato. Se ad esempio si usa `"sku": "standard"` nell'esempio precedente, questo errore viene generato perché il parametro `sku` è un oggetto `Sku` che prevede un oggetto JSON inline, in questo caso `{ "name": "standard"}`, che è mappato al tipo `SkuName` previsto.

## <a name="next-steps"></a>Passaggi successivi

Ora che si conoscono i modelli di utilizzo comuni delle librerie di Azure per Python, vedere gli esempi autonomi seguenti per esplorare scenari specifici di gestione e librerie client:

- [Esempio: Creare un gruppo di risorse](azure-sdk-example-resource-group.md)
- [Esempio: Usare Archiviazione di Azure](azure-sdk-example-storage.md)
- [Esempio: Effettuare il provisioning di un'app Web e distribuire il codice](azure-sdk-example-web-app.md)
- [Esempio: Effettuare il provisioning ed eseguire query su un database](azure-sdk-example-database.md)
- [Esempio: Effettuare il provisioning di una macchina virtuale](azure-sdk-example-virtual-machines.md)

È possibile provare questi esempi in qualsiasi ordine, perché non sono sequenziali né interdipendenti.
