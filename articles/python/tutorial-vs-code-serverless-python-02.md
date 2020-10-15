---
title: 'Passaggio 2: Creare una funzione Python per Funzioni di Azure con VS Code'
description: "Passaggio 2 dell'esercitazione: informazioni sull'uso dell'estensione Funzioni di Azure per VS Code."
ms.topic: conceptual
ms.date: 09/17/2020
ms.custom: devx-track-python, seo-python-october2019
ms.openlocfilehash: 24e158023ba748e6137471ae4833a970a185853b
ms.sourcegitcommit: 723441eda0eb4ff893123201a9e029b7becf5ecc
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/08/2020
ms.locfileid: "91846732"
---
# <a name="2-create-a-python-function-for-azure-functions"></a>2: Creare una funzione Python per Funzioni di Azure

[Passaggio precedente: Configurare l'ambiente](tutorial-vs-code-serverless-python-01.md)

Questo articolo illustra come creare una funzione Python per Funzioni di Azure con Visual Studio Code. Il codice per Funzioni di Azure viene gestito all'interno di un _progetto_ di Funzioni, che viene creato prima della creazione del codice.

1. Nell'area **Azure: Functions** (Azure: Funzioni) aperta facendo clic sull'icona di Azure sul lato sinistro selezionare l'icona di comando **Create New Project** (Crea nuovo progetto) oppure aprire il riquadro comandi (F1) e selezionare **Azure Functions: Create New Project**(Funzioni di Azure: Crea nuovo progetto).

    ![Creare un nuovo progetto nell'area Azure: Functions](media/tutorial-vs-code-serverless-python/create-a-new-project-in-azure-functions-explorer.png)

1. Fornire le informazioni richieste ai prompt visualizzati:

    | Prompt | valore | Descrizione |
    | --- | --- | --- |
    | Specify a folder for the project (Specificare una cartella per il progetto) | Cartella attualmente aperta | Cartella in cui creare il progetto. Se necessario, è possibile creare il progetto in una sottocartella. |
    | Selezionare un linguaggio per il progetto di app per le funzioni | **Python** | Linguaggio da usare per la funzione, che determina il modello usato per il codice. |
    | Selezionare l'interprete Python per creare un ambiente virtuale | Usare il percorso predefinito specificato oppure immettere manualmente il percorso di un interprete appropriato se non ne viene specificato nessuno. | Interprete Python da usare per un ambiente virtuale. |
    | Selezionare un modello per la prima funzione del progetto | **Trigger HTTP** | Una funzione che usa un trigger HTTP viene eseguita ogni volta che si effettua una richiesta HTTP all'endpoint della funzione. Sono disponibili diversi altri trigger per Funzioni di Azure. Per altre informazioni, vedere [Quali operazioni si possono eseguire con Funzioni?](/azure/azure-functions/functions-overview#what-can-i-do-with-functions). |
    | Specificare un nome di funzione | HttpExample | Il nome viene usato per una sottocartella che contiene il codice della funzione unitamente ai dati di configurazione e definisce anche il nome dell'endpoint HTTP. Usare "HttpExample" invece di accettare il valore predefinito "HTTPTrigger1" per distinguere la funzione dal trigger. |
    | Livello di autorizzazione | **Anonimo** | Con l'autorizzazione anonima la funzione è accessibile pubblicamente a chiunque. |

1. Dopo qualche istante viene visualizzato un messaggio per indicare che il nuovo progetto è stato creato. In **Esplora risorse** è presente la sottocartella creata per il progetto e Visual Studio Code apre il file *\_\_init\_\_.py* che contiene il codice predefinito della funzione:

    ![Risultato della creazione di un nuovo progetto Python in Funzioni di Azure](media/tutorial-vs-code-serverless-python/display-results-of-new-python-project-in-azure-functions.png)

    Se Visual Studio Code indica che non è stato selezionato un interprete Python all'apertura del file *\_\_init\_\_.py*, aprire il riquadro comandi (**F1**), selezionare il comando **Python: Select Interpreter** (Python: Seleziona interprete) e quindi selezionare l'ambiente virtuale nella cartella `.env` locale (creata come parte del progetto).

    ![Selezionare l'ambiente virtuale creato con il progetto Python](media/tutorial-vs-code-serverless-python/select-virtual-environment-created-with-the-python-project.png)

> [!TIP]
> Quando si vuole creare un'altra funzione nello stesso progetto, usare il comando **Create Function** (Crea funzione) nell'area **Azure: Functions** (Azure: Funzioni) oppure aprire il riquadro comandi (**F1**) e selezionare il comando **Azure Functions: Creare il comando Funzione** . Entrambi i comandi richiedono l'immissione di un nome di funzione, ovvero il nome dell'endpoint, e quindi creano una sottocartella con i file predefiniti.
>
> ![Creare funzioni usando Create Function nell'area Azure: Functions](media/tutorial-vs-code-serverless-python/create-new-functions-in-azure-functions-explorer.png)

> [!div class="nextstepaction"]
> [La funzione è stata creata: procedere con il passaggio 3 >>>](tutorial-vs-code-serverless-python-03.md)
