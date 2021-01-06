---
title: Scaricare l'app React di esempio
description: L'app di esempio completa viene fornita in un repository GitHub. Creare una copia tramite fork del repository, installare le dipendenze ed eseguire localmente.
ms.topic: tutorial
ms.date: 11/13/2020
ms.custom: devx-track-js
ms.openlocfilehash: 5050aca4c34f20f8b4c7cb7a8460815516abd7c7
ms.sourcegitcommit: 1c508f5ba73a12e4baeacc88ad9a8359301acb50
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/18/2020
ms.locfileid: "97687458"
---
# <a name="3-download-and-run-the-react-cognitive-services-image-analyzer-app"></a>3. Scaricare ed eseguire l'app React Image Analyzer di Servizi cognitivi

L'app di esempio completa viene fornita in un repository GitHub. Creare una copia tramite fork del repository, installare le dipendenze ed eseguire localmente.

## <a name="fork-the-sample-repo"></a>Creare una copia tramite fork del repository di esempio

Creare una copia tramite fork del repository, invece di limitarsi a clonarlo nel computer locale, per poter avere un proprio repository GitHub in cui eseguire il push delle modifiche. 

1. Aprire una finestra o una scheda del browser a parte per accedere a <a href="https://github.com/login" target="_blank">GitHub</a>. 
1. Passare al <a href="https://github.com/Azure-Samples/js-e2e-client-cognitive-services" target="_blank">repository di esempio di GitHub</a> nel Web browser. 

    ```http
    https://github.com/Azure-Samples/js-e2e-client-cognitive-services
    ```

1. In alto a destra nella pagina selezionare **Fork**. 
1. Selezionare **Code** quindi copiare l'URL del percorso per il fork. 

    :::image type="content" source="../../media/static-web-app/browser-screenshot-clone-github-sample-repository-fork.png" alt-text="Screenshot parziale del sito Web GitHub, selezionare **Code**, quindi copiare il percorso del fork.":::    

## <a name="create-local-development-environment"></a>Creare un ambiente di sviluppo locale

1. In una finestra terminale o bash clonare il fork nel computer locale. Sostituire `YOUR-ACCOUNT-NAME` con il nome dell'account GitHub.

    ```bash
    git clone https://github.com/YOUR-ACCOUNT-NAME/js-e2e-client-cognitive-services
    ```

1. Passare alla nuova directory e installare le dipendenze. 

    ```bash
    cd js-e2e-client-cognitive-services && npm install
    ```

## <a name="run-sample"></a>Eseguire l'esempio

1. Eseguire l'esempio. 

    ```bash
    npm start
    ```

    :::image type="content" source="../../media/static-web-app/browser-screenshot-react-cognitive-services-app-before-authentication.png" alt-text="Screenshot parziale del browser dell'esempio di Visione artificiale di Servizi cognitivi dell'app React per l'analisi delle immagini prima della chiave e del set di endpoint.":::    
    
1. Arrestare l'app. Chiudere la finestra del terminale o usare `control+c` nel terminale. 
    
## <a name="next-step"></a>Passaggio successivo

> [!div class="nextstepaction"]
> [Creare una risorsa di Visione artificiale e usarla nel codice](create-computer-vision-resource-use-in-code.md) 