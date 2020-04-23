---
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 11/26/2018
ms.topic: include
ms.openlocfilehash: ee0b2b0c8c557aa54255f28429bb3a174c0e21a9
ms.sourcegitcommit: 36e02e96b955ed0531f98b9c0f623f4acb508661
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/22/2020
ms.locfileid: "82030893"
---
### <a name="cli-fails-to-install-or-run-on-windows-subsystem-for-linux"></a>Non è possibile installare o eseguire l'interfaccia della riga di comando nel sottosistema Windows per Linux

Dato che il [sottosistema Windows per Linux (WSL)](/windows/wsl/about) è un livello di conversione delle chiamate di sistema basato sulla piattaforma Windows, potrebbe verificarsi un errore quando si prova a installare o eseguire l'interfaccia della riga di comando di Azure. L'interfaccia della riga di comando usa alcune funzionalità che potrebbero contenere un bug in WSL. Se si verifica un errore indipendentemente dal modo in cui si installa l'interfaccia della riga di comando, è probabile che il problema riguardi WSL e non il processo di installazione dell'interfaccia della riga di comando.

Per risolvere i problemi dell'installazione di WSL e possibilmente i problemi riscontrati:

* Se possibile, eseguire un processo di installazione identico in un computer o una VM Linux per verificare se ha esito positivo. Se riesce, il problema è quasi certamente correlato a WSL. Per avviare una VM Linux in Azure, vedere la documentazione su come [creare una VM Linux nel portale di Azure](/azure/virtual-machines/linux/quick-create-portal).
* Verificare di eseguire l'ultima versione di WSL. Per ottenere l'ultima versione, [aggiornare l'installazione di Windows 10](https://support.microsoft.com/help/4027667/windows-10-update).
* Verificare la presenza di [problemi aperti](https://github.com/Microsoft/WSL/issues) relativi a WSL che potrebbero risolvere il problema riscontrato.
  Spesso sono disponibili suggerimenti su soluzioni alternative al problema o informazioni su una versione in cui verrà risolto.
* Se non si trovano problemi esistenti per il problema riscontrato, [segnalare un nuovo problema relativo a WSL](https://github.com/Microsoft/WSL/issues/new) assicurandosi di includere la maggiore quantità di informazioni possibile.

Se i problemi di installazione o esecuzione in WSL persistono, valutare la possibilità di [installare l'interfaccia della riga di comando per Windows](../install-azure-cli-windows.md).
