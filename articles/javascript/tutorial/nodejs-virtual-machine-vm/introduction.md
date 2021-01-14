---
title: Macchina virtuale Linux dell'interfaccia della riga di comando di Azure
description: Creare una macchina virtuale Linux di Azure con un clone di un'app basata su Express.js da un repository GitHub.
ms.topic: tutorial
ms.date: 11/13/2020
ms.custom: devx-track-js
ms.openlocfilehash: c68a8442862eb03291f80609bb9767fb1c9b1b6b
ms.sourcegitcommit: ed31f841fa9680335658df6708f107e170d47ff0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/07/2021
ms.locfileid: "97974548"
---
# <a name="1-create-linux-virtual-machine-with-expressjs-app-using-azure-cli"></a>1. Creare una macchina virtuale Linux con l'app Express.js usando l'interfaccia della riga di comando di Azure

In questa esercitazione verrà creata una macchina virtuale Linux per un'app Express.js. La macchina virtuale è configurata con un file di configurazione cloud-init e include NGINX e un repository GitHub per un'app Express.js. Quando la macchina virtuale è in esecuzione, è possibile connettersi alla macchina virtuale con SSH, modificare l'app Web in modo da includere la registrazione della traccia e visualizzare l'app server Express.js pubblica in un Web browser.

* [**Codice di esempio**](https://github.com/Azure-Samples/js-e2e-vm)

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