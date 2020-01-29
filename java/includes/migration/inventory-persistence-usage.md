---
author: yevster
ms.author: yebronsh
ms.topic: include
ms.date: 1/20/2020
ms.openlocfilehash: 23289c7dc4b608c6fe8bb75af479ea877a2000d9
ms.sourcegitcommit: 3585b1b5148e0f8eb950037345bafe6a4f6be854
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/21/2020
ms.locfileid: "76288620"
---
### <a name="inventory-persistence-usage"></a>Inventario dell'utilizzo di persistenza

Qualsiasi utilizzo del file system nel server applicazioni richiede modifiche della configurazione o, in casi rari, dell'architettura. Il file system può essere usato da moduli Tomcat o dal codice dell'applicazione. È possibile identificare alcuni o tutti gli scenari seguenti.

#### <a name="read-only-static-content"></a>Contenuto statico di sola lettura

Se l'applicazione attualmente gestisce contenuto statico (ad esempio, tramite un'integrazione di Apache), sarà necessario collocarlo in una posizione alternativa. Si può scegliere di spostare il contenuto statico in Archiviazione BLOB di Azure e di aggiungere la rete di distribuzione dei contenuti di Azure per accelerare i download a livello globale. Per altre informazioni, vedere [Hosting di siti web statici in Archiviazione di Azure](/azure/storage/blobs/storage-blob-static-website) e [Abilitare la rete CDN di Azure per l'account di archiviazione](/azure/cdn/cdn-create-a-storage-account-with-cdn#enable-azure-cdn-for-the-storage-account).

#### <a name="dynamically-published-static-content"></a>Contenuto statico pubblicato dinamicamente

Se l'applicazione consente contenuto statico caricato/prodotto dall'applicazione ma non modificabile dopo la creazione, è possibile usare Archiviazione BLOB di Azure e la rete di distribuzione dei contenuti di Azure, come descritto sopra, con una funzione di Azure per gestire i caricamenti e l'aggiornamento della rete CDN. Nell'articolo [Caricamento e precaricamento nella rete CDN di contenuto statico con Funzioni di Azure](https://github.com/Azure-Samples/functions-java-push-static-contents-to-cdn) è riportata un'implementazione di esempio che è possibile usare.
