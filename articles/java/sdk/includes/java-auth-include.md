---
ms.openlocfilehash: 0a9b06932baa51c3cf003a4485a3a25261ffe91d
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/05/2020
ms.locfileid: "81671867"
---
Creare un [file di autenticazione](../java-sdk-azure-authenticate.md#mgmt-file) ed esportare una variabile di ambiente `AZURE_AUTH_LOCATION` nella riga di comando con il percorso completo del file.

```bash
export AZURE_AUTH_LOCATION=/Users/raisa/azure.auth
```

Il file di autenticazione consente di configurare l'oggetto `Azure` del punto di ingresso usato dalle librerie di gestione per definire, creare e configurare le risorse di Azure.

```java
// pull in the location of the security file from the environment 
final File credFile = new File(System.getenv("AZURE_AUTH_LOCATION"));

Azure azure = Azure
        .configure()
        .withLogLevel(LogLevel.NONE)
        .authenticate(credFile)
        .withDefaultSubscription();
```

[Altre informazioni](../java-sdk-azure-authenticate.md#mgmt-auth) sulle opzioni di autenticazione quando si usano le librerie di gestione di Azure per Java.