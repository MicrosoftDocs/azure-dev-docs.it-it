---
author: yevster
ms.author: yebronsh
ms.date: 1/20/2020
ms.openlocfilehash: 478059f598b9c39609a85647945ebef3df3cb7ec
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/05/2020
ms.locfileid: "81671637"
---
### <a name="inventory-persistence-usage"></a>Inventario dell'utilizzo di persistenza

Qualsiasi utilizzo del file system nel server applicazioni richiede modifiche della configurazione o, in casi rari, dell'architettura. È possibile identificare alcuni o tutti gli scenari seguenti.

#### <a name="read-only-static-content"></a>Contenuto statico di sola lettura

Se l'applicazione attualmente gestisce contenuto statico (ad esempio, tramite un'integrazione di Apache), sarà necessario collocarlo in una posizione alternativa. Si può scegliere di spostare il contenuto statico in Archiviazione BLOB di Azure e di aggiungere la rete di distribuzione dei contenuti di Azure per accelerare i download a livello globale. Per altre informazioni, vedere [Hosting di siti Web statici in Archiviazione di Azure](/azure/storage/blobs/storage-blob-static-website) e [Avvio rapido: Integrare un account di archiviazione di Azure con la rete CDN di Azure](/azure/cdn/cdn-create-a-storage-account-with-cdn#enable-azure-cdn-for-the-storage-account).

#### <a name="dynamically-published-static-content"></a>Contenuto statico pubblicato dinamicamente

Se l'applicazione consente contenuto statico caricato/prodotto dall'applicazione ma non modificabile dopo la creazione, è possibile usare Archiviazione BLOB di Azure e la rete di distribuzione dei contenuti di Azure, come descritto sopra, con una funzione di Azure per gestire i caricamenti e l'aggiornamento della rete CDN. Nell'articolo [Caricamento e precaricamento nella rete CDN di contenuto statico con Funzioni di Azure](https://github.com/Azure-Samples/functions-java-push-static-contents-to-cdn) è riportata un'implementazione di esempio che è possibile usare.
