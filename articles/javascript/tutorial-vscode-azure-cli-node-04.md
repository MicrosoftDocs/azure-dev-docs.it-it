---
title: Distribuire il codice dell'app nel Servizio app di Azure tramite l'interfaccia della riga di comando di Azure
description: Parte 4 dell'esercitazione, distribuire il sito Web con l'interfaccia della riga di comando di Azure
ms.topic: tutorial
ms.date: 09/24/2019
ms.custom: devx-track-js, devx-track-azurecli
ms.openlocfilehash: 6006c25b9f2cb77ed472d8e4cb7d0eb96e85da48
ms.sourcegitcommit: 1ddcb0f24d2ae3d1f813ec0f4369865a1c6ef322
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/27/2020
ms.locfileid: "92689122"
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

1. Eseguire i comandi seguenti per configurare le credenziali di distribuzione in Azure, sostituendo `username` e `pPassword` con le proprie credenziali. Quando viene completato, il comando visualizza l'output JSON.

    ```azurecli
    az webapp deployment user set --user-name <username> --password <password>
    ```

1. Eseguire il comando seguente per recuperare l'endpoint Git in cui eseguire il push del codice dell'app, sostituendo `<your_app_name>` con il nome usato per creare il Servizio app nel passaggio precedente:

    ```azurecli
    az webapp deployment source config-local-git --name <your_app_name>
    ```

    L'output di questo comando è simile al seguente:

    <pre>
    {
      "url": "https://username@msdocs-node-cli.scm.azurewebsites.net/msdocs-node-cli.git"
    }
    </pre>

1. Eseguire il comando seguente per impostare un nuovo host remoto in Git denominato `azure`, usando l'URL del passaggio precedente ma *omettendo il nome utente* . Usando l'esempio del passaggio precedente, il comando sarà come segue:

    ```bash
    git remote add azure https://msdocs-node-cli.scm.azurewebsites.net/msdocs-node-cli.git
    ```

1. Eseguire il comando seguente per distribuire il codice dell'app dal repository Git al Servizio app. Il comando chiede di immettere le credenziali:

    ```bash
    git push azure master
    ```

1. Mentre viene eseguito, il comando visualizza una serie di messaggi dell'host remoto. Al termine del processo, aggiornare il browser in cui è stato aperto l'URL dell'app per vedere il codice in esecuzione:

    ![Codice dell'app in esecuzione in Azure](media/azure-cli/remote-app.png)

> [!TIP]
> Se si verifica l'errore `Object #<eventemitter> has no method 'hrtime'`, è probabile che sia necessario impostare la versione del runtime del nodo nel sito. Il comando seguente indica al sito di usare la versione `6.9.1` del nodo. Se il sito richiede una versione diversa o successiva del nodo, specificare la versione semantica completa `major.minor.patch`.
>
> ```azurecli
> az webapp config appsettings set --name <your_app_name> --settings
> ```

> [!div class="nextstepaction"]
> [L'app è stata distribuita](tutorial-vscode-azure-cli-node-05.md) [Si è verificato un problema](https://www.research.net/r/PWZWZ52?tutorial=node-deployment&step=deploy-website)
