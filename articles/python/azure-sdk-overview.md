---
title: Usare Azure SDK per Python
description: Panoramica delle caratteristiche e delle funzionalità di Azure SDK per Python che consentono agli sviluppatori di aumentare la produttività per il provisioning, l'uso e la gestione di risorse di Azure.
ms.date: 05/13/2020
ms.topic: conceptual
ms.openlocfilehash: 856487b06e7a84b0659126d8959ce45b5309a103
ms.sourcegitcommit: fbbc341a0b9e17da305bd877027b779f5b0694cc
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/19/2020
ms.locfileid: "83631540"
---
# <a name="use-the-azure-sdk-for-python"></a>Usare Azure SDK per Python

Azure SDK per Python open source semplifica il provisioning, la gestione e l'uso di risorse di Azure dal codice applicativo Python.

## <a name="the-details-you-really-want-to-know"></a>Informazioni che è necessario conoscere

- L'SDK supporta Python 2.7 e Python 3.5.3 o versione successiva ed è stato testato anche con PyPy 5.4 e versioni successive.

- L'SDK è costituito da oltre 180 singole librerie Python correlate a servizi specifici di Azure.

- Installare le librerie necessarie con `pip install <library_name>`, usando i nomi delle librerie disponibili nell'[elenco di versioni](https://azure.github.io/azure-sdk/releases/latest/all/python.html). Per altre informazioni, vedere [Installare le librerie di Azure SDK](azure-sdk-install.md).

- Esistono librerie di "gestione" e "client" distinte (talvolta denominate librerie del "piano di gestione" e del "piano dati"). Ogni set serve a scopi diversi e viene usato da tipi di codice diversi. Per altre informazioni, vedere le sezioni seguenti più avanti in questo articolo:
  - [Effettuare il provisioning e la gestione di risorse di Azure con le librerie di gestione](#provision-and-manage-azure-resources-with-management-libraries)
  - [Connettersi e usare le risorse di Azure con le librerie client](#connect-to-and-use-azure-resources-with-client-libraries)

- La documentazione relativa all'SDK è disponibile nelle [informazioni di riferimento di Azure SDK per Python](/python/api/overview/azure/?view=azure-python), che sono organizzate in base a servizio di Azure, oppure nel [browser delle API Python](/python/api/?view=azure-python), che è organizzato in base al nome del pacchetto. Al momento, è spesso necessario fare clic su diversi livelli per ottenere le classi e i metodi a cui si è interessati. Ci scusiamo in anticipo per questa esperienza poco soddisfacente. Stiamo lavorando per migliorarla.

- Per provare le librerie, iniziare con [Esempio: Creare un gruppo di risorse](azure-sdk-example-resource-group.md) e proseguire con [Esempio: Usare Archiviazione di Azure](azure-sdk-example-storage.md).

### <a name="non-essential-but-still-interesting-details"></a>Dettagli non essenziali ma comunque interessanti

- Poiché l'interfaccia della riga di comando di Azure è scritta in Python usando le librerie di gestione dell'SDK, qualsiasi operazione eseguibile con i comandi dell'interfaccia della riga di comando di Azure può essere completata anche con uno script Python. Detto questo, i comandi dell'interfaccia della riga di comando offrono molte funzionalità utili, ad esempio l'esecuzione simultanea di più attività, la gestione automatica di operazioni asincrone, la formattazione dell'output come stringhe di connessione e così via. Di conseguenza, l'uso dell'interfaccia della riga di comando (o dell'equivalente Azure PowerShell) per gli script automatizzati di provisioning e gestione può risultare notevolmente più agevole rispetto alla scrittura di codice Python equivalente, a meno che non serva un grado di controllo molto più rigoroso sul processo.

- Azure SDK per Python è un livello Python sull'API REST di Azure sottostante, consentendo di usare tali API tramite paradigmi Python ben noti. Tuttavia, se si preferisce, è sempre possibile usare l'API REST direttamente dal codice Python.

- Il codice di origine per l'SDK è disponibile all'indirizzo [https://github.com/Azure/azure-sdk-for-python](https://github.com/Azure/azure-sdk-for-python). Essendo un progetto open source, i contributi sono benvenuti.

- Anche se è possibile usare l'SDK con interpreti come IronPython e Jython che non vengono testati, si potrebbero riscontrare incompatibilità e problemi isolati.

- Il repository di origine per la documentazione dell'SDK è disponibile all'indirizzo [https://github.com/MicrosoftDocs/azure-docs-sdk-python/](https://github.com/MicrosoftDocs/azure-docs-sdk-python/).

- Stiamo aggiornando le librerie client di Azure SDK per Python per condividere modelli cloud comuni come protocolli di autenticazione, registrazione, traccia, protocolli di trasporto, risposte memorizzate nel buffer e tentativi di ripetizione.

  - Questa funzionalità condivida è contenuta nella libreria [azure-core](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/core/azure-core).

  - Le librerie attualmente compatibili con la libreria Core sono riportate nella pagina [Versioni più recenti di Azure SDK per Python](https://azure.github.io/azure-sdk/releases/latest/#python). Queste librerie, principalmente le librerie client, sono a volte identificate come "track 2".

  - Le librerie di gestione, che non sono state ancora aggiornate, sono a volte identificate come "track 1".

- Per informazioni dettagliate sulle linee guida applicabili all'SDK, vedere [Linee guida di Python: introduzione](https://azure.github.io/azure-sdk/python_introduction.html).

## <a name="provision-and-manage-azure-resources-with-management-libraries"></a>Effettuare il provisioning e la gestione di risorse di Azure con le librerie di gestione

Le librerie di *gestione* (o "piano di gestione") dell'SDK, i cui nomi iniziano con `azure-mgmt-`, consentono di creare, sottoporre a provisioning e altrimenti gestire le risorse di Azure da script Python. Per tutti i servizi di Azure sono disponibili librerie di gestione corrispondenti.

Con le librerie di gestione è possibile scrivere script di configurazione e distribuzione per eseguire le stesse attività eseguibili tramite il [portale di Azure](https://portal.azure.com) o l'[interfaccia della riga di comando di Azure](/cli/azure/install-azure-cli). Come indicato in precedenza, l'interfaccia della riga di comando di Azure è scritta in Python e usa le librerie di gestione per implementare i vari comandi.

Per informazioni dettagliate sull'uso di ogni libreria specifica, vedere il file *README.md* o *README.rst* disponibile nella cartella di progetto della libreria nel [repository GitHub](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk) dell'SDK. È anche possibile trovare frammenti di codice aggiuntivi nella [documentazione di riferimento](/python/api?view=azure-python) e negli [esempi di Azure](https://docs.microsoft.com/samples/browse/?languages=python&products=azure).

## <a name="connect-to-and-use-azure-resources-with-client-libraries"></a>Connettersi e usare le risorse di Azure con le librerie client

Le librerie *client* (o "piano dati") dell'SDK consentono di scrivere il codice applicativo Python per interagire con servizi di cui è già stato effettuato il provisioning. L'SDK fornisce librerie client solo per i servizi che supportano un'API client.

Per informazioni dettagliate sull'uso di ogni libreria client, vedere il file *README.md* o *README.rst* disponibile nella cartella di progetto della libreria nel [repository GitHub](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk) dell'SDK. È anche possibile trovare frammenti di codice aggiuntivi nella [documentazione di riferimento](/python/api?view=azure-python) e negli [esempi di Azure](https://docs.microsoft.com/samples/browse/?languages=python&products=azure).

## <a name="inline-json-pattern-for-object-arguments"></a>Modello JSON inline per argomenti oggetto

Molte operazioni all'interno di Azure SDK supportano un modello comune che consente di esprimere gli argomenti oggetto come oggetti discreti o come JSON inline.

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

Quando si usa JSON, l'SDK converte automaticamente il file JSON inline nel tipo di oggetto appropriato per l'argomento in questione.

Gli oggetti possono anche avere argomenti oggetto annidati, nel qual caso si può anche usare codice JSON annidato.

Si supponga, ad esempio, di avere un'istanza dell'oggetto [`KeyVaultManagementClient`](/python/api/azure-mgmt-keyvault/azure.mgmt.keyvault.v2019_09_01.keyvaultmanagementclient?view=azure-python) e di chiamare il relativo metodo [`create_or_update`](/python/api/azure-mgmt-keyvault/azure.mgmt.keyvault.v2019_09_01.operations.vaultsoperations?view=azure-python#create-or-update-resource-group-name--vault-name--parameters--custom-headers-none--raw-false--polling-true----operation-config-). In questo caso, il terzo argomento è di tipo [`VaultCreateOrUpdateParameters`](/python/api/azure-mgmt-keyvault/azure.mgmt.keyvault.v2019_09_01.models.vaultcreateorupdateparameters?view=azure-python), che contiene un argomento di tipo [`VaultProperties`](/python/api/azure-mgmt-keyvault/azure.mgmt.keyvault.v2019_09_01.models.vaultproperties?view=azure-python). `VaultProperties`, a sua volta, contiene gli argomenti oggetto di tipo [`Sku`](/python/api/azure-mgmt-keyvault/azure.mgmt.keyvault.v2019_09_01.models.sku?view=azure-python) e [`list[AccessPolicyEntry`](/python/api/azure-mgmt-keyvault/azure.mgmt.keyvault.v2019_09_01.models.accesspolicyentry?view=azure-python). `Sku` contiene un oggetto [`SkuName`](/python/api/azure-mgmt-keyvault/azure.mgmt.keyvault.v2019_09_01.models.skuname?view=azure-python) e ogni oggetto `AccessPolicyEntry` contiene un oggetto [`Permissions`](/python/api/azure-mgmt-keyvault/azure.mgmt.keyvault.v2019_09_01.models.permissions?view=azure-python).

Per chiamare `create_or_update` con gli oggetti incorporati viene usato codice simile al seguente (presupponendo che `tenant_id` e `object_id` siano già definiti):

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

Poiché i due formati sono completamente equivalenti, è possibile scegliere quello che si preferisce usare e anche combinarli.

Se il formato JSON non è corretto, si riceve in genere un messaggio di errore analogo a "DeserializationError: Non è possibile deserializzare l'oggetto: tipo, AttributeError: all'oggetto 'str' non è associato un attributo 'Get'". Una causa comune di questo errore è che si specifica una singola stringa di una proprietà quando l'SDK prevede un oggetto JSON annidato. Se ad esempio si usa `"sku": "standard"` nell'esempio precedente, questo errore viene generato perché il parametro `sku` è un oggetto `Sku` che prevede un oggetto JSON inline, in questo caso `{ "name": "standard"}`, che è mappato al tipo `SkuName` previsto.

## <a name="next-step"></a>Passaggio successivo

> [!div class="nextstepaction"]
> [Installare le librerie SDK >>>](azure-sdk-install.md)

## <a name="get-help-and-connect-with-the-sdk-team"></a>Ottenere assistenza e contattare il team dell'SDK

- Visitare la documentazione di [Azure SDK per Python](https://aka.ms/python-docs)
- Pubblicare le domande per la community in [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-sdk-python).
- Aprire i problemi relativi all'SDK in [GitHub](https://github.com/Azure/azure-sdk-for-python/issues)
- Menzionare [@AzureSDK](https://twitter.com/AzureSdk/) su Twitter
