---
title: Installare manualmente l'interfaccia della riga di comando di Azure per Linux
description: Come installare manualmente l'interfaccia della riga di comando di Azure in Linux
author: dbradish-microsoft
ms.author: dbradish
manager: barbkess
ms.date: 09/09/2018
ms.topic: conceptual
ms.service: azure-cli
ms.devlang: azurecli
ms.openlocfilehash: 9a98da54f397c1fd03a7cc6b581a769afe84ef88
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/05/2020
ms.locfileid: "82031073"
---
# <a name="install-azure-cli-on-linux-manually"></a>Installare manualmente l'interfaccia della riga di comando di Azure in Linux

Se non è disponibile un pacchetto per l'interfaccia della riga di comando di Azure per la propria distribuzione, installare l'interfaccia manualmente eseguendo uno script.

[!INCLUDE [current-version](includes/current-version.md)]

> [!NOTE]
> Si consiglia vivamente di installare l'interfaccia della riga di comando con una gestione pacchetti. Uno strumento di gestione pacchetti consente di ottenere sempre gli aggiornamenti più recenti e garantisce la stabilità dei componenti dell'interfaccia della riga di comando. Verificare se esiste un pacchetto per la distribuzione prima di eseguire l'installazione manuale.

## <a name="prerequisites"></a>Prerequisites

L'interfaccia della riga di comando richiede il software seguente:

* [Python 3.6.x, 3.7.x o 3.8.x](https://www.python.org/downloads/). 
* [libffi](https://sourceware.org/libffi/)
* [OpenSSL 1.0.2](https://www.openssl.org/source/)

> [!IMPORTANT]
>
> L'interfaccia della riga di comando ha eliminato il supporto per Python 2.7 dalla versione `2.1.0`. Le nuove versioni non garantiscono più l'esecuzione corretta con Python 2.7.

## <a name="install-or-update"></a>Eseguire l'installazione o l'aggiornamento

Sia per l'installazione che per l'aggiornamento dell'interfaccia della riga di comando è necessario eseguire nuovamente lo script di installazione. Installare l'interfaccia della riga di comando eseguendo `curl`.

```bash
curl -L https://aka.ms/InstallAzureCli | bash
```

Lo script può anche essere scaricato ed eseguito nell'ambiente locale. Potrebbe essere necessario riavviare la shell per rendere effettive le modifiche.

È quindi possibile eseguire l'interfaccia della riga di comando di Azure con il comando `az`. Per accedere, usare il comando [az login](/cli/azure/reference-index#az-login).

[!INCLUDE [interactive-login](includes/interactive-login.md)]

Per altre informazioni sui diversi metodi di autenticazione, vedere [Accedere con l'interfaccia della riga di comando di Azure](authenticate-azure-cli.md).

## <a name="troubleshooting"></a>risoluzione dei problemi

Di seguito sono riportati alcuni problemi comuni che possono verificarsi durante l'installazione manuale. Se si verifica un problema non trattato in questo articolo, [segnalarlo in GitHub](https://github.com/Azure/azure-cli/issues).

### <a name="curl-object-moved-error"></a>Errore curl di tipo "Object Moved"

Se viene restituito da `curl` un errore relativo al parametro `-L` o se viene visualizzato un messaggio di errore che include il testo "Object Moved", provare a usare l'URL completo invece dell'URL di reindirizzamento `aka.ms`:

```bash
curl https://azurecliprod.blob.core.windows.net/install | bash
```

### <a name="az-command-not-found"></a>Comando `az` non trovato

Se dopo l'installazione non è possibile eseguire il comando e si usa `bash` o `zsh`, cancellare la cache di hash dei comandi della shell. Esegui

```bash
hash -r
```

e verificare se il problema è stato risolto.

Questo problema può verificarsi anche se la shell non è stata riavviata dopo l'installazione. Assicurarsi che il percorso del comando `az` sia in `$PATH`. La posizione del comando `az` è

```bash
<install path>/bin
```

### <a name="proxy-blocks-connection"></a>Il proxy blocca la connessione

[!INCLUDE[configure-proxy](includes/configure-proxy.md)]

Per ottenere gli script di installazione, il proxy deve consentire le connessioni HTTPS agli indirizzi seguenti:

* `https://aka.ms/`
* `https://azurecliprod.blob.core.windows.net/`
* `https://pypi.python.org`
* Endpoint usati da gestione pacchetti della distribuzione (se presente) per i pacchetti di base

[!INCLUDE[troubleshoot-wsl.md](includes/troubleshoot-wsl.md)]

## <a name="uninstall"></a>Disinstallare

[!INCLUDE [uninstall-boilerplate.md](includes/uninstall-boilerplate.md)]

Disinstallare l'interfaccia della riga di comando eliminando direttamente i file dal percorso scelto al momento dell'installazione. Il percorso di installazione predefinito è `$HOME`.

1. Rimuovere i file dell'interfaccia della riga di comando installati.

   ```bash
   rm -r <install location>/lib/azure-cli
   rm <install location>/bin/az
   ```

2. Modificare il file `$HOME/.bash_profile` per rimuovere la riga seguente:

   ```text
   <install location>/lib/azure-cli/az.completion
   ```

3. Se si usa `bash` o `zsh`, ricaricare la cache dei comandi della shell.

   ```bash
   hash -r
   ```

## <a name="next-steps"></a>Passaggi successivi

Ora che l'interfaccia della riga di comando di Azure è stata installata, passare a una breve presentazione delle relative funzionalità e dei comandi comuni.

> [!div class="nextstepaction"]
> [Introduzione all'interfaccia della riga di comando di Azure](get-started-with-azure-cli.md)
