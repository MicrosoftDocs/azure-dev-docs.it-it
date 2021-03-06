---
title: Distribuire app Node.js nel Servizio app di Azure tramite l'interfaccia della riga di comando di Azure
description: Parte 1 dell'esercitazione, introduzione e prerequisiti dell'interfaccia della riga di comando di Azure.
ms.topic: tutorial
ms.date: 09/24/2019
ms.custom: devx-track-js, devx-track-azurecli
ms.openlocfilehash: 87dfa69c31eabffe12cd579127bfdf8b74a333e7
ms.sourcegitcommit: 3536f174735cd3bb7da7e4b266fbf43349a22b67
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/12/2021
ms.locfileid: "103193480"
---
# <a name="deploy-to-azure-app-service-using-the-azure-cli"></a>Eseguire la distribuzione nel Servizio app di Azure tramite l'interfaccia della riga di comando di Azure

In questa esercitazione si distribuisce un'applicazione Node.js nel Servizio app di Azure tramite l'[interfaccia della riga di comando di Azure](/cli/azure/overview), che può essere eseguita in tutti i sistemi operativi. Con l'interfaccia della riga di comando è possibile creare risorse di Azure, configurare una pipeline di distribuzione tra un repository Git e Azure e visualizzare l'output `console.log` dell'app.

## <a name="prerequisites"></a>Prerequisites

- Una [sottoscrizione di Azure](#azure-subscription).
- [Node.js](https://nodejs.org/en/download)Node.js gestione pacchetti.
- [Git](https://git-scm.com/downloads), dopo il quale il comando `git --version` dovrebbe mostrare un numero di versione.
[!INCLUDE [Azure CLI](../../../includes/azure-cli-prepare-your-environment-no-header.md)]


### <a name="azure-subscription"></a>Sottoscrizione di Azure

Se non si ha una sottoscrizione di Azure, [iscriversi](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-node-git&mktingSource=vscode-tutorial-node-git) subito per ottenere un account gratuito con 200 USD in crediti di Azure per provare qualsiasi combinazione di servizi.

### <a name="sign-in-to-azure-with-azure-cli"></a>Accedere ad Azure con l'interfaccia della riga di comando di Azure

[!INCLUDE [Sign in ](../../../azure-cli/includes/interactive-login.md)]

## <a name="check-npm-version"></a>Controllare la versione di npm

Se Node.js è già installato, eseguire il comando `node -v` e verificare che la versione sia 10.x o superiore. Se è presente una versione meno recente, [eseguire l'aggiornamento](https://nodejs.org/en/download/) alla versione LTS (supporto a lungo termine) più recente.

> [!div class="nextstepaction"]
> [I prerequisiti sono stati installati](tutorial-vscode-azure-cli-node-02.md) [Si è verificato un problema](https://www.research.net/r/PWZWZ52?tutorial=node-deployment&step=getting-started)
