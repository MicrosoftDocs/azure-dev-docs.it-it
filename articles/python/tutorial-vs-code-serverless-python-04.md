---
title: 'Passaggio 4: Eseguire il debug in locale del codice Python in Funzioni di Azure con VS Code'
description: "Passaggio 4 dell'esercitazione: esecuzione del debugger di VS Code in locale per verificare il codice Python."
ms.topic: conceptual
ms.date: 05/19/2020
ms.custom: seo-python-october2019
ms.openlocfilehash: 167e1c2be4d379f7457d35f2e6fe4a226092ac08
ms.sourcegitcommit: 089b87e1631a9db145583eb274edac6f80d16367
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/20/2020
ms.locfileid: "83708570"
---
# <a name="4-debug-the-azure-functions-python-code-locally"></a>4: Eseguire il debug in locale del codice Python in Funzioni di Azure

[Passaggio precedente: Esaminare i file di codice](tutorial-vs-code-serverless-python-03.md)

È possibile eseguire il debug del codice Python di Funzioni di Azure in locale in Visual Studio Code.

1. Quando si crea il progetto per Funzioni, l'estensione Visual Studio Code crea in `.vscode/launch.json` anche una configurazione di avvio che contiene una singola configurazione denominata **Attach to Python Functions** (Collega a Funzioni per Python). Questa configurazione indica che è sufficiente premere F5 o usare la finestra di esplorazione di Debug per avviare il progetto:

    ![Configurazione della finestra di esplorazione di Debug per avviare un progetto Python](media/tutorial-vs-code-serverless-python/configuration-to-start-a-python-project-for-debugging.png)

1. Quando si avvia il debugger, viene aperto un terminale che mostra l'output di Funzioni di Azure, incluso un riepilogo degli endpoint disponibili. L'URL potrebbe essere diverso se è stato usato un nome diverso da "HttpExample":

    <pre>
    Http Functions:

            HttpExample: [GET,POST] http://localhost:7071/api/HttpExample
    </pre>

1. Usare **CTRL+clic** o **CMD+clic** sull'URL visualizzato nella finestra **Output** di Visual Studio Code per aprire l'indirizzo in un browser oppure avviare un browser e incollarvi lo stesso URL. In entrambi i casi l'endpoint è `api/<function_name>`, ma in questo caso è `api/HttpExample`. Dal momento però che l'URL non include alcun parametro name, nella finestra del browser dovrebbe essere visualizzato "Please pass a name on the query string or in the request body" (Passare un nome nella stringa di query o nel corpo della richiesta) a seconda del percorso nel codice.

    > [!TIP]
    > Se non si è in grado di accedere all'URL, l'esecuzione avviene dietro un proxy aziendale, (pertanto è possibile che siano impostate le variabili di ambiente `HTTP_PROXY` e `HTTPS_PROXY`), quindi impostare una variabile di ambiente denominata `NO_PROXY` su `localhost,127.0.0.1` e riprovare.

1. A questo punto, provare ad aggiungere un parametro name all'uso, ad esempio `http://localhost:7071/api/HttpExample?name=VS%20Code`. Nella finestra del browser verrà visualizzato il messaggio "Hello Visual Studio Code!" (Benvenuto in Visual Studio Code), per indicare che il percorso del codice è stato eseguito.

1. Per passare il valore del nome nel corpo di una richiesta JSON, è possibile usare uno strumento come curl con JSON inline:

    # <a name="bash"></a>[Bash](#tab/bash)

    ```bash
    # Mac OS/Linux: modify the URL if you're using a different function name
    curl --header "Content-Type: application/json" --request POST \
        --data '{"name":"Visual Studio Code"}' http://localhost:7071/api/HttpExample
    ```

    # <a name="powershell"></a>[PowerShell](#tab/powershell)

    ```powershell
    # Windows (escaping on the quotes is necessary; also modify the URL
    # if you're using a different function name)
    curl --header "Content-Type: application/json" --request POST \
        --data {"""name""":"""Visual Studio Code"""} http://localhost:7071/api/HttpExample
    ```

    In PowerShell è anche possibile usare il [cmdlet Invoke-WebRequest](/powershell/module/microsoft.powershell.utility/invoke-webrequest?view=powershell-6).

    ---

    In alternativa, creare un file come *data.json* che contenga `{"name":"Visual Studio Code"}` e usare il comando `curl --header "Content-Type: application/json" --request POST --data @data.json http://localhost:7071/api/HttpExample`.

1. Per testare il debug della funzione, impostare un punto di interruzione nella riga `name = req.params.get('name')` ed effettuare una nuova richiesta all'URL. Il debugger di Visual Studio Code dovrebbe arrestarsi su tale riga, consentendo di esaminare le variabili ed eseguire il codice un'istruzione alla volta. Per una breve procedura dettagliata sul debug di base, vedere l'[esercitazione sulla configurazione e l'esecuzione del debugger in Visual Studio Code](https://code.visualstudio.com/docs/python/python-tutorial#configure-and-run-the-debugger).

1. Se si ritiene di aver testato la funzione in locale in modo esauriente, arrestare il debugger usando n il comando di menu **Debug** > **Arresta debug** oppure il comando **Disconnect** sulla barra degli strumenti di debug.

> [!NOTE]
> Se viene visualizzato l'errore "Failed to verify 'AzureWebJobsStorage' connection specified in 'local.settings.json'." (Non è stato possibile verificare la connessione 'AzureWebJobsStorage' specificata in 'local.settings.json'), il file *local.settings.json* nel progetto contiene la riga `"AzureWebJobsStorage": "UseDevelopmentStorage=true"`. Questa riga indica che il debugger prevede l'uso dell'Emulatore di archiviazione di Azure in locale, che però non è installato. In questo caso, è possibile [installare l'Emulatore di archiviazione di Azure](/azure/storage/common/storage-use-emulator#get-the-storage-emulator), [avviare e inizializzare l'emulatore](/azure/storage/common/storage-use-emulator#start-and-initialize-the-storage-emulator) e riavviare il debugger.
>
> In alternativa, modificare la riga nel file JSON in `"AzureWebJobsStorage": ""` e riavviare il debugger.

> [!div class="nextstepaction"]
> [Il debugger è stato eseguito in locale: procedere con il passaggio 5 >>>](tutorial-vs-code-serverless-python-05.md)

[Si è verificato un problema](https://www.research.net/r/PWZWZ52?tutorial=vscode-functions-python&step=04-test-debug)
