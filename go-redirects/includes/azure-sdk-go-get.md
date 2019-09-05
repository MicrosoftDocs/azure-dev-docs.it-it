---
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 09/05/2018
ms.topic: include
ms.prod: azure
ms.technology: azure-cli
ms.openlocfilehash: a51c8667c74a2611ae8769aa42fd1a94f9253bc8
ms.sourcegitcommit: 9cd460ee16b637e701aa30078932878c0d0a7945
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/30/2019
ms.locfileid: "70180800"
---
[Azure SDK per Go](https://github.com/Azure/azure-sdk-for-go) è compatibile con Go 1.8 e versioni successive. Per gli ambienti che usano [profili di Azure Stack](/azure/azure-stack/user/azure-stack-version-profiles-go) il requisito minimo è Go versione 1.9.
Se è necessario installare Go, seguire le [istruzioni di installazione di Go](https://golang.org/doc/install).

È possibile scaricare Azure SDK per Go e le rispettive dipendenze tramite `go get`.

```bash
go get -u -d github.com/Azure/azure-sdk-for-go/...
```

> [!WARNING]
> Assicurarsi di scrivere `Azure` in lettere maiuscole nell'URL. In caso contrario, è possibile che si verifichino problemi di importazione correlati alla distinzione tra maiuscole/minuscole durante l'uso dell'SDK. È anche necessario scrivere `Azure` in lettere maiuscole nelle istruzioni di importazione.
