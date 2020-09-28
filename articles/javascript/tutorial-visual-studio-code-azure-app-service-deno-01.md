---
title: Distribuire app Deno nel servizio app di Azure dall'interfaccia della riga di comando di Azure
description: Parte 1 dell'esercitazione, introduzione e prerequisiti.
ms.topic: conceptual
ms.date: 06/01/2020
ms.custom: devx-track-javascript
ms.openlocfilehash: 0a725c0c1f59f01fd75e76e68ac533c7918a429b
ms.sourcegitcommit: 0699b984b85782b1c441289fa756f285eae853c3
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/14/2020
ms.locfileid: "90772934"
---
# <a name="deploy-deno-to-azure-app-service-using-visual-studio-code"></a>Distribuire Deno nel Servizio app di Azure con Visual Studio Code

In questa esercitazione si distribuisce un'applicazione Deno nel servizio app di Azure, in Linux o in Windows, usando l'interfaccia della riga di comando di Azure.

## <a name="prerequisites"></a>Prerequisiti

- Un account Azure con una sottoscrizione attiva. [Crearne uno gratuito](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-appservice-deno&mktingSource=vscode-tutorial-appservice-deno)
- [Visual Studio Code](https://code.visualstudio.com/)
- [Deno](https://deno.land/#installation)
- L'interfaccia della riga di comando di Azure installata e connessa

## <a name="install-or-run-in-azure-cloud-shell"></a>Installazione o esecuzione in Azure Cloud Shell

Il modo più semplice per iniziare a usare l'interfaccia della riga di comando di Azure consiste nell'eseguirla in un ambiente Azure Cloud Shell tramite il browser. Per informazioni su Cloud Shell, vedere [Guida introduttiva a Bash in Azure Cloud Shell](/azure/cloud-shell/quickstart).

Quando si è pronti per installare l'interfaccia della riga di comando, vedere le [istruzioni di installazione](/cli/azure/install-azure-cli).

Dopo aver installato l'interfaccia della riga di comando per la prima volta, eseguire `az --version` per verificare se è installata e se la versione è corretta.

> [!NOTE]
> Se si usa il modello di distribuzione classica di Azure, [installare l'interfaccia della riga di comando classica di Azure](/cli/azure/install-classic-cli).

## <a name="sign-in"></a>Accesso

Prima di usare qualsiasi comando dell'interfaccia della riga di comando con un'installazione locale, è necessario eseguire l'accesso con [az login](/cli/azure/reference-index#az-login).

[!INCLUDE [interactive-login](../azure-cli/includes/interactive-login.md)]

Dopo l'accesso, viene visualizzato un elenco di sottoscrizioni associate all'account Azure. Le informazioni della sottoscrizione con `isDefault: true` rappresentano la sottoscrizione attualmente attivata dopo l'accesso. Per selezionare un'altra sottoscrizione, usare il comando [az account set](/cli/azure/account#az-account-set) con l'ID sottoscrizione a cui passare. Per altre informazioni sulla selezione delle sottoscrizioni, vedere [Usare più sottoscrizioni di Azure](/cli/azure/manage-azure-subscriptions-azure-cli).

> [!div class="nextstepaction"]
> [L'interfaccia della riga di comando di Azure è stata installata e connessa](tutorial-visual-studio-code-azure-app-service-deno-02.md) [Si è verificato un problema](https://www.research.net/r/PWZWZ52?tutorial=deno-deployment-azureappservice&step=getting-started)

## <a name="next-steps"></a>Passaggi successivi

[!INCLUDE [tutorial-next-steps](includes/tutorial-next-steps.md)]

> [!div class="nextstepaction"]
> [L'esercitazione è stata completata](node-howto-deploy-web-app.md) [Si è verificato un problema](https://www.research.net/r/PWZWZ52?tutorial=deno-deployment-azureappservice&step=clean-up-resources)
