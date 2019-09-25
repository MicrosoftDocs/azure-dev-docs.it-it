---
title: Eseguire il debug in locale del codice Python in Funzioni di Azure con Visual Studio Code
description: "Passaggio 4 dell'esercitazione: esecuzione del debugger di VS Code in locale per verificare il codice Python."
services: functions
author: kraigb
manager: barbkess
ms.service: azure-functions
ms.topic: conceptual
ms.date: 09/02/2019
ms.author: kraigb
ms.openlocfilehash: 06e09ee8dd8128fe3ea65b7004a775c4dabbe161
ms.sourcegitcommit: 74e28a479c87a3a53592646420b78e69852dd86a
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/16/2019
ms.locfileid: "71019629"
---
# <a name="debug-the-function-code-locally"></a>Eseguire il debug del codice della funzione in locale

[Passaggio precedente: Esaminare i file di codice](tutorial-vs-code-serverless-python-03.md)

1. Quando si crea il progetto per Funzioni, l'estensione Visual Studio Code crea in `.vscode/launch.json` anche una configurazione di avvio che contiene una singola configurazione denominata **Attach to Python Functions** (Collega a Funzioni per Python). Questa configurazione indica che è sufficiente premere F5 o usare la finestra di esplorazione di Debug per avviare il progetto:

    ![Finestra di esplorazione di Debug che mostra la configurazione di avvio di Funzioni](media/tutorial-vs-code-serverless-python/launch-configuration.png)

1. Quando si avvia il debugger, viene aperto un terminale che mostra l'output di Funzioni di Azure, incluso un riepilogo degli endpoint disponibili. L'URL potrebbe essere diverso se è stato usato un nome diverso da "HttpExample":

    ```output
    Http Functions:

            HttpExample: [GET,POST] http://localhost:7071/api/HttpExample
    ```

1. Usare **CTRL+clic** o **CMD+clic** sull'URL visualizzato nella finestra **Output** di Visual Studio Code per aprire l'indirizzo in un browser oppure avviare un browser e incollarvi lo stesso URL. In entrambi i casi l'endpoint è `api/<function_name>`, ma in questo caso è `api/HttpExample`. Dal momento però che l'URL non include alcun parametro name, nella finestra del browser dovrebbe essere visualizzato "Please pass a name on the query string or in the request body" (Passare un nome nella stringa di query o nel corpo della richiesta) a seconda del percorso nel codice.

1. A questo punto, provare ad aggiungere un parametro name all'uso, ad esempio `http://localhost:7071/api/HttpExample?name=VS%20Code`. Nella finestra del browser verrà visualizzato il messaggio "Hello Visual Studio Code!" (Benvenuto in Visual Studio Code), per indicare che il percorso del codice è stato eseguito.

1. Per passare il valore del nome nel corpo di una richiesta JSON, è possibile usare uno strumento come curl con JSON inline:

    ```bash
    # Mac OS/Linux: modify the URL if you're using a different function name
    curl --header "Content-Type: application/json" --request POST \
        --data {"name":"Visual Studio Code"} http://localhost:7071/api/HttpExample
    ```

    ```ps
    # Windows (escaping on the quotes is necessary; also modify the URL
    # if you're using a different function name)
    curl --header "Content-Type: application/json" --request POST \
        --data {"""name""":"""Visual Studio Code"""} http://localhost:7071/api/HttpExample
    ```

    In alternativa, creare un file come *data.json* che contenga `{"name":"Visual Studio Code"}` e usare il comando `curl --header "Content-Type: application/json" --request POST --data @data.json http://localhost:7071/api/HttpExample`.

1. Per testare il debug della funzione, impostare un punto di interruzione nella riga `name = req.params.get('name')` ed effettuare una nuova richiesta all'URL. Il debugger di Visual Studio Code dovrebbe arrestarsi su tale riga, consentendo di esaminare le variabili ed eseguire il codice un'istruzione alla volta. Per una breve procedura dettagliata sul debug di base, vedere l'[esercitazione sulla configurazione e l'esecuzione del debugger in Visual Studio Code](https://code.visualstudio.com/docs/python/python-tutorial.md#configure-and-run-the-debugger).

1. Se si ritiene di aver testato la funzione in locale in modo esauriente, arrestare il debugger usando n il comando di menu **Debug** > **Arresta debug** oppure il comando **Disconnect** sulla barra degli strumenti di debug.

> [!div class="nextstepaction"]
> [Il debugger è stato eseguito in locale](tutorial-vs-code-serverless-python-05.md)

[Si è verificato un problema](https://www.research.net/r/PWZWZ52?tutorial=vscode-functions-python&step=04-test-debug)
