---
ms.openlocfilehash: 1e6e3f46fc7dd2f4ddf708be65941deb9ca18096
ms.sourcegitcommit: 69933dcce571b2686897b295b7822e207d944617
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/18/2020
ms.locfileid: "90772610"
---
Dopo aver installato l'estensione di Azure, accedere al proprio account Azure passando all'area **Azure**, selezionare **Accedi ad Azure** e seguire le istruzioni visualizzate. Se sono installate più estensioni di Azure, selezionare quella relativa all'area in cui si lavora, ad esempio Servizio app, Funzioni e così via.

![Accedere ad Azure tramite VS Code](../media/deploy-azure/sign-in-to-azure-through-visual-studio-code.png)

Dopo l'accesso verificare che **Azure: Accesso effettuato"**sia visualizzato sulla barra di stato e che la sottoscrizione o le sottoscrizioni siano visualizzate nell'area**Azure**:

![Barra di stato di Visual Studio Code che mostra l'account Azure](../media/deploy-azure/azure-account-status-bar-in-visual-studio-code.png)

![Area Azure: App Service (Azure: Servizio app) di Visual Studio Code che mostra le sottoscrizioni](../media/deploy-azure/view-azure-subscription-in-visual-studio-code-app-service-explorer.png)

> [!NOTE]
> Se viene visualizzato l'errore **"Cannot find subscription with name [subscription ID]"** (Non è possibile trovare la sottoscrizione denominata [ID sottoscrizione]), è possibile che si usi un proxy e che non sia possibile raggiungere l'API di Azure. Configurare le variabili di ambiente `HTTP_PROXY` e `HTTPS_PROXY` con le informazioni del proxy nel terminale:
>
> ```cmd
> # Windows
> set HTTPS_PROXY=https://username:password@proxy:8080
> set HTTP_PROXY=http://username:password@proxy:8080
> ```
>
> ```bash
> # macOS/Linux
> export HTTPS_PROXY=https://username:password@proxy:8080
> export HTTP_PROXY=http://username:password@proxy:8080
> ```
