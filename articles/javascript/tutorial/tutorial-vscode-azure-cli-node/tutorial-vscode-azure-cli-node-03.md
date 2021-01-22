---
title: Creare il Servizio app di Azure dall'interfaccia della riga di comando di Azure per ospitare l'app
description: Parte 3 dell'esercitazione, creare il servizio app con l'interfaccia della riga di comando di Azure
ms.topic: tutorial
ms.date: 01/13/2021
ms.custom: devx-track-js, devx-track-azurecli
ms.openlocfilehash: ad8e0836d62102521b557fd45b24291d52db447c
ms.sourcegitcommit: 593d177cfb5f56f236ea59389e43a984da30f104
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/18/2021
ms.locfileid: "98561517"
---
# <a name="create-the-app-service"></a>Creare il servizio app

[Passaggio precedente: Creare l'app](tutorial-vscode-azure-cli-node-02.md)

In questo passaggio si usa l'interfaccia della riga di comando di Azure per creare il Servizio app di Azure per ospitare il codice dell'app.

<a name="create-resource-group"></a>

## <a name="create-resource-group-and-set-as-default-value"></a>Crea gruppo di risorse e imposta come valore predefinito

1. In un terminale o al prompt dei comandi usare il comando seguente per creare un **gruppo di risorse** per il Servizio app. Un gruppo di risorse è essenzialmente una raccolta denominata delle risorse di un'app in Azure, ad esempio un sito Web, un database, Funzioni di Azure e così via.

    ```azurecli
    az group create --name myResourceGroup --location westus
    ```

    Il comando [`az group create`](/cli/azure/group#az_group_create) precedente dell'interfaccia della riga di comando di Azure crea un gruppo di risorse denominato `myResourceGroup` nel data center `westus`. È possibile cambiare questi valori come si desidera.

    Una volta eseguito, il comando visualizza un output JSON con i dettagli del gruppo di risorse.

1. Eseguire il comando [`az configure`](/cli/azure/config) seguente dell'interfaccia della riga di comando di Azure per impostare i valori predefiniti di gruppo di risorse e area per i comandi successivi. In questo modo si evita di specificare questi valori ogni volta. L'esecuzione di questo comando non venera un output.

    ```azurecli
    az configure --defaults group=myResourceGroup location=westus
    ```

## <a name="create-and-deploy-web-app-service-with-azure-cli-command"></a>Creare e distribuire il servizio app Web con il comando CLI di Azure

Eseguire il comando dell'interfaccia della riga di comando di Azure seguente,  [`az webapp up`](/cli/azure/webapp#az_webapp_up) , per creare e distribuire l'app del servizio app. Sostituire `<your_app_name>` con un nome univoco che diventa l'URL `http://<your_app_name>.azurewebsites.net` . 

```azurecli
az webapp up --name <your_app_name> --logs --launch-browser
```

Il `--logs` comando Visualizza il flusso di log subito dopo l'avvio del webapp. Il `--launch-browser` comando apre il browser predefinito per la nuova app. È possibile usare lo stesso comando per ridistribuire l'intera app. 

## <a name="troubleshooting"></a>Risoluzione dei problemi

* Se si riceve un messaggio di errore relativo a un parametro necessario ma mancante, `--resource-group`, tornare all'inizio dell'articolo e impostare i valori predefiniti oppure specificare il parametro e il valore. 

## <a name="next-steps"></a>Passaggi successivi

Altre informazioni sui comandi per la webapp con il gruppo di comandi Azure [webapp](/cli/azure/webapp) o il gruppo di comandi del [servizio app](/cli/azure/appservice) di Azure. 

> [!div class="nextstepaction"]
> [Il Servizio app è stato creato](tutorial-vscode-azure-cli-node-04.md) [Si è verificato un problema](https://www.research.net/r/PWZWZ52?tutorial=node-deployment&step=create-website)
