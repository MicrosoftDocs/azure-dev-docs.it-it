---
ms.service: multiple
ms.date: 9/20/2018
ms.topic: include
ms.openlocfilehash: bfa7bcefff45bc325d7bba3aa2b9b3a8f0d439fa
ms.sourcegitcommit: 2efdb9d8a8f8a2c1914bd545a8c22ae6fe0f463b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/15/2019
ms.locfileid: "68280762"
---
Creare un file di testo denominato `azureauth.json`. Incollare l'output JSON ottenuto con la creazione dell'entità servizio.

Salvare questo file nel sistema in una posizione sicura e leggibile dal codice. Usare PowerShell per impostare una variabile di ambiente denominata `AZURE_AUTH_LOCATION` con il percorso completo del file, ad esempio:

```powershell
[Environment]::SetEnvironmentVariable("AZURE_AUTH_LOCATION", "C:\src\azureauth.json", "User")
```
