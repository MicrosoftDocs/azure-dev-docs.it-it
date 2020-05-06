---
title: Estensione alias dell'interfaccia della riga di comando di Azure
description: Come usare l'estensione alias dell'interfaccia della riga di comando di Azure
author: dbradish-microsoft
ms.author: dbradish
manager: barbkess
ms.date: 09/07/2018
ms.topic: conceptual
ms.service: azure-cli
ms.devlang: azurecli
ms.openlocfilehash: 4d324235360e017970a215226572e3effeb9b917
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/05/2020
ms.locfileid: "82030853"
---
# <a name="the-azure-cli-alias-extension"></a>Estensione alias dell'interfaccia della riga di comando di Azure

L'estensione alias consente agli utenti di definire comandi personalizzati per l'interfaccia della riga di comando di Azure usando comandi esistenti. Gli alias aiutano a mantenere il flusso di lavoro semplice, consentendo collegamenti. Essendo basati sul motore di modelli Jinja2, gli alias offrono anche elaborazione avanzata degli argomenti.

> [!NOTE]
> L'estensione alias è disponibile in anteprima pubblica. Le funzionalità e il formato del file di configurazione potrebbero subire modifiche.

## <a name="install-the-alias-extension"></a>Installare l'estensione alias

Per usare l'estensione alias, la versione minima necessaria dell'interfaccia della riga di comando di Azure è la **2.0.28**. Per controllare la versione dell'interfaccia della riga di comando, eseguire `az --version`. Se è necessario aggiornare l'installazione, seguire le istruzioni riportate in [Installare l'interfaccia della riga di comando di Azure](./install-azure-cli.md).

Installare l'estensione con il comando [az extension add](/cli/azure/extension#az-extension-add).

```azurecli-interactive
az extension add --name alias
```

Verificare l'installazione dell'estensione con [az extension list](/cli/azure/extension#az-extension-list). Se l'estensione alias è stata installata correttamente, viene elencata nell'output del comando.

```azurecli-interactive
az extension list --output table --query '[].{Name:name}'
```

```output
Name
------
alias
```

## <a name="keep-the-extension-up-to-date"></a>Mantenere aggiornata l'estensione

L'estensione alias è in fase di sviluppo attivo e vengono rilasciate regolarmente nuove versioni. Le nuove versioni non vengono installate quando si aggiorna l'interfaccia della riga di comando. Installare gli aggiornamenti dell'estensione con [az extension update](/cli/azure/extension#az-extension-update).

```azurecli-interactive
az extension update --name alias
```

## <a name="manage-aliases-for-the-azure-cli"></a>Gestire gli alias per l'interfaccia della riga di comando di Azure

L'estensione alias consente di creare e gestire alias per altri comandi dell'interfaccia della riga di comando. Per visualizzare tutti i comandi disponibili e informazioni dettagliate sui parametri, eseguire il comando alias con `--help`.

```azurecli-interactive
az alias --help
```

## <a name="create-simple-alias-commands"></a>Creare semplici comandi alias

Gli alias possono essere usati per abbreviare gruppi di comandi o nomi di comando esistenti. Ad esempio, è possibile abbreviare il gruppo di comandi `group` in `rg` e il comando `list` in `ls`.

```azurecli-interactive
az alias create --name rg --command group
az alias create --name ls --command list
```

I nuovi alias definiti possono quindi essere usati ovunque verrebbe usata la rispettiva definizione.

```azurecli-interactive
az rg list
az rg ls
az vm ls
```

Non includere `az` nel comando.

Gli alias possono anche essere abbreviazioni di comandi completi. L'esempio successivo elenca i gruppi di risorse disponibili e le rispettive località in un output di tabella:

```azurecli-interactive
az alias create --name ls-groups --command "group list --query '[].{Name:name, Location:location}' --output table"
```

È quindi possibile eseguire `ls-groups` come qualsiasi altro comando dell'interfaccia della riga di comando.

```azurecli-interactive
az ls-groups
```

## <a name="create-an-alias-command-with-arguments"></a>Creare un comando alias con argomenti

È anche possibile aggiungere argomenti posizionali a un comando alias includendoli come `{{ arg_name }}` nel nome dell'alias. Lo spazio vuoto all'interno delle parentesi graffe è obbligatorio.

```azurecli-interactive
az alias create --name "alias_name {{ arg1 }} {{ arg2 }} ..." --command "invoke_including_args"
```

L'alias di esempio successivo mostra come usare argomenti posizionali per ottenere l'indirizzo IP pubblico per una VM.

```azurecli-interactive
az alias create \
    --name "get-vm-ip {{ resourceGroup }} {{ vmName }}" \
    --command "vm list-ip-addresses --resource-group {{ resourceGroup }} --name {{ vmName }}
        --query [0].virtualMachine.network.publicIpAddresses[0].ipAddress"
```

Quando si esegue questo comando, si assegnano valori agli argomenti posizionali.

```azurecli-interactive
az get-vm-ip MyResourceGroup MyVM
```

Nei comandi con alias è anche possibile usare variabili di ambiente che vengono valutate in fase di esecuzione. L'esempio successivo aggiunge l'alias `create-rg` che crea un gruppo di risorse in `eastus` e aggiunge un tag `owner`. A questo tag verrà assegnato il valore della variabile di ambiente locale `USER`.

```azurecli-interactive
az alias create \
    --name "create-rg {{ groupName }}" \
    --command "group create --name {{ groupName }} --location eastus --tags owner=\$USER"
```

Per registrare la variabile di ambiente nel comando dell'alias, è necessario che il simbolo del dollaro `$` sia preceduto da un carattere di escape.

## <a name="process-arguments-using-jinja2-templates"></a>Elaborare gli argomenti con i modelli Jinja2

La sostituzione degli argomenti nell'estensione alias viene eseguita da [Jinja2](http://jinja.pocoo.org/docs/2.10/). I modelli Jinja2 consentono di modificare gli argomenti.

Con i modelli Jinja2 è possibile scrivere alias che accettano tipi di argomenti diversi rispetto al comando sottostante. Ad esempio, è possibile creare un alias che accetta un URL di archiviazione. Tale URL verrà quindi analizzato per passare il nome dell'account e quello del contenitore al comando di archiviazione.

```azurecli-interactive
az alias create \
    --name 'storage-ls {{ url }}' \
    --command "storage blob list
        --account-name {{ url.replace('https://', '').split('.')[0] }}
        --container-name {{ url.replace('https://', '').split('/')[1] }}"
```

Per informazioni sul motore di modelli Jinja2, vedere la [documentazione di Jinja2](http://jinja.pocoo.org/docs/2.10/templates/).

## <a name="alias-configuration-file"></a>File di configurazione degli alias

Un altro modo per creare e modificare gli alias consiste nel modificare il relativo file di configurazione. Le definizioni dei comandi alias vengono scritte in un file di configurazione che si trova in `$AZURE_USER_CONFIG/alias`. Il valore predefinito di `AZURE_USER_CONFIG` è `$HOME/.azure` in macOS e Linux e `%USERPROFILE%\.azure` in Windows. Il file di configurazione degli alias è scritto nel formato di file di configurazione INI. Il formato per i comandi alias è il seguente:

```ini
[alias_name]
command = invoked_commands
```

Per gli alias con argomenti posizionali, il formato per i comandi alias è il seguente:

```ini
[alias_name {{ arg1 }} {{ arg2 }} ...]
command = invoked_commands_including_args
```

## <a name="create-an-alias-command-with-arguments-via-the-alias-configuration-file"></a>Creare un comando alias con argomenti tramite il file di configurazione degli alias

L'esempio seguente mostra un alias per un comando con argomenti. Questo comando ottiene l'indirizzo IP pubblico di una macchina virtuale. I comandi con alias devono trovarsi tutti in un'unica riga e usare tutti gli argomenti nel nome alias.

```ini
[get-vm-ip {{ resourceGroup }} {{ vmName }}]
command = vm list-ip-addresses --resource-group {{ resourceGroup }} --name {{ vmName }} --query [0].virtualMachine.network.publicIpAddresses[0].ipAddress
```

## <a name="uninstall-the-alias-extension"></a>Disinstallare l'estensione alias

Per disinstallare l'estensione, usare il comando [az extension remove](/cli/azure/extension#az-extension-remove).

```azurecli-interactive
az extension remove --name alias
```

Se la disinstallazione viene eseguita a causa di un bug o di un altro problema dell'estensione, [segnalare il problema in GitHub](https://github.com/Azure/azure-cli-extensions/issues) in modo che possa essere fornita una correzione.
