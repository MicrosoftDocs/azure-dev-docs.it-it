---
title: Archiviazione cloud per la creazione di app scalabili, durevoli e a sicurezza elevata con Archiviazione di Azure
description: Informazioni sui servizi per archiviare grandi quantità di dati strutturati e non strutturati delle applicazioni per dispositivi mobili nel cloud.
author: codemillmatt
ms.assetid: 12bbb070-9b3c-4faf-8588-ccff02097224
ms.service: mobile-services
ms.topic: article
ms.date: 06/08/2020
ms.author: masoucou
ms.openlocfilehash: 7a1171ebe64b0353fc57180adce63ee702431207
ms.sourcegitcommit: f02ff55cef47581303a217cf1d310879cd722237
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/09/2020
ms.locfileid: "84632363"
---
# <a name="cloud-storage-for-highly-secure-durable-scalable-apps-with-azure-storage"></a>Archiviazione cloud per app scalabili, durable e a sicurezza elevata con Archiviazione di Azure

[Archiviazione di Azure](https://azure.microsoft.com/services/storage/) è la soluzione di archiviazione nel cloud di Microsoft per le applicazioni moderne che offre un archivio a scalabilità elevata per oggetti dati, un servizio di file system per il cloud, un archivio di messaggistica per messaggistica affidabile e un archivio NoSQL. Archiviazione di Azure presenta le caratteristiche seguenti:

- **Durabilità e disponibilità elevata:** La ridondanza garantisce che i dati siano al sicuro in caso di errori hardware temporanei. Si può anche scegliere di replicare i dati tra data center o aree geografiche per una protezione aggiuntiva da catastrofi locali o calamità naturali. Con questo tipo di replica, i dati mantengono disponibilità elevata in caso di interruzioni impreviste.
- **Sicurezza:** Tutti i dati scritti in Archiviazione di Azure vengono crittografati dal servizio. Archiviazione di Azure offre un controllo dettagliato su chi potrà accedere ai dati.
- **Scalabilità**: I servizi sono progettati per offrire scalabilità elevata in modo da soddisfare le esigenze di archiviazione dati e di prestazioni delle applicazioni moderne.
- **Servizio gestito:** Azure gestisce automaticamente le attività di manutenzione dell'hardware, gli aggiornamenti e i problemi critici.
- **Accessibilità:** I dati sono accessibili da ogni parte del mondo tramite HTTP o HTTPS. Microsoft fornisce le librerie client in diversi linguaggi, ad esempio .NET, Java, Node.js, Python, PHP, Ruby, Go e un'API REST avanzata. Lo scripting è supportato in Azure PowerShell o nell'interfaccia della riga di comando di Azure. Il portale di Azure e Azure Storage Explorer offrono soluzioni visive facili per l'uso dei dati.

Usare i servizi seguenti per abilitare l'archiviazione nel cloud nelle app per dispositivi mobili.

## <a name="azure-blob-storage"></a>Archiviazione BLOB di Azure

[Archiviazione BLOB di Azure](https://azure.microsoft.com/services/storage/blobs/) offre una soluzione di archiviazione di oggetti per il cloud. L'archiviazione BLOB è ottimizzata per archiviare quantità elevate di dati non strutturati che non seguono una definizione o un modello di dati specifico, ad esempio dati di testo o binari. Supporta un'ampia varietà di linguaggi usati dalle librerie client. L'archiviazione BLOB è progettata per:

- Inviare immagini o documenti direttamente a un browser.
- Archiviare file per l'accesso distribuito.
- Trasmettere in streaming audio e video.
- Eseguire la scrittura nei file di log.
- Archiviare i dati per backup e ripristino, ripristino di emergenza e archiviazione.
- Archiviare i dati a scopo di analisi da parte di un servizio locale o ospitato in Azure.

### <a name="azure-blob-storage-references"></a>Informazioni di riferimento per Archiviazione BLOB di Azure

- [Azure portal](https://portal.azure.com)
- [Documentazione di Archiviazione BLOB di Azure](/azure/storage/blobs/storage-blobs-introduction)
- [Guide introduttive](/azure/storage/blobs/storage-quickstart-blobs-portal)
- [Esempi](/azure/storage/common/storage-samples-dotnet?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

## <a name="azure-table-storage"></a>Archiviazione tabelle di Azure

L'[archiviazione tabelle di Azure](https://azure.microsoft.com/services/storage/tables/) è un servizio che archivia dati NoSQL strutturati nel cloud, mettendo a disposizione un archivio di chiavi/attributi senza schema. Il servizio Archiviazione tabelle di Azure consente di archiviare grandi quantità di dati strutturati. Il servizio è un archivio dati NoSQL che accetta chiamate autenticate dall'interno e dall'esterno del cloud di Azure. Le tabelle di Azure sono ideali per l'archiviazione di dati strutturati non relazionali. L'archiviazione tabelle viene in genere usata per:

- Archiviare terabyte di dati strutturati per la gestione di applicazioni su scala Web.
- Archiviare set di dati che non richiedono join complessi, chiavi esterne o stored procedure e che possono essere denormalizzati per l'accesso rapido.
- Eseguire query rapide sui dati mediante un indice cluster.
- Accedere ai dati usando il protocollo OData e le query LINQ con le librerie .NET dei Servizi dati di Windows Communication Foundation (WCF).

È possibile usare l'archiviazione tabelle di Azure per archiviare ed eseguire query su grandi set di dati strutturati non relazionali. Le tabelle vengono dimensionate in base all'aumento della domanda.

### <a name="azure-table-storage-references"></a>Informazioni di riferimento per l'archiviazione tabelle di Azure

- [Azure portal](https://portal.azure.com)
- [Documentazione di archiviazione tabelle di Azure](/azure/storage/tables/table-storage-overview)
- [Esempi](/azure/cosmos-db/tutorial-develop-table-dotnet?toc=https%3A%2F%2Fdocs.microsoft.com%2Fen-us%2Fazure%2Fstorage%2Ftables%2FTOC.json&bc=https%3A%2F%2Fdocs.microsoft.com%2Fen-us%2Fazure%2Fbread%2Ftoc.json)
- [Guide introduttive](/azure/storage/tables/table-storage-quickstart-portal)

## <a name="azure-queue-storage"></a>Archiviazione code di Azure

[Archiviazione code di Azure](https://azure.microsoft.com/services/storage/queues/) è un servizio per l'archiviazione di un numero elevato di messaggi. È possibile accedere ai messaggi ovunque ci si trovi con chiamate autenticate tramite HTTP o HTTPS. Un messaggio in coda avere dimensioni fino a 64 KB. Una coda può contenere milioni di messaggi, fino al limite di capacità totale dell'account di archiviazione. Le code vengono in genere usate per creare un backlog di lavoro da elaborare in modo asincrono.

###  <a name="azure-queue-storage-references"></a>Informazioni di riferimento per l'archiviazione code di Azure

- [Azure portal](https://portal.azure.com)
- [Documentazione dell'archiviazione code di Azure](/azure/storage/queues/)
- [Guide introduttive](/azure/storage/queues/storage-quickstart-queues-portal)
- [Esempi](/azure/storage/common/storage-samples-dotnet?toc=%2fazure%2fstorage%2fqueues%2ftoc.json)
