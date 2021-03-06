---
title: Streaming dei log dal servizio app di Azure
description: Parte 5 dell'esercitazione, visualizzazione dei log con l'interfaccia della riga di comando di Azure
ms.topic: tutorial
ms.date: 09/24/2019
ms.custom: devx-track-js, devx-track-azurecli
ms.openlocfilehash: f8da719269e157a2ab498834f00e4a360120b8a9
ms.sourcegitcommit: 1dfcc022a3098b1a1505e9458eada35f527ef070
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97658394"
---
# <a name="stream-logs-from-app-service"></a>Eseguire lo streaming dei log dal Servizio app

[Passaggio precedente: Distribuire l'app](tutorial-vscode-azure-cli-node-04.md)

In questo passaggio vengono visualizzati i log del Servizio app in esecuzione. Qualsiasi chiamata effettuata a `console.log` nel codice del sito viene visualizzata nel terminale.

1. Eseguire il comando seguente per avviare la registrazione, sostituendo `<your_app_name>` con il nome del Servizio app:

    ```azurecli
    az webapp log tail --name <your_app_name>
    ```

1. Dopo alcuni secondi nell'output dovrebbe essere visualizzato un messaggio che indica che si è connessi al servizio di streaming di log.

    <pre>
    2019-09-25T13:39:23  Welcome, you are now connected to log-streaming service. The default timeout is 2 hours. Change the timeout with the App Setting SCM_LOGSTREAM_TIMEOUT (in seconds).
    </pre>

1. Aggiornare la pagina alcune volte nel browser per generare altro output:

    <pre>
    GET / 304 2.327 ms - -
    GET / 304 0.957 ms - -
    GET / 304 2.435 ms - -
    </pre>

1. Premere **CTRL**+**C** per terminare la sessione di registrazione.

> [!div class="nextstepaction"]
> [I log vengono visualizzati](tutorial-vscode-azure-cli-node-06.md) [Si è verificato un problema](https://www.research.net/r/PWZWZ52?tutorial=node-deployment&step=tailing-logs)
