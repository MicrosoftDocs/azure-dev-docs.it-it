---
title: Distribuire app Node.js nel Servizio app di Azure tramite l'interfaccia della riga di comando di Azure
description: Parte 1 dell'esercitazione, introduzione e prerequisiti.
ms.topic: conceptual
ms.date: 09/24/2019
ms.openlocfilehash: 7abe3bf3d59072acf8b448b66e68908b5d824a8c
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/05/2020
ms.locfileid: "77709871"
---
# <a name="deploy-to-azure-app-service-using-the-azure-cli"></a>Eseguire la distribuzione nel Servizio app di Azure tramite l'interfaccia della riga di comando di Azure

In questa esercitazione si distribuisce un'applicazione Node.js nel Servizio app di Azure tramite l'[interfaccia della riga di comando di Azure](https://docs.microsoft.com/cli/azure/overview?view=azure-cli-latest), che può essere eseguita in tutti i sistemi operativi. Con l'interfaccia della riga di comando è possibile creare risorse di Azure, configurare una pipeline di distribuzione tra un repository Git e Azure e visualizzare l'output `console.log` dell'app.

## <a name="prerequisites"></a>Prerequisites

- Una [sottoscrizione di Azure](#azure-subscription).
- [Node.js e npm 6.x o versione successiva](https://nodejs.org/en/download), la gestione pacchetti Node.js.
- [Git](https://git-scm.com/downloads), dopo il quale il comando `git --version` dovrebbe mostrare un numero di versione.
- [Interfaccia della riga di comando di Azure](https://docs.microsoft.com/cli/azure/install-azure-cli).

In alternativa, è possibile usare l'[estensione Interfaccia della riga di comando di Azure per Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azurecli), che prevede la colorazione della sintassi, IntelliSense (completamenti) e frammenti quando si scrivono script per l'interfaccia della riga di comando di Azure.

Una seconda alternativa è [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview), che è possibile usare da Visual Studio Code usando l'[estensione Account Azure](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account).

### <a name="azure-subscription"></a>Sottoscrizione di Azure

Se non si ha una sottoscrizione di Azure, [iscriversi](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-node-git&mktingSource=vscode-tutorial-node-git) subito per ottenere un account gratuito con 200 USD in crediti di Azure per provare qualsiasi combinazione di servizi.

### <a name="sign-in-to-the-azure-cli"></a>Accedere all'interfaccia della riga di comando di Azure

Dopo aver installato l'interfaccia della riga di comando di Azure, eseguire il comando seguente da un terminale o al prompt dei comandi:

```azurecli
az login
```

Il comando apre una finestra del browser in cui viene chiesto di accedere ad Azure. Dopo aver eseguito l'accesso, la finestra del terminale mostra l'output JSON con i dettagli della sottoscrizione.

## <a name="check-npm-version"></a>Controllare la versione di npm

Se Node.js è già installato, eseguire il comando `node -v` e verificare che la versione sia 10.x o superiore. Se è presente una versione meno recente, [eseguire l'aggiornamento](https://nodejs.org/en/download/) alla versione LTS (supporto a lungo termine) più recente.

> [!div class="nextstepaction"]
> [I prerequisiti sono stati installati](tutorial-vscode-azure-cli-node-02.md) [Si è verificato un problema](https://www.research.net/r/PWZWZ52?tutorial=node-deployment&step=getting-started)
