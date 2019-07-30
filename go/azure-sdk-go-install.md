---
title: Installare Azure SDK per Go
description: Come installare, eseguire il vendoring e configurare Azure SDK per Go.
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 03/14/2018
ms.topic: conceptual
ms.devlang: go
ms.openlocfilehash: e7a4449ed29239e7cb08902cee299d74102a72bf
ms.sourcegitcommit: 2efdb9d8a8f8a2c1914bd545a8c22ae6fe0f463b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/15/2019
ms.locfileid: "68291939"
---
# <a name="install-the-azure-sdk-for-go"></a>Installare Azure SDK per Go

Introduzione ad Azure SDK per Go L'SDK consente di gestire i servizi di Azure dalle applicazioni Go e interagire con essi.

## <a name="get-the-azure-sdk-for-go"></a>Ottenere Azure SDK per Go

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

Alcuni servizi di Azure includono un proprio SDK per Go e non sono compresi nel pacchetto Azure SDK per Go di base. La tabella seguente elenca i servizi con SDK propri e i relativi nomi di pacchetto. Questi pacchetti vengono tutti considerati in anteprima.

| Service | Pacchetto |
|---------|---------|
| Archiviazione BLOB | [github.com/Azure/azure-storage-blob-go](https://github.com/Azure/azure-storage-blob-go) |
| Archiviazione file | [github.com/Azure/azure-storage-file-go](https://github.com/Azure/azure-storage-file-go) |
| Coda di archiviazione | [github.com/Azure/azure-storage-queue-go](https://github.com/Azure/azure-storage-queue-go) |
| Hub eventi | [github.com/Azure/azure-event-hubs-go](https://github.com/Azure/azure-event-hubs-go) |
| Bus di servizio | [github.com/Azure/azure-service-bus-go](https://github.com/Azure/azure-service-bus-go) |

## <a name="vendor-the-azure-sdk-for-go"></a>Eseguire il vendoring di Azure SDK per Go

È possibile eseguire il vendoring di Azure SDK per Go tramite [dep](https://github.com/golang/dep). Per motivi di stabilità è consigliabile eseguire il vendoring. Per usare `dep` nel proprio progetto, aggiungere `github.com/Azure/azure-sdk-for-go` a una sezione `[[constraint]]` di `Gopkg.toml`. Per eseguire ad esempio il vendoring sulla versione `14.0.0`, aggiungere la voce seguente:

```toml
[[constraint]]
name = "github.com/Azure/azure-sdk-for-go"
version = "14.0.0"
```

## <a name="include-the-azure-sdk-for-go-in-your-project"></a>Includere Azure SDK per Go nel progetto

Per usare i servizi di Azure dal codice di Go, importare eventuali servizi con cui si interagisce e i moduli `autorest` necessari.
Si ottiene un elenco completo dei moduli disponibili da GoDoc per i [servizi disponibili](https://godoc.org/github.com/Azure/azure-sdk-for-go) e per i [pacchetti AutoRest](https://godoc.org/github.com/Azure/go-autorest). I pacchetti più comuni necessari da `go-autorest` sono:

| Pacchetto | DESCRIZIONE |
|---------|-------------|
| [github.com/Azure/go-autorest/autorest][autorest] | Oggetti per la gestione dell'autenticazione del client del servizio |
| [github.com/Azure/go-autorest/autorest/azure][autorest/azure] | Costanti per le interazioni con i servizi di Azure |
| [github.com/Azure/go-autorest/autorest/adal][autorest/adal] | Meccanismi di autenticazione per l'accesso ai servizi di Azure |
| [github.com/Azure/go-autorest/autorest/to][autorest/to] | Helper di asserzione tipi per l'uso di strutture dei dati di Azure SDK |

[autorest]: https://godoc.org/github.com/Azure/go-autorest/autorest
[autorest/azure]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure
[autorest/adal]: https://godoc.org/github.com/Azure/go-autorest/autorest/adal
[autorest/to]: https://godoc.org/github.com/Azure/go-autorest/autorest/to

Il controllo delle versioni dei pacchetti Go e dei servizi di Azure viene eseguito in modo indipendente. Le versioni del servizio sono parte del percorso di importazione del modulo, disponibile nel modulo `services`. Il percorso completo per il modulo è il nome del servizio, seguito dalla versione in formato `YYYY-MM-DD` e quindi seguito nuovamente dal nome del servizio. Per importare ad esempio la versione `2017-03-30` del servizio di calcolo:

```go
import "github.com/Azure/azure-sdk-for-go/services/compute/mgmt/2017-03-30/compute"
```

È consigliabile usare la versione più recente di un servizio all'inizio dello sviluppo e mantenere la coerenza a livello di versione.
È possibile che le diverse versioni presentino modifiche ai requisiti del servizio che potrebbero provocare interruzioni del codice, anche se non vengono applicati aggiornamenti a Go SDK in tale intervallo di tempo.

Se è necessario uno snapshot collettivo dei servizi, è anche possibile selezionare una singola versione del profilo. La versione dell'unico profilo bloccato è ora `2017-03-09`, che potrebbe non includere le funzionalità più recenti dei servizi. I profili sono disponibili nel modulo `profiles`, con la rispettiva versione in formato `YYYY-MM-DD`. I servizi vengono raggruppati sotto la rispettiva versione del profilo. Per importare ad esempio il modulo di gestione delle risorse di Azure dal profilo `2017-03-09`:

```go
import "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/resources/mgmt/resources"
```

> [!WARNING]
> Sono disponibili anche profili `preview` e `latest`. L'uso di questi profili non è consigliato. Questi profili sono versioni incrementali ed è possibile che il comportamento dei servizi cambi in qualsiasi momento.

## <a name="next-steps"></a>Passaggi successivi

Per iniziare a usare Azure SDK per Go, provare a seguire una guida introduttiva.

* [Distribuire una macchina virtuale da un modello](azure-sdk-go-qs-vm.md)
* [Trasferire oggetti nell'archivio BLOB di Azure con Azure Blob SDK per Go](/azure/storage/blobs/storage-quickstart-blobs-go?toc=%2fgo%2fazure%2ftoc.json)
* [Connettersi a Database di Azure per PostgreSQL](/azure/postgresql/connect-go?toc=%2fgo%2fazure%2ftoc.json)

Per iniziare immediatamente a usare altri servizi in Go SDK, vedere il codice di esempio disponibile.

* [Eseguire l'autenticazione con i servizi di Azure](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/internal/iam)
* [Distribuire nuove macchine virtuali con l'autenticazione SSH](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute)
* [Distribuire un'immagine del contenitore in Istanze di Azure Container](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute)
* [Creare un cluster nel servizio Azure Kubernetes](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute)
* [Usare i servizi di Archiviazione di Azure](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/storage)
* [Tutti gli esempi per Azure SDK per Go](https://github.com/azure-samples/azure-sdk-for-go-samples)
