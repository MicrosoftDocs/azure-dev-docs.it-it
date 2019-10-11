---
title: Eseguire lo streaming dei log dal Servizio app di Azure in Visual Studio Code
description: Parte 4 dell'esercitazione, visualizzare i log.
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/20/2019
ms.author: kraigb
ms.openlocfilehash: 8484efc27120d374738a933244a3fce71c7c6acb
ms.sourcegitcommit: c04984b6367e922dbc5973af44f8cd0ca81ce157
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/30/2019
ms.locfileid: "71686207"
---
# <a name="stream-logs-from-azure-app-service"></a>Streaming dei log dal servizio app di Azure

[Passaggio precedente: Distribuire il sito Web](tutorial-vscode-azure-app-service-node-03.md)

Questo passaggio illustra come visualizzare qualsiasi output generato dal sito Web in esecuzione tramite chiamate a `console.log`. Questo output viene visualizzato nella finestra **Output** in Visual Studio Code.

1. Nell'area **Servizio app di Azure** fare clic con il pulsante destro del mouse sul nodo dell'app e scegliere **Start Streaming Logs** (Avvia lo streaming dei log).

    ![Comando Visualizza log in streaming](media/deploy-azure/view-logs.png)

1. Quando viene richiesto, scegliere di abilitare la registrazione e riavviare l'applicazione.

    ![Richiesta di abilitare la registrazione e riavviare](media/deploy-azure/enable-restart.png)

1. Una volta riavviata l'app, verrà aperta la finestra **Output** di VS Code con una connessione al flusso di log che mostra l'output.

    ```bash
    Connecting to log-streaming service...
    2019-09-20 17:33:51.428 INFO  - Container msdocs-vscode-node_2 for site msdocs-vscode-node initialized successfully.
    2019-09-20 17:33:56.500 INFO  - Container logs
    ```

1. Aggiornare la pagina Web alcune volte nel browser per visualizzare altro output dei log.

> [!div class="nextstepaction"]
> [I log vengono visualizzati](tutorial-vscode-azure-app-service-node-05.md) [Si è verificato un problema](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azureappservice&step=tailing-logs)
