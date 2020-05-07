---
title: Installare l'interfaccia della riga di comando di Azure in Linux con APT
description: Come installare l'interfaccia della riga di comando di Azure con la gestione pacchetti APT
author: dbradish-microsoft
ms.author: dbradish
manager: barbkess
ms.date: 10/14/2019
ms.topic: conceptual
ms.service: azure-cli
ms.devlang: azurecli
ms.openlocfilehash: 302b98717ee422de9bd60a57b18d900bcf5fcaf9
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/05/2020
ms.locfileid: "82030973"
---
# <a name="install-azure-cli-with-apt"></a>Installare l'interfaccia della riga di comando di Azure con APT

Se si esegue una distribuzione che include `apt`, ad esempio Ubuntu o Debian, è disponibile un pacchetto x86_64 per l'interfaccia della riga di comando di Azure. Questo pacchetto è supportato per i sistemi operativi seguenti con cui è stato testato:

* Ubuntu trusty, xenial, artful, bionic e disco
* Debian wheezy, jessie, stretch e buster

[!INCLUDE [current-version](includes/current-version.md)]

> [!NOTE]
>
> Il pacchetto per l'interfaccia della riga di comando di Azure installa uno specifico interprete Python e non usa l'istanza di Python del sistema.

## <a name="install"></a>Installazione

Sono disponibili due modi per installare l'interfaccia della riga di comando di Azure con le distribuzioni che supportano `apt`: con uno script all-in-one che esegue automaticamente i comandi di installazione e con istruzioni che è possibile eseguire manualmente come una procedura dettagliata.

### <a name="install-with-one-command"></a>Eseguire l'installazione con un unico comando

È disponibile uno script che esegue tutti i comandi di installazione in un unico passaggio. Eseguirlo con `curl` e inviarlo tramite pipe direttamente a `bash` o scaricare lo script in un file ed esaminarlo prima dell'esecuzione.

> [!IMPORTANT]
> Questo script è stato verificato solo per Ubuntu 16.04+ e Debian 8+. Potrebbe non funzionare in altre distribuzioni.
> Se si usa una distribuzione derivata, ad esempio Linux Mint, seguire le istruzioni di installazione manuale ed eseguire le procedure di risoluzione dei problemi necessarie.

```bash
curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash
```

### <a name="manual-install-instructions"></a>Istruzioni di installazione manuale

Se non si vuole eseguire uno script come utente con privilegi avanzati, oppure se lo script all-in-one non riesce, seguire questa procedura per installare l'interfaccia della riga di comando di Azure.

1. Ottenere i pacchetti necessari per il processo di installazione:

    ```bash
    sudo apt-get update
    sudo apt-get install ca-certificates curl apt-transport-https lsb-release gnupg
    ```

2. Scaricare e installare la chiave di firma Microsoft:

    ```bash
    curl -sL https://packages.microsoft.com/keys/microsoft.asc |
        gpg --dearmor |
        sudo tee /etc/apt/trusted.gpg.d/microsoft.asc.gpg > /dev/null
    ```

3. <div id="set-release"/>Aggiungere il repository software dell'interfaccia della riga di comando di Azure:

    ```bash
    AZ_REPO=$(lsb_release -cs)
    echo "deb [arch=amd64] https://packages.microsoft.com/repos/azure-cli/ $AZ_REPO main" |
        sudo tee /etc/apt/sources.list.d/azure-cli.list
    ```

4. Aggiornare le informazioni del repository e installare il pacchetto `azure-cli`:

    ```bash
    sudo apt-get update
    sudo apt-get install azure-cli
    ```

Eseguire l'interfaccia della riga di comando di Azure con il comando `az`. Per accedere, usare il comando [az login](/cli/azure/reference-index#az-login).

[!INCLUDE [interactive-login](includes/interactive-login.md)]

Per altre informazioni sui diversi metodi di autenticazione, vedere [Accedere con l'interfaccia della riga di comando di Azure](authenticate-azure-cli.md).

## <a name="troubleshooting"></a>Risoluzione dei problemi

Di seguito sono riportati alcuni problemi comuni che possono verificarsi durante l'installazione con `apt`. Se si verifica un problema non trattato in questo articolo, [segnalarlo in GitHub](https://github.com/Azure/azure-cli/issues).

### <a name="lsb_release-does-not-return-the-correct-base-distribution-version"></a>lsb_release non restituisce la versione della distribuzione base corretta

Alcune distribuzioni derivate da Ubuntu o Debian, come Linux Mint, potrebbero non restituire il nome di versione corretto tramite `lsb_release`. Questo valore viene usato nel processo di installazione per determinare il pacchetto da installare. Se si conosce il nome in codice della versione di Ubuntu o Debian da cui deriva la distribuzione in uso, è possibile impostare manualmente il valore di `AZ_REPO` durante l'[aggiunta del repository](#set-release). In caso contrario, cercare le informazioni relative alla distribuzione in uso in merito a come determinare il nome in codice della distribuzione base e impostare `AZ_REPO` sul valore corretto.

### <a name="no-package-for-your-distribution"></a>Nessun pacchetto per la distribuzione in uso

Talvolta dopo il rilascio di una distribuzione può trascorrere tempo prima che sia disponibile un pacchetto dell'interfaccia della riga di comando di Azure per tale distribuzione. L'interfaccia della riga di comando di Azure è progettata per essere resiliente rispetto alle versioni future delle dipendenze e usarne il minor numero possibile. Se non è disponibile alcun pacchetto per la distribuzione base in uso, provare un pacchetto per una distribuzione precedente.

A tale scopo, impostare manualmente il valore di `AZ_REPO` durante l'[aggiunta del repository](#set-release). Usare il repository `bionic` per le distribuzioni Ubuntu e `stretch` per le distribuzioni Debian. Le distribuzioni rilasciate prima di Ubuntu Trusty e Debian Wheezy non sono supportate.

### <a name="elementary-os-eos-fails-to-install-the-azure-cli"></a>Installazione dell'interfaccia della riga di comando di Azure non riuscita in elementary OS (eOS)

eOS non riesce a installare l'interfaccia della riga di comando di Azure perché `lsb_release` restituisce `HERA`, ovvero il nome della versione di eOS.  La soluzione consiste nel correggere il file `/etc/apt/sources.list.d/azure-cli.list` e modificare `hera main` in `bionic main`.

Contenuto del file originale:

```
deb [arch=amd64] https://packages.microsoft.com/repos/azure-cli/ hera main
```

Contenuto del file modificato

```
deb [arch=amd64] https://packages.microsoft.com/repos/azure-cli/ bionic main
```

### <a name="proxy-blocks-connection"></a>Il proxy blocca la connessione

[!INCLUDE[configure-proxy](includes/configure-proxy.md)]

Per usare sempre questo proxy, è opportuno configurare in modo esplicito `apt`. Assicurarsi che nella sezione `/etc/apt/apt.conf.d/` di un file di configurazione di `apt` siano presenti le righe seguenti. È consigliabile usare il file di configurazione globale esistente, un file di configurazione proxy esistente, `40proxies` o `99local`. In ogni caso, attenersi ai requisiti previsti dall'amministrazione del sistema.

```apt.conf
Acquire {
    http::proxy "http://[username]:[password]@[proxy]:[port]";
    https::proxy "https://[username]:[password]@[proxy]:[port]";
}
```

Se il proxy non usa l'autenticazione di base, __rimuovere__ la parte `[username]:[password]@` dell'URI del proxy. Per maggiori informazioni sulla configurazione del proxy, vedere la documentazione ufficiale di Ubuntu:

* [Pagina del manuale su apt.conf](http://manpages.ubuntu.com/manpages/bionic/en/man5/apt.conf.5.html)
* [Wiki di Ubuntu - Procedura per apt-get](https://help.ubuntu.com/community/AptGet/Howto#Setting_up_apt-get_to_use_a_http-proxy)

Per ottenere la chiave di firma Microsoft e il pacchetto dal repository, il proxy deve consentire le connessioni HTTPS all'indirizzo seguente:

* `https://packages.microsoft.com`

[!INCLUDE[troubleshoot-wsl.md](includes/troubleshoot-wsl.md)]

## <a name="update"></a>Aggiornamento

Usare `apt-get upgrade` per aggiornare il pacchetto dell'interfaccia della riga di comando.

   ```bash
   sudo apt-get update && sudo apt-get upgrade
   ```

> [!NOTE]
> Questo comando aggiorna tutti i pacchetti installati nel sistema che non hanno subito modifiche relative alle dipendenze.
> Per eseguire l'aggiornamento solo dell'interfaccia della riga di comando, usare `apt-get install`.
>
> ```bash
> sudo apt-get update && sudo apt-get install --only-upgrade -y azure-cli
> ```

## <a name="uninstall"></a>Disinstallare

[!INCLUDE [uninstall-boilerplate.md](includes/uninstall-boilerplate.md)]

1. Eseguire la disinstallazione con `apt-get remove`:

    ```bash
    sudo apt-get remove -y azure-cli
    ```

2. Se non si prevede di reinstallare l'interfaccia della riga di comando di Azure, rimuovere le relative informazioni del repository:

   ```bash
   sudo rm /etc/apt/sources.list.d/azure-cli.list
   ```

3. Se non si usano altri pacchetti Microsoft, rimuovere la chiave di firma:

    ```bash
    sudo rm /etc/apt/trusted.gpg.d/microsoft.asc.gpg
    ```

4. Rimuovere eventuali pacchetti non necessari:

   ```bash
   sudo apt autoremove
   ```

## <a name="next-steps"></a>Passaggi successivi

Ora che l'interfaccia della riga di comando di Azure è stata installata, passare a una breve presentazione delle relative funzionalità e dei comandi comuni.

> [!div class="nextstepaction"]
> [Introduzione all'interfaccia della riga di comando di Azure](get-started-with-azure-cli.md)
