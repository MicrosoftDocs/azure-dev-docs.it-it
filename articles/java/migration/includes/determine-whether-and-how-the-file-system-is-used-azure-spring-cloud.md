---
author: yevster
ms.author: yebronsh
ms.date: 5/26/2020
ms.openlocfilehash: 30d39530700806dc910c085c36a7834efa9c1414
ms.sourcegitcommit: 81577378a4c570ced1e9c6765f4a9eee8453c889
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/08/2020
ms.locfileid: "84507604"
---
#### <a name="determine-whether-and-how-the-file-system-is-used"></a>Determinare se e come viene usato il file system

Trovare tutte le istanze in cui i servizi eseguono la scrittura e/o la lettura dal file system locale. Identificare la posizione in cui vengono scritti e letti i file a breve termine/temporanei e i file di lunga durata.

> [!NOTE]
> Azure Spring Cloud fornisce 5 GB di spazio di archiviazione temporaneo per ogni istanza di Azure Spring Cloud, montato in `/tmp`. Se viene eseguita la scrittura di un numero di file temporanei in eccesso rispetto a tale limite o in un percorso diverso, sarà necessario apportare modifiche al codice.

<!-- The following two "static content" sections should be identical to the contents of includes\static-content.md except that here we use H5 headings. -->

##### <a name="read-only-static-content"></a>Contenuto statico di sola lettura

Se l'applicazione attualmente distribuisce contenuto statico, è necessario modificarne la posizione. Si può scegliere di spostare il contenuto statico in Archiviazione BLOB di Azure e di aggiungere la rete di distribuzione dei contenuti di Azure per accelerare i download a livello globale. Per altre informazioni, vedere [Hosting di siti Web statici in Archiviazione di Azure](/azure/storage/blobs/storage-blob-static-website) e [Avvio rapido: Integrare un account di archiviazione di Azure con la rete CDN di Azure](/azure/cdn/cdn-create-a-storage-account-with-cdn).

##### <a name="dynamically-published-static-content"></a>Contenuto statico pubblicato dinamicamente

Se l'applicazione consente contenuto statico caricato/prodotto dall'applicazione ma non modificabile dopo la creazione, è possibile usare Archiviazione BLOB di Azure e la rete di distribuzione dei contenuti di Azure, come descritto sopra, con una funzione di Azure per gestire i caricamenti e l'aggiornamento della rete CDN. Nell'articolo [Caricamento e precaricamento nella rete CDN di contenuto statico con Funzioni di Azure](https://github.com/Azure-Samples/functions-java-push-static-contents-to-cdn) è riportata un'implementazione di esempio che è possibile usare.
