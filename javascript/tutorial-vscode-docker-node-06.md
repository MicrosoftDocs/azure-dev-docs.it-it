---
title: Eseguire lo streaming dei log da un'app Node.js in contenitore da Visual Studio Code
description: Parte 5 dell'esercitazione, eseguire lo streaming dei log in Visual Studio Code
ms.topic: conceptual
ms.date: 09/20/2019
ms.openlocfilehash: 2ac930996bd910014565c4e329bec93015bd2a3a
ms.sourcegitcommit: e77f8f652128b798dbf972078a7b460ed21fb5f8
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/25/2019
ms.locfileid: "74466525"
---
# <a name="stream-logs-into-visual-studio-code"></a>Eseguire lo streaming dei log in Visual Studio Code

[Passaggio precedente: Apportare modifiche e ripetere la distribuzione](tutorial-vscode-docker-node-05.md)

Questo passaggio illustra come visualizzare qualsiasi output generato dal sito Web in esecuzione tramite chiamate a `console.log`. Questo output viene visualizzato nella finestra **Output** in Visual Studio Code.

1. Nell'area **Servizio app di Azure** fare clic con il pulsante destro del mouse sul nodo dell'app e scegliere **Start Streaming Logs** (Avvia lo streaming dei log).

    ![Comando Visualizza log in streaming](media/deploy-containers/stream-logs-command.png)

1. Quando viene richiesto, scegliere di abilitare la registrazione e riavviare l'applicazione.

    ![Richiesta di abilitare la registrazione e riavviare](media/deploy-azure/enable-restart.png)

1. Una volta riavviata l'app, viene visualizzato il pannello **Output** di Visual Studio Code con una connessione al flusso di log, a partire dal messaggio `Starting Live Log Stream`.

> [!div class="nextstepaction"]
> [I log vengono visualizzati](tutorial-vscode-docker-node-07.md) [Si è verificato un problema](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-docker-extension&step=tailing-logs)
