---
title: 'Passaggio 2: Creare una funzione Python serverless per Funzioni di Azure con VS Code'
description: Passaggio 2 dell'esercitazione, aggiungere una funzione Python serverless usando l'estensione Funzioni di Azure per VS Code.
ms.topic: conceptual
ms.date: 11/30/2020
ms.custom: devx-track-python, seo-python-october2019, contperf-fy21q2
ms.openlocfilehash: 97650e7d0f0676921ca70cf65db56b83bd4c0240
ms.sourcegitcommit: 4f9ce09cbf9663203c56f5b12ecbf70ea68090ed
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/06/2021
ms.locfileid: "97911481"
---
# <a name="2-create-a-python-function-for-azure-functions"></a>2: Creare una funzione Python per Funzioni di Azure

[Passaggio precedente: Configurare l'ambiente](tutorial-vs-code-serverless-python-01.md)

In questo articolo si crea una funzione Python per Funzioni di Azure serverless con Visual Studio Code. Il codice per Funzioni di Azure viene gestito all'interno di un _progetto_ di Funzioni, che viene creato prima della creazione del codice.

1. Nell'area **Azure: Functions** (Azure: Funzioni) aperta facendo clic sull'icona di Azure sul lato sinistro selezionare l'icona di comando **Create New Project** (Crea nuovo progetto) oppure aprire il riquadro comandi (F1) e selezionare **Azure Functions: Create New Project**(Funzioni di Azure: Crea nuovo progetto).

    ![Creare un nuovo progetto nell'area Azure: Functions](media/tutorial-vs-code-serverless-python/create-a-new-project-in-azure-functions-explorer.png)

1. Fornire le informazioni richieste ai prompt visualizzati:

    | Prompt | valore | Descrizione |
    | --- | --- | --- |
    | Selezionare una cartella che conterrà il progetto di funzioni | Cartella attualmente aperta | Cartella in cui creare il progetto. Se necessario, è possibile creare il progetto in una sottocartella. |
    | Selezionare una lingua | **Python** | Linguaggio da usare per la funzione, che determina il modello usato per il codice. |
    | Selezionare l'interprete Python per creare un ambiente virtuale | Usare il percorso predefinito specificato oppure immettere manualmente il percorso di un interprete appropriato se non ne viene specificato nessuno. | Interprete Python da usare per un ambiente virtuale. |
    | Selezionare un modello per la prima funzione del progetto | **Trigger HTTP** | Una funzione che usa un trigger HTTP viene eseguita ogni volta che si effettua una richiesta HTTP all'endpoint della funzione. Sono disponibili diversi altri trigger per Funzioni di Azure. Per altre informazioni, vedere [Quali operazioni si possono eseguire con Funzioni?](/azure/azure-functions/functions-overview#what-can-i-do-with-functions). |
    | Specificare un nome di funzione | HttpExample | Il nome viene usato per una sottocartella che contiene il codice della funzione unitamente ai dati di configurazione e definisce anche il nome dell'endpoint HTTP. Usare "HttpExample" invece di accettare il valore predefinito "HTTPTrigger1" per distinguere la funzione dal trigger. |
    | Livello di autorizzazione | **Anonimo** | Con l'autorizzazione anonima la funzione è accessibile pubblicamente a chiunque. |

1. Il processo crea un elemento **Progetto locale** visualizzato nella finestra di esplorazione, sotto il quale è presente una raccolta denominata **Functions**, che contiene la funzione con il nome indicato (in questo caso **HttpExample**):

    ![Visualizzazione della finestra di esplorazione di un nuovo progetto Python in Funzioni di Azure](media/tutorial-vs-code-serverless-python/explorer-view-new-python-project-in-azure-functions.png)

    Se Visual Studio Code indica che non è stato selezionato un interprete Python all'apertura del file *\_\_init\_\_.py*, aprire il riquadro comandi (**F1**), selezionare il comando **Python: Select Interpreter** (Python: Seleziona interprete) e quindi selezionare l'ambiente virtuale nella cartella `.venv` locale (creata come parte del progetto).

    ![Selezionare l'ambiente virtuale creato con il progetto Python](media/tutorial-vs-code-serverless-python/select-virtual-environment-created-with-the-python-project.png)

1. Viene anche aperto l'editor di VS Code con il file *\_\_init\_\_.py* che contiene il codice predefinito della funzione. Nella parte superiore dell'editor notare il controllo **HttpExample > __init.py__**:

    ![Controllo di selezione file per una funzione in VS Code](media/tutorial-vs-code-serverless-python/file-selector-in-azure-functions-editor-01.png)

    Se si seleziona **__init.py__**, VS Code visualizza tutti i file che compongono la funzione: il codice in *\_\_init\_\_.py*, la configurazione in *function.json* e i dati di esempio in *sample.dat*. È possibile selezionare qualsiasi file per spostarsi da uno all'altro:

    ![Controllo di selezione file per i file di codice della funzione](media/tutorial-vs-code-serverless-python/file-selector-in-azure-functions-editor-02.png)

    Analogamente, se si seleziona **HttpExample**, VS Code visualizza tutti i file e le cartelle a livello di *progetto*:

    ![Controllo di selezione progetti per una funzione in VS Code](media/tutorial-vs-code-serverless-python/file-selector-in-azure-functions-editor-03.png)

> [!TIP]
> Quando si vuole creare un'altra funzione nello stesso progetto, usare il comando **Create Function** (Crea funzione) nell'area **Azure: Functions** (Azure: Funzioni) oppure aprire il riquadro comandi (**F1**) e selezionare il comando **Azure Functions: Creare il comando Funzione** . Entrambi i comandi chiedono di specificare un nome di funzione (che corrisponde al nome dell'endpoint), quindi creano un'altra sottocartella della funzione nel progetto, con specifici file *\_\_init\_\_.py*, *function.json* e *sample.dat*.
>
> ![Creare funzioni usando Create Function nell'area Azure: Functions](media/tutorial-vs-code-serverless-python/create-new-functions-in-azure-functions-explorer.png)

> [!div class="nextstepaction"]
> [La funzione è stata creata: procedere con il passaggio 3 >>>](tutorial-vs-code-serverless-python-03.md)

[Problemi? Segnalarli](https://aka.ms/python-functions-qs-ms-survey).
