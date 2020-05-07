---
title: Parametri per la configurazione delle operazioni - Azure SDK pe Python
description: Codice C generato da Azure SDK per Python
ms.date: 03/07/2018
ms.topic: conceptual
ms.custom: seo-python-october2019
ms.openlocfilehash: b0aaaf5bcd51bce42ab38830d5dbce508db226b1
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/05/2020
ms.locfileid: "80441696"
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
