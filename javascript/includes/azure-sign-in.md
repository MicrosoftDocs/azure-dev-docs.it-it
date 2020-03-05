---
ms.openlocfilehash: c9d46571c22c9bee49352dd4c1f492b2c9ff60a8
ms.sourcegitcommit: aa2c66b0fecce51862cc9115f68d39c770f0b2ae
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/27/2020
ms.locfileid: "77709833"
---
Dopo aver installato l'estensione di Azure, accedere al proprio account Azure passando all'area **Azure**, selezionare **Accedi ad Azure** e seguire le istruzioni visualizzate. Se sono installate più estensioni di Azure, selezionare quella relativa all'area in cui si lavora, ad esempio Servizio app, Funzioni e così via.

![Accedere ad Azure tramite VS Code](../media/deploy-azure/azure-sign-in.png)

Dopo aver eseguito l'accesso, verificare che l'indirizzo di posta elettronica dell'account Azure (o connesso) sia visualizzato sulla barra di stato e che la sottoscrizione o le sottoscrizioni siano visualizzate nell'area **Azure**:

![Barra di stato di VS Code che mostra l'account Azure](../media/deploy-azure/azure-account-status-bar.png)

![Area di Azure in VS Code che mostra le sottoscrizioni](../media/deploy-azure/azure-subscription-view.png)

> [!NOTE]
> Se viene visualizzato l'errore **"Cannot find subscription with name [subscription ID]"** (Non è possibile trovare la sottoscrizione denominata [ID sottoscrizione]), è possibile che si usi un proxy e che non sia possibile raggiungere l'API di Azure. Configurare le variabili di ambiente `HTTP_PROXY` e `HTTPS_PROXY` con le informazioni del proxy nel terminale:
>
> ```bash
> # MacOS/Linux
> export HTTPS_PROXY=https://username:password@proxy:8080
> export HTTP_PROXY=http://username:password@proxy:8080
> ```
>
> ```cmd
> # Windows
> set HTTPS_PROXY=https://username:password@proxy:8080
> set HTTP_PROXY=http://username:password@proxy:8080
> ```
