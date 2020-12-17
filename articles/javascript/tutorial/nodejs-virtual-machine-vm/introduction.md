---
title: Macchina virtuale Linux dell'interfaccia della riga di comando di Azure
description: Creare una macchina virtuale Linux di Azure con un clone di un'app basata su Express.js da un repository GitHub.
ms.topic: tutorial
ms.date: 11/13/2020
ms.custom: devx-track-js
ms.openlocfilehash: 674bf37acda9fcd9f6df7b84602600ad65ada3d9
ms.sourcegitcommit: c8330128d5d6a71859933a890ecdf047cb950996
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/15/2020
ms.locfileid: "97522121"
---
# <a name="1-create-linux-virtual-machine-with-expressjs-app-using-azure-cli"></a>1. Creare una macchina virtuale Linux con l'app Express.js usando l'interfaccia della riga di comando di Azure

In questa esercitazione verrà creata una macchina virtuale Linux per un'app Express.js. La macchina virtuale è configurata con un file di configurazione cloud-init e include NGINX e un repository GitHub per un'app Express.js. Quando la macchina virtuale è in esecuzione, è possibile connettersi alla macchina virtuale con SSH, modificare l'app Web in modo da includere la registrazione della traccia e visualizzare l'app server Express.js pubblica in un Web browser.

Questa esercitazione include le attività seguenti:

* Accedere ad Azure con l'interfaccia della riga di comando di Azure
* Creare una risorsa della VM Linux di Azure con l'interfaccia della riga di comando di Azure
    * Aprire la porta pubblica 80
    * Installare l'app Web Express.js demo da un repository GitHub
    * Installare le dipendenze dell'app Web
    * Avviare l'app Web
* Creare una risorsa di Monitoraggio di Azure con l'interfaccia della riga di comando di Azure
    * Connettersi alla macchina virtuale con SSH
    * Installare la libreria client di Azure SDK con npm
    * Aggiungere il codice della libreria client di Application Insights per creare la traccia personalizzata
* Visualizzare l'app Web dal browser
    * Richiedere alla route `/trace` di generare la traccia personalizzata nel log di Application Insights
    * Visualizzare il numero di tracce raccolte nei log con l'interfaccia della riga di comando di Azure
    * Visualizzare l'elenco di tracce con il portale di Azure
* Rimuovere le risorse con l'interfaccia della riga di comando di Azure

[!INCLUDE [Create or use existing Azure Subscription ](../../includes/environment-subscription-h2.md)]

## <a name="prerequisites"></a>Prerequisiti

- SSH per connettersi alla macchina virtuale: Usare un terminale moderno, ad esempio la shell Bash, che includa SSH.
[!INCLUDE [Azure CLI](../../../includes/azure-cli-prepare-your-environment-no-header.md)]


## <a name="next-step"></a>Passaggio successivo

> [!div class="nextstepaction"]
> [Creare un gruppo di risorse per le risorse di Azure](create-azure-monitoring-application-insights-web-resource.md) 