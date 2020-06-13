---
title: Modelli di utilizzo delle librerie di Azure per Python
description: Panoramica dei modelli di utilizzo comuni delle librerie di Azure SDK per Python
ms.date: 05/26/2020
ms.topic: conceptual
ms.openlocfilehash: f712dc41233b8301e370c9eb63786d8e2d7f8c70
ms.sourcegitcommit: efab6be74671ea4300162e0b30aa8ac134d3b0a9
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/01/2020
ms.locfileid: "84256276"
---
# <a name="azure-libraries-for-python-usage-patterns"></a>Modelli di utilizzo delle librerie di Azure per Python

Azure SDK per Python è costituito esclusivamente da numerose librerie indipendenti, elencate nella [pagina di indice di Azure SDK per Python](https://azure.github.io/azure-sdk/releases/latest/all/python.html).

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

## <a name="inline-json-pattern-for-object-arguments"></a>Modello JSON inline per argomenti oggetto

Molte operazioni all'interno di Azure SDK consentono di esprimere gli argomenti oggetto come oggetti discreti o come JSON inline.

Si supponga, ad esempio, di avere un oggetto [`ResourceManagementClient`](/python/api/azure-mgmt-resource/azure.mgmt.resource.resources.v2019_10_01.resourcemanagementclient?view=azure-python) tramite il quale si crea un gruppo di risorse con il relativo metodo [`create_or_update`](/python/api/azure-mgmt-resource/azure.mgmt.resource.resources.v2019_10_01.operations.resourcegroupsoperations?view=azure-python#create-or-update-resource-group-name--parameters--custom-headers-none--raw-false----operation-config-). Il secondo argomento di questo metodo è di tipo [`ResourceGroup`](/python/api/azure-mgmt-resource/azure.mgmt.resource.resources.v2019_10_01.models.resourcegroup?view=azure-python).

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

Si supponga, ad esempio, di avere un'istanza dell'oggetto [`KeyVaultManagementClient`](/python/api/azure-mgmt-keyvault/azure.mgmt.keyvault.v2019_09_01.keyvaultmanagementclient?view=azure-python) e di chiamare il relativo metodo [`create_or_update`](/python/api/azure-mgmt-keyvault/azure.mgmt.keyvault.v2019_09_01.operations.vaultsoperations?view=azure-python#create-or-update-resource-group-name--vault-name--parameters--custom-headers-none--raw-false--polling-true----operation-config-). In questo caso, il terzo argomento è di tipo [`VaultCreateOrUpdateParameters`](/python/api/azure-mgmt-keyvault/azure.mgmt.keyvault.v2019_09_01.models.vaultcreateorupdateparameters?view=azure-python), che contiene un argomento di tipo [`VaultProperties`](/python/api/azure-mgmt-keyvault/azure.mgmt.keyvault.v2019_09_01.models.vaultproperties?view=azure-python). `VaultProperties`, a sua volta, contiene gli argomenti oggetto di tipo [`Sku`](/python/api/azure-mgmt-keyvault/azure.mgmt.keyvault.v2019_09_01.models.sku?view=azure-python) e [`list[AccessPolicyEntry`](/python/api/azure-mgmt-keyvault/azure.mgmt.keyvault.v2019_09_01.models.accesspolicyentry?view=azure-python). `Sku` contiene un oggetto [`SkuName`](/python/api/azure-mgmt-keyvault/azure.mgmt.keyvault.v2019_09_01.models.skuname?view=azure-python) e ogni oggetto `AccessPolicyEntry` contiene un oggetto [`Permissions`](/python/api/azure-mgmt-keyvault/azure.mgmt.keyvault.v2019_09_01.models.permissions?view=azure-python).

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

Poiché i due formati sono equivalenti, è possibile scegliere quello che si preferisce usare e anche combinarli.

Se il formato JSON non è corretto, si riceve in genere un messaggio di errore analogo a "DeserializationError: Non è possibile deserializzare l'oggetto: tipo, AttributeError: all'oggetto 'str' non è associato un attributo 'Get'". Una causa comune di questo errore è che si specifica una singola stringa di una proprietà quando la libreria prevede un oggetto JSON annidato. Se ad esempio si usa `"sku": "standard"` nell'esempio precedente, questo errore viene generato perché il parametro `sku` è un oggetto `Sku` che prevede un oggetto JSON inline, in questo caso `{ "name": "standard"}`, che è mappato al tipo `SkuName` previsto.

## <a name="next-steps"></a>Passaggi successivi

Ora che si conoscono i modelli di utilizzo comuni delle librerie di Azure per Python, vedere gli esempi autonomi seguenti per esplorare scenari specifici di gestione e librerie client:

- [Esempio: Creare un gruppo di risorse](azure-sdk-example-resource-group.md)
- [Esempio: Usare Archiviazione di Azure](azure-sdk-example-storage.md)
- [Esempio: Effettuare il provisioning di un app Web e distribuire il codice](azure-sdk-example-web-app.md)
- [Esempio: Effettuare il provisioning di una macchina virtuale](azure-sdk-example-virtual-machines.md)

È possibile provare questi esempi in qualsiasi ordine, perché non sono sequenziali né interdipendenti.
