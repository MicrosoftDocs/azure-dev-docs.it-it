---
title: Eseguire l'applicazione di Funzioni di Azure in locale in Visual Studio Code
description: Eseguire il progetto di Funzioni di Azure in locale per testarlo prima della distribuzione in Azure. Impostare un punto di interruzione immediatamente prima che la funzione serverless restituisca la risposta.
ms.topic: tutorial
ms.date: 03/02/2021
ms.custom: devx-track-js, contperf-fy21q2
ms.openlocfilehash: e16daa0f9c3db2edf2335c3f35277b1b95fcfef8
ms.sourcegitcommit: 576c878c338d286060010646b96f3ad0fdbcb814
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/04/2021
ms.locfileid: "102117825"
---
# <a name="3-test-the-function-locally"></a>3. Testare la funzione in locale

[Passaggio precedente: Creare l'app per le funzioni](tutorial-vscode-serverless-node-create-local.md)

In questo passaggio si esegue il progetto di Funzioni di Azure in locale per testarlo prima di distribuirlo in Azure. Impostare un punto di interruzione immediatamente prima che la funzione serverless restituisca la risposta.

Quando è stata creata l'app per le funzioni, l'estensione Funzioni di Azure ha aggiunto automaticamente una configurazione di avvio di VS Code al progetto, disponibile nel file *.vscode/launch.json*. Questa configurazione usa lo stesso runtime eseguito in Azure, quindi si è certi che il codice sorgente funzioni prima della distribuzione nel cloud.

## <a name="run-the-local-serverless-function"></a>Eseguire la funzione serverless locale

1. In Visual Studio Code premere **F5** (oppure usare il comando di menu **Debug** > **Avvia debug**) per avviare il debugger e collegarsi all'host di Funzioni di Azure. Questo comando usa automaticamente la configurazione di debug creata da Funzioni di Azure.

1. L'output di Functions Core Tools viene visualizzato nel pannello **Terminale** di VS Code. Dopo aver volta avviato l'host, premere **ALT**+clic sull'URL locale visualizzato nell'output per aprire il browser ed eseguire la funzione:

    ![Output visualizzato nel pannello Terminale di VS Code durante il debug in locale](../media/functions-extension/local-test-output.png)

1. Il codice creato dal modello di trigger HTTP predefinito analizza un parametro di query `name` per personalizzare la risposta. Nel browser aggiungere `?name=<yourname>` all'URL per visualizzare correttamente l'output della risposta:

    ![Funzione di trigger HTTP che analizza i parametri dell'URL](../media/functions-extension/local-test-browser.png)

## <a name="set-and-stop-at-break-point-in-serverless-app"></a>Impostare e arrestare l'esecuzione in corrispondenza del punto di interruzione nell'app serverless

1. Con la funzione in esecuzione in locale, è possibile impostare i punti di interruzione su parti diverse del codice. Aprire il file *index.js*, quindi fare clic sul margine a sinistra della riga 11 nella finestra dell'editor. Viene visualizzato un puntino rosso per indicare un punto di interruzione. Ora rimuovere l'argomento `?name=` dall'URL nel browser. Quando il browser effettua la richiesta, VS Code arresta il codice della funzione in corrispondenza del punto di interruzione:

    ![VS Code arrestato in un punto di interruzione](../media/functions-extension/debugging-breakpoint.png)

    Per altre informazioni su punti di interruzione e debug in VS Code, vedere [Debug](https://code.visualstudio.com/docs/editor/debugging).

> [!div class="nextstepaction"]
> [L'app per le funzioni è stata eseguita in locale](tutorial-vscode-serverless-node-deploy-hosting.md) [Si è verificato un problema](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azurefunctions&step=run-app)
