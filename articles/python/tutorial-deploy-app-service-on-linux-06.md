---
title: 'Passaggio 6: Streaming dei log dal servizio app di Azure in VS Code'
description: "Passaggio 6 dell'esercitazione: streaming dei log delle app in Visual Studio Code"
ms.topic: conceptual
ms.date: 11/20/2020
ms.custom: devx-track-python, seo-python-october2019
ms.openlocfilehash: 01df1219302f4edf21845c999337354a065352ac
ms.sourcegitcommit: 29930f1593563c5e968b86117945c3452bdefac1
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/23/2020
ms.locfileid: "95485680"
---
# <a name="6-stream-logs-from-azure-app-service-into-visual-studio-code"></a>6: Eseguire lo streaming dei log dal Servizio app di Azure in Visual Studio Code

[Passaggio precedente: Distribuire l'app](tutorial-deploy-app-service-on-linux-05.md)

Usare questa procedura per eseguire lo streaming dei log in Visual Studio Code da un servizio app di Azure.

1. In Visual Studio Code aprire l'area **Azure: App Service** (Azure: App Service), fare clic con il pulsante destro del mouse sul servizio app e scegliere **Start streaming logs** (Avvia streaming dei log):

   ![Avvio dello streaming dei log dall'area App Service (Servizio app)](media/deploy-azure/start-streaming-logs-in-visual-studio-code.png)

   Se viene richiesto di abilitare la registrazione dei file e riavviare l'app Web, scegliere **Yes** (Sì). Durante il riavvio dell'app la finestra **Output** in VS Code visualizza lo stato dell'operazione. Quando l'app viene riavviata, selezionare di nuovo il comando **Start streaming logs** (Avvia lo streaming dei log). L'abilitazione della registrazione è un processo che viene eseguito una volta sola.

1. La finestra **Output** in VS Code visualizza "Starting Live Log Stream" (Avvio del flusso dei log in tempo reale) e l'output dei log inizia a essere visualizzato. Provare ad aggiornare l'app Web nel browser per generare altre informazioni sui log.

1. Per arrestare lo streaming dei log (senza disabilitare la registrazione), fare clic con il pulsante destro del mouse sull'app nell'area **Azure: App Service** (Azure: Servizio app) e scegliere **Stop streaming logs** (Arresta streaming dei log).

> [!div class="nextstepaction"]
> [I log vengono visualizzati: procedere con il passaggio 7 >>>](tutorial-deploy-app-service-on-linux-07.md)

[Problemi? Segnalarli](https://aka.ms/FlaskVSCQuickstartHelp).
