---
title: Eseguire l'interfaccia della riga di comando di Azure in un contenitore Docker
description: Come eseguire un contenitore Docker che ospita l'interfaccia della riga di comando di Azure
author: dbradish-microsoft
ms.author: dbradish
manager: barbkess
ms.date: 01/29/2018
ms.topic: conceptual
ms.service: azure-cli
ms.devlang: azurecli
ms.openlocfilehash: 96a2bcf311cb504b08d44da3ea6033dbad9034b8
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/05/2020
ms.locfileid: "82031293"
---
# <a name="run-azure-cli-in-a-docker-container"></a>Eseguire l'interfaccia della riga di comando di Azure in un contenitore Docker

È possibile usare Docker per eseguire un contenitore Linux autonomo con l'interfaccia della riga di comando di Azure preinstallata. Docker consente di iniziare a usare rapidamente un ambiente isolato nel quale eseguire l'interfaccia della riga di comando. L'immagine può anche essere usata come base per le proprie distribuzioni.

## <a name="run-in-a-docker-container"></a>Eseguire in un contenitore Docker

> [!NOTE]
> L'interfaccia della riga di comando di Azure è stata trasferita in [Registro contenitori di Microsoft](https://azure.microsoft.com/services/container-registry). Gli attuali tag nell'hub Docker sono ancora supportati, ma le nuove versioni saranno disponibili solo come mcr.microsoft.com/azure-cli.

Installare l'interfaccia della riga di comando con `docker run`.

   ```bash
   docker run -it mcr.microsoft.com/azure-cli
   ```

> [!NOTE]
> Per acquisire le chiavi SSH dall'ambiente dell'utente, usare `-v ${HOME}/.ssh:/root/.ssh` per montare le chiavi SSH nell'ambiente.
>
> ```bash
> docker run -it -v ${HOME}/.ssh:/root/.ssh mcr.microsoft.com/azure-cli
> ```

L'interfaccia della riga di comando viene installata nell'immagine come il comando `az` in `/usr/local/bin`. Per accedere, eseguire il comando [az login](/cli/azure/reference-index#az-login).

[!INCLUDE [interactive-login](includes/interactive-login.md)]

Per altre informazioni sui diversi metodi di autenticazione, vedere [Accedere con l'interfaccia della riga di comando di Azure](authenticate-azure-cli.md).

## <a name="update-docker-image"></a>Aggiornare l'immagine Docker

L'aggiornamento con Docker richiede sia il pull della nuova immagine che la nuova creazione di tutti i contenitori esistenti. Per questo motivo è consigliabile evitare di usare un contenitore che ospita l'interfaccia della riga di comando come archivio dati.

Aggiornare l'immagine locale con `docker pull`.

```bash
docker pull mcr.microsoft.com/azure-cli
```

## <a name="uninstall-docker-image"></a>Disinstallare l'immagine Docker

[!INCLUDE [uninstall-boilerplate.md](includes/uninstall-boilerplate.md)]

Dopo aver arrestato i contenitori che eseguono l'immagine dell'interfaccia della riga di comando, rimuovere l'immagine.

```bash
docker rmi mcr.microsoft.com/azure-cli
```

## <a name="next-steps"></a>Passaggi successivi

Ora che si è pronti per usare l'interfaccia della riga di comando di Azure, passare a una breve presentazione delle relative funzionalità e dei comandi comuni.

> [!div class="nextstepaction"]
> [Introduzione all'interfaccia della riga di comando di Azure](get-started-with-azure-cli.md)
