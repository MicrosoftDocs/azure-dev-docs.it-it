---
ms.openlocfilehash: d64a5c908915b5d020f27e1c501aee852f04ae38
ms.sourcegitcommit: 74e28a479c87a3a53592646420b78e69852dd86a
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/16/2019
ms.locfileid: "71019659"
---
Dopo aver installato l'estensione di Azure, accedere al proprio account Azure passando all'area **Azure: App Service** (Azure: Servizio app), selezionare **Sign in to Azure** (Accedi ad Azure) e seguire i prompt.

![Accedere ad Azure tramite VS Code](../media/deploy-azure/azure-sign-in.png)

Dopo aver eseguito l'accesso, verificare che l'indirizzo di posta elettronica dell'account Azure sia visualizzato nella barra di stato e che la sottoscrizione o le sottoscrizioni siano visualizzate nell'area **Azure: App Service** (Azure: Servizio app):

![Barra di stato di VS Code che mostra l'account Azure](../media/deploy-azure/azure-account-status-bar.png)

![Area Azure: App Service (Azure: Servizio app) di VS Code che mostra le sottoscrizioni](../media/deploy-azure/azure-subscription-view.png)

> [!NOTE]
> Se viene visualizzato l'errore **"Cannot find subscription with name [subscription ID]"** (Non è possibile trovare la sottoscrizione denominata [ID sottoscrizione]), è possibile che si usi un proxy e che non sia possibile raggiungere l'API di Azure. Configurare le variabili di ambiente `HTTP_PROXY` e `HTTPS_PROXY` con le informazioni del proxy nel terminale:
>
> ```sh
> # macOS/Linux
> export HTTPS_PROXY=https://username:password@proxy:8080
> export HTTP_PROXY=http://username:password@proxy:8080
>
> # Windows
> set HTTPS_PROXY=https://username:password@proxy:8080
> set HTTP_PROXY=http://username:password@proxy:8080
> ```
