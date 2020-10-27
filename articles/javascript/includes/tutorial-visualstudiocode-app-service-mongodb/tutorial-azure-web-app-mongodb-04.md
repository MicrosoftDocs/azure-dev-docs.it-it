---
title: include file tutorial-azure-web-app-mongodb-04.md
description: include file tutorial-azure-web-app-mongodb-04.md
ms.date: 10/13/2020
ms.topic: include
ms.custom: devx-track-javascript
ms.openlocfilehash: 73e629427359a625ea3ec4796c2ec7000a21536a
ms.sourcegitcommit: 8a2a7df568c69fff2080ffab248409040efda1ac
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/19/2020
ms.locfileid: "92183870"
---
Questa sezione dell'esercitazione aggiunge un database MongoDB locale all'applicazione usando un'estensione per Visual Studio Code e contenitori Docker.

## <a name="configure-visual-studio-code-to-run-containers"></a>Configurare Visual Studio Code per l'esecuzione di contenitori

In questa sezione viene configurato l'ambiente di sviluppo per l'esecuzione di due contenitori, uno per il progetto Node.js e uno per il contenitore MongoDB. Poiché in questa sezione vengono usati contenitori di sviluppo di Visual Studio Code, la configurazione dei contenitori viene salvata nella cartella **.devcontainer** . È possibile eseguire il commit di questa cartella nel controllo del codice sorgente in modo che anche il team possa avere accesso a un database MongoDB locale.  

1. In Visual Studio Code usare il riquadro comandi (CTRL+MAIUSC+P) per selezionare **Remote Containers: Add Development Container Configuration Files** (Contenitori remoti: Aggiungi file di configurazione contenitori di sviluppo). 

1. Selezionare **Node.js e Mongo DB** nell'elenco.

    :::image type="content" source="../../media/tutorial-end-to-end-app-cosmos/vscode-configure-development-container.png" alt-text="Screenshot parziale del riquadro comandi di Visual Studio Code."::: 

1. Nel file **\.devcontainer\devcontainer.json** individuare la proprietà **forwardPorts** , rimuovere il commento dalla proprietà e aggiungere `8080` alla matrice. Se si vuole accedere al contenitore MongoDB anche con la shell, aggiungere la porta `27017` alla matrice.  

    :::image type="content" source="../../media/tutorial-end-to-end-app-cosmos/vscode-dev-container-configuration-forward-ports.png" alt-text="Screenshot parziale del riquadro comandi di Visual Studio Code."::: 

## <a name="run-web-app-locally-with-database"></a>Eseguire l'app Web in locale con il database

In questa sezione verrà eseguito l'ambiente di sviluppo con entrambi i contenitori e verrà quindi visualizzato il sito Web. 

1. Selezionare l'icona verde **Remote Containers** (Contenitori remoti) nell'angolo in basso a sinistra di Visual Studio Code. Verrà aperto il riquadro comandi. 

    :::image type="content" source="../../media/tutorial-end-to-end-app-cosmos/vscode-remote-container-icon.png" alt-text="Screenshot parziale del riquadro comandi di Visual Studio Code."::: 

1. Dal riquadro comandi selezionare **Remote Containers: Reopen in Container** (Contenitori remoti: Riapri nel contenitore). Quando si apre il progetto con i contenitori per la prima volta, vengono estratte e avviate le immagini Node.js e MongoDB. L'operazione potrebbe richiedere alcuni minuti. 

    Quando i contenitori sono in esecuzione, il terminale di Visual Studio Code visualizza il terminale del contenitore Node.js. 

    È anche possibile usare il comando `ls` per visualizzare i file. Si noti che i file usano un volume condiviso con il computer locale. Le modifiche apportate all'interno del contenitore Node.js nei file di codice vengono salvate nei file locali.

1. Avviare il progetto nel terminale con il comando seguente:

    ```console
    npm start
    ```

1. Aprire un browser con l'URL dell'app Web locale:

    ```
    http://localhost:8080/
    ```

1. Immettere i dati nei campi e inviare il modulo. I dati vengono visualizzati utilizzando il rendering React sul lato server. 

    :::image type="content" source="../../media/tutorial-end-to-end-app-cosmos/nodejs-app-connected-mongodb-form.png" alt-text="Screenshot parziale del riquadro comandi di Visual Studio Code.":::

1. Al termine dell'esplorazione dell'app, arrestare i contenitori usando il riquadro comandi per selezionare **Remote Containers: Reopen Locally...** (Contenitori remoti: Riapri in locale). Questo comando arresta i contenitori, ma li lascia nel computer locale. 

## <a name="want-to-know-more"></a>Per saperne di più 

Connettersi al contenitore MongoDB con un'estensione per Visual Studio Code, **[MongoDB for VS Code](https://marketplace.visualstudio.com/items?itemName=mongodb.mongodb-vscode)** (MongoDB per Visual Studio Code), per visualizzare i dati.

Se si preferisce usare la shell **mongo** , connettersi al contenitore MongoDB con un terminale di Visual Studio Code aprendo una nuova finestra di Visual Studio Code, quindi usando **Remote Containers: Attach to Running Container...** (Contenitori remoti: Collega a contenitore in esecuzione) e selezionare il contenitore che termina con `-db`. Una volta collegata la finestra al contenitore, aprire un terminale di Visual Studio Code. È possibile accedere immediatamente alla shell Mongo usando il comando seguente:

```console
mongo
```

Per pulire i contenitori, usare l'estensione per Visual Studio Code **[Docker](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker)** .