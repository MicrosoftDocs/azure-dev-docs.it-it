---
title: Formati di output per l'interfaccia della riga di comando di Azure
description: Informazioni su come formattare l'output dei comandi dell'interfaccia della riga di comando di Azure in tabelle, elenchi o JSON.
author: dbradish-microsoft
ms.author: dbradish
manager: barbkess
ms.date: 09/23/2019
ms.topic: conceptual
ms.service: azure-cli
ms.devlang: azurecli
ms.openlocfilehash: 7f28a5cc7acd7e5d41e1ab91424a9f360a25b702
ms.sourcegitcommit: 36e02e96b955ed0531f98b9c0f623f4acb508661
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/22/2020
ms.locfileid: "82031033"
---
# <a name="output-formats-for-azure-cli-commands"></a>Formati di output per i comandi dell'interfaccia della riga di comando di Azure

L'interfaccia della riga di comando di Azure usa JSON come formato di output predefinito, ma offre anche altri formati.  Usare il parametro `--output` (`--out` o `-o`) per formattare l'output dell'interfaccia della riga di comando. I valori degli argomenti e i tipi di output sono i seguenti:

--output | Descrizione
---------|-------------------------------
`json`   | Stringa JSON. Questa è l'impostazione predefinita
`jsonc`  | Stringa JSON colorata
`yaml`   | YAML, alternativa leggibile dal computer a JSON
`table`  | Tabella ASCII con chiavi come intestazioni di colonna
`tsv`    | Valori delimitati da tabulazioni, senza chiavi
`none`   | Nessun output oltre a errori e avvisi

## <a name="json-output-format"></a>Formato di output JSON

L'esempio seguente mostra l'elenco di macchine virtuali nelle proprie sottoscrizioni nel formato JSON predefinito.

```azurecli-interactive
az vm list --output json
```

Nell'output seguente sono stati omessi alcuni campi per brevità e le informazioni di identificazione sono state sostituite.

```json
[
  {
    "availabilitySet": null,
    "diagnosticsProfile": null,
    "hardwareProfile": {
      "vmSize": "Standard_DS1"
    },
    "id": "/subscriptions/.../resourceGroups/DEMORG1/providers/Microsoft.Compute/virtualMachines/DemoVM010",
    "instanceView": null,
    "licenseType": null,
    "location": "westus",
    "name": "DemoVM010",
    "networkProfile": {
      "networkInterfaces": [
        {
          "id": "/subscriptions/.../resourceGroups/demorg1/providers/Microsoft.Network/networkInterfaces/DemoVM010VMNic",
          "primary": null,
          "resourceGroup": "demorg1"
        }
      ]
    },
          ...
          ...
          ...
]
```

## <a name="yaml-output-format"></a>Formato di output YAML

Il formato `yaml` stampa l'output come [YAML](http://yaml.org/), un formato di serializzazione dei dati in testo normale. YAML tende a essere più facile da leggere rispetto a JSON, a cui può comunque essere eseguito facilmente il mapping. Alcune applicazioni e alcuni comandi dell'interfaccia della riga di comando accettano YAML come input di configurazione invece di JSON.

```azurecli-interactive
az vm list --out yaml
```

Nell'output seguente sono stati omessi alcuni campi per brevità e le informazioni di identificazione sono state sostituite.

```yaml
- availabilitySet: null
  diagnosticsProfile: null
  hardwareProfile:
    vmSize: Standard_DS1_v2
  id: /subscriptions/.../resourceGroups/DEMORG1/providers/Microsoft.Compute/virtualMachines/DemoVM010
  identity: null
  instanceView: null
  licenseType: null
  location: westus
  name: ExampleVM1
  networkProfile:
    networkInterfaces:
    - id: /subscriptions/.../resourceGroups/DemoRG1/providers/Microsoft.Network/networkInterfaces/DemoVM010Nic
      primary: null
      resourceGroup: DemoRG1
  ...
...
```

## <a name="table-output-format"></a>Formato dell'output della tabella

Il formato `table` stampa l'output come tabella ASCII, facilitando la lettura e l'analisi. Gli oggetti annidati non sono inclusi nell'output della tabella, ma possono comunque essere filtrati come parte di una query. Alcuni campi non sono inclusi nella tabella, quindi questo formato è ottimale quando si vuole una panoramica rapida dei dati nella quale gli utenti possono eseguire ricerche.

```azurecli-interactive
az vm list --out table
```

```output
Name         ResourceGroup    Location
-----------  ---------------  ----------
DemoVM010    DEMORG1          westus
demovm212    DEMORG1          westus
demovm213    DEMORG1          westus
KBDemo001VM  RGDEMO001        westus
KBDemo020    RGDEMO001        westus
```

È possibile usare il parametro `--query` per personalizzare le proprietà e le colonne che si desidera mostrare nell'output. L'esempio seguente mostra come selezionare solo il nome della macchina virtuale e il nome del gruppo di risorse nel comando `list`.

```azurecli-interactive
az vm list --query "[].{resource:resourceGroup, name:name}" -o table
```

```output
Resource    Name
----------  -----------
DEMORG1     DemoVM010
DEMORG1     demovm212
DEMORG1     demovm213
RGDEMO001   KBDemo001VM
RGDEMO001   KBDemo020
```

> [!NOTE]
> Per impostazione predefinita, alcune chiavi non vengono stampate nella visualizzazione tabella. Si tratta di `id`, `type` e `etag`. Se è necessario includerle nell'output, è possibile usare la funzionalità JMESPath per il reinserimento delle chiavi in modo da modificare il nome delle chiavi ed evitare i filtri.
>
> ```azurecli-interactive
> az vm list --query "[].{objectID:id}" -o table
> ```

Per altre informazioni sull'uso delle query per filtrare i dati, vedere [Usare query JMESPath con l'interfaccia della riga di comando di Azure](/cli/azure/query-azure-cli).

## <a name="tsv-output-format"></a>Formato TSV dell'output

Il formato `tsv` dell'output restituisce valori delimitati da tabulazioni e da nuove righe senza formattazione aggiuntiva, chiavi o altri simboli. Questo formato consente di usare agevolmente l'output in altri comandi e strumenti che devono elaborare il testo in una determinata forma. Come il formato `table`, `tsv` non stampa gli oggetti annidati.

Se si usa l'esempio precedente con l'opzione `tsv` si genera il risultato separato da tabulazioni.

```azurecli-interactive
az vm list --out tsv
```

```output
None    None        /subscriptions/.../resourceGroups/DEMORG1/providers/Microsoft.Compute/virtualMachines/DemoVM010    None    None    westus    DemoVM010            None    Succeeded    DEMORG1    None            Microsoft.Compute/virtualMachines    cbd56d9b-9340-44bc-a722-25f15b578444
None    None        /subscriptions/.../resourceGroups/DEMORG1/providers/Microsoft.Compute/virtualMachines/demovm212    None    None    westus    demovm212            None    Succeeded    DEMORG1    None            Microsoft.Compute/virtualMachines    4bdac85d-c2f7-410f-9907-ca7921d930b4
None    None        /subscriptions/.../resourceGroups/DEMORG1/providers/Microsoft.Compute/virtualMachines/demovm213    None    None    westus    demovm213            None    Succeeded    DEMORG1    None            Microsoft.Compute/virtualMachines    2131c664-221a-4b7f-9653-f6d542fbfa34
None    None        /subscriptions/.../resourceGroups/RGDEMO001/providers/Microsoft.Compute/virtualMachines/KBDemo001VM    None    None    westus    KBDemo001VM            None    Succeeded    RGDEMO001    None            Microsoft.Compute/virtualMachines    14e74761-c17e-4530-a7be-9e4ff06ea74b
None    None        /subscriptions/.../resourceGroups/RGDEMO001/providers/Microsoft.Compute/virtualMachines/KBDemo020   None    None    westus    KBDemo020            None    Succeeded    RGDEMO001    None            Microsoft.Compute/virtualMachines    36baa9-9b80-48a8-b4a9-854c7a858ece
```

Una restrizione del formato di output TSV è che non esiste alcuna garanzia relativa all'ordinamento dell'output. L'interfaccia della riga di comando prova a mantenere l'ordinamento elencando le chiavi nella risposta JSON in ordine alfabetico e quindi stampandone i valori per l'output in formato TSV. Questo comportamento non garantisce però che l'ordine sia sempre identico, in quanto il formato della risposta del servizio di Azure può cambiare.

Per applicare un ordinamento coerente, è necessario usare il parametro `--query` e il formato dell'[elenco a selezione multipla](query-azure-cli.md#get-multiple-values). Quando un comando dell'interfaccia della riga di comando restituisce un singolo dizionario JSON, usare il formato generico `[key1, key2, ..., keyN]` per forzare l'ordinamento delle chiavi.  Per i comandi dell'interfaccia della riga di comando che restituiscono una matrice, usare il formato generico `[].[key1, key2, ..., keyN]` per ordinare i valori delle colonne.

Ad esempio, per ordinare le informazioni visualizzate in precedenza in base all'ID, alla località, al gruppo di risorse e al nome della macchina virtuale:

```azurecli-interactive
az vm list --out tsv --query '[].[id, location, resourceGroup, name]'
```

```output
/subscriptions/.../resourceGroups/DEMORG1/providers/Microsoft.Compute/virtualMachines/DemoVM010    westus    DEMORG1    DemoVM010
/subscriptions/.../resourceGroups/DEMORG1/providers/Microsoft.Compute/virtualMachines/demovm212    westus    DEMORG1    demovm212
/subscriptions/.../resourceGroups/DEMORG1/providers/Microsoft.Compute/virtualMachines/demovm213    westus    DEMORG1    demovm213
/subscriptions/.../resourceGroups/RGDEMO001/providers/Microsoft.Compute/virtualMachines/KBDemo001VM     westus  RGDEMO001       KBDemo001VM
/subscriptions/.../resourceGroups/RGDEMO001/providers/Microsoft.Compute/virtualMachines/KBDemo020       westus  RGDEMO001       KBDemo020
```

L'esempio seguente illustra come l'output `tsv` possa essere inviato tramite pipe ad altri comandi in Bash. La query viene usata per filtrare l'output e forzare l'ordinamento. `grep` seleziona gli elementi che contengono il testo "RGD" e quindi il comando `cut` seleziona il quarto campo per mostrare il nome della macchina virtuale nell'output.

```azurecli-interactive
az vm list --out tsv --query '[].[id, location, resourceGroup, name]' | grep RGD | cut -f4
```

```output
KBDemo001VM
KBDemo020
```

## <a name="set-the-default-output-format"></a>Configurare il formato di output predefinito

Usare il comando `az configure` interattivo per configurare l'ambiente e le impostazioni predefinite per i formati dell'output. Il formato di output predefinito è `json`.

```azurecli-interactive
az configure
```

```output
Welcome to the Azure CLI! This command will guide you through logging in and setting some default values.

Your settings can be found at /home/defaultuser/.azure/config
Your current configuration is as follows:

  ...

Do you wish to change your settings? (y/N): y

What default output format would you like?
 [1] json - JSON formatted output that most closely matches API responses.
 [2] jsonc - Colored JSON formatted output that most closely matches API responses.
 [3] table - Human-readable output format.
 [4] tsv - Tab- and Newline-delimited. Great for GREP, AWK, etc.
 [5] yaml - YAML formatted output. An alternative to JSON. Great for configuration files.
 [6] none - No output, except for errors and warnings.
Please enter a choice [1]:
```

Per altre informazioni sulla configurazione dell'ambiente, vedere [Configurazione dell'interfaccia della riga di comando di Azure](/cli/azure/azure-cli-configuration).
