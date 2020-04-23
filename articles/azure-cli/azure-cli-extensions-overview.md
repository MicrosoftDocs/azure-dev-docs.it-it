---
title: Estensioni dell'interfaccia della riga di comando di Azure
description: Uso delle estensioni con l'interfaccia della riga di comando di Azure
author: dbradish-microsoft
ms.author: dbradish
manager: barbkess
ms.date: 09/07/2018
ms.topic: conceptual
ms.service: azure-cli
ms.devlang: azurecli
ms.openlocfilehash: 73488fc464ed052f22f071f6373fb6b8f682b778
ms.sourcegitcommit: 36e02e96b955ed0531f98b9c0f623f4acb508661
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/22/2020
ms.locfileid: "82030983"
---
# <a name="use-extensions-with-azure-cli"></a>Usare le estensioni con l'interfaccia della riga di comando di Azure 

L'interfaccia della riga di comando di Azure consente di caricare estensioni. Le estensioni sono file wheel di Python che non vengono forniti con l'interfaccia della riga di comando, ma vengono eseguiti come comandi dell'interfaccia della riga di comando.
Con le estensioni è possibile accedere ai comandi sperimentali e in versione preliminare, nonché scrivere le proprie interfacce della riga di comando. Questo articolo illustra come gestire le estensioni e risponde a domande comuni relative al loro uso.

## <a name="find-extensions"></a>Trovare le estensioni

Per visualizzare le estensioni fornite e gestite da Microsoft, usare il comando [az extension list-available](/cli/azure/extension#az-extension-list-available).

```azurecli-interactive
az extension list-available --output table
```

L'[elenco delle estensioni](azure-cli-extensions-list.md) è anche disponibile nel sito della documentazione.

## <a name="install-extensions"></a>Installare le estensioni

Una volta trovata un'estensione da installare, usare [az extension add](https://docs.microsoft.com/cli/azure/extension#az-extension-add) per ottenerla. Se l'estensione è elencata in `az extension list-available`, è possibile installarla tramite il nome.

```azurecli-interactive
az extension add --name <extension-name>
```

Se l'estensione proviene da una risorsa esterna o se è disponibile un collegamento diretto a essa, indicare l'URL di origine o il percorso locale. L'estensione _deve_ essere un file wheel di Python compilato.

```azurecli-interactive
az extension add --source <URL-or-path>
```

Dopo aver installato un'estensione, questa viene visualizzata nel valore della variabile della shell `$AZURE_EXTENSION_DIR`. Se questa variabile non è impostata, per impostazione predefinita il valore è `$HOME/.azure/cliextensions` su Linux e macOS e `%USERPROFILE%\.azure\cliextensions` in Windows.

## <a name="update-extensions"></a>Aggiornare le estensioni

Se un'estensione è stata installata in base al nome, aggiornarla con [az extension update](https://docs.microsoft.com/cli/azure/extension#az-extension-update).

```azurecli-interactive
az extension update --name <extension-name>
```

In caso contrario, è possibile aggiornare l'estensione dall'origine seguendo le istruzioni riportate in [Installare le estensioni](#install-extensions).

Se il nome di un'estensione non può essere risolto dall'interfaccia della riga di comando, disinstallare l'estensione e provare a reinstallarla. È anche possibile che l'estensione sia diventata parte dell'interfaccia della riga di comando di base.
Provare ad aggiornare l'interfaccia della riga di comando come descritto in [Installare l'interfaccia della riga di comando di Azure](install-azure-cli.md) e verificare se i comandi dell'estensione sono stati aggiunti.

## <a name="uninstall-extensions"></a>Disinstallare le estensioni

Se un'estensione non è più necessaria, rimuoverla con [az extension remove](https://docs.microsoft.com/cli/azure/extension#az-extension-remove).

```azurecli-interactive
az extension remove --name <extension-name>
```

È anche possibile rimuovere manualmente l'estensione eliminandola dalla posizione in cui è stata installata. La variabile della shell `$AZURE_EXTENSION_DIR` definisce dove vengono installati i moduli.
Se questa variabile non è impostata, per impostazione predefinita il valore è `$HOME/.azure/cliextensions` su Linux e macOS e `%USERPROFILE%\.azure\cliextensions` in Windows.

```bash
rm -rf $AZURE_EXTENSION_DIR/<extension-name>
```

## <a name="faq"></a>Domande frequenti

Di seguito sono fornite alcune risposte ad altre domande comuni sulle estensioni dell'interfaccia della riga di comando.

### <a name="what-file-formats-are-allowed-for-installation"></a>Quali sono i formati di file per cui è consentita l'installazione?

Al momento è possibile installare come estensioni solo i file wheel di Python compilati.

### <a name="can-extensions-replace-existing-commands"></a>Le estensioni possono sostituire i comandi esistenti?

Sì. Le estensioni possono sostituire i comandi esistenti, ma prima di eseguire un comando sostituito l'interfaccia della riga di comando mostrerà un avviso.

### <a name="how-can-i-tell-if-an-extension-is-in-pre-release"></a>Come è possibile stabilire se un'estensione non sia stata rilasciata?

La documentazione e il controllo delle versioni di un'estensione indicano se si tratta della versione non definitiva. Microsoft rilascia spesso comandi in anteprima come estensioni dell'interfaccia della riga di comando, con l'opzione di trasferirli nel prodotto principale dell'interfaccia della riga di comando in un secondo momento. Quando i comandi vengono trasferiti all'esterno delle estensioni, l'estensione precedente deve essere disinstallata. 

### <a name="can-extensions-depend-upon-each-other"></a>Le estensioni possono dipendere l'una dall'altra?

No. Dato che l'interfaccia della riga di comando non garantisce un ordine di caricamento, è possibile che le dipendenze non vengano soddisfatte. La rimozione di un'estensione non ha alcun effetto sulle altre.

### <a name="are-extensions-updated-along-with-the-cli"></a>Le estensioni vengono aggiornate insieme all'interfaccia della riga di comando?

No. Le estensioni devono essere aggiornate separatamente, come descritto in [Aggiornare le estensioni](#update-extensions).

### <a name="how-to-develop-our-own-extension"></a>Come è possibile sviluppare un'estensione personalizzata?
Fare riferimento al repository ufficiale per altre informazioni. [Azure/azure-cli-extensions](https://github.com/Azure/azure-cli/tree/master/doc/extensions)
