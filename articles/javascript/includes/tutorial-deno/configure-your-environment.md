---
title: file di inclusione 1
description: file di inclusione 1
ms.topic: include
ms.date: 06/01/2020
ms.custom: devx-track-js, devx-track-azurecli
ms.openlocfilehash: 8f0bda8929699f61fd3c79f9ae6fb38762e622c9
ms.sourcegitcommit: 5541f993c01ce356e1b0eaa8f95aea9051c3c21e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/03/2020
ms.locfileid: "93278663"
---
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

[!INCLUDE [interactive-login](../../../azure-cli/includes/interactive-login.md)]

Dopo l'accesso, viene visualizzato un elenco di sottoscrizioni associate all'account Azure. Le informazioni della sottoscrizione con `isDefault: true` rappresentano la sottoscrizione attualmente attivata dopo l'accesso. Per selezionare un'altra sottoscrizione, usare il comando [az account set](/cli/azure/account#az-account-set) con l'ID sottoscrizione a cui passare. Per altre informazioni sulla selezione delle sottoscrizioni, vedere [Usare più sottoscrizioni di Azure](/cli/azure/manage-azure-subscriptions-azure-cli).
