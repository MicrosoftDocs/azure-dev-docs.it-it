---
title: Distribuire app Deno nel servizio app di Azure dall'interfaccia della riga di comando di Azure
description: In questa esercitazione si distribuisce un'applicazione Deno nel servizio app di Azure, in Linux o in Windows, usando l'interfaccia della riga di comando di Azure.
ms.topic: tutorial
ms.date: 10/13/2020
ms.custom: scenarios:getting-started, languages:JavaScript, devx-track-javascript
ms.openlocfilehash: f1f8c93954d2e4cbb8f5bd525a518aae03ec9667
ms.sourcegitcommit: 593d177cfb5f56f236ea59389e43a984da30f104
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/18/2021
ms.locfileid: "98561074"
---
# <a name="deploy-deno-apps-to-azure-app-service-from-the-azure-cli"></a>Distribuire app Deno nel servizio app di Azure dall'interfaccia della riga di comando di Azure

Distribuire un'applicazione Deno nel Servizio app di Azure (in Linux o Windows) usando l'interfaccia della riga di comando di Azure. Il runtime di Deno per il Servizio app di Azure viene fornito con un'immagine Docker sperimentale. 

![Esecuzione del server demo](../media/deploy-azure/deno-hello-world.png)

## <a name="1-prepare-your-environment"></a>1. Preparare l'ambiente

- Un account Azure con una sottoscrizione attiva. [Crearne uno gratuito](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-appservice-deno&mktingSource=vscode-tutorial-appservice-deno)
- Installare [Visual Studio Code](https://code.visualstudio.com/)
- Installare [Deno](https://deno.land/#installation)
[!INCLUDE [Azure CLI](../../includes/azure-cli-prepare-your-environment-no-header.md)]


## <a name="2-sign-in-to-azure-cli"></a>2. Accedere all'interfaccia della riga di comando di Azure

Se si usa l'interfaccia della riga di comando di Azure dalla riga di comando locale, prima di usare i comandi dell'interfaccia della riga di comando è necessario eseguire l'accesso con [az login](/cli/azure/reference-index#az-login).

[!INCLUDE [interactive-login](../../azure-cli/includes/interactive-login.md)]

3. Dopo l'accesso, viene visualizzato un elenco di sottoscrizioni associate all'account Azure. Le informazioni della sottoscrizione con `isDefault: true` rappresentano la sottoscrizione attualmente attivata dopo l'accesso. 

4. Se è necessario selezionare un'altra sottoscrizione, usare il comando [az account set](/cli/azure/account#az-account-set) con l'ID sottoscrizione a cui passare. Per altre informazioni sulla selezione delle sottoscrizioni, vedere [Usare più sottoscrizioni di Azure](/cli/azure/manage-azure-subscriptions-azure-cli).

## <a name="3-create-local-deno-api-app"></a>3. Creare l'app per le API Deno locale

Creare un'app Deno usando il server Web predefinito di Deno. L'app verrà quindi eseguita in locale.

1. In un terminale o un prompt dei comandi passare alla posizione in cui creare una nuova cartella dell'app denominata`deno-demo`.

1. Creare un nuovo file denominato `demo.ts`.
1. Deno accetta il codice in esecuzione direttamente dagli URL. Scrivere un server HTTP che risponde a tutte le richieste con "Hello World". Usare il codice seguente:

    ```typescript
    import { serve } from "https://deno.land/std@0.54.0/http/server.ts"
    const handler = serve({ port: 80 })

    console.log("Serving at 80")

    for await (const req of handler) {
     req.respond({ body: "Hello World!\n" })
    }
    ```

1. Eseguire l'app con lo script seguente:

    ```bash
    deno run --allow-net ./demo.ts
    ```

1. Testare l'app aprendo un browser all'indirizzo `http://localhost:80`. Il sito dovrebbe essere simile al seguente:

    ![Esecuzione del server demo](../media/deploy-azure/deno-hello-world.png)

    È anche possibile eseguire questo codice digitando `deno run --allow-net https://gist.githubusercontent.com/khaosdoctor/cd2bbb28e682feb8d20a7aba47fc1e17/raw/92de998fd11f2a24ae40bbcb84f5262cfe9389b2/deno-demo.ts`

1. Premere **CTRL**+**C** nel terminale per arrestare il server.

## <a name="4-deploy-deno-app-to-azure"></a>4. Distribuire l'app Deno in Azure

Distribuire l'app Deno in Azure usando l'interfaccia della riga di comando di Azure.

1. Creare un gruppo di risorse denominato `deno-quickstart` con il comando seguente:

    ```azurecli
    az group create --name deno-quickstart --location eastus
    ```

    Se si decide di cambiare il nome del gruppo di risorse, assicurarsi di aggiornare tutti i flag `-g` nei passaggi seguenti

1. Creare un piano di servizio app denominato `deno-plan` che conterrà il sito Web usando questo comando:

    ```azurecli
    az appservice plan create --resource-group deno-quickstart --name deno-plan --is-linux
    ```

1. Successivamente, verrà creata l'app Web stessa. Questo comando creerà un nuovo servizio app e lo assocerà al piano creato in precedenza. Impostare il tag `<your-app-name>` sul nome da assegnare all'app Web. Tenere presente che deve essere univoco.

    ```azurecli
    az webapp create -n <your-app-name> --resource-group deno-quickstart -p deno-plan -i anthonychu/azure-webapps-deno:1.0.2
    ```

    Questo servizio app esegue l'immagine Docker `anthonychu/azure-webapps-deno:1.0.2`, che fornisce la funzionalità di base per l'esecuzione di qualsiasi codice Deno. Il completamento di questo processo può richiedere alcuni secondi.

## <a name="5-configure-app-service-deno-container"></a>5. configurare il contenitore deno del servizio app

1. Indicare all'app Web dove ottenere l'immagine del contenitore Docker per il nome dell'immagine Deno sperimentale:

    ```azurecli
    az webapp config container set --name <your-app-name> --resource-group deno-quickstart -i anthonychu/azure-webapps-deno:1.0.2 -r 'https://index.docker.io' -u '' -p  '' -t true
    ```

1. Configurare l'app Web in modo che non abbia alcun file di avvio:

    ```azurecli
    az webapp config set --name <your-app-name> --resource-group deno-quickstart --startup-file ''

1. Set the webapp to have runtime environment variables:

    ```azurecli
    az webapp config appsettings set --name <your-app-name> --resource-group deno-quickstart --settings WEBSITE_RUN_FROM_PACKAGE=1 WEBSITES_ENABLE_APP_SERVICE_STORAGE=true
    ```

## <a name="6-configure-deno-app-deployment-to-web-app"></a>6. Configurare la distribuzione dell'app Deno in un'app Web 

L'app Web di Azure è configurata e pronta per l'uso ma non ha ancora il pacchetto Deno. Inserire l'app in un pacchetto `.zip`, indicare all'app Web che il file si trova nel computer locale, quindi configurare il file di avvio, che si trova nel file ZIP. 

1. Passare alla cartella `deno-demo`

    ```bash
    cd deno-demo
    ```

1. Eseguire il comando `zip`:

    ```bash
    zip demo demo.ts
    ```

    Il risultato di questo comando sarà un file denominato `demo.zip` situato nella stessa cartella del file `demo.ts`.

1. Configurare il pacchetto come origine del codice per la distribuzione:

    ```azurecli
    az webapp deployment source config-zip --name <your-app-name> --resource-group deno-quickstart --src ./demo.zip
    ```

1. Configurare il file nel pacchetto per l'esecuzione:

    ```azurecli
    az webapp config set --name <your-app-name> --resource-group deno-quickstart --startup-file 'deno run --allow-net demo.ts'
    ```

1. Testare l'applicazione passando a `https://<your-app-name>.azurewebsites.net`. 

## <a name="7-clean-up-resources"></a>7. Pulire le risorse

Eliminare il gruppo di risorse, eliminando quindi anche le risorse dell'app Web, con il comando seguente:

```azurecli
az group delete deno-quickstart
```

## <a name="next-steps"></a>Passaggi successivi

Altre informazioni su:
* [Informazioni su come configurare le impostazioni dell'app](../how-to/configure-web-app-settings.md)
* [Eseguire la distribuzione nel Servizio app](./deploy-nodejs-azure-app-service-with-visual-studio-code.md) con le estensioni di Visual Studio Code
* [Eseguire la distribuzione in una macchina virtuale](./nodejs-virtual-machine-vm/introduction.md)
* [Distribuire la funzione Deno](https://github.com/anthonychu/azure-functions-deno-worker) come [gestore personalizzato](/azure/azure-functions/functions-custom-handlers)