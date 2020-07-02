---
ms.openlocfilehash: e58e36b140c1512600497bffbbd149334904981f
ms.sourcegitcommit: 553da4e9aa988e5bb823364244ea81961cee5bc7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/01/2020
ms.locfileid: "85792171"
---
Dopo aver installato l'estensione di Azure in VS Code, accedere al proprio account Azure passando all'area **Azure**. Selezionare **Accedi ad Azure** e seguire le istruzioni visualizzate. Se sono installate più estensioni di Azure, selezionare quella relativa all'area in cui si lavora, ad esempio Servizio app, Funzioni e così via.

![Accedere ad Azure tramite VS Code](../media/deploy-azure/azure-sign-in.png)

Dopo aver eseguito l'accesso, verificare che l'indirizzo di posta elettronica dell'account Azure (o connesso) sia visualizzato sulla barra di stato e che la sottoscrizione o le sottoscrizioni siano visualizzate nell'area **Azure**:

![Barra di stato di VS Code che mostra l'account Azure](../media/deploy-azure/azure-account-status-bar.png)

![Area di Azure in VS Code che mostra le sottoscrizioni](../media/deploy-azure/azure-subscription-view.png)

> [!NOTE]
> Se viene visualizzato l'errore **"Cannot find subscription with name [subscription ID]"** (Non è possibile trovare la sottoscrizione denominata [ID sottoscrizione]), è possibile che si usi un proxy e che non sia possibile raggiungere l'API di Azure. Configurare le variabili di ambiente `HTTP_PROXY` e `HTTPS_PROXY` con le informazioni del proxy nel terminale:
>
> # <a name="bash"></a>[Bash](#tab/bash)
>
> ```bash
> export HTTPS_PROXY=https://username:password@proxy:8080
> export HTTP_PROXY=http://username:password@proxy:8080
> ```
>
> # <a name="powershell"></a>[PowerShell](#tab/powershell)
>
> ```powershell
> $env: HTTPS_PROXY = "https://username:password@proxy:8080"
> $env: HTTP_PROXY = "http://username:password@proxy:8080"
> ```
>
> # <a name="cmd"></a>[Cmd](#tab/cmd)
>
> ```cmd
> set HTTPS_PROXY=https://username:password@proxy:8080
> set HTTP_PROXY=http://username:password@proxy:8080
> ```
>
> ---
