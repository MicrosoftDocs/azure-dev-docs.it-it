---
title: 'Procedura dettagliata, parte 5: Autenticare le app Python con i servizi di Azure'
description: Informazioni sulle dipendenze dell'app principale (prevalentemente librerie Azure SDK), le istruzioni import necessarie e le variabili di ambiente che devono essere impostate.
ms.date: 08/24/2020
ms.topic: conceptual
ms.custom: devx-track-python
ms.openlocfilehash: 666ddd3222e724c316c6cf975bbb1e292f4525cd
ms.sourcegitcommit: 075f39972e390e79ed09a3fcfdbfc776727e08fc
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/06/2021
ms.locfileid: "97952463"
---
# <a name="part-5-main-app-dependencies-import-statements-and-environment-variables"></a>Parte 5: Dipendenze dell'app principale, istruzioni import e variabili di ambiente

[Parte precedente: Esempio di implementazione dell'app principale](walkthrough-tutorial-authentication-04.md)

Questa parte esamina le librerie di Python introdotte nell'app principale e le variabili di ambiente richieste dal codice. Dopo la distribuzione in Azure, le impostazioni dell'applicazione vengono usate nel servizio app di Azure per fornire le variabili di ambiente.

## <a name="dependencies-and-import-statements"></a>Dipendenze e istruzioni import

Il codice dell'app richiede diverse librerie: Flask, la libreria di richieste HTTP standard, e le librerie di Azure per Active Directory ([azure.identity](/python/api/overview/azure/identity-readme)), Key Vault ([azure.keyvault.secrets](/python/api/overview/azure/keyvault-secrets-readme)) e archiviazione code ([azure.storage.queue](/python/api/overview/azure/storage-queue-readme)). Queste librerie sono incluse nel file *requirements.txt* dell'app:

```txt
flask
requests
azure.identity
azure.keyvault.secrets
azure.storage.queue
```

Quando l'app viene distribuita nel servizio app di Azure, Azure installa automaticamente questi requisiti nel server host. Per l'esecuzione in locale, vengono installati nell'ambiente con `pip install -r requirements.txt`.

Nella parte superiore del codice, quindi, sono riportate le istruzioni import necessarie per le parti delle librerie che vengono usate:

```python
from flask import Flask, request, jsonify
import requests, random, string, os
from datetime import datetime
from azure.identity import DefaultAzureCredential
from azure.keyvault.secrets import SecretClient
from azure.storage.queue import QueueClient
```

## <a name="environment-variables"></a>Variabili di ambiente

Il codice dell'app dipende da quattro variabili di ambiente:

| Variabile | valore |
| --- | --- |
| THIRD_PARTY_API_ENDPOINT | L'URL dell'API di terze parti, ad esempio `https://msdocs-example-api.azurewebsites.net/api/RandomNumber` descritto nella [Parte 3](walkthrough-tutorial-authentication-03.md). |
| KEY_VAULT_URL | L'URL dell'istanza di Azure Key Vault in cui è stata archiviata la chiave di accesso per l'API di terze parti. |
| THIRD_PARTY_API_SECRET_NAME | Il nome del segreto in Key Vault che contiene la chiave di accesso per l'API di terze parti. |
| STORAGE_QUEUE_URL | L'URL di una coda di archiviazione di Azure configurata in Azure, ad esempio `https://msdocsmainappexample.queue.core.windows.net/code-requests` (vedere la [parte 4](walkthrough-tutorial-authentication-04.md)). Poiché il nome della coda è incluso alla fine dell'URL, non viene visualizzato in nessun punto del codice. |

Per l'esecuzione in locale, queste variabili vengono create all'interno della shell dei comandi in uso. Se l'app viene distribuita in una macchina virtuale, verranno create variabili simili lato server.

Per la distribuzione nel servizio app di Azure, tuttavia, non si ha accesso al server stesso. In questo caso, creare *impostazioni dell'applicazione* con gli stessi nomi, che vengono quindi riconosciute dall'app come variabili di ambiente. 

Gli script di provisioning creano queste impostazioni usando il comando [`az webapp config appsettings set`](/cli/azure/webapp/config/appsettings#az-webapp-config-appsettings-set) dell'interfaccia della riga di comando di Azure. Tutte e quattro le variabili vengono impostate con un unico comando.

Per creare le impostazioni tramite il portale di Azure, vedere [Configurare un'app del servizio app nel portale di Azure](/azure/app-service/configure-common).

> [!div class="nextstepaction"]
> [Parte 6: Codice di avvio dell'app principale >>>](walkthrough-tutorial-authentication-06.md)
