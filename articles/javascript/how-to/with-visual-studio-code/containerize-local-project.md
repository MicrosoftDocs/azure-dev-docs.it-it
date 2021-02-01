---
title: Crea il contenitore dal progetto locale
description: Creare una Dockerfile del progetto di sviluppo locale con Visual Studio Code
ms.topic: how-to
ms.date: 01/28/2021
ms.custom: devx-track-js
ms.openlocfilehash: a299e4647e3257627b62242a4ab63b16fc5590a3
ms.sourcegitcommit: 3f8aa923e4626b31cc533584fe3b66940d384351
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/01/2021
ms.locfileid: "99231549"
---
# <a name="create-a-container-image-from-your-local-javascript-project"></a>Creare un'immagine del contenitore dal progetto JavaScript locale

L'esecuzione di un'app in un contenitore consente di distribuire un'esperienza coerente per gli utenti dell'app Web. Poiché Docker presenta una curva di apprendimento ripido ad alcuni, Visual Studio Code fornisce un'estensione che semplifica le attività comuni di Docker.

## <a name="prepare-your-environment"></a>Preparare l'ambiente 

[Docker](https://www.docker.com/) deve essere installato e in esecuzione. Verificare questa operazione con il comando seguente:

```bash
docker system info
```

Questo comando restituisce un errore se Docker non è installato e non è in esecuzione. 

## <a name="create-a-container"></a>Creare un contenitore

1. Aprire Visual Studio in un progetto JavaScript esistente. 
1. Nella barra attività selezionare l'icona **estensioni** , quindi cercare `docker` e selezionare l' [estensione](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker) **Docker** .
1. Installare l'estensione Docker, quindi ricaricare Visual Studio Code.

    ![Installazione dell'estensione Docker per Visual Studio Code](../../media/node-howto-e2e/visual-studio-code-docker-extension.png)

    L'estensione Docker per Visual Studio Code include un comando per la generazione di un *Dockerfile* e del file *docker-compose.yml* per un progetto esistente.

1. Selezionare l'icona Docker sulla barra delle attività, quindi visualizzare i contenitori Docker nella barra laterale.

    :::image type="content" source="../../media/howto-containerize-local-project/docker-extension-activity-bar-side-bar.png" alt-text="Cercare &quot;git branch&quot; e selezionare &quot;git: create Branch&quot;.":::

## <a name="view-available-docker-commands"></a>Visualizzare i comandi di Docker disponibili

Per vedere i comandi Docker disponibili, visualizzare il riquadro comandi premendo **F1** e quindi digitare `docker`.

![Comandi supportati dall'estensione Docker per Visual Studio Code ](../../media/node-howto-e2e/visual-studio-code-available-docker-codes.png)

## <a name="create-a-dockerfile-in-your-project"></a>Creare un Dockerfile nel progetto

1. Se si usa il controllo del codice sorgente, ad esempio **git**, assicurarsi di non avere altre modifiche. Ciò consente di visualizzare i file creati automaticamente.

1. Selezionare **docker: aggiungere i file di Docker all'area di lavoro** con le impostazioni per il progetto. 

    Queste impostazioni sono comuni per un progetto di Node.js:

    |Impostazione|Valore|
    |--|--|
    |Piattaforma applicativa|Node.js|
    |Package.js|package.json|
    |Porta da esporre|8080 o il valore autodectected|
    |Includere file Docker facoltativi (dockerignore) |sì|

    ![Dockerfile generato in Visual Studio Code](../../media/node-howto-e2e/visual-studio-code-complete-dockerfile.png)

    Il comando Docker genera un *Dockerfile* completo e i file Docker-compose che possono essere usati immediatamente.

## <a name="build-image-from-your-project"></a>Crea immagine dal progetto

1. Selezionare **F1**, immettere `dockerb` nel riquadro comandi e selezionare il comando **docker: Build Image** . 
1. Scegliere il *Dockerfile* appena generato. 
1. Se il package.jssu ha una proprietà Name, viene usato come nome dell'immagine del contenitore. 
    Se non si dispone di un package.jsin, specificare un tag con il formato `ALIAS/IMAGE-NAME` dove alias è l'alias Docker e image-name è il nome dell'immagine del progetto. Un tag di esempio è `diberry/express-web-app` . 
1. Premere **invio** per aprire la finestra del terminale integrato in cui viene visualizzato l'output dell'immagine Docker in fase di compilazione.

    ![Output di creazione dell'immagine Docker](../../media/node-howto-e2e/docker-build-image-output.png)

    Il comando automatizza il processo di esecuzione `docker build` .

1. Selezionare l'icona Docker sulla barra delle attività, quindi selezionare **Immagini** per visualizzare la nuova immagine nell'elenco immagini. 
    
    È anche possibile notare altre nuove immagini nell'elenco. Docker estrae l'immagine su cui si basa il contenitore.  

## <a name="run-local-container-project"></a>Esegui progetto contenitore locale

1. Selezionare l'icona Docker sulla barra delle attività.
1. Fare clic con il pulsante destro del mouse sul nome dell'immagine dall'elenco di **Immagini** e scegliere **Esegui**.

    :::image type="content" source="../../media/howto-containerize-local-project/docker-extension-running-container-visual-studio-code.png" alt-text="Fare clic con il pulsante destro del mouse sul nome dell'immagine dall'elenco di immagini e scegliere Esegui.":::

## <a name="push-local-container-image-to-dockerhub"></a>Push dell'immagine del contenitore locale in DockerHub

L'immagine deve essere disponibile da un registro per creare un'app Web di Azure dall'immagine. L'immagine può essere disponibile pubblicamente in un registro della community o in un registro privato a cui si accede con l'autenticazione come [Azure container Registry](/azure/container-registry/). 

Per eseguire il push dell'immagine, assicurarsi di avere già effettuato l'autenticazione con DockerHub eseguendo `docker login` dall'interfaccia della riga di comando e immettendo le credenziali dell'account.

1. In Visual Studio Code, visualizzare il riquadro comandi con F1.
1. Immettere `dockerpush` e selezionare il `Docker: Push` comando. 
1. Selezionare il tag image appena compilato, ad esempio, `diberry/express-web-app` e premere **invio**. 
1. Il comando automatizza la chiamata di `docker push` e visualizza l'output nel terminale integrato.

## <a name="push-local-container-image-to-azure-container-registry"></a>Push dell'immagine del contenitore locale in Azure Container Registry

Leggere i passaggi per eseguire l' [autenticazione e il push nel container Registry di Azure](../with-azure-cli/create-container-registry-resource.md).

## <a name="next-steps"></a>Passaggi successivi

* [Creare una risorsa di Azure Container Registry](../with-azure-cli/create-container-registry-resource.md)