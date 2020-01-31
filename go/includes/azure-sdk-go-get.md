---
ms.date: 09/05/2018
ms.technology: azure-cli
ms.openlocfilehash: af03f2efbb74e55dfcd14b6a2ac894a74eba321f
ms.sourcegitcommit: 4cf22356d6d4817421b551bd53fcba76bdb44cc1
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2020
ms.locfileid: "76871885"
---
[Azure SDK per Go](https://github.com/Azure/azure-sdk-for-go) è compatibile con Go 1.8 e versioni successive. Per gli ambienti che usano [profili di Azure Stack](/azure/azure-stack/user/azure-stack-version-profiles-go) il requisito minimo è Go versione 1.9.
Se è necessario installare Go, seguire le [istruzioni di installazione di Go](https://golang.org/doc/install).

È possibile scaricare Azure SDK per Go e le rispettive dipendenze tramite `go get`.

```bash
go get -u -d github.com/Azure/azure-sdk-for-go/...
```

> [!WARNING]
> Assicurarsi di scrivere `Azure` in lettere maiuscole nell'URL. In caso contrario, è possibile che si verifichino problemi di importazione correlati alla distinzione tra maiuscole/minuscole durante l'uso dell'SDK. È anche necessario scrivere `Azure` in lettere maiuscole nelle istruzioni di importazione.
