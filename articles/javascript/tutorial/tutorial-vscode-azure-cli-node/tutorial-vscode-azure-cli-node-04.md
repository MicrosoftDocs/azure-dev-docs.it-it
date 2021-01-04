---
title: Distribuire il codice dell'app nel Servizio app di Azure tramite l'interfaccia della riga di comando di Azure
description: Parte 4 dell'esercitazione, distribuire il sito Web con l'interfaccia della riga di comando di Azure
ms.topic: tutorial
ms.date: 12/14/2020
ms.custom: devx-track-js, devx-track-azurecli
ms.openlocfilehash: 7dc0369615f58e8677b479b28c2223d3fa865b19
ms.sourcegitcommit: 1dfcc022a3098b1a1505e9458eada35f527ef070
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97658410"
---
# <a name="deploy-the-app-to-app-service"></a>Distribuire l'app nel Servizio app

[Passaggio precedente: Creare il Servizio app](tutorial-vscode-azure-cli-node-03.md)

In questo passaggio il codice dell'app Node.js viene distribuito nel Servizio app di Azure tramite un processo di base che consiste nell'eseguire il push del repository Git locale in Azure.

1. In un terminale o al prompt dei comandi eseguire i comandi seguenti per inizializzare il repository Git locale ed effettuare un commit iniziale. La cartella *node_modules* viene ignorata perché è specificata nel file con estensione *gitignore* creato quando è stato eseguito il generatore Express in precedenza.

    ```bash
    git init
    git add -A
    git commit -m "Initial Commit"
    ```

1. Eseguire il comando seguente per [configurare le credenziali per la distribuzione a livello utente con l'interfaccia della riga di comando di Azure](/azure/app-service/deploy-configure-credentials), sostituendo `username` e `password` con nuove credenziali specifiche solo per la distribuzione. Queste credenziali sono diverse dalle credenziali della sottoscrizione di Azure. 

    ```azurecli
    az webapp deployment user set --user-name <username> --password <password>
    ```

1. Eseguire il comando seguente per [recuperare l'endpoint Git con l'interfaccia della riga di comando di Azure](/cli/azure/webapp/deployment/source?view=azure-cli-latest&preserve-view=false) in cui eseguire il push del codice dell'app, sostituendo `<your_app_name>` con il nome usato per creare il servizio app nel passaggio precedente:

    ```azurecli
    az webapp deployment source config-local-git --name <your_app_name>
    ```

    L'output di questo comando è simile al seguente:

    <pre>
    {
      "url": "https://username@msdocs-node-cli.scm.azurewebsites.net/msdocs-node-cli.git"
    }
    </pre>

1. Eseguire il comando seguente per impostare un nuovo host remoto in Git denominato `azure`, usando l'URL del passaggio precedente ma *omettendo il nome utente*. Usando l'esempio del passaggio precedente, il comando sarà come segue:

    ```bash
    git remote add azure https://msdocs-node-cli.scm.azurewebsites.net/msdocs-node-cli.git
    ```

1. Eseguire il comando seguente per distribuire il codice dell'app dal repository Git al Servizio app. Il comando chiede di immettere le credenziali:

    ```bash
    git push azure <DEFAULT-BRANCH-NAME>
    ```

1. Mentre viene eseguito, il comando visualizza una serie di messaggi dell'host remoto. Al termine del processo, aggiornare il browser in cui è stato aperto l'URL dell'app per vedere il codice in esecuzione:

    ![Codice dell'app in esecuzione in Azure](../../media/azure-cli/remote-app.png)

> [!div class="nextstepaction"]
> [L'app è stata distribuita](tutorial-vscode-azure-cli-node-05.md) [Si è verificato un problema](https://www.research.net/r/PWZWZ52?tutorial=node-deployment&step=deploy-website)
