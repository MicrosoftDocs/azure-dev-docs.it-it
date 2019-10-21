---
title: 'Esercitazione: Streaming dei log dal servizio app di Azure in VS Code'
description: "Passaggio 6 dell'esercitazione: streaming dei log delle app in Visual Studio Code"
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/12/2019
ms.author: kraigb
ms.custom: seo-python-october2019
ms.openlocfilehash: a60c8fd0202e935960f14a9ab5570f86a78fab6e
ms.sourcegitcommit: 6012460ad8d6ff112226b8f9ea6da397ef77712d
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/11/2019
ms.locfileid: "72278922"
---
# <a name="tutorial-stream-logs-from-azure-app-service-into-visual-studio-code"></a>Esercitazione: Eseguire lo streaming dei log dal Servizio app di Azure in Visual Studio Code

[Passaggio precedente: Distribuire l'app](tutorial-deploy-app-service-on-linux-05.md)

Usare questa procedura per eseguire lo streaming dei log in Visual Studio Code da un servizio app di Azure.

1. In Visual Studio Code aprire l'area **Azure: App Service** (Azure: App Service), fare clic con il pulsante destro del mouse sul servizio app e scegliere **Start streaming logs** (Avvia streaming dei log):

   ![Avvio dello streaming dei log dall'area App Service (Servizio app)](media/deploy-azure/start-streaming-logs-in-visual-studio-code.png)

1. Quando viene richiesto di abilitare la registrazione dei file e riavviare l'app Web, scegliere **Yes** (Sì). Durante il riavvio dell'app la finestra **Output** in VS Code visualizza lo stato dell'operazione. L'abilitazione della registrazione è un processo che viene eseguito una volta sola.

1. Dopo aver abilitato la registrazione fare clic con il pulsante destro del mouse sul servizio app e scegliere di nuovo **Start streaming logs** (Avvia streaming dei log). La finestra **Output** in VS Code visualizza "Starting Live Log Stream" (Avvio del flusso dei log in tempo reale) e l'output dei log inizia a essere visualizzato. Provare ad aggiornare l'app Web nel browser per generare altre informazioni sui log.

1. Per arrestare lo streaming dei log (senza disabilitare la registrazione), fare clic con il pulsante destro del mouse sull'app nell'area **Azure: App Service** (Azure: Servizio app) e scegliere **Stop streaming logs** (Arresta streaming dei log).

> [!div class="nextstepaction"]
> [I log vengono visualizzati](tutorial-deploy-app-service-on-linux-07.md)

[Si è verificato un problema](https://www.research.net/r/PWZWZ52?tutorial=vscode-appservice-python&step=06-stream-logs)
