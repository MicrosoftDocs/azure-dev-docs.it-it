---
ms.openlocfilehash: c78b25e2111e48ed9337bcd85f6585c4bdd4a081
ms.sourcegitcommit: 418e446e6ada5d50df283401df4f6b6370a356b9
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/25/2020
ms.locfileid: "96035515"
---
Dopo aver installato l'estensione di Azure, accedere al proprio account Azure:

1. Passare alla finestra di esplorazione di **Azure**
1. Selezionare **Accedi ad Azure** e seguire le istruzioni. Se sono installate più estensioni di Azure, selezionare quella relativa all'area in cui si lavora, ad esempio Servizio app, Funzioni e così via.

    ![Accedere ad Azure tramite VS Code](../media/deploy-azure/sign-in-to-azure-through-visual-studio-code.png)

1. Dopo l'accesso verificare che **Azure: Accesso effettuato"**sia visualizzato sulla barra di stato e che la sottoscrizione o le sottoscrizioni siano visualizzate nell'area** Azure**:

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
