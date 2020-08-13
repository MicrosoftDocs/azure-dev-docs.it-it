---
title: Configurazione dei proxy per le librerie di Azure
description: Usare le variabili di ambiente HTTP[S]_PROXY per definire un proxy per un intero script o un'app oppure usare argomenti denominati facoltativi per i costruttori client o i metodi di operazioni.
ms.date: 06/16/2020
ms.topic: conceptual
ms.custom: devx-track-python
ms.openlocfilehash: 86eaa5b8dc0ddfb529643a5c015938344582e62b
ms.sourcegitcommit: 980efe813d1f86e7e00929a0a3e1de83514ad7eb
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/07/2020
ms.locfileid: "87982703"
---
# <a name="how-to-configure-proxies-for-the-azure-libraries"></a>Come configurare i proxy per le librerie di Azure

Il formato dell'URL di un server proxy è `http[s]://[username:password@]<ip_address_or_domain>:<port>/`, dove la combinazione nome utente:password è facoltativa.

È quindi possibile configurare un proxy a livello globale usando le variabili di ambiente oppure si può specificare un proxy passando un argomento denominato `proxies` a un singolo costruttore client o a un metodo di operazione.

## <a name="global-configuration"></a>Configurazione globale

Per configurare un proxy a livello globale per lo script o l'app, definire la variabile di ambiente `HTTP_PROXY` o `HTTPS_PROXY` con l'URL del server. Queste variabili sono supportate in qualsiasi versione delle librerie di Azure.

Queste variabili di ambiente vengono ignorate se si passa il parametro `use_env_settings=False` a un costruttore di oggetti client o a un metodo di operazione.

### <a name="from-python-code"></a>Dal codice Python

```python
import os
os.environ["HTTP_PROXY"] = "http://10.10.1.10:1180"

# Alternate URL and variable forms:
# os.environ["HTTP_PROXY"] = "http://username:password@10.10.1.10:1180"
# os.environ["HTTPS_PROXY"] = "http://10.10.1.10:1180"
# os.environ["HTTPS_PROXY"] = "http://username:password@10.10.1.10:1180"
```

### <a name="from-the-cli"></a>Dall'interfaccia della riga di comando

# <a name="cmd"></a>[cmd](#tab/cmd)

```cmd
rem Non-authenticated HTTP server:
set HTTP_PROXY=http://10.10.1.10:1180

rem Authenticated HTTP server:
set HTTP_PROXY=http://username:password@10.10.1.10:1180

rem Non-authenticated HTTPS server:
set HTTPS_PROXY=http://10.10.1.10:1180

rem Authenticated HTTPS server:
set HTTPS_PROXY=http://username:password@10.10.1.10:1180
```

# <a name="bash"></a>[Bash](#tab/bash)

```bash
# Non-authenticated HTTP server:
HTTP_PROXY=http://10.10.1.10:1180

# Authenticated HTTP server:
HTTP_PROXY=http://username:password@10.10.1.10:1180

# Non-authenticated HTTPS server:
HTTPS_PROXY=http://10.10.1.10:1180

# Authenticated HTTPS server:
HTTPS_PROXY=http://username:password@10.10.1.10:1180
```

---

## <a name="per-client-or-per-method-configuration"></a>Configurazione per client o per metodo

Per configurare un proxy per un oggetto client o un metodo di operazione specifico, specificare un server proxy con un argomento denominato `proxies`.

Ad esempio, il codice seguente tratto dall'articolo [Esempio: Usare le librerie di Azure con Archiviazione di Azure](azure-sdk-example-storage.md) specifica un proxy HTTPS con le credenziali utente con il costruttore `BlobClient`. In questo caso l'oggetto deriva dalla libreria azure.storage.blob, che è basata su azure.core.

```python
# Earlier code omitted for brevity

blob_client = BlobClient(storage_url, container_name="blob-container-01",
    blob_name="sample-blob.txt", credential=credential,
    proxies={ "https": "https://username:password@10.10.1.10:1180" }
)

# Other forms that the proxy URL might take:
# proxies={ "http": "http://10.10.1.10:1180" }
# proxies={ "http": "http://username:password@10.10.1.10:1180" }
# proxies={ "https": "https://10.10.1.10:1180" }
```
