---
title: Distribuire l'app Express.js/MongoDB con Visual Studio Code - Servizio app/CosmosDB
description: In questa esercitazione si usa un'app Node.js con un database MongoDB tramite l'API nativa MongoDB. Distribuire l'applicazione Node.js nel servizio app di Azure (in Linux), quindi verificare che l'app ospitata funzioni.
ms.topic: tutorial
ms.date: 12/03/2020
ms.custom: scenarios:getting-started, languages:JavaScript, devx-track-javascript
ms.openlocfilehash: 8c40801607e11a4b929f0bb76926122d26217805
ms.sourcegitcommit: 550b165d0b910f4ea9652d8401dd4fc93f057f05
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/04/2020
ms.locfileid: "96610969"
---
# <a name="deploy-expressjs-mongodb-app-to-app-service-from-visual-studio-code"></a>Distribuire l'app MongoDB Express.js nel servizio app da Visual Studio Code

Distribuire l'applicazione Express.js che si connette a MongoDB nel Servizio app di Azure (in Linux) e in un'istanza di CosmosDB. 

L'attività di programmazione viene eseguita automaticamente. Questa esercitazione è incentrata sull'uso corretto degli ambienti di Azure locali e remoti all'interno Visual Studio Code con le estensioni di Azure.

## <a name="top-tasks"></a>Attività principali

Questa esercitazione include diverse **attività principali di Azure** per sviluppatori JavaScript:

* Creare una risorsa di CosmosDB per l'hosting del database di MongoDB
* Creare una risorsa del Servizio app per l'hosting dell'app Express.js
* Distribuire l'app Express.js nel Servizio app

## <a name="sample-application"></a>Applicazione di esempio

L'[app Express.js di esempio](https://github.com/Azure-Samples/js-e2e-express-mongo) è costituita dagli elementi seguenti:

* **Server Express.js** ospitato sulla porta 8080
* Semplice motore di **visualizzazione lato server React.js**
* Funzioni delle **API native di MongoDB** per l'inserimento, l'eliminazione e la ricerca di dati

:::image type="content" source="../media/tutorial-end-to-end-app-cosmos/nodejs-app-connected-mongodb-form.png" alt-text="Semplice app Node.js connessa al database MongoDB.":::

## <a name="create-or-use-existing-azure-subscription"></a>Creare una sottoscrizione di Azure o usarne una esistente 

* Un account Azure con una sottoscrizione attiva. [È possibile crearne uno gratuitamente](https://azure.microsoft.com/free/).

## <a name="install-software"></a>Installazione del software

- [Node.js 12 (LTS) e npm](https://nodejs.org/en/download), lo strumento di gestione pacchetti Node.js installato nel computer locale.
- [Visual Studio Code](https://code.visualstudio.com/) installato nel computer locale. 
- Estensioni per Visual Studio Code:
    - [Estensione del servizio app di Azure](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice) per Visual Studio Code (installata da Visual Studio Code).
    - [Database di Azure](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb)

## <a name="create-a-cosmosdb-database-resource-for-mongodb"></a>Creare una risorsa di database di CosmosDB per MongoDB

Creare prima di tutto una risorsa di Cosmos perché questa operazione richiederà qualche minuto. 

1. In Visual Studio Code selezionare l'icona **Azure** nel menu più a sinistra, quindi selezionare la sezione **Database**. 

    Se la sezione **Database** non è visibile, verificare di aver selezionato la sezione nel menu principale di Azure **...** . 

    :::image type="content" source="../media/tutorial-end-to-end-app-cosmos/vscode-azure-extension-select-database-section.png" alt-text="Screenshot parziale dell'icona Remote Containers (Contenitori remoti) di Visual Studio Code"::: 

1. Nella sezione **Database** della finestra di esplorazione di Azure selezionare la sottoscrizione facendovi clic sopra con il tasto destro del mouse e quindi scegliere **Crea server**.
1. Nel riquadro comandi **Create new Azure Database Server** (Crea nuovo server di database Azure) selezionare **Azure Cosmos DB per l'API MongoDB**. 
1. Seguire le istruzioni riportate nella tabella seguente per informazioni su come vengono usati i valori. La creazione del database può richiedere fino a 15 minuti.

    |Proprietà|Valore|
    |--|--|
    |Immettere un nome univoco globale per la nuova risorsa in **Nome account**.| Immettere un valore, ad esempio `cosmos-mongodb-YOUR-NAME`, per la risorsa. Sostituire `YOUR-NAME` con il nome o l'ID univoco. Questo nome univoco viene usato anche come parte dell'URL per accedere alla risorsa in un browser.|
    |Selezionare o creare un gruppo di risorse.|Se è necessario creare un gruppo di risorse, usare una convenzione di denominazione che consenta di identificare il proprietario, lo scopo e l'area, ad esempio `westus-cosmostutorial-joesmith`.|
    |Località|Il percorso della risorsa. Per questa esercitazione, selezionare un'area nelle vicinanze.|

    La creazione della risorsa può richiedere fino a 15 minuti. È possibile ignorare la sezione successiva se il tempo a disposizione è limitato, ma occorre ricordare di completare tale sezione dopo qualche minuto.

## <a name="get-cosmosdb-connection-string"></a>Ottenere la stringa di connessione di CosmosDB

Sempre nella finestra di esplorazione Database di Azure fare clic con il pulsante destro del mouse sul nome della risorsa, quindi scegliere **Copy Connection String** (Copia stringa di connessione) per copiare la stringa di connessione. Sarà necessaria in seguito nell'esercitazione per il file della variabile di ambiente.

:::image type="content" source="../media/tutorial-end-to-end-app-cosmos/vscode-databases-extenion-copy-connection-string.png" alt-text="Modulo dell'app Web Express.js e dati risultanti da MongoDB locale.":::

## <a name="download-and-run-the-sample-expressjs-app"></a>Scaricare ed eseguire l'app Express.js di esempio

L'app Web Express.js viene fornita all'utente. Scaricare l'app, installare le dipendenze ed eseguire l'app. 

1. [Scaricare il repository GitHub compresso](https://github.com/Azure-Samples/js-e2e-express-mongo.git) nel computer locale e quindi decomprimerlo in una cartella. 
1. Aprire la cartella in Visual Studio Code. È possibile fare clic con il pulsante destro del mouse sulla cartella e scegliere **Apri con Code** oppure usare il comando equivalente dell'interfaccia della riga di comando all'interno della cartella:

    ```bash
    code .
    ```

1. Modificare il file di ambiente, `.env`, aggiungendo la proprietà della stringa di connessione per CosmosDB come valore `DATABASE_URL` della proprietà. 

    ```bash
    ENVIRONMENT=development
    DATABASE_NAME=
    DATABASE_COLLECTION_NAME=
    DATABASE_URL=
    ```

1. In Visual Studio Code aprire una finestra del terminale ed eseguire il comando seguente per installare le dipendenze dell'esempio e avviare l'app Web.

    ```bash
    npm install && npm start
    ```

1. Visualizzare l'app Web nel computer locale in un browser.

    ```url
    http://localhost:8080/
    ```

    :::image type="content" source="../media/tutorial-end-to-end-app-cosmos/nodejs-app-connected-mongodb-form.png" alt-text="Semplice app Node.js connessa al database MongoDB.":::

## <a name="create-web-app-resource-and-deploy-expressjs-app"></a>Creare una risorsa di tipo app Web e distribuire l'app Express.js

Usare l'estensione per Visual Studio Code per il Servizio app per creare una risorsa del servizio app e distribuire l'app Web Express.js nella risorsa.

1. Passare alla finestra di esplorazione di Azure. Fare clic con il pulsante destro del mouse sulla sottoscrizione e quindi scegliere `Create new web app...`.

    :::image type="content" source="../media/tutorial-end-to-end-app-cosmos/create-web-app-with-extension.png" alt-text="Screenshot parziale di Visual Studio Code con l'estensione del servizio app di Azure per creare un'app Web.":::

1. Seguire le istruzioni riportate nella tabella seguente per informazioni su come vengono usati i valori.

    |Proprietà|Valore|
    |--|--|
    |Immettere un nome univoco globale per la nuova app Web.| Immettere un valore, ad esempio `web-app-with-mongodb-YOUR-NAME`, per la risorsa del servizio app. Sostituire `<YOUR-NAME>` con il nome o l'ID univoco. Questo nome univoco viene usato anche come parte dell'URL per accedere alla risorsa in un browser.|
    |Selezionare un runtime per l'app Linux.|Selezionare `Node 12 LTS`.|

1. Al termine del processo di creazione dell'app, viene visualizzato un messaggio di stato nell'angolo in basso a destra di Visual Studio Code con la possibilità di scegliere tra `Deploy` e `View output`. Selezionare `Deploy`.

    :::image type="content" source="../media/tutorial-end-to-end-app-cosmos/vscode-app-extension-create-web-app-deploy-web-app.png" alt-text="Screenshot parziale di Visual Studio Code, con l'estensione del servizio app di Azure per distribuire l'app Web immediatamente dopo la sua creazione.":::

    Se il messaggio di stato non è più visibile, è possibile avviare la distribuzione selezionando la finestra di esplorazione di Azure, facendo clic con il pulsante destro del mouse sul nome della risorsa e quindi scegliendo **Distribuisci nell'app Web...** .

1. Durante il processo di distribuzione una notifica consente di selezionare un'opzione per visualizzare la **finestra di output**.  Questa consente di visualizzare lo stato in sequenza della distribuzione. 

1. Al termine della distribuzione, viene visualizzata una notifica. Selezionare **Log in streaming** per visualizzare i log in sequenza. 

    :::image type="content" source="../media/tutorial-end-to-end-app-cosmos/vscode-app-service-deployed.png" alt-text="Il servizio è stato distribuito. &quot;Log in streaming&quot;.":::

    :::image type="content" source="../media/tutorial-end-to-end-app-cosmos/vscode-app-service-stream-logs.png" alt-text="Al termine della distribuzione, viene visualizzata una notifica che consente di selezionare &quot;Log in streaming&quot;.":::    

1. Aprire il sito Web in un browser, sostituire il testo `YOUR-RESOURCE_NAME` con il nome della risorsa: `https://YOUR-RESOURCE_NAME.azurewebsites.net`.
1. Usare l'app Web, aggiungendo ed eliminando elementi. 

## <a name="clean-up-resources"></a>Pulire le risorse 

Al termine di questa esercitazione, è necessario rimuovere le due risorse create per evitare di ricevere addebiti. 

1. In Visual Studio Code usare la finestra di esplorazione di Azure per Database, fare clic con il pulsante destro del mouse sulla risorsa e quindi scegliere **Elimina account**.
1. In Visual Studio Code usare la finestra di esplorazione di Azure per Servizio app, fare clic con il pulsante destro del mouse sulla risorsa e quindi scegliere **Elimina**.

## <a name="next-steps"></a>Passaggi successivi

Continuare a ottenere informazioni sul Servizio app e CosmosDB:
* [Configurare un'app Node.js per il Servizio app di Azure](/azure/app-service/configure-language-nodejs?pivots=platform-linux)
* [Connettersi tramite SSH](/azure/app-service/configure-linux-open-ssh-session)
* [Eseguire la migrazione di dati a CosmosDB](/azure/dms/tutorial-mongodb-cosmos-db?toc=/azure/cosmos-db/toc.json?toc=/azure/cosmos-db/toc.json)
