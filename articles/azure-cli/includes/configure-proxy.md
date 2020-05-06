---
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 05/28/2019
ms.topic: include
ms.prod: azure
ms.technology: azure-cli
ms.openlocfilehash: 676f33377a4e7122941bc789c51465b7f34aa1d3
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/05/2020
ms.locfileid: "82030883"
---
Se non è possibile connettersi a una risorsa esterna a causa di un proxy, assicurarsi di aver impostato correttamente le variabili `HTTP_PROXY` e `HTTPS_PROXY` nella shell. Per sapere quali host e porte usare per questi proxy, è necessario contattare l'amministratore di sistema.

Questi valori vengono rispettati da numerosi programmi Linux, inclusi quelli usati nel processo di installazione. Per impostare questi valori:

```bash
# No auth
export HTTP_PROXY=http://[proxy]:[port]
export HTTPS_PROXY=https://[proxy]:[port]

# Basic auth
export HTTP_PROXY=http://[username]:[password]@[proxy]:[port]
export HTTPS_PROXY=https://[username]:[password]@[proxy]:[port]
```

> [!IMPORTANT]
> Se si è protetti da un proxy, è necessario impostare queste variabili della shell per connettersi ai servizi di Azure con l'interfaccia della riga di comando.
> Se non si usa l'autenticazione di base, è consigliabile esportare queste variabili nel file `.bashrc`.
> Seguire sempre i criteri di sicurezza aziendale e i requisiti definiti dall'amministratore di sistema.
