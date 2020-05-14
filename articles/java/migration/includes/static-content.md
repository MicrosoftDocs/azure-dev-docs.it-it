---
author: yevster
ms.author: yebronsh
ms.date: 4/10/2020
ms.openlocfilehash: c6f4f3b58ff7d5a8733a0f2df2ab0bf3c6eb6fe4
ms.sourcegitcommit: 226ebca0d0e3b918928f58a3a7127be49e4aca87
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/08/2020
ms.locfileid: "82988743"
---
#### <a name="read-only-static-content"></a>Contenuto statico di sola lettura

Se l'applicazione attualmente distribuisce contenuto statico, è necessario modificarne la posizione. Si può scegliere di spostare il contenuto statico in Archiviazione BLOB di Azure e di aggiungere la rete di distribuzione dei contenuti di Azure per accelerare i download a livello globale. Per altre informazioni, vedere [Hosting di siti Web statici in Archiviazione di Azure](/azure/storage/blobs/storage-blob-static-website) e [Avvio rapido: Integrare un account di archiviazione di Azure con la rete CDN di Azure](/azure/cdn/cdn-create-a-storage-account-with-cdn).

#### <a name="dynamically-published-static-content"></a>Contenuto statico pubblicato dinamicamente

Se l'applicazione consente contenuto statico caricato/prodotto dall'applicazione ma non modificabile dopo la creazione, è possibile usare Archiviazione BLOB di Azure e la rete di distribuzione dei contenuti di Azure, come descritto sopra, con una funzione di Azure per gestire i caricamenti e l'aggiornamento della rete CDN. Nell'articolo [Caricamento e precaricamento nella rete CDN di contenuto statico con Funzioni di Azure](https://github.com/Azure-Samples/functions-java-push-static-contents-to-cdn) è riportata un'implementazione di esempio che è possibile usare.
