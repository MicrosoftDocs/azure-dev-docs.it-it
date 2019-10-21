---
title: Distribuire l'applicazione di Funzioni di Azure da Visual Studio Code
description: Parte 4 dell'esercitazione, distribuire l'app per le funzioni nel cloud.
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/23/2019
ms.author: kraigb
ms.openlocfilehash: 53d0dd11567084d42de71a0f737cf8b9f5fc5249
ms.sourcegitcommit: c04984b6367e922dbc5973af44f8cd0ca81ce157
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/30/2019
ms.locfileid: "71685927"
---
# <a name="deploy-the-functions-app"></a>Distribuire l'app per le funzioni

[Passaggio precedente: Testare la funzione in locale](tutorial-vscode-serverless-node-03.md)

1. In VS Code selezionare il logo di Azure per aprire l'area **Azure**, quindi in **Funzioni** selezionare la freccia verso l'alto blu per distribuire l'app:

    ![Comando per la distribuzione in Funzioni di Azure](media/functions-extension/deploy-app.png)

    In alternativa, è possibile eseguire la distribuzione aprendo il **riquadro comandi** (**F1**), immettendo 'deploy to function app' ed eseguendo il comando **Funzioni di Azure: Deploy to Function App** (Distribuisci nell'app per le funzioni).

1. Quando viene chiesto di **selezionare l'app per le funzioni in Azure**, scegliere **Create new Function app in Azure** (Crea nuova app per le funzioni in Azure).

1. Al prompt successivo immettere un nome univoco globale per l'app per le funzioni e premere **INVIO**. I caratteri validi per un nome di app per le funzioni sono 'a-z', '0-9' e '-'.

1. Al prompt successivo selezionare un'[area](https://azure.microsoft.com/en-us/regions/) di Azure vicina.

1. Il pannello **Output** di VS Code per **Funzioni di Azure** visualizza lo stato di avanzamento:

    ![Pannello Output di VS Code che visualizza lo stato di avanzamento della distribuzione](media/functions-extension/deploy-progress.png)

1. Una volta completata la distribuzione, passare all'area **Funzioni di Azure**, espandere il nodo relativo alla propria sottoscrizione di Azure, espandere il nodo dell'app per le funzioni e quindi espandere **Funzioni (sola lettura)** . Fare clic con il pulsante destro del mouse sul nome della funzione e scegliere **Copy Function Url** (Copia l'URL della funzione):

    ![Comando di copia dell'URL della funzione](media/functions-extension/copy-function-url-command.png)

1. Incollare l'URL nel browser e aggiungere un argomento `?name=<yourname>`. Il browser dovrebbe quindi mostrare la funzione in esecuzione nel cloud:

    ![Funzione in esecuzione nel cloud](media/functions-extension/remote-test-browser.png)

1. Se si desidera, apportare alcune modifiche al codice della funzione in *index.js* oppure aggiungere altre funzioni con altri trigger. Dopo aver eseguito i test in locale, distribuire di nuovo il codice come descritto nei passaggi precedenti per testare tali modifiche nel cloud.

    > [!TIP]
    > Poiché verrà distribuita l'intera applicazione per le funzioni, le modifiche apportate alle singole funzioni verranno distribuite contemporaneamente.

> [!div class="nextstepaction"]
> [L'app per le funzioni è stata distribuita](tutorial-vscode-serverless-node-05.md) [Si è verificato un problema](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azurefunctions&step=deploy-app)