---
title: 'Esercitazione: Streaming dei log in Visual Studio Code dal servizio app di Azure per un contenitore'
description: Parte 4 dell'esercitazione, visualizzazione dei log del servizio app di Azure per monitorarne il comportamento.
ms.topic: conceptual
ms.date: 09/12/2019
ms.custom: seo-python-october2019
ms.openlocfilehash: e9b54e53763b729f5042cfb0d8df438650fd362f
ms.sourcegitcommit: e77f8f652128b798dbf972078a7b460ed21fb5f8
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/25/2019
ms.locfileid: "74466995"
---
# <a name="tutorial-stream-logs-from-azure-app-service-for-a-container"></a>Esercitazione: Streaming dei log dal Servizio app di Azure per un contenitore

[Passaggio precedente: Apportare modifiche e ripetere la distribuzione](tutorial-deploy-containers-03.md)

Usare questa procedura per eseguire lo streaming dei log in Visual Studio Code da un servizio app di Azure per un contenitore.

In VS Code è possibile visualizzare i log del sito in esecuzione nel servizio app di Azure, che acquisiscono l'output nella console in modo analogo alle istruzioni `print` e lo instradano nel pannello **Output** di VS Code.

1. Trovare l'app nell'area **Azure: Servizio app**, fare clic su di essa con il pulsante destro del mouse e scegliere **Start Streaming Logs** (Avvia lo streaming dei log).

1. Quando viene richiesto, scegliere**Yes** (Sì) per abilitare la registrazione e riavviare l'app. Una volta riavviata l'app, verrà aperto il pannello Output di VS Code con una connessione al flusso di log.

1. Dopo alcuni secondi verrà visualizzato un messaggio che indica che si è connessi al servizio di streaming di log.

    ```bash
    Connecting to log stream...
    2018-09-27T20:14:26  Welcome, you are now connected to log-streaming service.

    2018-09-27 20:14:59.269 INFO  - Starting container for site

    2018-09-27 20:14:59.270 INFO  - docker run -d -p 24138:8000 --name vsdocs-django-sample-container_0 -e WEBSITES_PORT=8000 -e WEBSITE_SITE_NAME=vsdocs-django-sample-container -e WEBSITE_AUTH_ENABLED=False -e WEBSITE_ROLE_INSTANCE_ID=0 -e WEBSITE_INSTANCE_ID=02c705ae24eaf5f298e553a9c2724b9fe4485707c2d1c36137cd02931091e561 -e HTTP_LOGGING_ENABLED=1 vsdocsregistry.azurecr.io/python-sample-vscode-django-tutorial:latest

    2018-09-27 20:15:06.216 INFO  - Container vsdocs-django-sample-container_0 for site vsdocs-django-sample-container initialized successfully.
    ```

1. Spostarsi all'interno dell'app per visualizzare output aggiuntivo per varie richieste HTTP.

> [!div class="nextstepaction"]
> [I log vengono visualizzati](tutorial-deploy-containers-05.md)

[Si è verificato un problema](https://www.research.net/r/PWZWZ52?tutorial=vscode-appservice-containers&step=04-stream-logs)
