---
title: Creare il Servizio app di Azure dall'interfaccia della riga di comando di Azure per ospitare l'app
description: Parte 3 dell'esercitazione, creare il servizio app con l'interfaccia della riga di comando di Azure
ms.topic: tutorial
ms.date: 12/18/2020
ms.custom: devx-track-js, devx-track-azurecli
ms.openlocfilehash: abd434d2222e5bdd758be42856a569e24def08f8
ms.sourcegitcommit: 1c508f5ba73a12e4baeacc88ad9a8359301acb50
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/18/2020
ms.locfileid: "97687437"
---
# <a name="create-the-app-service"></a>Creare il servizio app

[Passaggio precedente: Creare l'app](tutorial-vscode-azure-cli-node-02.md)

In questo passaggio si usa l'interfaccia della riga di comando di Azure per creare il Servizio app di Azure per ospitare il codice dell'app.

1. In un terminale o al prompt dei comandi usare il comando seguente per creare un **gruppo di risorse** per il Servizio app. Un gruppo di risorse è essenzialmente una raccolta denominata delle risorse di un'app in Azure, ad esempio un sito Web, un database, Funzioni di Azure e così via.

    ```azurecli
    az group create --name myResourceGroup --location westus
    ```

    Il comando [`az group create`](/cli/azure/group?view=azure-cli-latest#az_group_create) precedente dell'interfaccia della riga di comando di Azure crea un gruppo di risorse denominato `myResourceGroup` nel data center `westus`. È possibile cambiare questi valori come si desidera.

    Una volta eseguito, il comando visualizza un output JSON con i dettagli del gruppo di risorse.

1. Eseguire il comando [`az configure`](/cli/azure/config?view=azure-cli-latest) seguente dell'interfaccia della riga di comando di Azure per impostare i valori predefiniti di gruppo di risorse e area per i comandi successivi. In questo modo si evita di specificare questi valori ogni volta. L'esecuzione di questo comando non venera un output.

    ```azurecli
    az configure --defaults group=myResourceGroup location=westus
    ```

1. Eseguire il comando [`az appservice plan create`](/cli/azure/appservice/plan?view=azure-cli-latest#az_appservice_plan_create) seguente dell'interfaccia della riga di comando di Azure per creare un **piano di servizio app** che definisce la macchina virtuale sottostante usata dal servizio app:

    ```azurecli
    az appservice plan create --name myPlan --sku F1
    ```

    Il comando precedente specifica un [piano di hosting gratuito](../../core/what-is-azure-for-javascript-development.md#free-tier-resources) (`--sku F1`), denominato `myPlan`, che usa una macchina virtuale condivisa. 

1. Eseguire il comando [`az webapp create`](/cli/azure/webapp?view=azure-cli-latest#az_webapp_create) seguente dell'interfaccia della riga di comando di Azure per creare il servizio app, sostituendo `<your_app_name>` con un nome univoco che diventa l'URL `http://<your_app_name>.azurewebsites.net`, con la [versione più recente del runtime Node.js](/cli/azure/webapp?view=azure-cli-latest#az_webapp_list_runtimes&preserve-view=false). 

    ```azurecli
    az webapp create --name <your_app_name> --plan myPlan -g --runtime "node|12-lts"
    ```


1. Eseguire il comando [`az webapp browse`](/cli/azure/webapp?view=azure-cli-latest#az_webapp_browse) seguente dell'interfaccia della riga di comando di Azure per aprire un browser con il servizio app appena creato, anche in questo caso sostituendo `<your_app_name>` con il nome usato:

    ```azurecli
    az webapp browse --name <your_app_name>
    ```

1. Poiché per l'app non è stato distribuito codice personalizzato, il browser dovrebbe visualizzare una pagina predefinita:

    ![Pagina predefinita del Servizio app](../../media/azure-cli/azure-default-page.png)

## <a name="troubleshooting"></a>Risoluzione dei problemi

* Se si riceve un messaggio di errore relativo a un parametro necessario ma mancante, `--resource-group`, tornare all'inizio dell'articolo e impostare i valori predefiniti. 

> [!div class="nextstepaction"]
> [Il Servizio app è stato creato](tutorial-vscode-azure-cli-node-04.md) [Si è verificato un problema](https://www.research.net/r/PWZWZ52?tutorial=node-deployment&step=create-website)
