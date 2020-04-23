---
title: Opzioni di configurazione dell'interfaccia della riga di comando di Azure
description: Come configurare l'interfaccia della riga di comando di Azure
author: dbradish-microsoft
ms.author: dbradish
manager: barbkess
ms.date: 06/11/2018
ms.topic: conceptual
ms.service: azure-cli
ms.devlang: azurecli
ms.openlocfilehash: ff5f9f5a5add52bc05009a42aeb00855eb2703fa
ms.sourcegitcommit: 36e02e96b955ed0531f98b9c0f623f4acb508661
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/22/2020
ms.locfileid: "82030843"
---
# <a name="azure-cli-configuration"></a>Configurazione dell'interfaccia della riga di comando di Azure

L'interfaccia della riga di comando di Azure consente all'utente di configurare impostazioni come registrazione, raccolta di dati e valori predefiniti degli argomenti.
L'interfaccia della riga di comando offre un comando utile per gestire alcune impostazioni predefinite, `az configure`. Altri valori possono essere impostati in un file di configurazione o con variabili di ambiente.

I valori di configurazione usati dall'interfaccia della riga di comando vengono valutati nell'ordine di precedenza seguente, con priorità agli elementi nella parte superiore dell'elenco.

1. Parametri della riga di comando
2. Variabili di ambiente
3. Valori nel file di configurazione o impostati con `az configure`

## <a name="cli-configuration-with-az-configure"></a>Configurazione dell'interfaccia della riga di comando con az configure

È possibile impostare i valori predefiniti per l'interfaccia della riga di comando con il comando [az configure](/cli/azure/reference-index#az-configure).
Questo comando accetta un argomento, `--defaults`, ovvero un elenco separato da spazi di coppie `key=value`. I valori specificati vengono usati dall'interfaccia della riga di comando al posto di argomenti obbligatori.

La tabella seguente contiene un elenco delle chiavi di configurazione disponibili.

| Nome | Descrizione |
|------|-------------|
| group | Gruppo di risorse predefinito da usare per tutti i comandi. |
| posizione | Percorso predefinito da usare per tutti i comandi. |
| Web | Nome predefinito dell'app da usare per i comandi `az webapp`. |
| vm | Nome predefinito della VM da usare per i comandi `az vm`. |
| vmss | Il nome del set di scalabilità di macchine virtuali predefinito da usare per i comandi `az vmss`. |
| acr | Nome predefinito del registro contenitori da usare per i comandi `az acr`. |

Ad esempio, ecco come impostare il gruppo di risorse predefinito e il percorso predefinito per tutti i comandi.

```azurecli-interactive
az configure --defaults location=westus2 group=MyResourceGroup
```

## <a name="cli-configuration-file"></a>File di configurazione dell'interfaccia della riga di comando

Il file di configurazione dell'interfaccia della riga di comando contiene altre impostazioni che vengono usate per gestire il comportamento dell'interfaccia della riga di comando. Il file di configurazione stesso si trova in `$AZURE_CONFIG_DIR/config`. Il valore predefinito di `AZURE_CONFIG_DIR` è `$HOME/.azure` su Linux e macOS e `%USERPROFILE%\.azure` su Windows.

I file di configurazione vengono scritti nel formato di file INI. Questo formato di file è definito da intestazioni di sezione, seguite da un elenco di voci chiave-valore.

* Le intestazioni di sezione vengono scritte come `[section-name]`. I nomi delle sezioni distinguono tra maiuscole e minuscole.
* Le voci vengono scritte come `key=value`. I nomi delle chiavi non distinguono tra maiuscole e minuscole.
* I commenti corrispondono a qualsiasi riga che inizia con `#` o `;`. I commenti inline non sono consentiti.

I valori booleani non rispettano la distinzione tra maiuscole e minuscole e sono rappresentati dai valori seguenti.

* __True__: 1, yes, true, on
* __False__: 0, no, false, off

Di seguito è riportato un esempio di file di configurazione dell'interfaccia della riga di comando che disabilita qualsiasi richiesta di conferma e imposta la registrazione nella directory `/var/log/azure`.

```ini
[core]
disable_confirm_prompt=Yes

[logging]
enable_log_file=yes
log_dir=/var/log/azure
```

Vedere la sezione seguente per informazioni dettagliate su tutti i valori di configurazione disponibili e sul rispettivo significato. Per dettagli completi sul formato di file INI, vedere la [documentazione di Python su INI](https://docs.python.org/3/library/configparser.html#supported-ini-file-structure).

## <a name="cli-configuration-values-and-environment-variables"></a>Valori di configurazione dell'interfaccia della riga di comando e variabili di ambiente

La tabella seguente contiene tutte le sezioni e i nomi di opzione che è possibile inserire in un file di configurazione. Le variabili di ambiente corrispondenti vengono impostate come `AZURE_{section}_{name}`, in lettere maiuscole. Ad esempio, il valore predefinito `storage_account` per `batchai` viene impostato nella variabile `AZURE_BATCHAI_STORAGE_ACCOUNT`.

Quando si specifica un valore predefinito, questo argomento non è più richiesto da alcun comando. Viene invece usato il valore predefinito.

| Sezione | Nome      | Type | Descrizione|
|---------|-----------|------|------------|
| __core__ | output | string | Formato di output predefinito. Può essere uno di `json`, `jsonc`, `tsv` o `table`. |
| | disable\_confirm\_prompt | boolean | Consente di attivare o disattivare i prompt di conferma. |
| | collect\_telemetry | boolean | Consente a Microsoft di raccogliere dati anonimi sull'utilizzo dell'interfaccia della riga di comando. Per informazioni sulla privacy, vedere le [Condizioni d'uso dell'interfaccia della riga di comando di Azure](https://aka.ms/AzureCliLegal). |
| __logging__ | enable\_log\_file | boolean | Consente di attivare o disattivare la registrazione. |
| | log\_dir | string | Directory in cui scrivere i log. Per impostazione predefinita, questo valore è `${AZURE_CONFIG_DIR}/logs`. |
| __storage__ | connection\_string | string | Stringa di connessione predefinita da usare per i comandi `az storage`. |
| | account | string | Nome di account predefinito da usare per i comandi `az storage`. |
| | Key | string | Chiave di account predefinita da usare per i comandi `az storage`. |
| | sas\_token | string | Token della firma di accesso condiviso predefinito da usare per i comandi `az storage`. |
| __batchai__ | storage\_account | string | Account di archiviazione predefinito da usare per i comandi `az batchai`. |
| | storage\_key | string | Chiave di archiviazione predefinita da usare per i comandi `az batchai`. |
| __batch__ | account | string | Nome dell'account Azure Batch predefinito da usare per i comandi `az batch`. |
| | access\_key | string | Chiave di accesso predefinita da usare per i comandi `az batch`. Usata solo con l'autorizzazione `aad`. |
| | endpoint | string | Endpoint predefinito a cui connettersi per i comandi `az batch`. |
| | auth\_mode | string | Modalità di autorizzazione da usare per i comandi `az batch`. Può essere `shared_key` o `aad`. |
| __cloud__ | name | string | Il cloud predefinito per tutti i comandi `az`.  I valori possibili sono `AzureCloud` (predefinito), `AzureChinaCloud`, `AzureUSGovernment`, `AzureGermanCloud`. Per cambiare cloud, è possibile usare il comando `az cloud set –name`.  Per un esempio, vedere [Gestire i cloud con l'interfaccia della riga di comando di Azure](manage-clouds-azure-cli.md). |

> [!NOTE]
> È possibile che nel file di configurazione vengano visualizzati altri valori, ma tali valori vengono gestiti direttamente tramite i comandi dell'interfaccia della riga di comando, incluso `az configure`. I valori elencati nella tabella precedente sono gli unici valori che è consigliabile modificare personalmente.
