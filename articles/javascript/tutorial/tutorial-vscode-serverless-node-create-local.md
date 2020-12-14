---
title: Creare l'applicazione di Funzioni di Azure da Visual Studio Code
description: Creare un'applicazione locale di Funzioni di Azure (serverless) contenente una funzione che usa un trigger HTTP. Un'app di Funzioni di Azure può contenere molte funzioni con trigger diversi. Il trigger HTTP in particolare gestisce il traffico HTTP in ingresso.
ms.topic: tutorial
ms.date: 11/05/2020
ms.custom: devx-track-js, contperfq2
ms.openlocfilehash: 3c5d747245314029a12b1c502c066324efd70dc8
ms.sourcegitcommit: 550b165d0b910f4ea9652d8401dd4fc93f057f05
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/04/2020
ms.locfileid: "96605931"
---
# <a name="2-create-the-local-functions-app-with-the-visual-studio-code-_functions_-extension"></a>2. Creare l'app per le funzioni locale con l'estensione _Funzioni_ di Visual Studio Code

[Passaggio precedente: Introduzione e prerequisiti](tutorial-vscode-serverless-node-install.md)

In questo passaggio viene creata un'applicazione locale di Funzioni di Azure (serverless) contenente una funzione che usa un [trigger HTTP](/azure/azure-functions/functions-reference-node#http-triggers-and-bindings). Un'app di Funzioni di Azure può contenere molte funzioni con [trigger diversi](/azure/azure-functions/functions-triggers-bindings). Il trigger HTTP in particolare gestisce il traffico HTTP in ingresso.

1. In un terminale o al prompt dei comandi eseguire Visual Studio Code da una cartella adatta per il progetto:

    ```bash
    # Create and navigate to a project folder

    # Run VS Code in that folder
    code .
    ```

1. In Visual Studio Code selezionare il logo di Azure per aprire l'area **Funzioni di Azure**, quindi selezionare il comando **Crea progetto**:

    ![Creare un'app per le funzioni locale in VS Code](../media/functions-extension/create-function-app-project.png)

1. Ai primi due prompt selezionare la cartella corrente, quindi selezionare **JavaScript** come linguaggio.

1. Quando viene chiesto di **selezionare un modello per la prima funzione del progetto**, scegliere **Trigger HTTP**:

    ![Selezionare il trigger per la funzione](../media/functions-extension/create-function-choose-template.png)

1. Quando viene chiesto di **specificare un nome per la funzione**, immettere **HttpExample**. Evitare di usare il nome "HttpTrigger" predefinito, perché è uguale a quello del trigger e potrebbe generare confusione.

    ![Immissione di un nome di funzione](../media/functions-extension/create-function-name.png)

1. Per **Livello di autorizzazione**, selezionare **Anonimo**:

    ![ Per "Livello di autorizzazione", selezionare "Anonimo"](../media/functions-extension/create-function-anonymous-auth.png)

1. Dopo alcuni secondi, VS Code completa la creazione del progetto. È disponibile una cartella denominata per la funzione, *HttpExample*, che contiene tre file:

    | Nome file | Descrizione |
    | --- | --- |
    | *index.js* |  Il codice sorgente che risponde alla richiesta HTTP. |
    | *function.json* | La [configurazione di binding](/azure/azure-functions/functions-triggers-bindings) per il trigger HTTP. |
    | *sample.dat* | Un file di dati segnaposto per dimostrare che la cartella contiene altri file. È possibile eliminare questo file, se si desidera, perché non viene usato nell'esercitazione. |

    ![Risultato della creazione di un'app per le funzioni](../media/functions-extension/create-function-app-results.png)

## <a name="http-function-javascript-template-code"></a>Codice del modello JavaScript di funzione HTTP

Viene fornito il codice di base per rispondere alla richiesta HTTP. Se si ha familiarità con la richiesta HTTP (il parametro _req_) e con gli oggetti risposta, la funzione dovrebbe essere già nota. Le informazioni della risposta si restituiscono con l'oggetto **context** della proprietà `res`.  

```javascript
module.exports = async function (context, req) {
    context.log('JavaScript HTTP trigger function processed a request.');

    const name = (req.query.name || (req.body && req.body.name));
    const responseMessage = name
        ? "Hello, " + name + ". This HTTP triggered function executed successfully."
        : "This HTTP triggered function executed successfully. Pass a name in the query string or in the request body for a personalized response.";

    context.res = {
        // status: 200, /* Defaults to 200 */
        body: responseMessage
    };
}
```

Ogni tipo di [trigger](/azure/azure-functions/functions-triggers-bindings?tabs=csharp) della funzione fornisce una funzione modello che consente di concentrarsi immediatamente sul codice dell'applicazione. Se si passa da Express.js a Funzioni di Azure, [vedere le modifiche necessarie](/azure/azure-functions/shift-expressjs?tabs=javascript) da apportare nell'applicazione. 

> [!div class="nextstepaction"]
> [L'app per le funzioni è stata creata](tutorial-vscode-serverless-node-test-local.md) [Si è verificato un problema](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azurefunctions&step=create-app)
