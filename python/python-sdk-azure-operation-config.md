---
title: Parametri per la configurazione delle operazioni - Azure SDK pe Python
description: Codice C generato da Azure SDK per Python
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 03/07/2018
ms.topic: conceptual
ms.devlang: python
ms.custom: seo-python-october2019
ms.openlocfilehash: 0730cec8470a3b55421c6c0cafa08f88819cb1d8
ms.sourcegitcommit: 6012460ad8d6ff112226b8f9ea6da397ef77712d
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/11/2019
ms.locfileid: "72279083"
---
# <a name="parameters-for-operation-configuration"></a>Parametri per la configurazione delle operazioni

È possibile specificare parametri aggiuntivi per i metodi sulle operazioni in Azure SDK per Python.

I parametri aggiuntivi vengono forniti in `kwargs`. Questa funzionalità è denominata *operation_config*.

Le opzioni per la configurazione delle operazioni sono le seguenti:

|Nome parametro|Type|Ruolo|
|----------------------|------|---------------|
| verify |`bool`|Indica se verificare il certificato SSL. Il valore predefinito è true.|
|  cert |`str`| Percorso del certificato locale per la verifica lato client.|
|  timeout |`int`| Timeout in secondi per stabilire una connessione al server.|
|  allow_redirects |`bool` | Indica se consentire i reindirizzamenti.|
|  max_redirects  |`int`| Numero massimo di reindirizzamenti consentiti.|
|  proxies  |`dict` |Impostazioni del server proxy.|
|  use_env_proxies |`bool` |Indica se leggere le impostazioni del proxy dalle variabili di ambiente locali.|
|  retries  |`int` | Numero totale di tentativi.|
|  enable_http_logger | `bool`| Abilita i log di HTTP in modalità di debug (False per impostazione predefinita).|
