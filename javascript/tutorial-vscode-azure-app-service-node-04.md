---
title: Eseguire lo streaming dei log dal Servizio app di Azure in Visual Studio Code
description: Parte 4 dell'esercitazione, visualizzare i log.
ms.topic: conceptual
ms.date: 09/20/2019
ms.openlocfilehash: cc140d7751f9b014f1a16065fd4c65b481c7d1ae
ms.sourcegitcommit: e77f8f652128b798dbf972078a7b460ed21fb5f8
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/25/2019
ms.locfileid: "74466813"
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
