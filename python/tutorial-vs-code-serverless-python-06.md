---
title: 'Esercitazione: Aggiungere una seconda funzione Python a Funzioni di Azure con Visual Studio Code'
description: "Passaggio 6 dell'esercitazione: espansione di un progetto di Funzioni di Azure mediante l'aggiunta di una seconda funzione."
services: functions
author: kraigb
manager: barbkess
ms.service: azure-functions
ms.topic: conceptual
ms.date: 09/02/2019
ms.author: kraigb
ms.custom: seo-python-october2019
ms.openlocfilehash: 07f806dc58eb5aede65f0dca67e1bc59ce495a25
ms.sourcegitcommit: bed07b313eeab51281d1a6d4eba67a75524b2f57
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/09/2019
ms.locfileid: "72172428"
---
# <a name="tutorial-add-a-second-python-function-to-azure-functions"></a>Esercitazione: Aggiungere una seconda funzione Python a Funzioni di Azure

[Passaggio precedente: Distribuire in Azure](tutorial-vs-code-serverless-python-05.md)

Dopo la prima distribuzione, è possibile apportare modifiche al codice, ad esempio aggiungendo altre funzioni, e ridistribuirle nella stessa app per le funzioni.

1. Nell'area **Azure: Functions** (Azure: Funzioni) selezionare il comando **Create Function** (Crea funzione) oppure usare **Azure Functions: Create Function** (Funzioni di Azure: Crea funzione) nel riquadro comandi. Specificare i dettagli seguenti per la funzione:

    - Template: Trigger HTTP
    - Nome: "DigitsOfPi"
    - Authorization level (Livello di autorizzazione): Anonima

1. In Esplora risorse di Visual Studio Code è presente una sottocartella il cui nome corrisponde a quello della funzione e che ancora una colta contiene i file denominati *\_\_init\_\_.py*, *function.json* e *sample.dat*.

1. Sostituire il contenuto di *\_\_init\_\_.py* in modo che corrisponda al codice seguente, che genera una stringa contenente il valore di PI fino a un numero di cifre specificato nell'URL (questo codice usa solo un parametro URL).

    ```python
    import logging

    import azure.functions as func

    """ Adapted from the second, shorter solution at http://www.codecodex.com/wiki/Calculate_digits_of_pi#Python
    """

    def pi_digits_Python(digits):
        scale = 10000
        maxarr = int((digits / 4) * 14)
        arrinit = 2000
        carry = 0
        arr = [arrinit] * (maxarr + 1)
        output = ""

        for i in range(maxarr, 1, -14):
            total = 0
            for j in range(i, 0, -1):
                total = (total * j) + (scale * arr[j])
                arr[j] = total % ((j * 2) - 1)
                total = total / ((j * 2) - 1)

            output += "%04d" % (carry + (total / scale))
            carry = total % scale

        return output;

    def main(req: func.HttpRequest) -> func.HttpResponse:
        logging.info('DigitsOfPi HTTP trigger function processed a request.')

        digits_param = req.params.get('digits')

        if digits_param is not None:
            try:
                digits = int(digits_param)
            except ValueError:
                digits = 10   # A default

            if digits > 0:
                digit_string = pi_digits_Python(digits)

                # Insert a decimal point in the return value
                return func.HttpResponse(digit_string[:1] + '.' + digit_string[1:])

        return func.HttpResponse(
             "Please pass the URL parameter ?digits= to specify a positive number of digits.",
             status_code=400
        )
    ```

1. Poiché il codice supporta solo HTTP GET, modificare *function.JSON* in modo che la raccolta `"methods"` contenga solo `"get"` (ovvero rimuovere `"post"`). Il file completo dovrebbe essere simile al seguente:

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
            "get"
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

1. Avviare il debugger premendo F5 o selezionando il comando di menu **Debug** > **Avvia debug**. La finestra **Output** dovrebbe ora visualizzare entrambi gli endpoint del progetto:

    ```output
    Http Functions:

            DigitsOfPi: [GET] http://localhost:7071/api/DigitsOfPi

            HttpExample: [GET,POST] http://localhost:7071/api/HttpExample
    ```

1. In un browser o da cURL effettuare una richiesta a `http://localhost:7071/api/DigitsOfPi?digits=125` e osservare l'output. È possibile notare che l'algoritmo del codice non è del tutto accurato, ma è comunque migliorabile dall'utente. Al termine, arrestare il debugger.

1. Ridistribuire il codice usando l'opzione **Deploy to Function App** (Distribuisci nell'app per le funzioni) nell'area **Azure: Functions** (Azure: Funzioni). Se richiesto, selezionare l'app per le funzioni creata in precedenza.

1. Al termine della distribuzione, che richiede qualche minuto, nella finestra **Output** vengono visualizzati gli endpoint pubblici con cui è possibile ripetere i test.

> [!div class="nextstepaction"]
> [È stata aggiunta una seconda funzione](tutorial-vs-code-serverless-python-07.md)

[Si è verificato un problema](https://www.research.net/r/PWZWZ52?tutorial=vscode-functions-python&step=06-second-function)
