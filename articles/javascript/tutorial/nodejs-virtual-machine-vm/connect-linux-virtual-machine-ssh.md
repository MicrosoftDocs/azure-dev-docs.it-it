---
title: Da SSH a macchina virtuale
description: Usare SSH per connettersi alla macchina virtuale Linux.  Se si usa un sistema operativo Mac, Windows o Linux moderno, è necessario che sia già installato il client SSH basato su terminale.
ms.topic: tutorial
ms.date: 11/13/2020
ms.custom: devx-track-js
ms.openlocfilehash: 7c496a907b6cbac894e92d0ecccdf32411a370ab
ms.sourcegitcommit: ed749b136f0d6b876fd5866ba4a151c73af5b71f
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/17/2020
ms.locfileid: "94674721"
---
# <a name="4-connect-to-linux-virtual-machine-using-ssh"></a>4. Connettersi a una macchina virtuale Linux con SSH

In questa sezione dell'esercitazione usare SSH in un terminale per connettersi alla macchina virtuale. [SSH](https://www.ssh.com/ssh/) è uno strumento comune fornito con numerose shell moderne, tra cui Azure Cloud Shell. 

## <a name="connect-with-ssh-and-change-web-app"></a>Connettersi con SSH e modificare l'app Web

Usare la stessa finestra del terminale o della shell come nei passaggi precedenti. 

1. Connettersi alla macchina virtuale remota con il comando seguente. Questo processo presuppone che il client SSH riesca a trovare le chiavi SSH, create come parte della creazione della macchina virtuale e posizionate nel computer locale. Se viene chiesto se si vuole continuare la connessione, rispondere `yes`. Al termine della connessione, il prompt del terminale dovrebbe cambiare indicando la macchina virtuale remota. 

    Sostituire `YOUR-PUBLIC-IP-ADDRESS` con l'indirizzo IP pubblico della macchina virtuale. 

    ```console
    ssh azureuser@YOUR-PUBLIC-IP-ADDRESS
    ``` 

1. Usare il comando seguente per comprendere la posizione nella macchina virtuale. La posizione dovrebbe essere la radice azureuser: `/home/azureuser`. 

    ```bash
    pwd
    ```

1. L'app Web si trova nella sottodirectory `myapp`. Passare alla directory `myapp` ed elencare il contenuto:

    ```bash
    cd myapp && ls -l
    ```

    Verrà visualizzato contenuto che rappresenta il repository GitHub clonato nella macchina virtuale e i file del pacchetto npm:
    
    ```console
    -rw-r--r--   1 root root   891 Nov 11 20:23 cloud-init-github.txt
    -rw-r--r--   1 root root  1347 Nov 11 20:23 index-logging.js
    -rw-r--r--   1 root root   282 Nov 11 20:23 index.js
    drwxr-xr-x 190 root root  4096 Nov 11 20:23 node_modules
    -rw-r--r--   1 root root 84115 Nov 11 20:23 package-lock.json
    -rw-r--r--   1 root root   329 Nov 11 20:23 package.json
    -rw-r--r--   1 root root   697 Nov 11 20:23 readme.md
    ```

1. Installare la [libreria client di Azure SDK per Application Insights](https://www.npmjs.com/package/applicationinsights).

    ```bash
    sudo npm install --save applicationinsights
    ```

1. Usare l'editor [Nano](https://www.nano-editor.org/dist/latest/nano.html#Editor-Basics) per modificare il file `package.json`.

    ```bash
    sudo nano package.json
    ```

1. Modificare lo script di avvio del file includendo una variabile di ambiente. Sostituire `REPLACE-WITH-YOUR-KEY` con il valore della chiave di strumentazione.

    ```json
    "start": "APPINSIGHTS_INSTRUMENTATIONKEY=REPLACE-WITH-YOUR-KEY pm2 start index.js --watch --log /var/log/pm2.log"
    ```

1. Arrestare e riavviare PM2 con i comandi seguenti:

    ```bash
    sudo npm run-script stop && sudo npm start
    ```

    La libreria client di Azure è ora presente nella directory _node_modules_ e la chiave viene passata all'app come variabile di ambiente. Il passaggio successivo consiste nell'aggiungere il codice necessario a `index.js`. 

1. Lasciare il terminale aperto e connesso alla VM: verrà usato nel passaggio successivo.

## <a name="next-step"></a>Passaggio successivo

> [!div class="nextstepaction"]
> [Aggiungere il codice client di Azure SDK per accedere al cloud di Azure](azure-monitor-application-insights-nodejs-expressjs-code.md) 