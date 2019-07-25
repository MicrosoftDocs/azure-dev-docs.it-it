---
title: Configurazione delle operazioni di Azure SDK per Python
description: Codice C generato da Azure SDK per Python
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 03/07/2018
ms.topic: conceptual
ms.devlang: python
ms.openlocfilehash: 9638aa4602f96e2da0155a7b3840e5be4857eb98
ms.sourcegitcommit: 2efdb9d8a8f8a2c1914bd545a8c22ae6fe0f463b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/15/2019
ms.locfileid: "68285512"
---
# <a name="operation-config"></a>Configurazione delle operazioni 

I metodi per le operazioni includono parametri aggiuntivi che possono essere forniti in `kwargs`. Questo approccio è definito operation_config.

Le opzioni per la configurazione delle operazioni sono le seguenti:

|Nome parametro|Type|Ruolo|
|----------------------|------|---------------|
| verify |`bool`|Indica se verificare il certificato SSL. Il valore predefinito è true.|
|  cert |`str`| Percorso per il certificato locale per la verifica lato client.|
|  timeout |`int`| Timeout in secondi per stabilire una connessione al server.|
|  allow_redirects |`bool` | Indica se consentire i reindirizzamenti.|
|  max_redirects  |`int`| Numero massimo di reindirizzamenti consentiti.|
|  proxies  |`dict` |Impostazioni del server proxy.|
|  use_env_proxies |`bool` |Indica se leggere le impostazioni del proxy dalle variabili di ambiente locali.|
|  retries  |`int` | Numero totale di tentativi.|
|  enable_http_logger | `bool`| Abilita i log di HTTP in modalità di debug (False per impostazione predefinita).|
