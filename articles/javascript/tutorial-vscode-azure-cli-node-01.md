---
title: Distribuire app Node.js nel Servizio app di Azure tramite l'interfaccia della riga di comando di Azure
description: Parte 1 dell'esercitazione, introduzione e prerequisiti dell'interfaccia della riga di comando di Azure.
ms.topic: tutorial
ms.date: 09/24/2019
ms.custom: devx-track-js, devx-track-azurecli
ms.openlocfilehash: fab8d3af108fb5b81f8360ec84320da5e4ca5c15
ms.sourcegitcommit: 291768a67862336267c67819e913c16710e3875e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/24/2020
ms.locfileid: "95820667"
---
# <a name="deploy-to-azure-app-service-using-the-azure-cli"></a>Eseguire la distribuzione nel Servizio app di Azure tramite l'interfaccia della riga di comando di Azure

In questa esercitazione si distribuisce un'applicazione Node.js nel Servizio app di Azure tramite l'[interfaccia della riga di comando di Azure](/cli/azure/overview?view=azure-cli-latest), che può essere eseguita in tutti i sistemi operativi. Con l'interfaccia della riga di comando è possibile creare risorse di Azure, configurare una pipeline di distribuzione tra un repository Git e Azure e visualizzare l'output `console.log` dell'app.

## <a name="prerequisites"></a>Prerequisites

- Una [sottoscrizione di Azure](#azure-subscription).
- [Node.js e npm 6.x o versione successiva](https://nodejs.org/en/download), la gestione pacchetti Node.js.
- [Git](https://git-scm.com/downloads), dopo il quale il comando `git --version` dovrebbe mostrare un numero di versione.
- [Installare l'interfaccia della riga di comando di Azure](/cli/azure/install-azure-cli) o usare [Azure Cloud Shell](https://shell.azure.com.)

### <a name="azure-subscription"></a>Sottoscrizione di Azure

Se non si ha una sottoscrizione di Azure, [iscriversi](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-node-git&mktingSource=vscode-tutorial-node-git) subito per ottenere un account gratuito con 200 USD in crediti di Azure per provare qualsiasi combinazione di servizi.

### <a name="sign-in-to-azure-with-azure-cli"></a>Accedere ad Azure con l'interfaccia della riga di comando di Azure

[!INCLUDE [Sign in ](../azure-cli/includes/interactive-login.md)]

## <a name="check-npm-version"></a>Controllare la versione di npm

Se Node.js è già installato, eseguire il comando `node -v` e verificare che la versione sia 10.x o superiore. Se è presente una versione meno recente, [eseguire l'aggiornamento](https://nodejs.org/en/download/) alla versione LTS (supporto a lungo termine) più recente.

> [!div class="nextstepaction"]
> [I prerequisiti sono stati installati](tutorial-vscode-azure-cli-node-02.md) [Si è verificato un problema](https://www.research.net/r/PWZWZ52?tutorial=node-deployment&step=getting-started)
