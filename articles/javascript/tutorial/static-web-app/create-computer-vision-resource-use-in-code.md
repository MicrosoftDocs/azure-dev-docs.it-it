---
title: Creare una risorsa di Visione artificiale
description: Creare la risorsa di Visione artificiale di Servizi cognitivi e impostarla su variabili di ambiente.
ms.topic: tutorial
ms.date: 11/13/2020
ms.custom: devx-track-js
ms.openlocfilehash: 4ac324171f47ab8795169c5dd453d1e6451e8906
ms.sourcegitcommit: 4dac39849ba2e48034ecc91ef578d11aab796e58
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/20/2020
ms.locfileid: "94993440"
---
# <a name="3-create-computer-vision-resource-and-use-in-code"></a>3. Creare una risorsa di Visione artificiale e usarla nel codice

In questo passaggio, creare la risorsa di Visione artificiale e impostarla su variabili di ambiente. 

## <a name="create-azure-resources"></a>Creare le risorse di Azure

La creazione di un gruppo di risorse consente di trovare facilmente le risorse ed eliminarle al termine dell'operazione.

Al termine di questa procedura, è necessario avere **la chiave e l'endpoint** per la risorsa.

1. In un terminale o in una shell Bash, immettere il [comando dell'interfaccia della riga di comando di Azure per creare un gruppo di risorse di Azure](/cli/azure/group?view=azure-cli-latest#az_group_create) con il nome `rg-demo`:

    ```azurecli
    az group create \
        --location eastus \
        --name rg-demo 
    ```
1. Eseguire il comando seguente per [creare una risorsa di Visione artificiale](/cli/azure/cognitiveservices/account?view=azure-cli-latest#az-cognitiveservices-account-create):


    ```azurecli
    az cognitiveservices account create \
        --name demo-ComputerVision \
        --resource-group rg-demo \
        --kind ComputerVision \
        --sku F0 \
        --location eastus \
        --yes
    ```

1. Nei risultati trovare e copiare il valore della `properties.endpoint`. che sarà necessario più avanti.

    ```json
    ...
    "properties":{
        ...
        "endpoint": "https://eastus.api.cognitive.microsoft.com/",
        ...
    }
    ...
    ```

1. Eseguire il [comando](/cli/azure/cognitiveservices/account/keys?view=azure-cli-latest#az-cognitiveservices-account-keys-list) seguente per ottenere le chiavi. 

    ```azurecli
    az cognitiveservices account keys list \
    --name demo-ComputerVision \
    --resource-group rg-demo
    ```

1. Copiare una delle chiavi: sarà necessaria in un secondo momento.

    ```json
    {
      "key1": "8eb7f878bdce4e96b26c89b2b8d05319",
      "key2": "c2067cea18254bdda71c8ba6428c1e1a"
    }
    ```

## <a name="add-environment-variables-to-your-local-environment"></a>Aggiungere variabili di ambiente all'ambiente locale

Per usare la risorsa, la chiave e l'endpoint devono essere disponibili nel codice locale. Questa codebase li archivia nelle variabili di ambiente:

* REACT_APP_COMPUTERVISIONKEY
* REACT_APP_COMPUTERVISIONENDPOINT 

1. Eseguire il comando riportato di seguito per aggiungere queste variabili all'ambiente.

    # <a name="bash"></a>[Bash](#tab/bash)
    
    ```bash
    export REACT_APP_COMPUTERVISIONKEY="REPLACE-WITH-YOUR-KEY"
    export REACT_APP_COMPUTERVISIONENDPOINT="REPLACE-WITH-YOUR-ENDPOINT"
    ```
    
    # <a name="cmd"></a>[cmd](#tab/cmd)
    
    ```cmd
    set REACT_APP_COMPUTERVISIONKEY="REPLACE-WITH-YOUR-KEY"
    set REACT_APP_COMPUTERVISIONENDPOINT="REPLACE-WITH-YOUR-ENDPOINT"
    ```
    ---

## <a name="add-environment-variables-to-your-remote-environment"></a>Aggiungere variabili di ambiente all'ambiente remoto

Quando si usano app Web statiche di Azure, le variabili di ambiente, come i segreti, devono essere passate dall'azione GitHub all'app Web statica. L'azione GitHub compila l'app, inclusi la chiave e l'endpoint di Visione artificiale passati dai segreti di GitHub per tale repository, quindi esegue il push del codice con le variabili di ambiente nell'app Web statica.

1. Nel repository GitHub in un Web browser, selezionare **Settings** (Impostazioni), quindi **Secrets** (Segreti) e infine **New repository secret** (Nuovo segreto del repository).

    :::image type="content" source="../../media/static-web-app/browser-screenshot-github-create-new-repository-secret.png" alt-text="Screenshot parziale del browser dell'esempio di Visione artificiale di Servizi cognitivi dell'app React per l'analisi delle immagini prima della chiave e del set di endpoint.":::

1. Immettere lo stesso nome e valore per l'endpoint usato nella sezione precedente. Quindi, creare un altro segreto con lo stesso nome e il medesimo valore della chiave usati nella sezione precedente. 
    
    :::image type="content" source="../../media/static-web-app/browser-screenshot-github-add-secret.png" alt-text="Immettere lo stesso nome e valore per l'endpoint. Quindi, creare un altro segreto con lo stesso nome e il medesimo valore della chiave.":::

## <a name="run-react-app-with-computervision-resource"></a>Eseguire l'app React con la risorsa Visione artificiale

Questa app React controlla le modifiche per ricompilare ed eseguire nuovamente l'app. Apportare una modifica per forzare una ricompilazione.

1. **Immettere una nuova riga** in `./src/VisualAi.js` subito dopo la prima riga vuota (riga 4). Questa modifica provoca una ricompilazione del sito Web in esecuzione localmente.

    :::image type="content" source="../../media/static-web-app/browser-screenshot-react-computervision-app-start-up.png" alt-text="Screenshot parziale del browser dell'esempio di Visione artificiale di Servizi cognitivi dell'app React pronto per l'immissione dell'URL o la selezione di INVIO.":::

1. Lasciare vuoto il campo di testo e **selezionare il pulsante Analyze**. 

    :::image type="content" source="../../media/static-web-app/browser-screenshot-react-computervision-app-image-analysis-result.png" alt-text="Screenshot parziale del browser dei risultati dell'esempio di Visione artificiale di Servizi cognitivi dell'app React.":::

    L'immagine viene selezionata in modo casuale da un catalogo di immagini definite in `./src/DefaultImages.js`. 

1. Continuare a selezionare il pulsante **Analyze** per visualizzare le altre immagini e i risultati. 

## <a name="next-step"></a>Passaggio successivo

> [!div class="nextstepaction"]
> [Creare l'app Web statica di Azure](create-static-web-app-visual-studio-code-extension.md)