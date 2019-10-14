---
title: 'Esercitazione: Creare una funzione Python per Funzioni di Azure con Visual Studio Code'
description: "Passaggio 2 dell'esercitazione: informazioni sull'uso dell'estensione Funzioni di Azure per VS Code."
services: functions
author: kraigb
manager: barbkess
ms.service: azure-functions
ms.topic: conceptual
ms.date: 09/02/2019
ms.author: kraigb
ms.custom: seo-python-october2019
ms.openlocfilehash: 691e64ae9b407ba4277ddde2a62a583623e53484
ms.sourcegitcommit: bed07b313eeab51281d1a6d4eba67a75524b2f57
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/09/2019
ms.locfileid: "72172478"
---
# <a name="tutorial-create-a-python-function-for-azure-functions"></a>Esercitazione: Creare una funzione Python per Funzioni di Azure

[Passaggio precedente: Prerequisiti](tutorial-vs-code-serverless-python-01.md)

1. Il codice pere Funzioni di Azure viene gestito all'interno di un _progetto_ di Funzioni, che viene creato prima di creare il codice. Nell'area **Azure: Functions** (Azure: Funzioni) aperta facendo clic sull'icona di Azure sul lato sinistro selezionare l'icona di comando **Create New Project** oppure aprire il riquadro comandi (F1) e selezionare **Azure Functions: Create New Project**(ASA: Crea nuovo progetto).

    ![Pulsante Create New Project nell'area Functions](media/tutorial-vs-code-serverless-python/project-create-new.png)

1. Fornire le informazioni richieste ai prompt visualizzati:

    | Prompt | Valore | DESCRIZIONE |
    | --- | --- | --- |
    | Specify a folder for the project (Specificare una cartella per il progetto) | Cartella attualmente aperta | Cartella in cui creare il progetto. Se necessario, è possibile creare il progetto in una sottocartella. |
    | Selezionare un linguaggio per il progetto di app per le funzioni | **Python** | Linguaggio da usare per la funzione, che determina il modello usato per il codice. |
    | Selezionare un modello per la prima funzione del progetto | **Trigger HTTP** | Una funzione che usa un trigger HTTP viene eseguita ogni volta che si effettua una richiesta HTTP all'endpoint della funzione. Sono disponibili diversi altri trigger per Funzioni di Azure. Per altre informazioni, vedere [Quali operazioni si possono eseguire con Funzioni?](/azure/azure-functions/functions-overview#what-can-i-do-with-functions). |
    | Specificare un nome di funzione | HttpExample | Il nome viene usato per una sottocartella che contiene il codice della funzione unitamente ai dati di configurazione e definisce anche il nome dell'endpoint HTTP. Usare "HttpExample" invece di accettare il valore predefinito "HTTPTrigger" per distinguere la funzione dal trigger. |
    | Livello di autorizzazione | **Anonimo** | Con l'autorizzazione anonima la funzione è accessibile pubblicamente a chiunque. |
    | Specificare come aprire il progetto | **Open in current window** (Apri nella finestra corrente) | Apre il progetto nella finestra corrente di Visual Studio Code. |

1. Dopo qualche istante viene visualizzato un messaggio per indicare che il nuovo progetto è stato creato. In **Esplora risorse** è presente la sottocartella creata per il progetto e Visual Studio Code apre il file *\_\_init\_\_.py* che contiene il codice predefinito della funzione:

    [![Risultato della creazione di un nuovo progetto Funzioni per Python](media/tutorial-vs-code-serverless-python/project-create-results.png)](media/tutorial-vs-code-serverless-python/project-create-results.png)

    > [!NOTE]
    > Se Visual Studio Code indica che non è stato selezionato un interprete Python all'apertura del file *\_\_init\_\_.py*, aprire il riquadro comandi (**F1**), selezionare il comando **Python: Select Interpreter** (Python: Seleziona interprete) e quindi selezionare l'ambiente virtuale nella cartella `.env` locale (creata come parte del progetto). L'ambiente deve essere basato su Python 3.6 x in modo specifico, come indicato nell'articolo precedente in [Prerequisiti](tutorial-vs-code-serverless-python-01.md#prerequisites).
    >
    > ![Selezione dell'ambiente virtuale creato con il progetto](media/tutorial-vs-code-serverless-python/select-venv-interpreter.png)

> [!TIP]
> Quando si vuole creare un'altra funzione nello stesso progetto, usare il comando **Create Function** (Crea funzione) nell'area **Azure: Functions** (Azure: Funzioni) oppure aprire il riquadro comandi (**F1**) e selezionare il comando **Azure Functions: Creare il comando Funzione** . Entrambi i comandi richiedono l'immissione di un nome di funzione, ovvero il nome dell'endpoint, e quindi creano una sottocartella con i file predefiniti.
>
> ![Comando New Function (Nuova funzione) nell'area Azure: Functions (Azure: Functions)](media/tutorial-vs-code-serverless-python/function-create-new.png)

> [!div class="nextstepaction"]
> [La funzione è stata creata](tutorial-vs-code-serverless-python-03.md)

[Si è verificato un problema](https://www.research.net/r/PWZWZ52?tutorial=vscode-functions-python&step=02-create-function)
