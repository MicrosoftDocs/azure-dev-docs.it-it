---
title: Eseguire lo streaming dei log dal Servizio app di Azure in Visual Studio Code
description: Parte 4 dell'esercitazione, visualizzare i log.
ms.topic: conceptual
ms.date: 03/04/2020
ms.openlocfilehash: e6c1f4f87a28b33dac51d6ffc59c0cd9dfdc429d
ms.sourcegitcommit: 56e5f51daf6f671f7b6e84d4c6512473b35d31d2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/07/2020
ms.locfileid: "78893540"
---
# <a name="stream-logs-from-azure-app-service"></a>Streaming dei log dal servizio app di Azure

[Passaggio precedente: Distribuire il sito Web](tutorial-vscode-azure-app-service-node-03.md)

Questo passaggio illustra come visualizzare qualsiasi output generato dall'app in esecuzione tramite chiamate a `console.log`. Questo output viene visualizzato nella finestra **Output** in Visual Studio Code.

1. Nell'area **Servizio app di Azure** fare clic con il pulsante destro del mouse sul nodo dell'app e scegliere **Start Streaming Logs** (Avvia lo streaming dei log).

    ![Comando Visualizza log in streaming](media/deploy-azure/start-streaming-logs.png)

1. Quando viene richiesto, scegliere di abilitare la registrazione e riavviare l'applicazione.

    ![Richiesta di abilitare la registrazione e riavviare](media/deploy-azure/enable-restart.png)

1. Una volta riavviata l'app, verrà aperta la finestra **Output** di VS Code con una connessione al flusso di log che mostra l'output.

    <pre>
    Connecting to log stream...
    2020-03-04T19:29:44  Welcome, you are now connected to log-streaming service. The default timeout is 2 hours.
    Change the timeout with the App Setting SCM_LOGSTREAM_TIMEOUT (in seconds).
    </pre>

1. Aggiornare la pagina Web alcune volte nel browser per visualizzare altro output dei log.

> [!div class="nextstepaction"]
> [I log vengono visualizzati](tutorial-vscode-azure-app-service-node-05.md) [Si è verificato un problema](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azureappservice&step=tailing-logs)
