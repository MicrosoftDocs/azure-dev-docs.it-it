---
title: Creare l'applicazione di Funzioni di Azure da Visual Studio Code
description: Parte 2 dell'esercitazione, creare l'app di Funzioni di Azure
ms.topic: conceptual
ms.date: 09/23/2019
ms.openlocfilehash: fb7b5f3bab0dbde0a04671126ad7aa8e5be16ba0
ms.sourcegitcommit: 21ddeb9bd9abd419d143dc2ca8a7c821a1758cf9
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/11/2020
ms.locfileid: "79089963"
---
# <a name="create-the-local-functions-app"></a>Creare l'app per le funzioni locale

[Passaggio precedente: Introduzione e prerequisiti](tutorial-vscode-serverless-node-01.md)

In questo passaggio viene creata un'applicazione locale di Funzioni di Azure contenente una funzione che usa un [trigger HTTP](https://docs.microsoft.com/azure/azure-functions/functions-reference-node#http-triggers-and-bindings). Un'app di Funzioni di Azure può contenere molte funzioni con [trigger diversi](https://docs.microsoft.com/azure/azure-functions/functions-triggers-bindings). Il trigger HTTP in particolare gestisce il traffico HTTP in ingresso.

1. In un terminale o al prompt dei comandi eseguire Visual Studio Code da una cartella adatta per il progetto:

    ```bash
    # Create and navigate to a project folder

    # Run VS Code in that folder
    code .
    ```

1. In VS Code selezionare il logo di Azure per aprire l'area **Funzioni di Azure**, quindi selezionare il comando **Crea progetto**:

    ![Creare un'app per le funzioni locale in VS Code](media/functions-extension/create-function-app-project.png)

1. Ai primi due prompt selezionare la cartella corrente, quindi selezionare **JavaScript** come linguaggio.

1. Quando viene chiesto di **selezionare un modello per la prima funzione del progetto**, scegliere **Trigger HTTP**:

    ![Selezionare il trigger per la funzione](media/functions-extension/create-function-choose-template.png)

1. Quando viene chiesto di **specificare un nome per la funzione**, immettere **HttpExample**. Evitare di usare il nome "HttpTrigger" predefinito, perché è uguale a quello del trigger e potrebbe generare confusione.

    ![Immissione di un nome di funzione](media/functions-extension/create-function-name.png)

1. Per **Livello di autorizzazione**, selezionare **Anonimo**:

    ![Immissione di un nome di funzione](media/functions-extension/create-function-anonymous-auth.png)

1. Dopo alcuni secondi, VS Code completa la creazione del progetto. È disponibile una cartella denominata per la funzione, *HttpExample*, che contiene tre file:

    | Nome file | Descrizione |
    | --- | --- |
    | *index.js* |  Il codice sorgente che risponde alla richiesta HTTP. |
    | *function.json* | La [configurazione di binding](/azure/azure-functions/functions-triggers-bindings) per il trigger HTTP. |
    | *sample.dat* | Un file di dati segnaposto per dimostrare che la cartella contiene altri file. È possibile eliminare questo file, se si desidera, perché non viene usato nell'esercitazione. |

    ![Risultato della creazione di un'app per le funzioni](media/functions-extension/create-function-app-results.png)

> [!div class="nextstepaction"]
> [L'app per le funzioni è stata creata](tutorial-vscode-serverless-node-03.md) [Si è verificato un problema](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azurefunctions&step=create-app)
