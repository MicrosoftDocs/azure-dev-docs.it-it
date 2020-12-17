---
ms.custom: devx-track-js
ms.topic: include
ms.date: 10/16/2020
ms.openlocfilehash: 624ba9739549b260018e05450598d17460f2e5d8
ms.sourcegitcommit: c1ef7aa8ed2e88e98b190e42cffde52cf301958d
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/10/2020
ms.locfileid: "97096435"
---
Se si usano già estensioni dei servizi di Azure, si è già connessi ed è possibile ignorare questo passaggio. Se non si usano estensioni dei servizi di Azure, continuare in questa sezione per accedere ad Azure.

Dopo aver installato l'estensione di un servizio di Azure in Visual Studio Code, è necessario accedere all'account Azure passando alla finestra di esplorazione di **Azure**, selezionando **Accedi ad Azure** e quindi seguendo le istruzioni. Se sono installate più estensioni di Azure, selezionare quella relativa all'area in cui si lavora, ad esempio Servizio app, Funzioni e così via.

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
