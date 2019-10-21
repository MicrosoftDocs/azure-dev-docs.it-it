---
title: Streaming dei log dal servizio app di Azure
description: Parte 5 dell'esercitazione, visualizzare i log
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/24/2019
ms.author: kraigb
ms.openlocfilehash: ce112d535d1f89a7e153f80b9fb80032a977b8d8
ms.sourcegitcommit: c04984b6367e922dbc5973af44f8cd0ca81ce157
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/30/2019
ms.locfileid: "71686148"
---
# <a name="stream-logs-from-app-service"></a>Eseguire lo streaming dei log dal Servizio app

[Passaggio precedente: Distribuire l'app](tutorial-vscode-azure-cli-node-04.md)

In questo passaggio vengono visualizzati i log del Servizio app in esecuzione. Qualsiasi chiamata effettuata a `console.log` nel codice del sito viene visualizzata nel terminale.

1. Eseguire il comando seguente per avviare la registrazione, sostituendo `<your_app_name>` con il nome del Servizio app:

    ```bash
    az webapp log tail --name <your_app_name>
    ```

1. Dopo alcuni secondi dovrebbe essere visualizzato un messaggio che indica che si è connessi al servizio di streaming di log.

    ```bash
    2019-09-25T13:39:23  Welcome, you are now connected to log-streaming service. The default timeout is 2 hours. Change the timeout with the App Setting SCM_LOGSTREAM_TIMEOUT (in seconds).
    ```

1. Aggiornare la pagina alcune volte nel browser per generare altro output:

    ```bash
    GET / 304 2.327 ms - -
    GET / 304 0.957 ms - -
    GET / 304 2.435 ms - -
    ```

1. Premere **CTRL**+**C** per terminare la sessione di registrazione.

> [!div class="nextstepaction"]
> [I log vengono visualizzati](tutorial-vscode-azure-cli-node-06.md) [Si è verificato un problema](https://www.research.net/r/PWZWZ52?tutorial=node-deployment&step=tailing-logs)