---
title: Installare la libreria client di Application Insights
description: Aggiungere la libreria client di Azure SDK al codice nella macchina virtuale per iniziare a raccogliere i log delle app nel cloud di Azure.
ms.topic: tutorial
ms.date: 11/13/2020
ms.custom: devx-track-js
ms.openlocfilehash: a6fb29dd059922ead67b4cef5107c9407f40e294
ms.sourcegitcommit: ed749b136f0d6b876fd5866ba4a151c73af5b71f
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/17/2020
ms.locfileid: "94674711"
---
# <a name="5-install-azure-sdk-client-library-to-monitor-web-app"></a>5. Installare la libreria client di Azure SDK per monitorare l'app Web

In questo passaggio, aggiungere la libreria client di Azure SDK al codice nella macchina virtuale per iniziare a raccogliere i log delle app nel cloud di Azure.

## <a name="edit-indexjs-for-logging-with-azure-monitor-application-insights"></a>Modificare il file index.js per la registrazione con Application Insights di Monitoraggio di Azure

1. Per modificare il file `index.js`, usare l'editor di testo [Nano](https://www.nano-editor.org/dist/latest/nano.html#Editor-Basics) fornito nella macchina virtuale. 

    ```bash
    sudo nano index.js
    ```

1. Modificare il file `index.js` per aggiungere la libreria client e il codice di registrazione, evidenziato di seguito. Molte shell Bash consentono di copiare e incollare direttamente in Nano. 

    :::code language="JavaScript" source="~/../js-e2e-vm/index-logging.js" highlight="5-28" :::

1. Al termine, usare `Control+x` per uscire, quindi `y` per salvare le modifiche. Le modifiche apportate all'app Web vengono controllate da PM2; questa modifica ha causato il riavvio dell'app, senza dover riavviare la macchina virtuale. 

1. In un Web browser testare l'app con la nuova route `trace`:

    ```http
    http://REPLACE-WITH-YOUR-IP/trace
    ```

    Il browser mostra la risposta `tracing...` con l'indirizzo IP dell'utente.

## <a name="viewing-the-vm-logs-for-nginx-and-pm2"></a>Visualizzazione dei log della VM per NGINX e PM2

La macchina virtuale raccoglie i log per NGINX e PM2, che sono disponibili per la visualizzazione.

| Service | Percorso log|
|--|--|
|NGINX| /var/log/nginx/access.log|
|PM2| /var/log/pm2.log|

1. Visualizzare il log delle macchine virtuali per il servizio proxy NGINX. Nella stessa shell Bash, usare il comando seguente per visualizzare il log:

    ```bash
     cat /var/log/nginx/access.log
    ```

    Il log include la chiamata dal computer locale. 

    ```console
     "GET /trace HTTP/1.1" 200 10 "-"
    ```

1. Visualizzare il log della macchina virtuale per il servizio PM2. Nella stessa shell Bash, usare il comando seguente per visualizzare il log:

    ```bash
     cat /var/log/pm2.log
    ```

    Il log include la chiamata dal computer locale. 

    ```console
    Hello world app listening on port 3000!
    testing from trace route 76.22.73.183
    ```

1. L'esercitazione non si connetterà di nuovo alla macchina virtuale. Usare il comando seguente nella shell Bash per uscire dalla connessione SSH. 

    ```bash
    exit
    ```

## <a name="next-step"></a>Passaggio successivo

> [!div class="nextstepaction"]
> [Visualizzare i log nel portale di Azure](azure-monitor-application-insights-logs.md) 