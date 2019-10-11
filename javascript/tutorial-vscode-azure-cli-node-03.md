---
title: Creare il Servizio app di Azure dall'interfaccia della riga di comando di Azure per ospitare l'app
description: Parte 3 dell'esercitazione, creare il Servizio app
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/24/2019
ms.author: kraigb
ms.openlocfilehash: f8349d352d275abcc8126bb9dd1794d408045b29
ms.sourcegitcommit: c04984b6367e922dbc5973af44f8cd0ca81ce157
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/30/2019
ms.locfileid: "71686142"
---
# <a name="create-the-app-service"></a>Creare il servizio app

[Passaggio precedente: Creare l'app](tutorial-vscode-azure-cli-node-02.md)

In questo passaggio si usa l'interfaccia della riga di comando di Azure per creare il Servizio app di Azure per ospitare il codice dell'app.

1. In un terminale o al prompt dei comandi usare il comando seguente per creare un **gruppo di risorse** per il Servizio app. Un gruppo di risorse è essenzialmente una raccolta denominata delle risorse di un'app in Azure, ad esempio un sito Web, un database, Funzioni di Azure e così via.

    ```bash
    az group create --name myResourceGroup --location westus
    ```

    Il comando precedente crea un gruppo di risorse denominato `myResourceGroup` nel data center `westus`. È possibile cambiare questi valori come si desidera.

    Una volta eseguito, il comando visualizza un output JSON con i dettagli del gruppo di risorse.

1. Eseguire il comando seguente per impostare i valori predefiniti per gruppo di risorse e area per i comandi successivi. In questo modo si evita di specificare questi valori ogni volta. L'esecuzione di questo comando non venera un output.

    ```bash
    az configure --defaults group=myResourceGroup location=westus
    ```

1. Eseguire il comando seguente per creare un **piano di servizio app** che definisce la macchina virtuale sottostante usata dal Servizio app:

    ```bash
    az appservice plan create --name myPlan --sku F1
    ```

    Il comando precedente specifica un piano di hosting gratuito (`--sku F1`), denominato `myPlan`, che usa una macchina virtuale condivisa. Anche il questo caso, il comando non mostra un output JSON quando viene completato.

1. Eseguire il comando seguente per creare il Servizio app, sostituendo `<your_app_name>` con un nome univoco che diventa l'URL, `http://<your_app_name>.azurewebsites.net`. Si noti che il comando di PowerShell è leggermente diverso. L'argomento `--runtime "node|6.9"` indica ad Azure di usare il nodo versione 6.9.x nel server.

    ```bash
    az webapp create --name <your_app_name> --plan myPlan --runtime "node|6.9"
    ```

    > [!TIP]
    > È anche possibile dichiarare la versione del nodo desiderata in `package.json`. Azure applica questa impostazione durante la distribuzione. Ad esempio, la voce `package.json` seguente indica ad Azure di usare almeno la versione 7.0.0 del nodo:
    >
    > ``` json
    > "engines": {
    >     "node": ">7.0.0"
    > },
    > ```

1. Eseguire il comando seguente per aprire un browser con il Servizio app appena creato, anche in questo caso sostituendo `<your_app_name>` con il nome usato:

    ```bash
    az webapp browse --name <your_app_name>
    ```

1. Poiché per l'app non è stato distribuito codice personalizzato, il browser dovrebbe visualizzare una pagina predefinita:

    ![Pagina predefinita del Servizio app](media/azure-cli/azure-default-page.png)

> [!div class="nextstepaction"]
> [Il Servizio app è stato creato](tutorial-vscode-azure-cli-node-04.md) [Si è verificato un problema](https://www.research.net/r/PWZWZ52?tutorial=node-deployment&step=create-website)
