---
title: Installare l'interfaccia della riga di comando classica di Azure
description: Installare l'interfaccia della riga di comando classica di Azure per Mac, Linux e Windows per iniziare a usare i servizi di Azure
author: dbradish-microsoft
ms.author: dbradish
manager: barbkess
ms.date: 06/11/2018
ms.topic: conceptual
ms.service: azure-cli
ms.devlang: azurecli
ms.openlocfilehash: 0cc1d7811223bf6f473c2c4516d0919306aa74c7
ms.sourcegitcommit: 36e02e96b955ed0531f98b9c0f623f4acb508661
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/22/2020
ms.locfileid: "82030763"
---
# <a name="install-the-azure-classic-cli"></a>Installare l'interfaccia della riga di comando classica di Azure

> [!IMPORTANT]
> Questo argomento illustra come installare l'interfaccia della riga di comando classica di Azure. L'interfaccia della riga di comando classica è deprecata e deve essere usata solo con il modello di distribuzione classica.
> Per tutte le altre distribuzioni, usare l'[interfaccia della riga di comando di Azure](/cli/azure).

Installare rapidamente l'interfaccia della riga di comando di Azure per usare un set di comandi open source basati sulla shell per creare e gestire le risorse in Microsoft Azure. Sono disponibili diverse opzioni per installare questi strumenti multipiattaforma nel computer in uso:

* **Pacchetto npm**: esegue npm (Gestione pacchetti per JavaScript) per installare il pacchetto dell'interfaccia della riga di comando classica di Azure nella distribuzione o nel sistema operativo Linux in uso. Richiede Node.js e npm.
* **Programma di installazione**: è possibile scaricare un programma di installazione per agevolare l'installazione in Mac o Windows.
* **Contenitore Docker**: per iniziare a usare l'interfaccia della riga di comando classica in un contenitore Docker pronto per l'esecuzione. Richiede un host Docker.

Per altre opzioni e informazioni, vedere il repository dei progetti in [GitHub](https://github.com/azure/azure-xplat-cli).

Dopo l'installazione dell'interfaccia della riga di comando classica di Azure, effettuare la connessione con `azure login` ed eseguire i comandi `azure` dalla propria interfaccia della riga di comando (Bash, terminale, prompt dei comandi e così via) per usare le risorse di Azure.

## <a name="option-1-install-an-npm-package"></a>Opzione 1: Installare un pacchetto npm

Per installare l'interfaccia della riga di comando classica da un pacchetto npm, verificare che siano state caricate e installate le [versioni più recenti di Node.js e npm](https://nodejs.org/en/download/package-manager/). Eseguire quindi `npm install` per installare il pacchetto dell'interfaccia della riga di comando di Azure:

```bash
npm install -g azure-cli
```

Nelle distribuzioni Linux potrebbe essere necessario usare `sudo` per la corretta esecuzione del comando `npm`, come segue:

```bash
sudo npm install -g azure-cli
```

> [!NOTE]
> Se è necessario installare o aggiornare Node.js e npm nel sistema operativo, è consigliabile installare Node.js LTS versione 4.x o successiva. Se si usa una versione precedente, potrebbero verificarsi errori di installazione.

Se si preferisce, è anche possibile scaricare un file TAR dalla pagina delle [versioni di GitHub](https://github.com/Azure/azure-xplat-cli/releases). Installare quindi il pacchetto npm scaricato come indicato di seguito (nelle distribuzioni Linux potrebbe essere necessario usare `sudo`):

```bash
npm install -g <path to downloaded tar file>
```

## <a name="option-2-use-an-installer"></a>Opzione 2: Usare un programma di installazione

Se si usa un computer Mac o Windows, sono disponibili programmi di installazione DMG e MSI nella pagina delle [versioni di GitHub](https://github.com/Azure/azure-xplat-cli/releases).

> [!TIP]
> In Windows è anche possibile scaricare l'[Installazione guidata piattaforma Web](https://go.microsoft.com/?linkid=9828653) per installare l'interfaccia della riga di comando classica. Questo programma di installazione consente di installare altri strumenti Azure SDK e da riga di comando.

## <a name="option-3-use-a-docker-container"></a>Opzione 3: Usare un contenitore Docker

Se il computer è stato configurato come host [Docker](https://docs.docker.com/engine/understanding-docker/) è possibile eseguire l'interfaccia della riga di comando classica di Azure in un contenitore Docker. Eseguire questo comando (nelle distribuzioni Linux potrebbe essere necessario usare `sudo`):

```bash
docker run -it microsoft/azure-cli:0.10.17
```

## <a name="run-azure-classic-cli-commands"></a>Eseguire i comandi dell'interfaccia della riga di comando classica di Azure

Dopo aver installato l'interfaccia della riga di comando classica di Azure, eseguire il comando `azure` dalla propria interfaccia di riga di comando (Bash, terminale, prompt dei comandi e così via). Ad esempio, per eseguire il comando della Guida, digitare quanto segue:

```azurecli-interactive
azure help
```

> [!NOTE]
> In alcune distribuzioni di Linux potrebbe essere visualizzato un errore simile a `/usr/bin/env: ‘node’: No such file or directory`. Questo errore è causato dalle installazioni recenti di Node.js che vengono installate in /usr/bin/nodejs. Per correggerlo, creare un collegamento simbolico per /usr/bin/node eseguendo questo comando:

```bash
sudo ln -s /usr/bin/nodejs /usr/bin/node
```

Per visualizzare la versione dell'interfaccia della riga di comando classica di Azure installata, digitare quanto segue:

```azurecli-interactive
azure --version
```

> [!NOTE]
> Quando si usa per la prima volta l'interfaccia della riga di comando classica di Azure, viene visualizzato un messaggio che chiede se consentire a Microsoft di raccogliere informazioni sull'utilizzo. La partecipazione è facoltativa. Se si sceglie di partecipare, è possibile interrompere in qualsiasi momento eseguendo `azure telemetry --disable`. Per abilitare la partecipazione in qualsiasi momento, eseguire `azure telemetry --enable`.

## <a name="update-the-classic-cli"></a>Aggiornare l'interfaccia della riga di comando classica

Microsoft può rilasciare versioni aggiornate dell'interfaccia della riga di comando classica di Azure. Reinstallare l'interfaccia della riga di comando classica usando il programma di installazione per il proprio sistema operativo oppure eseguire il contenitore Docker più recente. Se sono installate le versioni più recenti di npm e Node.js, in alternativa è possibile eseguire l'aggiornamento digitando quanto segue. Nelle distribuzioni Linux potrebbe essere necessario usare `sudo`.

```bash
npm update -g azure-cli
```

## <a name="enable-tab-completion"></a>Attivare il completamento con tasto TAB

Il completamento con tasto TAB dei comandi dell'interfaccia della riga di comando classica è supportato per Mac e Linux.

Per abilitarlo in zsh, eseguire:

```bash
echo '. <(azure --completion)' >> .zshrc
```

Per abilitarlo in bash, eseguire:

```bash
azure --completion >> ~/azure.completion.sh
echo 'source ~/azure.completion.sh' >> ~/.bash_profile
```

## <a name="next-steps"></a>Passaggi successivi

* Per altre informazioni sull'interfaccia della riga di comando classica di Azure, il download del codice sorgente e per segnalare problemi o collaborare al progetto, vedere il [repository GitHub per l'interfaccia della riga di comando classica di Azure](https://github.com/azure/azure-xplat-cli).
