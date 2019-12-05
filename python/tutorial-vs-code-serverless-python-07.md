---
title: 'Esercitazione: Aggiungere un binding di archiviazione per Funzioni di Azure in Python con Visual Studio Code'
description: Passaggio 7 dell'esercitazione, aggiunta di un binding in Python per scrivere messaggi nell'archiviazione di Azure.
ms.topic: conceptual
ms.date: 09/02/2019
ms.custom: seo-python-october2019
ms.openlocfilehash: 8f949b0673ed39f51b3a14ffd9a6dd572176dcba
ms.sourcegitcommit: e77f8f652128b798dbf972078a7b460ed21fb5f8
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/25/2019
ms.locfileid: "74466825"
---
# <a name="tutorial-add-a-storage-binding-for-azure-functions-in-python"></a>Esercitazione: Aggiungere un binding di archiviazione per Funzioni di Azure in Python

[Passaggio precedente: Distribuire una seconda funzione](tutorial-vs-code-serverless-python-06.md)

È possibile aggiungere un binding di archiviazione per Funzioni di Azure. Un _binding_ consente di collegare il codice della funzione alle risorse, ad esempio archiviazione di Azure, senza scrivere codice di accesso ai dati.

Il binding viene definito nel file *function.json* e può rappresentare sia l'input che l'output. Una funzione può usare più binding di input e output, ma un solo trigger. Per altre informazioni, vedere [Concetti su trigger e binding di Funzioni di Azure](/azure/azure-functions/functions-triggers-bindings).

In questa sezione viene aggiunto un binding di archiviazione alla funzione HttpExample creata in precedenza in questa esercitazione. La funzione usa questo binding per scrivere messaggi nell'archiviazione a ogni richiesta. L'archiviazione in questione usa lo stesso account di archiviazione predefinito usato dall'app per le funzioni. Se si prevede di usare l'archiviazione in modo intensivo, tuttavia, è consigliabile creare un account separato.

1. Sincronizzare le impostazioni remote per il progetto Funzioni di Azure nel file *local.settings.json* aprendo il riquadro comandi e selezionando **Funzioni di Azure: Download Remote Settings** (Scarica impostazioni remote). Aprire il file *local.settings.json* e verificare che contenga un valore per `AzureWebJobsStorage`. Questo valore è la stringa di connessione per l'account di archiviazione.

1. Nella cartella `HttpExample` fare clic con il pulsante destro del mouse sul file *function.json* e scegliere **Aggiungi binding**:

    ![Comando Aggiungi binding in Esplora risorse di Visual Studio Code](media/tutorial-vs-code-serverless-python/add-binding-command-to-azure-functions-in-visual-studio-code.png)

1. Nei prompt che seguono in Visual Studio Code selezionare o specificare i valori seguenti:

    | Prompt | Valore da specificare |
    | --- | --- |
    | Impostare la direzione di binding | fo |
    | Selezionare il binding con direzione in uscita | Archiviazione code di Azure |
    | Il nome usato per identificare questo binding nel codice | msg |
    | La coda a cui verrà inviato il messaggio | outqueue |
    | Selezionare l'impostazione da *local.settings.json* (richiesta della connessione all'archiviazione) | AzureWebJobsStorage |

1. Dopo aver effettuato queste selezioni, verificare che il binding seguente venga aggiunto al file *function.json*:

    ```json
        {
          "type": "queue",
          "direction": "out",
          "name": "msg",
          "queueName": "outqueue",
          "connection": "AzureWebJobsStorage"
        }
    ```

1. Una volta configurato il binding, è possibile usarlo nel codice della funzione. Anche in questo caso, il binding appena definito viene visualizzato nel codice come argomento della funzione `main` in *\_\_init\_\_.py*. Ad esempio, è possibile modificare il file *\_\_init\_\_.py* in HttpExample in modo che corrisponda a quanto segue, che mostra l'uso dell'argomento `msg` per scrivere un messaggio con timestamp con il nome usato nella richiesta. I commenti illustrano le modifiche specifiche:

    ```python
    import logging
    import datetime  # MODIFICATION: added import
    import azure.functions as func

    # MODIFICATION: the added binding appears as an argument; func.Out[func.QueueMessage]
    # is the appropriate type for an output binding with "type": "queue" (in function.json).
    def main(req: func.HttpRequest, msg: func.Out[func.QueueMessage]) -> func.HttpResponse:
        logging.info('Python HTTP trigger function processed a request.')

        name = req.params.get('name')
        if not name:
            try:
                req_body = req.get_json()
            except ValueError:
                pass
            else:
                name = req_body.get('name')

        if name:
            # MODIFICATION: write the a message to the message queue, using msg.set
            msg.set(f"Request made for {name} at {datetime.datetime.now()}")

            return func.HttpResponse(f"Hello {name}!")
        else:
            return func.HttpResponse(
                 "Please pass a name on the query string or in the request body",
                 status_code=400
            )
    ```

1. Per testare queste modifiche in locale, avviare di nuovo il debugger in Visual Studio Code premendo F5 o selezionando il comando di menu **Debug** > **Avvia debug**. Come prima, la finestra **Output** dovrebbe mostrare gli endpoint del progetto.

1. In un browser visitare l'URL `http://localhost:7071/api/HttpExample?name=VS%20Code` per creare una richiesta all'endpoint HttpExample, che dovrebbe anche scrivere un messaggio nella coda.

1. Per verificare che il messaggio sia stato scritto nella coda "outqueue" (come indicato nel binding), è possibile usare uno dei tre metodi seguenti:

    1. Accedere al [portale di Azure](https://portal.azure.com)e passare al gruppo di risorse contenente il progetto Funzioni. All'interno di tale gruppo di risorse individuare e passare all'account di archiviazione per il progetto, quindi passare a **Code**. In questa pagina passare a "outqueue", che dovrebbe visualizzare tutti i messaggi registrati.

    1. Esplorare ed esaminare la coda con Azure Storage Explorer, che si integra con Visual Studio, come descritto in [Connettere funzioni ad Archiviazione di Azure con Visual Studio Code](/azure/azure-functions/functions-add-output-binding-storage-queue-vs-code), in particolare nella sezione [Esaminare la coda di output](/azure/azure-functions/functions-add-output-binding-storage-queue-vs-code#examine-the-output-queue).

    1. Usare l'interfaccia della riga di comando di Azure per eseguire una query sulla coda di archiviazione, come descritto in [Eseguire una query sulla coda di archiviazione](/azure/azure-functions/functions-add-output-binding-storage-queue-python#query-the-storage-queue).

1. Per eseguire il test nel cloud, ridistribuire il codice usando l'opzione **Deploy to Function App** (Distribuisci nell'app per le funzioni) nell'area **Azure: Funzioni**. Se richiesto, selezionare l'app per le funzioni creata in precedenza. Al termine della distribuzione, che richiede qualche minuto, la finestra **Output** mostra di nuovo gli endpoint pubblici con cui è possibile ripetere i test.

> [!div class="nextstepaction"]
> [Il binding di archiviazione è stato aggiunto](tutorial-vs-code-serverless-python-08.md)

[Si è verificato un problema](https://www.research.net/r/PWZWZ52?tutorial=python-functions-extension&step=07-storage-binding)
