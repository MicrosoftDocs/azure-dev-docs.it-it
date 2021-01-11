---
title: Creare una macchina virtuale Linux
description: Usare l'interfaccia della riga di comando di Azure per creare e configurare la macchina virtuale. A questo punto dell'esercitazione, l'utente dovrebbe aver aperto una finestra del terminale ed eseguito l'accesso al cloud di Azure con l'interfaccia della riga di comando di Azure nella sottoscrizione in cui intende creare la macchina virtuale.
ms.topic: tutorial
ms.date: 01/05/2021
ms.custom: devx-track-js
ms.openlocfilehash: a618c9584775a7c384f05ef01a563943c48f2b3a
ms.sourcegitcommit: 075f39972e390e79ed09a3fcfdbfc776727e08fc
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/06/2021
ms.locfileid: "97952503"
---
# <a name="3-create-linux-virtual-machine-using-azure-cli"></a>3. Creare una macchina virtuale Linux usando l'interfaccia della riga di comando di Azure

In questa sezione dell'esercitazione, usare l'interfaccia della riga di comando di Azure per creare e configurare la macchina virtuale. A questo punto dell'esercitazione, l'utente dovrebbe aver aperto una finestra del terminale ed eseguito l'accesso al cloud di Azure nella sottoscrizione in cui intende creare la macchina virtuale. 

È possibile completare tutti i passaggi da una singola istanza dell'interfaccia della riga di comando di Azure. Se si chiude la finestra o si passa da una finestra all'altra quando si usa l'interfaccia della riga di comando di Azure, ad esempio tra Cloud Shell e il terminale locale, sarà necessario eseguire di nuovo l'accesso. 

## <a name="create-a-cloud-init-file-to-expedite-linux-virtual-machine-creation"></a>Creare un file cloud-init per velocizzare la creazione di macchine virtuali Linux

Questa esercitazione usa un file di configurazione cloud-init per creare sia il server proxy inverso NGINX che il server Express.js. NGINX viene usato per trasferire la porta Express.js (3000) alla porta pubblica (80). 

Il comando `runcmd` ha diverse attività:
* scaricare e installare Node.js
* clonare il repository Express.js di esempio
* installare le dipendenze Express.js
* avviare l'app Express.js con PM2

1. Creare un file locale denominato `cloud-init-github.txt` e salvarvi il contenuto seguente oppure è possibile [salvare il file del repository](https://github.com/Azure-Samples/js-e2e-vm/blob/main/cloud-init-github.txt) nel computer locale. Il file [cloud-init](https://cloudinit.readthedocs.io/en/latest/topics/examples.html#yaml-examples) formattato deve esistere nella stessa cartella del percorso del terminale per i comandi dell'interfaccia della riga di comando di Azure.

    :::code language="yaml" source="~/../js-e2e-vm/cloud-init-github.txt" :::

## <a name="create-a-virtual-machine-resource"></a>Creare una risorsa di macchina virtuale 

Immettere il [comando dell'interfaccia della riga di comando di Azure](/cli/azure/vm?view=azure-cli-latest#az_vm_create) in un terminale per creare una risorsa di Azure di una macchina virtuale Linux. Il comando crea la macchina virtuale dal file cloud-init e genera le chiavi SSH. Il comando in esecuzione visualizza la posizione di archiviazione delle chiavi. 

```azurecli
az vm create \
  --resource-group rg-demo-vm-eastus \
  --name demo-vm \
  --location eastus \
  --image UbuntuLTS \
  --admin-username azureuser \
  --generate-ssh-keys \
  --custom-data cloud-init-github.txt
```

Il processo potrebbe richiedere alcuni minuti. Al termine del processo, l'interfaccia della riga di comando di Azure restituisce le informazioni sulla nuova risorsa. Mantenere il valore `publicIpAddress`, necessario per visualizzare l'app Web in un browser e per connettersi alla macchina virtuale. 
     

## <a name="open-port-for-virtual-machine"></a>Aprire la porta per la macchina virtuale

Al momento della creazione, la macchina virtuale _non_ ha porte aperte. Aprire la porta 80 con il [comando dell'interfaccia della riga di comando di Azure](/cli/azure/vm?view=azure-cli-latest#az_vm_open_port) seguente in modo che l'app Web sia disponibile pubblicamente:

```azurecli
az vm open-port \
  --port 80 \
  --resource-group rg-demo-vm-eastus \
  --name demo-vm
```

## <a name="browse-to-web-site"></a>Passare al sito Web

1. Usare l'indirizzo IP pubblico in un Web browser per assicurarsi che la macchina virtuale sia disponibile e in esecuzione. Modificare l'URL in modo da usare il valore di `publicIpAddress`.

    ```HTTP
    http://YOUR-PUBLIC-IP-ADDRESS
    ```

    L'immagine seguente rappresenta l'app Web, ma l'app userà un indirizzo IP diverso. In caso di errore del gateway della risorsa, riprovare tra qualche minuto, l'avvio dell'app Web potrebbe richiedere qualche minuto. 

    :::image type="content" source="../../media/tutorial-vm/basic-web-app.png" alt-text="App semplice servita dalla macchina virtuale Linux in Azure.":::

    Il file di codice iniziale per l'app Web ha una singola route che visualizza l'indirizzo IP del client, passato tramite il proxy NGINX. 

    :::code language="JavaScript" source="~/../js-e2e-vm/index.js" :::

1. Lasciare il terminale aperto: verrà usato nel corso dell'esercitazione.

## <a name="next-step"></a>Passaggio successivo

> [!div class="nextstepaction"]
> [Connettersi alla macchina virtuale con SSH](connect-linux-virtual-machine-ssh.md) 