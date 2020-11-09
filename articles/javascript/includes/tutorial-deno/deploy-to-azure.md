---
title: file di inclusione 3
description: file di inclusione 3
ms.topic: tutorial
ms.date: 06/01/2020
ms.custom: devx-track-js
ms.openlocfilehash: d186468b865ddf221c5232d5312018dd3d3858af
ms.sourcegitcommit: 5541f993c01ce356e1b0eaa8f95aea9051c3c21e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/03/2020
ms.locfileid: "93278671"
---
In questo passaggio si distribuisce l'app Deno in Azure usando l'interfaccia della riga di comando di Azure.

1. Creare un gruppo di risorse denominato `deno-quickstart` con il comando seguente:

    ```bash
    az groups create -n deno-quickstart -l eastus
    ```

    Se si decide di cambiare il nome del gruppo di risorse, assicurarsi di aggiornare tutti i flag `-g` nei passaggi seguenti

1. Creare un piano di servizio app denominato `deno-plan` che conterrà il sito Web usando questo comando:

    ```bash
    az appservice plan create -g deno-quickstart -n deno-plan --is-linux
    ```

1. Successivamente, verrà creata l'app Web stessa. Questo comando creerà un nuovo servizio app e lo assocerà al piano creato in precedenza. Impostare il tag `<your-app-name>` sul nome da assegnare all'app Web. Tenere presente che deve essere univoco.

    ```bash
    az webapp create -n <your-app-name> -g deno-quickstart -p deno-plan -i anthonychu/azure-webapps-deno:1.0.2
    ```

    Questo servizio app esegue un'immagine Docker, che fornisce la funzionalità di base per l'esecuzione di qualsiasi codice Deno. Il completamento di questo processo può richiedere alcuni secondi.

1. Dopo la creazione, sarà necessario configurare alcune variabili. A questo scopo, eseguire il comando seguente:

    ```bash
    az webapp config container set -n <your-app-name> -g deno-quickstart -i anthonychu/azure-webapps-deno:1.0.2 -r 'https://index.docker.io' -u '' -p  '' -t true && \
    az webapp config set -n <your-app-name> -g deno-quickstart --startup-file '' && \
    az webapp config appsettings set -n <your-app-name> -g deno-quickstart --settings WEBSITE_RUN_FROM_PACKAGE=1 WEBSITES_ENABLE_APP_SERVICE_STORAGE=true
    ```

A questo punto il servizio app è configurato ed è in attesa di ricevere l'app del passaggio precedente. Tuttavia, per eseguirlo, è necessario che l'app sia inserita in un pacchetto `.zip`. A questo scopo, seguire la procedura seguente:

1. Passare alla cartella `deno-demo`

    ```bash
    cd deno-demo
    ```

1. Eseguire il comando `zip`:

    ```bash
    zip demo demo.ts
    ```

    Il risultato di questo comando sarà un file denominato `demo.zip` situato nella stessa cartella del file `demo.ts`.

1. Dopo la creazione del pacchetto è possibile caricare il file nel servizio app da eseguire:

    ```bash
    az webapp deployment source config-zip -n <your-app-name> -g deno-quickstart --src ./demo.zip && \
    az webapp config set -n <your-app-name> -g deno-quickstart --startup-file 'deno run --allow-net demo.ts'
    ```

1. Testare l'applicazione passando a `https://<your-app-name>.azurewebsites.net`