---
title: 'Passaggio 6: Aggiungere una seconda funzione Python serverless a Funzioni di Azure con VS Code'
description: Passaggio 6 dell'esercitazione, espansione di un progetto di Funzioni di Azure con l'aggiunta di una seconda funzione serverless.
ms.topic: conceptual
ms.date: 11/30/2020
ms.custom: devx-track-python, seo-python-october2019, contperf-fy21q2
ms.openlocfilehash: 41c97fecd590417d406b94124319348a03d6db7f
ms.sourcegitcommit: 4f9ce09cbf9663203c56f5b12ecbf70ea68090ed
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/06/2021
ms.locfileid: "97911361"
---
# <a name="6-add-a-second-python-function-to-azure-functions"></a>6: Aggiungere una seconda funzione Python a Funzioni di Azure

[Passaggio precedente: Distribuire in Azure](tutorial-vs-code-serverless-python-05.md)

Dopo la prima distribuzione, è possibile apportare modifiche al codice, ad esempio aggiungendo altre funzioni Python, e quindi ridistribuirle nella stessa app per le funzioni di Azure.

1. Nell'area **Azure: Functions** (Azure: Funzioni) selezionare il comando **Create Function** (Crea funzione) oppure usare **Azure Functions: Create Function** (Funzioni di Azure: Crea funzione) nel riquadro comandi. Specificare i dettagli seguenti per la funzione:

    - Template: Trigger HTTP
    - Nome: "DigitsOfPi"
    - Authorization level (Livello di autorizzazione): Anonima

    La sezione **Progetto locale** nella finestra di esplorazione di Funzioni di Azure ora visualizza una funzione *DigitsOfPi*. Nell'editor è possibile passare tra i file *\_\_init\_\_.py*, *function.json* e *sample.dat* della funzione.

1. Sostituire il contenuto di *\_\_init\_\_.py* in modo che corrisponda al codice seguente, che genera una stringa contenente il valore di PI fino a un numero di cifre specificato nell'URL (questo codice usa solo un parametro URL):

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

        return output

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

1. Avviare il debugger premendo F5 o selezionando il comando di menu **Esegui** > **Avvia debug**. La finestra **Output** dovrebbe ora visualizzare entrambi gli endpoint del progetto:

    <pre>
    Http Functions:
            DigitsOfPi: [GET] http://localhost:7071/api/DigitsOfPi
            HttpExample: [GET,POST] http://localhost:7071/api/HttpExample
    </pre>

1. In un browser o da cURL effettuare una richiesta a `http://localhost:7071/api/DigitsOfPi?digits=125` e osservare l'output. È possibile notare che l'algoritmo del codice non è del tutto accurato, ma è comunque migliorabile dall'utente. Al termine, arrestare il debugger.

1. Ridistribuire il codice usando l'opzione **Deploy to Function App** (Distribuisci nell'app per le funzioni) nell'area **Azure: Functions** (Azure: Funzioni). Se richiesto, selezionare l'app per le funzioni creata in precedenza.

1. Dopo alcuni minuti la distribuzione viene completata e la finestra **Output** mostra gli endpoint pubblici con cui è possibile ripetere i test.

> [!div class="nextstepaction"]
> [È stata aggiunta una seconda funzione: procedere con il passaggio 7 >>>](tutorial-vs-code-serverless-python-07.md)

[Problemi? Segnalarli](https://aka.ms/python-functions-qs-ms-survey).
