---
title: Apportare modifiche al codice dell'app e ridistribuirla in Azure
description: Parte 6 dell'esercitazione, apportare modifiche e ripetere la distribuzione
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/24/2019
ms.author: kraigb
ms.openlocfilehash: f656e7414d6b7b6cecc28d69f108bfe92eb82894
ms.sourcegitcommit: c04984b6367e922dbc5973af44f8cd0ca81ce157
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/30/2019
ms.locfileid: "71686130"
---
# <a name="make-changes-and-redeploy"></a>Apportare modifiche e ripetere la distribuzione

[Passaggio precedente: Eseguire lo streaming dei log](tutorial-vscode-azure-cli-node-05.md)

In questo passaggio si apportano modifiche al codice dell'app, se ne esegue il commit nel repository Git locale e quindi si ridistribuisce il sito eseguendo il push in Azure.

1. Nella cartella `myExpressApp` aprire il file *views/index.pug* e sostituire il messaggio della riga 5 con `p Welcome to Azure!`.

    ![Modifica del file index.pug](media/azure-cli/editpugfile.png)

1. Salvare il file.

1. In un terminale o al prompt dei comandi eseguire il commit delle modifiche in Git con il comando seguente:

    ```bash
    git commit -a -m "Edited message"
    ```

1. Eseguire il push delle modifiche nell'host Git remoto denominato Azure creato in precedenza:

    ```bash
    git push azure master
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
    remote: Copying file: 'views\index.pug'
    remote: Deleting app_offline.htm
    remote: Using start-up script bin/www from package.json.
    remote: Generated web.config.
    remote: The package.json file does not specify node.js engine version constraints.
    remote: The node.js application will run with the default node.js version 6.9.5.
    remote: Selected npm version 3.10.10
    remote: ..
    remote: Finished successfully.
    remote: Running post deployment command(s)...
    remote: Deployment successful.
    To https://msdocs-node-cli.scm.azurewebsites.net/msdocs-node-cli.git
       a98bad8..9fd89a2  master -> master
    ```

1. Aggiornare l'app nel browser per vedere tali modifiche:

    ![Modifiche pubblicate visibili nel browser](media/azure-cli/remote-app-changes.png)

> [!div class="nextstepaction"]
> [Le modifiche vengono visualizzate](tutorial-vscode-azure-cli-node-07.md) [Si è verificato un problema](https://www.research.net/r/PWZWZ52?tutorial=node-deployment&step=publishing-changes)
