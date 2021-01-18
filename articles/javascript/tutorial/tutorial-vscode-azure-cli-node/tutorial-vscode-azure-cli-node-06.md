---
title: Apportare modifiche al codice dell'app e ridistribuirla in Azure
description: Parte 6 dell'esercitazione, apportare modifiche e ripetere la distribuzione con l'interfaccia della riga di comando di Azure
ms.topic: tutorial
ms.date: 09/24/2019
ms.custom: devx-track-js, devx-track-azurecli
ms.openlocfilehash: 1c80c76759d22195fc7268d4b072a7a25c3a1745
ms.sourcegitcommit: 75a1f26aaff48a89631805df4b4a0c006de6a271
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/12/2021
ms.locfileid: "98128173"
---
# <a name="part-6-make-changes-and-redeploy"></a>Parte 6: Apportare modifiche e ripetere la distribuzione

[Passaggio precedente: Eseguire lo streaming dei log](tutorial-vscode-azure-cli-node-05.md)

In questo passaggio si apportano modifiche al codice dell'app, se ne esegue il commit nel repository Git locale e quindi si ridistribuisce il sito eseguendo il push in Azure.

1. Nella cartella `myExpressApp` aprire il file *src/server.js* e modificare il messaggio, `Welcome to Azure!`.

    ![Modifica del file src/server.js](../../media/azure-cli/edit-server-file.png)

1. Salvare il file.

1. In un terminale o al prompt dei comandi eseguire il commit delle modifiche in Git con il comando seguente:

    ```bash
    git commit -a -m "Edited message"
    ```

1. Eseguire il push delle modifiche nell'host Git remoto denominato Azure creato in precedenza:

    ```bash
    git push azure <DEFAULT-BRANCH-NAME>
    ```

1. Poiché il Servizio app è già connesso al repository Git, l'output del comando mostra che le modifiche vengono automaticamente pubblicate in Azure: 

    ```output
    Enumerating objects: 7, done.
    Counting objects: 100% (7/7), done.
    Delta compression using up to 4 threads
    Compressing objects: 100% (4/4), done.
    Writing objects: 100% (4/4), 405 bytes | 202.00 KiB/s, done.
    Total 4 (delta 2), reused 0 (delta 0)
    remote: Updating branch 'master'.
    remote: Updating submodules.
    remote: Preparing deployment for commit id '9fd89a25b6'.
    remote: Generating deployment script.
    remote: Running deployment command...
    remote: Handling node.js deployment.
    remote: Creating app_offline.htm
    remote: KuduSync.NET from: 'D:\home\site\repository' to: 'D:\home\site\wwwroot'
    remote: Copying file: 'src\server.js'
    remote: Deleting app_offline.htm
    remote: Using start-up script bin/www from package.json.
    remote: Generated web.config.
    remote: The package.json file does not specify node.js engine version constraints.
    remote: The node.js application will run with the 
    remote: ..
    remote: Finished successfully.
    remote: Running post deployment command(s)...
    remote: Deployment successful.
    To https://msdocs-node-cli.scm.azurewebsites.net/msdocs-node-cli.git
       a98bad8..9fd89a2  master -> master
    ```

1. Aggiornare l'app nel browser per vedere tali modifiche:

    ![Modifiche pubblicate visibili nel browser](../../media/azure-cli/remote-app-changes.png)

> [!div class="nextstepaction"]
> [Le modifiche vengono visualizzate](tutorial-vscode-azure-cli-node-07.md) [Si è verificato un problema](https://www.research.net/r/PWZWZ52?tutorial=node-deployment&step=publishing-changes)
