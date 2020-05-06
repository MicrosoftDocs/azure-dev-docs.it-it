---
title: Eseguire query sui risultati dei comandi con l'interfaccia della riga di comando di Azure
description: Informazioni su come eseguire query JMESPath sull'output dei comandi dell'interfaccia della riga di comando di Azure.
author: dbradish-microsoft
ms.author: dbradish
manager: barbkess
ms.date: 09/23/2019
ms.topic: conceptual
ms.service: azure-cli
ms.devlang: azurecli
ms.openlocfilehash: c9f0afdd626ac8d9af027db02275e5a553595473
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/05/2020
ms.locfileid: "82030743"
---
# <a name="query-azure-cli-command-output"></a>Eseguire query sull'output dei comandi dell'interfaccia della riga di comando di Azure

L'interfaccia della riga di comando di Azure usa l'argomento `--query` per eseguire una [query JMESPath](http://jmespath.org) sui risultati dei comandi. JMESPath è un linguaggio di query per JSON che offre la possibilità di selezionare e modificare i dati dell'output dell'interfaccia della riga di comando. Le query vengono eseguite sull'output JSON prima di qualsiasi formattazione per la visualizzazione.

L'argomento `--query` è supportato da tutti i comandi dell'interfaccia della riga di comando di Azure. Questo articolo illustra come usare le funzionalità di JMESPath con una serie di esempi brevi e semplici.

## <a name="dictionary-and-list-cli-results"></a>Risultati dell'interfaccia della riga di comando di tipo dizionario ed elenco

Anche quando si usa un formato di output diverso da JSON, i risultati dei comandi dell'interfaccia della riga di comando vengono gestiti prima di tutto come JSON per query. L'interfaccia della riga di comando restituisce risultati sotto forma di matrice o di dizionario JSON. Le matrici sono sequenze di oggetti che possono essere indicizzati e i dizionari sono oggetti non ordinati accessibili tramite chiavi. I comandi che _potrebbero_ restituire più di un oggetto restituiscono una matrice e i comandi che restituiscono _sempre_ _solo_ un singolo oggetto restituiscono un dizionario.

## <a name="get-properties-in-a-dictionary"></a>Ottenere le proprietà in un dizionario

Usando i risultati dei dizionari è possibile accedere alle proprietà dal livello principale semplicemente mediante la chiave. Il carattere `.` (__sottoespressione__) viene usato per accedere alle proprietà dei dizionari annidati. Prima di introdurre le query, esaminare l'output non modificato del comando `az vm show`:

```azurecli-interactive
az vm show -g QueryDemo -n TestVM -o json
```

Il comando restituirà come output un dizionario. Alcuni contenuti sono stati omessi.

```json
{
  "additionalCapabilities": null,
  "availabilitySet": null,
  "diagnosticsProfile": {
    "bootDiagnostics": {
      "enabled": true,
      "storageUri": "https://xxxxxx.blob.core.windows.net/"
    }
  },
  ...
  "osProfile": {
    "adminPassword": null,
    "adminUsername": "azureuser",
    "allowExtensionOperations": true,
    "computerName": "TestVM",
    "customData": null,
    "linuxConfiguration": {
      "disablePasswordAuthentication": true,
      "provisionVmAgent": true,
      "ssh": {
        "publicKeys": [
          {
            "keyData": "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDMobZNJTqgjWn/IB5xlilvE4Y+BMYpqkDnGRUcA0g9BYPgrGSQquCES37v2e3JmpfDPHFsaR+CPKlVr2GoVJMMHeRcMJhj50ZWq0hAnkJBhlZVWy8S7dwdGAqPyPmWM2iJDCVMVrLITAJCno47O4Ees7RCH6ku7kU86b1NOanvrNwqTHr14wtnLhgZ0gQ5GV1oLWvMEVg1YFMIgPRkTsSQKWCG5lLqQ45aU/4NMJoUxGyJTL9i8YxMavaB1Z2npfTQDQo9+womZ7SXzHaIWC858gWNl9e5UFyHDnTEDc14hKkf1CqnGJVcCJkmSfmrrHk/CkmF0ZT3whTHO1DhJTtV stramer@contoso",
            "path": "/home/azureuser/.ssh/authorized_keys"
          }
        ]
      }
    },
    "secrets": [],
    "windowsConfiguration": null
  },
  ....
}
```

Il comando seguente ottiene le chiavi pubbliche SSH autorizzate alla connessione alla VM mediante l'aggiunta di una query:

```azurecli-interactive
az vm show -g QueryDemo -n TestVM --query osProfile.linuxConfiguration.ssh.publicKeys -o json
```

```json
[
  {
    "keyData": "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDMobZNJTqgjWn/IB5xlilvE4Y+BMYpqkDnGRUcA0g9BYPgrGSQquCES37v2e3JmpfDPHFsaR+CPKlVr2GoVJMMHeRcMJhj50ZWq0hAnkJBhlZVWy8S7dwdGAqPyPmWM2iJDCVMVrLITAJCno47O4Ees7RCH6ku7kU86b1NOanvrNwqTHr14wtnLhgZ0gQ5GV1oLWvMEVg1YFMIgPRkTsSQKWCG5lLqQ45aU/4NMJoUxGyJTL9i8YxMavaB1Z2npfTQDQo9+womZ7SXzHaIWC858gWNl9e5UFyHDnTEDc14hKkf1CqnGJVcCJkmSfmrrHk/CkmF0ZT3whTHO1DhJTtV stramer@contoso",
    "path": "/home/azureuser/.ssh/authorized_keys"
  }
]
```

## <a name="get-a-single-value"></a>Ottenere un singolo valore

Uno dei casi più comuni è quando è necessario ottenere _un_ solo valore da un comando dell'interfaccia della riga di comando, ad esempio l'ID o il nome di una risorsa di Azure, un nome utente oppure una password. In questo caso, è in genere preferibile archiviare il valore in una variabile di ambiente locale. Per ottenere una singola proprietà, assicurarsi prima di tutto che la query restituisca una sola proprietà. L'ultimo esempio viene modificato per ottenere solo il nome utente dell'amministratore:

```azurecli-interactive
az vm show -g QueryDemo -n TestVM --query 'osProfile.adminUsername' -o json
```

```JSON
"azureuser"
```

Sembra un singolo valore valido, ma si noti che nell'output vengono restituiti i caratteri `"`. Ciò indica che l'oggetto è una stringa JSON. È importante notare che quando si assegna questo valore direttamente come output del comando a una variabile di ambiente, le virgolette potrebbero __non__ essere interpretate dalla shell:

```bash
USER=$(az vm show -g QueryDemo -n TestVM --query 'osProfile.adminUsername' -o json)
echo $USER
```

```output
"azureuser"
```

Non è certamente questo il risultato previsto. In questo caso, è consigliabile usare un formato di output che non racchiude i valori restituiti con informazioni sul tipo. L'opzione di output ottimale offerta dall'interfaccia della riga di comando a questo scopo è `tsv`, ossia valori delimitati da tabulazioni. In particolare, quando si recupera un singolo valore (non un dizionario o un elenco), l'output di `tsv` sarà certamente senza virgolette.

```azurecli-interactive
az vm show -g QueryDemo -n TestVM --query 'osProfile.adminUsername' -o tsv
```

```output
azureuser
```

Per altre informazioni sul formato di output `tsv`, vedere [Formati dell'output - Formato TVS dell'output](format-output-azure-cli.md#tsv-output-format)

## <a name="get-multiple-values"></a>Ottenere più valori

Per ottenere più di una proprietà, racchiudere le espressioni tra parentesi quadre `[ ]` (__elenco a selezione multipla__) sotto forma di elenco con valori delimitati da virgole. Per ottenere simultaneamente il nome della macchina virtuale, l'utente amministratore e la chiave SSH, usare questo comando:

```azurecli-interactive
az vm show -g QueryDemo -n TestVM --query '[name, osProfile.adminUsername, osProfile.linuxConfiguration.ssh.publicKeys[0].keyData]' -o json
```

```json
[
  "TestVM",
  "azureuser",
  "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDMobZNJTqgjWn/IB5xlilvE4Y+BMYpqkDnGRUcA0g9BYPgrGSQquCES37v2e3JmpfDPHFsaR+CPKlVr2GoVJMMHeRcMJhj50ZWq0hAnkJBhlZVWy8S7dwdGAqPyPmWM2iJDCVMVrLITAJCno47O4Ees7RCH6ku7kU86b1NOanvrNwqTHr14wtnLhgZ0gQ5GV1oLWvMEVg1YFMIgPRkTsSQKWCG5lLqQ45aU/4NMJoUxGyJTL9i8YxMavaB1Z2npfTQDQo9+womZ7SXzHaIWC858gWNl9e5UFyHDnTEDc14hKkf1CqnGJVcCJkmSfmrrHk/CkmF0ZT3whTHO1DhJTtV stramer@contoso"
]
```

Questi valori vengono elencati nella matrice di risultati in base all'ordine in cui sono stati specificati nella query. Poiché il risultato è una matrice, non sono presenti chiavi associate ai risultati.

## <a name="rename-properties-in-a-query"></a>Rinominare le proprietà in una query

Per ottenere un dizionario invece di una matrice durante l'esecuzione di query per più valori, usare l'operatore `{ }` (__hash a selezione multipla__).
Il formato di un hash a selezione multipla è `{displayName:JMESPathExpression, ...}`.
`displayName` sarà la stringa visualizzata nell'output e `JMESPathExpression` è l'espressione JMESPath da valutare. Modificare l'esempio della sezione precedente cambiando l'elenco a selezione multipla in un hash:

```azurecli-interactive
az vm show -g QueryDemo -n TestVM --query '{VMName:name, admin:osProfile.adminUsername, sshKey:osProfile.linuxConfiguration.ssh.publicKeys[0].keyData }' -o json
```

```json
{
  "VMName": "TestVM",
  "admin": "azureuser",
  "ssh-key": "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDMobZNJTqgjWn/IB5xlilvE4Y+BMYpqkDnGRUcA0g9BYPgrGSQquCES37v2e3JmpfDPHFsaR+CPKlVr2GoVJMMHeRcMJhj50ZWq0hAnkJBhlZVWy8S7dwdGAqPyPmWM2iJDCVMVrLITAJCno47O4Ees7RCH6ku7kU86b1NOanvrNwqTHr14wtnLhgZ0gQ5GV1oLWvMEVg1YFMIgPRkTsSQKWCG5lLqQ45aU/4NMJoUxGyJTL9i8YxMavaB1Z2npfTQDQo9+womZ7SXzHaIWC858gWNl9e5UFyHDnTEDc14hKkf1CqnGJVcCJkmSfmrrHk/CkmF0ZT3whTHO1DhJTtV stramer@contoso"
}
```

## <a name="get-properties-in-an-array"></a>Ottenere le proprietà in una matrice

Una matrice non ha proprietà specifiche, ma può essere indicizzata. Questa funzionalità è illustrata nell'ultimo esempio con l'espressione `publicKeys[0]`, che ottiene il primo elemento della matrice `publicKeys`. Non è prevista alcuna garanzia relativa all'ordinamento dell'output dell'interfaccia della riga di comando, quindi è consigliabile evitare l'uso dell'indicizzazione a meno che non sia sicuri dell'ordine o che l'elemento ottenuto non sia rilevante. Per accedere alle proprietà degli elementi di una matrice, sono disponibili due operazioni, ovvero _rendere flat_ e _applicare filtri_. Questa sezione illustra la procedura per rendere flat una matrice.

Per rendere flat una matrice è necessario usare l'operatore `[]` di JMESPath. Tutte le espressioni dopo l'operatore `[]` vengono applicate a ogni elemento nella matrice corrente.
Se `[]` viene visualizzato all'inizio della query, il risultato dei comandi dell'interfaccia della riga di comando viene reso flat. I risultati di `az vm list` possono essere esaminati con questa funzionalità.
Per ottenere il nome, il sistema operativo e il nome dell'amministratore per ogni VM in un gruppo di risorse:

```azurecli-interactive
az vm list -g QueryDemo --query '[].{Name:name, OS:storageProfile.osDisk.osType, admin:osProfile.adminUsername}' -o json
```

```json
[
  {
    "Name": "Test-2",
    "OS": "Linux",
    "admin": "sttramer"
  },
  {
    "Name": "TestVM",
    "OS": "Linux",
    "admin": "azureuser"
  },
  {
    "Name": "WinTest",
    "OS": "Windows",
    "admin": "winadmin"
  }
]
```

Se vengono combinati con il formato dell'output `--output table`, i nomi delle colonne vengono associati al valore `displayKey` dell'hash a selezione multipla:

```azurecli-interactive
az vm list -g QueryDemo --query '[].{Name:name, OS:storageProfile.osDisk.osType, Admin:osProfile.adminUsername}' --output table
```

```output
Name     OS       Admin
-------  -------  ---------
Test-2   Linux    sttramer
TestVM   Linux    azureuser
WinTest  Windows  winadmin
```

> [!NOTE]
>
> Determinate chiavi vengono escluse tramite filtro e non vengono stampate nella visualizzazione della tabella. Si tratta delle chiavi `id`, `type` e `etag`. Per visualizzare questi valori è possibile modificare il nome della chiave in un hash a selezione multipla.
>
> ```azurecli-interactive
> az vm show -g QueryDemo -n TestVM --query "{objectID:id}" -o table
> ```

È possibile rendere flat qualsiasi matrice, non solo il risultato principale restituito dal comando. Nell'ultima sezione l'espressione `osProfile.linuxConfiguration.ssh.publicKeys[0].keyData` è stata usata per ottenere la chiave pubblica SSH per l'accesso. Per ottenere _ogni_ chiave pubblica SSH, l'espressione può essere scritta come `osProfile.linuxConfiguration.ssh.publicKeys[].keyData`.
Questa espressione di query rende flat la matrice `osProfile.linuxConfiguration.ssh.publicKeys` e quindi esegue l'espressione `keyData` in ogni elemento:

```azurecli-interactive
az vm show -g QueryDemo -n TestVM --query '{VMName:name, admin:osProfile.adminUsername, sshKeys:osProfile.linuxConfiguration.ssh.publicKeys[].keyData }' -o json
```

```json
{
  "VMName": "TestVM",
  "admin": "azureuser",
  "sshKeys": [
    "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDMobZNJTqgjWn/IB5xlilvE4Y+BMYpqkDnGRUcA0g9BYPgrGSQquCES37v2e3JmpfDPHFsaR+CPKlVr2GoVJMMHeRcMJhj50ZWq0hAnkJBhlZVWy8S7dwdGAqPyPmWM2iJDCVMVrLITAJCno47O4Ees7RCH6ku7kU86b1NOanvrNwqTHr14wtnLhgZ0gQ5GV1oLWvMEVg1YFMIgPRkTsSQKWCG5lLqQ45aU/4NMJoUxGyJTL9i8YxMavaB1Z2npfTQDQo9+womZ7SXzHaIWC858gWNl9e5UFyHDnTEDc14hKkf1CqnGJVcCJkmSfmrrHk/CkmF0ZT3whTHO1DhJTtV stramer@contoso\n"
  ]
}
```

## <a name="filter-arrays"></a>Applicare filtri alle matrici

L'altra operazione usata per ottenere dati da una matrice è l'_applicazione di filtri_. L'applicazione di filtri viene eseguita con l'operatore `[?...]` di JMESPath.
Questo operatore accetta un predicato come contenuto. Un predicato è qualsiasi istruzione che può essere valutata e restituisce `true` o `false`. Le espressioni in cui il predicato restituisce `true` vengono incluse nell'output.

JMESPath offre gli operatori di confronto e logici standard, che includono `<`, `<=`, `>`, `>=`, `==` e `!=`.
JMESPath supporta anche operatori logici e (`&&`) oppure (`||`), ma non (`!`). Le espressioni possono essere raggruppate entro parentesi, consentendo di creare espressioni del predicato più complesse. Per dettagli completi sui predicati e sulle operazioni logiche, vedere la [specifica di JMESPath](http://jmespath.org/specification.html).

Nell'ultima sezione è stata resa flat una matrice per ottenere l'elenco completo di tutte le VM in un gruppo di risorse. Questo output può essere limitato solo a VM Linux mediante i filtri:

```azurecli-interactive
az vm list -g QueryDemo --query "[?storageProfile.osDisk.osType=='Linux'].{Name:name,  admin:osProfile.adminUsername}" --output table
```

```output
Name    Admin
------  ---------
Test-2  sttramer
TestVM  azureuser
```

> [!IMPORTANT]
>
> In JMESPath le stringhe sono sempre racchiuse tra virgolette singole (`'`). Se si usano le virgolette doppie come parte di una stringa in un predicato di filtro, si otterrà un output vuoto.

JMESPath include anche funzioni predefinite utili per l'applicazione di filtri. Una di queste funzioni è `contains(string, substring)`, che verifica se una stringa contiene una sottostringa. Le espressioni vengono valutate prima della chiamata della funzione, quindi il primo argomento può essere un'espressione JMESPath completa. L'esempio successivo trova tutte le macchine virtuali che usano le risorse di archiviazione SSD per il rispettivo disco del sistema operativo:

```azurecli-interactive
az vm list -g QueryDemo --query "[?contains(storageProfile.osDisk.managedDisk.storageAccountType,'SSD')].{Name:name, Storage:storageProfile.osDisk.managedDisk.storageAccountType}" -o json
```

```json
[
  {
    "Name": "TestVM",
    "Storage": "StandardSSD_LRS"
  },
  {
    "Name": "WinTest",
    "Storage": "StandardSSD_LRS"
  }
]
```

Questa query è un po' troppo lunga. La chiave `storageProfile.osDisk.managedDisk.storageAccountType` viene indicata due volte e questa chiave viene reimpostata nell'output. Un modo per abbreviare l'espressione consiste nell'applicare il filtro dopo avere reso flat e avere selezionato i dati.

```azurecli-interactive
az vm list -g QueryDemo --query "[].{Name:name, Storage:storageProfile.osDisk.managedDisk.storageAccountType}[?contains(Storage,'SSD')]" -o json
```

```json
[
  {
    "Name": "TestVM",
    "Storage": "StandardSSD_LRS"
  },
  {
    "Name": "WinTest",
    "Storage": "StandardSSD_LRS"
  }
]
```

Per le matrici di grandi dimensioni, potrebbe risultare più veloce applicare il filtro prima di selezionare i dati.

Per l'elenco completo delle funzioni, vedere [JMESPath specification - Built-in Functions](http://jmespath.org/specification.html#built-in-functions) (Specifica JMESPath - Funzioni predefinite).

## <a name="change-output"></a>Modificare l'output

Le funzioni JMESPath hanno anche un'altra finalità, ovvero eseguire operazioni sui risultati di una query. Qualsiasi funzione che restituisce un valore non booleano cambia il risultato di un'espressione.
È ad esempio possibile ordinare i dati in base al valore di una proprietà con `sort_by(array, &sort_expression)`. JMESPath usa un operatore speciale, `&`, per espressioni che devono essere valutate successivamente come parte di una funzione. L'esempio seguente mostra come ordinare un elenco di VM in base a dimensioni del disco del sistema operativo:

```azurecli-interactive
az vm list -g QueryDemo --query "sort_by([].{Name:name, Size:storageProfile.osDisk.diskSizeGb}, &Size)" --output table
```

```output
Name     Size
-------  ------
TestVM   30
Test-2   32
WinTest  127
```

Per l'elenco completo delle funzioni, vedere [JMESPath specification - Built-in Functions](http://jmespath.org/specification.html#built-in-functions) (Specifica JMESPath - Funzioni predefinite).

## <a name="experiment-with-queries-interactively"></a>Sperimentare le query in modo interattivo

Per iniziare a sperimentare con JMESPath, il pacchetto [JMESPath-terminal](https://github.com/jmespath/jmespath.terminal) di Python offre un ambiente interattivo per l'uso delle query. I dati vengono inviati tramite pipe come input e quindi le query vengono scritte ed eseguite nell'editor.

```bash
pip install jmespath-terminal
az vm list --output json | jpterm
```
