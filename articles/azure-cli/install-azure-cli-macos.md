---
title: Installare l'interfaccia della riga di comando di Azure per macOS
description: Come installare l'interfaccia della riga di comando di Azure in macOS
author: dbradish-microsoft
ms.author: dbradish
manager: barbkess
ms.date: 11/05/2018
ms.topic: conceptual
ms.service: azure-cli
ms.devlang: azurecli
ms.openlocfilehash: 862ebf9144d7e81be6dda550eba108198f38d397
ms.sourcegitcommit: 36e02e96b955ed0531f98b9c0f623f4acb508661
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/22/2020
ms.locfileid: "82031063"
---
# <a name="install-azure-cli-on-macos"></a>Installare l'interfaccia della riga di comando di Azure in macOS

Per la piattaforma macOS, è possibile installare l'interfaccia della riga di comando di Azure con la [gestione pacchetti Homebrew](https://brew.sh). Homebrew rende più semplice mantenere l'installazione dell'interfaccia della riga di comando sempre aggiornata. Il pacchetto dell'interfaccia della riga di comando è stato testato in macOS 10.9 e versioni successive.

[!INCLUDE [current-version](includes/current-version.md)]

## <a name="install-with-homebrew"></a>Eseguire l'installazione con Homebrew

Homebrew è il modo più semplice per gestire l'installazione dell'interfaccia della riga di comando. Permette di eseguire agevolmente le operazioni di installazione, aggiornamento e disinstallazione.
Se Homebrew non è disponibile nel sistema, [installare Homebrew](https://docs.brew.sh/Installation.html) prima di continuare.

Per installare l'interfaccia della riga di comando, è possibile aggiornare le informazioni nel repository Homebrew ed eseguire il comando `install`:

```bash
brew update && brew install azure-cli
```

> [!IMPORTANT]
>
> L'interfaccia della riga di comando di Azure prevede una dipendenza dal pacchetto `python3` di Homebrew e lo installerà.
> Per l'interfaccia della riga di comando di Azure è garantita la compatibilità con l'ultima versione di `python3` pubblicata in Homebrew.

È quindi possibile eseguire l'interfaccia della riga di comando di Azure con il comando `az`. Per accedere, usare il comando [az login](/cli/azure/reference-index#az-login).

[!INCLUDE [interactive-login](includes/interactive-login.md)]

Per altre informazioni sui diversi metodi di autenticazione, vedere [Accedere con l'interfaccia della riga di comando di Azure](authenticate-azure-cli.md).

## <a name="troubleshooting"></a>Risoluzione dei problemi

Se si verifica un problema durante l'installazione dell'interfaccia della riga di comando con Homebrew, alcuni errori comuni sono i seguenti. Se si verifica un problema non trattato in questo articolo, [segnalarlo in GitHub](https://github.com/Azure/azure-cli/issues).

### <a name="completion-is-not-working"></a>Il completamento non funziona

La formula Homebrew dell'interfaccia della riga di comando di Azure installa un file di completamento denominato `az` nella directory dei completamenti gestiti da Homebrew (il percorso predefinito è `/usr/local/etc/bash_completion.d/`). Per abilitare il completamento, seguire le istruzioni di Homebrew [qui](https://docs.brew.sh/Shell-Completion).

### <a name="unable-to-find-python-or-installed-packages"></a>Impossibile trovare Python o i pacchetti installati

Potrebbe esserci una mancata corrispondenza tra le versioni oppure un altro problema durante l'installazione di Homebrew. L'interfaccia della riga di comando non usa un ambiente virtuale Python, quindi si basa sull'individuazione della versione di Python installata. Una possibile soluzione è installare e ricollegare la dipendenza `python3` da Homebrew.

```bash
brew update && brew install python3 && brew upgrade python3
brew link --overwrite python3
```

### <a name="cli-version-1x-is-installed"></a>È stata installata l'interfaccia della riga di comando versione 1.x

Se è stata installata una versione obsoleta, è possibile che la cache di Homebrew non sia aggiornata. Seguire le istruzioni per l'[aggiornamento](#update).

### <a name="proxy-blocks-connection"></a>Il proxy blocca la connessione

Potrebbe risultare impossibile ottenere risorse da Homebrew, a meno che non sia stato configurato correttamente per l'uso del proxy. Seguire le [istruzioni per la configurazione del proxy di Homebrew](https://docs.brew.sh/Manpage#using-homebrew-behind-a-proxy).

> [!IMPORTANT]
> Se si è protetti da un proxy, è necessario impostare `HTTP_PROXY` e `HTTPS_PROXY` per connettersi ai servizi di Azure con l'interfaccia della riga di comando.
> Se non si usa l'autenticazione di base, è consigliabile esportare queste variabili nel file `.bashrc`.
> Seguire sempre i criteri di sicurezza aziendale e i requisiti definiti dall'amministratore di sistema.

Per ottenere le risorse bottle da Homebrew, il proxy deve consentire le connessioni HTTPS agli indirizzi seguenti:

* `https://formulae.brew.sh`
* `https://homebrew.bintray.com`

## <a name="update"></a>Aggiornamento

L'interfaccia della riga di comando viene aggiornata regolarmente con correzioni di bug, miglioramenti, nuove funzioni e funzionalità in anteprima. Ogni due settimane circa è disponibile una nuova versione. Aggiornare le informazioni del repository locale e quindi aggiornare il pacchetto `azure-cli`.

```bash
brew update && brew upgrade azure-cli
```

## <a name="uninstall"></a>Disinstallare

[!INCLUDE [uninstall-boilerplate.md](includes/uninstall-boilerplate.md)]

Usare Homebrew per disinstallare il pacchetto `azure-cli`.

```bash
brew uninstall azure-cli
```

## <a name="other-installation-methods"></a>Altri metodi di installazione

Se non è possibile usare Homebrew per installare l'interfaccia della riga di comando di Azure nell'ambiente in uso, è possibile usare le istruzioni manuali per Linux. Si noti che questo processo non è ufficialmente supportato per la compatibilità con macOS. È sempre consigliabile usare uno strumento di gestione pacchetti come Homebrew. Usare il metodo di installazione manuale solo se non sono disponibili altre opzioni.

Per le istruzioni di installazione manuale, vedere [Installare manualmente l'interfaccia della riga di comando di Azure in Linux](install-azure-cli-linux.md).

## <a name="next-steps"></a>Passaggi successivi

Ora che l'interfaccia della riga di comando di Azure è stata installata, passare a una breve presentazione delle relative funzionalità e dei comandi comuni.

> [!div class="nextstepaction"]
> [Introduzione all'interfaccia della riga di comando di Azure](get-started-with-azure-cli.md)
