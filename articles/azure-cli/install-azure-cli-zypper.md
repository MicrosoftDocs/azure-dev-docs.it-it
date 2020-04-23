---
title: Installare l'interfaccia della riga di comando di Azure in Linux con zypper
description: Come installare l'interfaccia della riga di comando di Azure con zypper
author: dbradish-microsoft
ms.author: dbradish
manager: barbkess
ms.date: 09/09/2018
ms.topic: conceptual
ms.service: azure-cli
ms.devlang: azurecli
ms.openlocfilehash: d07fe2e807bd6e1fac6d0e9f883bcc8092be46bb
ms.sourcegitcommit: 36e02e96b955ed0531f98b9c0f623f4acb508661
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/22/2020
ms.locfileid: "82031003"
---
# <a name="install-azure-cli-with-zypper"></a>Installare l'interfaccia della riga di comando di Azure con zypper

Per le distribuzioni Linux con `zypper`, ad esempio openSUSE o SLES, è disponibile un pacchetto per l'interfaccia della riga di comando di Azure. Questo pacchetto è stato testato con openSUSE Leap 15.1 e SLES 15.

[!INCLUDE [current-version](includes/current-version.md)]

[!INCLUDE [rpm-warning](includes/rpm-warning.md)]

## <a name="install"></a>Installazione

1. Installare `curl`:

   ```bash
   sudo zypper install -y curl
   ```

2. Importare la chiave del repository Microsoft:

   ```bash
   sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
   ```

3. Creare le informazioni del repository `azure-cli` locale:

   ```bash
   sudo zypper addrepo --name 'Azure CLI' --check https://packages.microsoft.com/yumrepos/azure-cli azure-cli
   ```

4. Aggiornare l'indice del pacchetto `zypper` ed eseguire l'installazione:

   ```bash
   sudo zypper install --from azure-cli azure-cli
   ```
   Digitare 2 per procedere con l'installazione ignorando alcune dipendenze.

È quindi possibile eseguire l'interfaccia della riga di comando di Azure con il comando `az`. Per accedere, usare il comando [az login](/cli/azure/reference-index#az-login).

[!INCLUDE [interactive-login](includes/interactive-login.md)]

Per altre informazioni sui diversi metodi di autenticazione, vedere [Accedere con l'interfaccia della riga di comando di Azure](authenticate-azure-cli.md).

## <a name="troubleshooting"></a>Risoluzione dei problemi

Di seguito sono riportati alcuni problemi comuni che possono verificarsi durante l'installazione con `zypper`. Se si verifica un problema non trattato in questo articolo, [segnalarlo in GitHub](https://github.com/Azure/azure-cli/issues).

### <a name="install-on-sles-12-or-other-systems-without-python-36"></a>Eseguire l'installazione in SLES 12 o altri sistemi senza Python 3.6

In SLES 12 il pacchetto python3 predefinito è 3.4 e non è supportato dall'interfaccia della riga di comando di Azure. È possibile creare prima una versione superiore di python3 dall'origine. Quindi è possibile scaricare il pacchetto dell'interfaccia della riga di comando di Azure e installarlo senza dipendenza.
```bash
$ sudo zypper install -y gcc gcc-c++ make ncurses patch wget tar zlib-devel zlib openssl-devel
# Download Python source code
$ PYTHON_VERSION="3.6.9"
$ PYTHON_SRC_DIR=$(mktemp -d)
$ wget -qO- https://www.python.org/ftp/python/$PYTHON_VERSION/Python-$PYTHON_VERSION.tgz | tar -xz -C "$PYTHON_SRC_DIR"
# Build Python
$ $PYTHON_SRC_DIR/*/configure
$ make
$ sudo make install
#Download azure-cli package 
$ AZ_VERSION=$(zypper --no-refresh info azure-cli |grep Version | awk -F': ' '{print $2}' | awk '{$1=$1;print}')
$ wget https://packages.microsoft.com/yumrepos/azure-cli/azure-cli-$AZ_VERSION.x86_64.rpm
#Install without dependency
$ sudo rpm -ivh --nodeps azure-cli-$AZ_VERSION.x86_64.rpm
```

### <a name="proxy-blocks-connection"></a>Il proxy blocca la connessione

[!INCLUDE[configure-proxy](includes/configure-proxy.md)]

Per usare sempre questo proxy, è opportuno configurare in modo esplicito `zypper` tramite `yast2`. A questo scopo, eseguire il comando `yast2 proxy` come utente con privilegi avanzati e inserire le informazioni richieste nel modulo. Se nel sistema è disponibile un gestore finestre, è anche possibile usare il riquadro `Network Services > Proxy` in `YaST Control Center`.

Per la configurazione avanzata o per altre informazioni, vedere la [documentazione relativa alla configurazione dei proxy in OpenSUSE](https://www.suse.com/documentation/slms1/book_slms/data/sec_wy_config_updates_proxy.html).

Per ottenere la chiave di firma Microsoft e il pacchetto dal repository, il proxy deve consentire le connessioni HTTPS agli indirizzi seguenti:

* `https://packages.microsoft.com`
* `https://download.opensuse.org`

[!INCLUDE[troubleshoot-wsl.md](includes/troubleshoot-wsl.md)]

## <a name="update"></a>Aggiornamento

È possibile aggiornare il pacchetto con il comando `zypper update`.

```bash
sudo zypper refresh
sudo zypper update azure-cli
```

## <a name="uninstall"></a>Disinstallare

[!INCLUDE [uninstall-boilerplate.md](includes/uninstall-boilerplate.md)]

1. Rimuovere il pacchetto dal sistema.

    ```bash
    sudo zypper remove -y azure-cli
    ```

2. Se non si prevede di reinstallare l'interfaccia della riga di comando, rimuovere le informazioni del repository.

   ```bash
   sudo zypper removerepo azure-cli
   ```

3. Se non si usano altri pacchetti Microsoft, rimuovere la chiave di firma Microsoft.

   ```bash
   MSFT_KEY=`rpm -qa gpg-pubkey /* --qf "%{version}-%{release} %{summary}\n" | grep Microsoft | awk '{print $1}'`
   sudo rpm -e --allmatches gpg-pubkey-$MSFT_KEY
   ```

## <a name="next-steps"></a>Passaggi successivi

Ora che l'interfaccia della riga di comando di Azure è stata installata, passare a una breve presentazione delle relative funzionalità e dei comandi comuni.

> [!div class="nextstepaction"]
> [Introduzione all'interfaccia della riga di comando di Azure](get-started-with-azure-cli.md)
