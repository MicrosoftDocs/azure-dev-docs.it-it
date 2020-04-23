---
title: Installare l'interfaccia della riga di comando di Azure in Linux con yum
description: Come installare l'interfaccia della riga di comando di Azure con yum
author: dbradish-microsoft
ms.author: dbradish
manager: barbkess
ms.date: 11/26/2019
ms.topic: conceptual
ms.service: azure-cli
ms.devlang: azurecli
ms.openlocfilehash: a98a51e4dc3ac85d27e27ef9b9164a7f98431d31
ms.sourcegitcommit: 36e02e96b955ed0531f98b9c0f623f4acb508661
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/22/2020
ms.locfileid: "82031113"
---
# <a name="install-azure-cli-with-yum"></a>Installare l'interfaccia della riga di comando di Azure con yum

Per le distribuzioni di Linux con `yum`, ad esempio RHEL, Fedora o CentOS, è disponibile un pacchetto per l'interfaccia della riga di comando di Azure. Questo pacchetto è stato testato con RHEL 7.7, RHEL 8, Fedora 24 e versioni successive, CentOS 7 e CentOS 8.

[!INCLUDE [current-version](includes/current-version.md)]

[!INCLUDE [rpm-warning](includes/rpm-warning.md)]

## <a name="install"></a>Installazione

1. Importare la chiave del repository Microsoft.

   ```bash
   sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
   ```

2. Creare le informazioni del repository `azure-cli` locale.

   ```bash
   sudo sh -c 'echo -e "[azure-cli]
   name=Azure CLI
   baseurl=https://packages.microsoft.com/yumrepos/azure-cli
   enabled=1
   gpgcheck=1
   gpgkey=https://packages.microsoft.com/keys/microsoft.asc" > /etc/yum.repos.d/azure-cli.repo'
   ```

3. Eseguire l'installazione con il comando `yum install`.

   ```bash
   sudo yum install azure-cli
   ```

Eseguire l'interfaccia della riga di comando di Azure con il comando `az`. Per accedere, usare il comando [az login](/cli/azure/reference-index#az-login).

[!INCLUDE [interactive-login](includes/interactive-login.md)]

Per altre informazioni sui diversi metodi di autenticazione, vedere [Accedere con l'interfaccia della riga di comando di Azure](authenticate-azure-cli.md).

## <a name="troubleshooting"></a>Risoluzione dei problemi

Di seguito sono riportati alcuni problemi comuni che possono verificarsi durante l'installazione con `yum`. Se si verifica un problema non trattato in questo articolo, [segnalarlo in GitHub](https://github.com/Azure/azure-cli/issues).

### <a name="install-on-rhel-76-or-other-systems-without-python-3"></a>Eseguire l'installazione in RHEL 7.6 o altri sistemi senza Python 3

Se possibile, aggiornare il sistema a una versione con supporto ufficiale per il pacchetto `python3`. In caso contrario, è prima necessario installare un pacchetto `python3` ed [eseguire la compilazione dall'origine](https://github.com/linux-on-ibm-z/docs/wiki/Building-Python-3.6.x) o eseguire l'installazione tramite alcuni [repository aggiuntivi](https://developers.redhat.com/blog/2018/08/13/install-python3-rhel/). È quindi possibile scaricare il pacchetto e installarlo senza dipendenza.
```bash
$ sudo yum install yum-utils
$ sudo yumdownloader azure-cli
$ sudo rpm -ivh --nodeps azure-cli-*.rpm
```

Se è stato configurato Python 3, ma viene ancora restituito un errore `python3: command not found` quando si tenta di eseguire l'interfaccia della riga di comando, è necessario aggiungerlo al percorso.
```bash
$ scl enable rh-python36 bash
```

### <a name="proxy-blocks-connection"></a>Il proxy blocca la connessione

[!INCLUDE[configure-proxy](includes/configure-proxy.md)]

Per usare sempre questo proxy, è opportuno configurare in modo esplicito `yum`. Assicurarsi che nella sezione `[main]` di `/etc/yum.conf` siano presenti le righe seguenti:

```yum.conf
[main]
# ...
proxy=http://[proxy]:[port] # If your proxy requires https, change http->https
proxy_username=[username] # Only required for basic auth
proxy_password=[password] # Only required for basic auth
```

Per ottenere la chiave di firma Microsoft e il pacchetto dal repository, il proxy deve consentire le connessioni HTTPS all'indirizzo seguente:

* `https://packages.microsoft.com`

[!INCLUDE[troubleshoot-wsl.md](includes/troubleshoot-wsl.md)]

## <a name="update"></a>Aggiornamento

Aggiornare l'interfaccia della riga di comando di Azure con il comando `yum update`.

```bash
sudo yum update azure-cli
```

## <a name="uninstall"></a>Disinstallare

[!INCLUDE [uninstall-boilerplate.md](includes/uninstall-boilerplate.md)]

1. Rimuovere il pacchetto dal sistema.

   ```bash
   sudo yum remove azure-cli
   ```

2. Se non si prevede di reinstallare l'interfaccia della riga di comando, rimuovere le informazioni del repository.

   ```bash
   sudo rm /etc/yum.repos.d/azure-cli.repo
   ```

3. Se non si usano altri pacchetti Microsoft, rimuovere la chiave di firma.

   ```bash
   MSFT_KEY=`rpm -qa gpg-pubkey /* --qf "%{version}-%{release} %{summary}\n" | grep Microsoft | awk '{print $1}'`
   sudo rpm -e --allmatches gpg-pubkey-$MSFT_KEY
   ```

## <a name="next-steps"></a>Passaggi successivi

Ora che l'interfaccia della riga di comando di Azure è stata installata, passare a una breve presentazione delle relative funzionalità e dei comandi comuni.

> [!div class="nextstepaction"]
> [Introduzione all'interfaccia della riga di comando di Azure](get-started-with-azure-cli.md)
