---
title: 'Procedura dettagliata, parte 3: Autenticare le app Python con i servizi di Azure'
description: Esame dell'implementazione dell'API di terze parti di esempio con Funzioni di Azure e come l'endpoint viene protetto con una chiave di accesso.
ms.date: 08/24/2020
ms.topic: conceptual
ms.custom: devx-track-python
ms.openlocfilehash: 7c0098988265fef5b6b0f5e4a654f54c9bed4594
ms.sourcegitcommit: 29b161c450479e5d264473482d31e8d3bf29c7c0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/06/2020
ms.locfileid: "91764504"
---
# <a name="part-3-example-third-party-api-implementation"></a>Parte 3: Implementazione dell'API di terze parti di esempio

[Parte precedente: Problemi di autenticazione](walkthrough-tutorial-authentication-02.md)

Nello scenario di esempio, l'endpoint pubblico dell'app principale usa un'API di terze parti protetta da una chiave di accesso. Questa sezione illustra un'implementazione dell'API di terze parti con Funzioni di Azure, ma l'API può essere implementata in altri modi e distribuita in un server cloud o in un host Web diverso. L'unico aspetto importante è che l'endpoint sia protetto da una chiave di accesso specifica che deve essere inclusa in ogni richiesta client. Qualsiasi app che richiama questa API, quindi, deve gestire in modo sicuro tale chiave.

A scopo dimostrativo, questa API viene distribuita all'endpoint `https://msdocs-api-example.azurewebsites.net/api/RandomNumber`. Per chiamare l'API, tuttavia, è necessario specificare la chiave di accesso `d0c5atM1cr0s0ft` in un parametro URL `?code=` o in una proprietà `'x-functions-key'` dell'intestazione HTTP. Ad esempio, provare questo URL in un browser o curl: [https://msdocs-api-example.azurewebsites.net/api/RandomNumber?code=d0c5atM1cr0s0ft](https://msdocs-api-example.azurewebsites.net/api/RandomNumber?code=d0c5atM1cr0s0ft).

Se la chiave di accesso è valida, l'endpoint restituisce una risposta JSON che contiene una singola proprietà, ovvero "value", il cui valore è un numero compreso tra 1 e 999, ad esempio `{"value": 959}`.

L'endpoint viene implementato in Python e distribuito in Funzioni di Azure. Il codice è indicato di seguito:

```python
import logging
import random
import json

import azure.functions as func

def main(req: func.HttpRequest) -> func.HttpResponse:
    logging.info('RandomNumber invoked via HTTP trigger.')

    random_value = random.randint(1, 1000)
    dict = { "value" : random_value }
    return func.HttpResponse(json.dumps(dict))
```

Nel repository di esempio, questo codice si trova in *third_party_api/RandomNumber/\_\_init\_\_.py*. La cartella, *RandomNumber*, fornisce il nome della funzione e *\_\_init\_\_.py* contiene il codice. Un altro file in tale cartella, *function.json*, descrive quando viene attivata la funzione. Altri file nella cartella padre *third_party_api* forniscono i dettagli per l'"app" Funzioni di Azure che ospita la funzione stessa.

Per distribuire il codice, lo script di provisioning dell'esempio esegue le operazioni seguenti:

1. Creare un account di archiviazione di supporto per Funzioni di Azure con il comando dell'interfaccia della riga di comando di Azure [`az storage account create`](/cli/azure/storage/account#az-storage-account-create).

1. Creare un'"app" Funzioni di Azure con il comando dell'interfaccia della riga di comando di Azure [`az function app create`](/cli/azure/functionapp#az-functionapp-create).

1. Dopo aver atteso 60 secondi per il completamento del provisioning dell'host, distribuire il codice usando il comando di [Azure Functions Core Tools](/azure/azure-functions/functions-run-local?tabs=linux%2Ccsharp%2Cbash) [`func azure functionapp publish`](/azure/azure-functions/functions-run-local?tabs=linux%2Ccsharp%2Cbash#project-file-deployment).

1. Assegnare la chiave di accesso `d0c5atM1cr0s0ft` alla funzione. Vedere [Protezione di Funzioni di Azure](/azure/azure-functions/security-concepts) per informazioni generali sui tasti funzione.

    Nello script di provisioning questo passaggio viene eseguito tramite una chiamata API REST all'[API di gestione delle chiavi di funzioni](https://github.com/Azure/azure-functions-host/wiki/Key-management-API) perché l'interfaccia della riga di comando di Azure attualmente non supporta questa particolare funzionalità. Per chiamare l'API REST, lo script di provisioning deve prima di tutto usare un'altra chiamata API REST per recuperare la chiave master dell'app per le funzioni.

    È anche possibile assegnare chiavi di accesso tramite il [portale di Azure](https://portal.azure.com). Nella pagina dell'app Funzioni selezionare **Funzioni**, quindi selezionare la funzione specifica da proteggere (denominata `RandomNumber` in questo esempio). Nella pagina della funzione selezionare **Tasti funzione** per aprire la pagina in cui è possibile creare e gestire le chiavi.

> [!div class="nextstepaction"]
> [Parte 4: Esempio di implementazione dell'app principale >>>](walkthrough-tutorial-authentication-04.md)
