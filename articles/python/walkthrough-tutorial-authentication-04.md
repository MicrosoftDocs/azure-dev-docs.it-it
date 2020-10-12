---
title: 'Procedura dettagliata, parte 4: Autenticare le app Python con i servizi di Azure'
description: Panoramica dell'implementazione dell'app principale, incluso tutto il codice.
ms.date: 08/24/2020
ms.topic: conceptual
ms.custom: devx-track-python
ms.openlocfilehash: e2a43f7e204ba3f077beea7cc878076111f71313
ms.sourcegitcommit: 29b161c450479e5d264473482d31e8d3bf29c7c0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/06/2020
ms.locfileid: "91764740"
---
# <a name="part-4-example-main-application-implementation"></a>Parte 4: Esempio di implementazione dell'applicazione principale

[Parte precedente: Implementazione dell'API di terze parti di esempio](walkthrough-tutorial-authentication-03.md)

L'app principale in questo scenario è una semplice app Flask distribuita nel Servizio app di Azure. L'app fornisce un endpoint API pubblico denominato */api/v1/getcode*, che genera un codice per altri scopi nell'app (ad indicare, con autenticazione a due fattori per gli utenti umani).

Per visualizzare l'endpoint in azione, visitare [https://msdocs-main-app-example.azurewebsites.net/api/v1/getcode](https://msdocs-main-app-example.azurewebsites.net/api/v1/getcode) in un browser o effettuare una richiesta usando curl.

L'app principale fornisce anche una semplice home page che visualizza un collegamento all'endpoint API. Questa parte dell'app può essere visualizzata in [https://msdocs-main-app-example.azurewebsites.net](https://msdocs-main-app-example.azurewebsites.net).

Lo script di provisioning dell'esempio esegue le operazioni seguenti:

1. Creare l'host di Servizio app e distribuire il codice con il comando dell'interfaccia della riga di comando di Azure [`az webapp up`](/cli/azure/webapp#az-webapp-up).

1. Effettuare il provisioning di un account di Archiviazione di Azure per l'app principale (usando [`az storage account create`](/cli/azure/storage/account#az-storage-account-create)).

1. Creare una coda nell'account di archiviazione denominata "code-requests" (usando [`az storage queue create`](/cli/azure/storage/queue#az-storage-queue-create)).

1. Per assicurarsi che l'app possa scrivere nella coda, usare [`az role assignment create`](/cli/azure/role/assignment#az-role-assignment-create) per assegnare il ruolo "Collaboratore ai dati della coda di archiviazione" all'app. Per altre informazioni sui ruoli, vedere [Come assegnare le autorizzazioni per i ruoli con l'interfaccia della riga di comando di Azure](/azure/role-based-access-control/role-assignments-cli).

Il codice dell'app principale è riportato di seguito. Le spiegazioni di dettagli importanti sono fornite nelle parti successive di questa serie.

```python
from flask import Flask, request, jsonify
import requests, random, string, os
from datetime import datetime
from azure.keyvault.secrets import SecretClient
from azure.identity import DefaultAzureCredential
from azure.storage.queue import QueueClient

app = Flask(__name__)

# Retrieve the URL of the third-party API endpoint we invoke.
number_url = os.environ["THIRD_PARTY_API_ENDPOINT"]

# Authenticate with Azure. First, obtain the credential object. To run locally,
# you must have the necessary environment variables set up to provide the
# local service principal information. When deployed to the cloud, the app must
# have managed identity enabled in Azure App Service.
credential = DefaultAzureCredential()

# Next, get a client object for the Key Vault. The client object is strictly
# a client-side construct provided by the Azure libraries as a layer on top
# of the Azure REST API.
key_vault_url = os.environ["KEY_VAULT_URL"]
keyvault_client = SecretClient(vault_url=key_vault_url, credential=credential)

# Obtain the secret: for this step to work you must add the app's identity to
# the key vault's access policies for secret management. When deployed to the cloud
# the identity is the name of the app in App Service; when running locally, the
# identity is the local service principal.
api_secret_name = os.environ["THIRD_PARTY_API_SECRET_NAME"]
vault_secret = keyvault_client.get_secret(api_secret_name)

# The "secret" from Key Vault is an object with multiple properties. The access key
# we want for the third-party API is in the secret's value property.
access_key = vault_secret.value

#Set up the Storage queue client to which we write messages
queue_url = os.environ["STORAGE_QUEUE_URL"]
queue_client = QueueClient.from_queue_url(queue_url=queue_url, credential=credential)


@app.route('/', methods=['GET'])
def home():
    return f'Home page of the main app. Make a request to <a href="./api/v1/getcode">/api/v1/getcode</a>.'


def random_char(num):
       return ''.join(random.choice(string.ascii_letters) for x in range(num))


@app.route('/api/v1/getcode', methods=['GET'])
def get_code():
    headers = {
        'Content-Type': 'application/json',
        'x-functions-key': access_key
        }

    r = requests.get(url = number_url, headers = headers)

    if (r.status_code != 200):
        return "Could not get you a code.", r.status_code

    data = r.json()
    chars1 = random_char(3)
    chars2 = random_char(3)
    code_value = f"{chars1}-{data['value']}-{chars2}"
    code = { "code": code_value, "timestamp" : str(datetime.utcnow()) }

    # Log a queue message with the code for, say, a process that invalidates
    # the code after a certain period of time.
    queue_client.send_message(code)

    return jsonify(code)


if __name__ == '__main__':
    app.run()
```

> [!div class="nextstepaction"]
> [Parte 5: Dipendenze dell'app principale, importazioni e variabili di ambiente >>>](walkthrough-tutorial-authentication-05.md)
