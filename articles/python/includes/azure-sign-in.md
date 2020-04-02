---
ms.openlocfilehash: 90ac4162e1a6032bcb1f3cfab7f249e4538314f7
ms.sourcegitcommit: 1bd9ec6a4115e9162e33b76a933869788e6ab702
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/31/2020
ms.locfileid: "80441476"
---
Dopo aver installato l'estensione di Azure, accedere al proprio account Azure passando all'area **Azure**, selezionare **Accedi ad Azure** e seguire le istruzioni visualizzate. Se sono installate più estensioni di Azure, selezionare quella relativa all'area in cui si lavora, ad esempio Servizio app, Funzioni e così via.

![Accedere ad Azure tramite VS Code](../media/deploy-azure/sign-in-to-azure-through-visual-studio-code.png)

Dopo aver eseguito l'accesso, verificare che l'indirizzo di posta elettronica dell'account Azure (o connesso) sia visualizzato sulla barra di stato e che la sottoscrizione o le sottoscrizioni siano visualizzate nell'area **Azure**:

![Barra di stato di Visual Studio Code che mostra l'account Azure](../media/deploy-azure/azure-account-status-bar-in-visual-studio-code.png)

![Area Azure: App Service (Azure: Servizio app) di Visual Studio Code che mostra le sottoscrizioni](../media/deploy-azure/view-azure-subscription-in-visual-studio-code-app-service-explorer.png)

> [!NOTE]
> Se viene visualizzato l'errore **"Cannot find subscription with name [subscription ID]"** (Non è possibile trovare la sottoscrizione denominata [ID sottoscrizione]), è possibile che si usi un proxy e che non sia possibile raggiungere l'API di Azure. Configurare le variabili di ambiente `HTTP_PROXY` e `HTTPS_PROXY` con le informazioni del proxy nel terminale:
>
> ```bash
> # macOS/Linux
> export HTTPS_PROXY=https://username:password@proxy:8080
> export HTTP_PROXY=http://username:password@proxy:8080
> ```
>
> ```cmd
> # Windows
> set HTTPS_PROXY=https://username:password@proxy:8080
> set HTTP_PROXY=http://username:password@proxy:8080
> ```