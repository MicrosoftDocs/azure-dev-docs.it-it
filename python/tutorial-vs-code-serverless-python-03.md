---
title: 'Esercitazione: Esaminare i file di codice Python per Funzioni di Azure in Visual Studio Code'
description: Passaggio 3 dell'esercitazione, informazioni sul codice Python del modello fornito da Funzioni di Azure.
services: functions
author: kraigb
manager: barbkess
ms.service: azure-functions
ms.topic: conceptual
ms.date: 09/02/2019
ms.author: kraigb
ms.custom: seo-python-october2019
ms.openlocfilehash: c09323e35c20a0b9fb5162c08f7fa223969d83fe
ms.sourcegitcommit: bed07b313eeab51281d1a6d4eba67a75524b2f57
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/09/2019
ms.locfileid: "72172114"
---
# <a name="tutorial-examine-the-python-code-files-in-visual-studio-code"></a>Esercitazione: Esaminare i file di codice Python in Visual Studio Code

[Passaggio precedente: Creare la funzione](tutorial-vs-code-serverless-python-02.md)

Nella sottocartella della funzione appena creata sono presenti tre file: *\_\_init\_\_.py*, che contiene il codice della funzione, *function.json*, che descrive la funzione a Funzioni di Azure, e *sample.dat* che è un file di dati di esempio. Se si preferisce, è possibile eliminare *sample.dat*, perché è disponibile solo per mostrare che è possibile aggiungere altri file alla sottocartella.

Esaminiamo prima di tutto *function.json*, quindi il codice in *\_\_init\_\_.py*.

## <a name="functionjson"></a>function.json

Il file function.json fornisce le informazioni di configurazione necessarie per l'endpoint di Funzioni di Azure:

```json
{
  "scriptFile": "__init__.py",
  "bindings": [
    {
      "authLevel": "anonymous",
      "type": "httpTrigger",
      "direction": "in",
      "name": "req",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "type": "http",
      "direction": "out",
      "name": "$return"
    }
  ]
}
```

La proprietà `scriptFile` identifica il file di avvio per il codice e il codice deve contenere una funzione Python denominata `main`. È possibile fattorizzare il codice in più file, purché il file specificato contenga una funzione `main`.

L'elemento `bindings` contiene due oggetti, uno per descrivere le richieste in ingresso e l'altro per descrivere la risposta HTTP. Per le richieste in ingresso (`"direction": "in"`), la funzione risponde alle richieste HTTP GET o POST e non richiede l'autenticazione. La risposta (`"direction": "out"`) è una risposta HTTP che restituisce qualsiasi valore restituito dalla funzione Python `main`.

## <a name="__initpy__"></a>\_\_init.py\_\_

Quando si crea una nuova funzione, Funzioni di Azure fornisce codice Python predefinito in *\_\_init\_\_.py*:

```python
import logging

import azure.functions as func


def main(req: func.HttpRequest) -> func.HttpResponse:
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
        return func.HttpResponse(f"Hello {name}!")
    else:
        return func.HttpResponse(
             "Please pass a name on the query string or in the request body",
             status_code=400
        )
```

Di seguito sono riportate le parti importanti del codice:

- È necessario importare `func` da `azure.functions`. L'importazione del modulo di registrazione è facoltativa ma consigliata.
- La funzione Python `main` necessaria riceve un oggetto `func.HttpRequest` denominato `req` e restituisce un valore di tipo `func.HttpResponse`. È possibile ottenere altre informazioni sulle funzionalità di questi oggetti nella documentazione di riferimento per [func.HttpRequest](/python/api/azure-functions/azure.functions.httprequest?view=azure-python) e [func.HttpResponse](/python/api/azure-functions/azure.functions.httpresponse?view=azure-python).
- Il corpo di `main` elabora quindi la richiesta e genera una risposta. In questo caso, il codice cerca un parametro `name` nell'URL. Se non lo trova, verifica se il corpo della richiesta contiene codice JSON (usando `func.HttpRequest.get_json`) e se il codice JSON contiene un valore `name` (usando il metodo `get` dell'oggetto JSON restituito da `get_json`).
- Se il nome viene trovato, il codice restituisce la stringa "Hello" con il nome aggiunto alla fine; in caso contrario, restituisce un messaggio di errore.

> [!div class="nextstepaction"]
> [I file di codice sono stati esaminati](tutorial-vs-code-serverless-python-04.md)

[Si è verificato un problema](https://www.research.net/r/PWZWZ52?tutorial=vscode-functions-python&step=03-examine-code-files)
