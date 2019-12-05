---
title: Eseguire l'applicazione di Funzioni di Azure in locale in Visual Studio Code
description: Parte 3 dell'esercitazione, eseguire l'app in locale per testarla.
ms.topic: conceptual
ms.date: 09/23/2019
ms.openlocfilehash: 2a7cb5e5c433c90d74cd3b7771ce90529f617fcb
ms.sourcegitcommit: e77f8f652128b798dbf972078a7b460ed21fb5f8
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/25/2019
ms.locfileid: "74466577"
---
# <a name="test-the-function-locally"></a>Testare la funzione in locale

[Passaggio precedente: Creare l'app per le funzioni](tutorial-vscode-serverless-node-02.md)

In questo passaggio si esegue il progetto di Funzioni di Azure in locale per testarlo prima di distribuirlo in Azure.

Quando è stata creata l'app per le funzioni, l'estensione Funzioni di Azure ha aggiunto automaticamente una configurazione di avvio di VS Code al progetto, disponibile nel file *.vscode/launch.json*. Questa configurazione usa lo stesso runtime eseguito in Azure, quindi si è certi che il codice sorgente funzioni prima della distribuzione nel cloud.

1. In Visual Studio Code premere **F5** (oppure usare il comando di menu **Debug** > **Avvia debug**) per avviare il debugger e collegarsi all'host di Funzioni di Azure. Questo comando usa automaticamente la singola configurazione di debug creata da Funzioni di Azure.

1. L'output di Functions Core Tools viene visualizzato nel pannello **Terminale** di VS Code. Una volta avviato l'host, premere **CTRL**+clic sull'URL locale visualizzato nell'output per aprire il browser ed eseguire la funzione:

    ![Output visualizzato nel pannello Terminale di VS Code durante il debug in locale](media/functions-extension/local-test-output.png)

1. Il codice creato dal modello di trigger HTTP predefinito analizza un parametro di query `name` per personalizzare la risposta. Nel browser aggiungere `?name=<yourname>` all'URL per visualizzare correttamente l'output della risposta:

    ![Funzione di trigger HTTP che analizza i parametri dell'URL](media/functions-extension/local-test-browser.png)

1. Con la funzione in esecuzione in locale, è possibile impostare i punti di interruzione su parti diverse del codice. Per altre informazioni sui punti di interruzione e sul debug in VS Code, vedere [Debug](https://code.visualstudio.com/docs/editor/debugging). Aprire il file *index.js*, quindi fare clic sul margine a sinistra della riga 11 nella finestra dell'editor. Viene visualizzato un puntino rosso per indicare un punto di interruzione. Ora rimuovere l'argomento `?name=` dall'URL nel browser. Quando il browser effettua la richiesta, VS Code arresta il codice della funzione in corrispondenza del punto di interruzione:

    ![VS Code arrestato in un punto di interruzione](media/functions-extension/debugging-breakpoint.png)

> [!div class="nextstepaction"]
> [L'app per le funzioni è stata eseguita in locale](tutorial-vscode-serverless-node-04.md) [Si è verificato un problema](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azurefunctions&step=run-app)
