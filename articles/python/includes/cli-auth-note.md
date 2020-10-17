---
ms.openlocfilehash: f644d66895b7b8bcce345cd42c6efedfaddb4240
ms.sourcegitcommit: 29b161c450479e5d264473482d31e8d3bf29c7c0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/06/2020
ms.locfileid: "91764478"
---
Questo codice usa l'autenticazione basata sull'interfaccia della riga di comando (`AzureCliCredential`) perché illustra azioni che altrimenti si potrebbero eseguire direttamente con l'interfaccia della riga di comando di Azure. In entrambi i casi si usa la stessa identità per l'autenticazione.

Per usare tale codice in uno script di produzione (ad esempio per automatizzare la gestione delle VM), invece `DefaultAzureCredential` (scelta consigliata) o un metodo basato su entità servizio come descritto in [Come autenticare le app Python con i servizi di Azure](../azure-sdk-authenticate.md).
