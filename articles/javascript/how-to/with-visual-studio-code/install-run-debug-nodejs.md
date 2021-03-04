---
title: Sviluppare Node.js con Visual Studio Code
description: Informazioni sui passaggi per lo sviluppo e il debug del progetto di Node.js JavaScript con Visual Studio.
ms.topic: how-to
ms.date: 01/28/2021
ms.custom: devx-track-js
ms.openlocfilehash: b61bfaf9cc6b57a8a481e56841b584550e9a9b99
ms.sourcegitcommit: 576c878c338d286060010646b96f3ad0fdbcb814
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/04/2021
ms.locfileid: "102118229"
---
# <a name="how-to-develop-and-debug-nodejs-with-visual-studio-code"></a>Come sviluppare ed eseguire il debug di Node.js con Visual Studio Code

Informazioni sui passaggi per lo sviluppo e il debug del progetto di Node.js JavaScript con Visual Studio. 

## <a name="prepare-your-environment"></a>Preparare l'ambiente

1. Installare [Visual Studio Code](https://code.visualstudio.com/). 
1. Installare [git](https://git-scm.com/). Visual Studio Code si integra con git per fornire la gestione del *controllo del codice sorgente* nella [barra laterale](https://code.visualstudio.com/docs/getstarted/userinterface).

1. Ottenere una stringa di connessione al database mongoDB.

    Se non si dispone di un database mongoDB disponibile, è possibile:
    * Scegliere di eseguire questo progetto locale in una configurazione a più contenitori in cui uno dei contenitori è un database mongoDB. Installare l'estensione [Docker](https://www.docker.com/) e [Remote-Containers](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers) per ottenere una configurazione a più contenitori con uno dei contenitori che eseguono un database MongoDB locale. 
    * Scegliere di creare una risorsa [Azure Cosmos DB](/azure/cosmos-db/) per un database MongoDB. Scopri di più con questa [esercitazione](../../tutorial/deploy-nodejs-mongodb-app-service-from-visual-studio-code.md#create-a-cosmos-db-database-resource-for-mongodb).

## <a name="clone-sample-project-to-local-computer"></a>Clonare il progetto di esempio nel computer locale

Per iniziare, scaricare il progetto di esempio usando la procedura seguente:

1. Aprire Visual Studio Code.

1. Premere **F1** per visualizzare il riquadro comandi.

1. Al prompt del riquadro comandi immettere `gitcl`, selezionare il comando **Git: Clone** e premere **INVIO**.

    ![Comando gitcl nel prompt del riquadro comandi di Visual Studio Code](../../media/node-howto-e2e/visual-studio-code-git-clone.png)

1. Quando viene richiesto l'**URL del repository**, immettere `https://github.com/scotch-io/node-todo`, quindi premere **INVIO**.

1. Selezionare o creare la directory locale in cui si vuole clonare il progetto.

    ![Esplora risorse di Visual Studio Code](../../media/node-howto-e2e/visual-studio-code-explorer.png)

## <a name="use-the-integrated-bash-terminal-to-install-dependencies"></a>Usare il terminale bash integrato per installare le dipendenze

Trattandosi di un progetto Node.js, è prima di tutto necessario verificare che tutte le dipendenze del progetto siano installate da npm.  

1. Premere **CTRL**+ **`** per visualizzare il terminale integrato di Visual Studio Code. 

1. Immettere `yarn` e premere **INVIO**.  

     ![Esecuzione del comando yarn in Visual Studio Code](../../media/node-howto-e2e/visual-studio-code-install-yarn.png)

## <a name="navigate-the-project-files-and-code"></a>Esplorare il codice e i file di progetto

Per orientarsi nella base di codici, si esamineranno alcuni esempi di funzionalità di esplorazione messe a disposizione da Visual Studio Code.

1. Premere **CTRL**+**P**.

1. Immettere `.js` per visualizzare tutti i file JSON/JavaScript presenti nel progetto e la directory padre di ogni file 

1. Selezionare *server.js*, ovvero lo script di avvio dell'app.

1. Posizionare il mouse sulla variabile **database** importata alla riga 6 per visualizzarne il tipo. La possibilità di analizzare rapidamente variabili, moduli e tipi in un file è utile durante lo sviluppo dei progetti. 

    ![Individuare il tipo in Visual Studio Code con la guida visualizzata al passaggio del mouse](../../media/node-howto-e2e/visual-studio-code-hover-help.png)

1. Facendo clic all'interno di una variabile, ad esempio **database**, è possibile vedere tutti i riferimenti a tale variabile presenti nello stesso file. Per visualizzare tutti i riferimenti a una variabile nel progetto, fare clic con il pulsante destro del mouse sulla variabile e dal menu di scelta rapida scegliere **Trova tutti i riferimenti**.

    ![Trovare tutti i riferimenti con Visual Studio Code](../../media/node-howto-e2e/visual-studio-code-find-all-references.png)

1. Oltre a passare il mouse su una variabile per individuarne il tipo, è possibile esaminare la definizione di una variabile, anche se si trova in un altro file. Ad esempio, fare clic con il pulsante destro del mouse su **database.localUrl** (riga 12) e quindi scegliere **Visualizza definizione** dal menu di scelta rapida.

    ![Visualizzare la definizione della variabile in Visual Studio Code](../../media/node-howto-e2e/visual-studio-code-peek-definition.png)

## <a name="use-visual-studio-code-autocompletion-with-mongodb"></a>Usare Visual Studio Code il completamento automatico con mongoDB

La stringa di connessione di MongoDB è hardcoded nella dichiarazione della proprietà `database.localUrl`. In questa sezione il codice verrà modificato per recuperare la stringa di connessione da una variabile di ambiente e verranno fornite informazioni sulla funzionalità di completamento automatico di Visual Studio Code.  

1. Aprire il file *server.js*

1. Sostituire il codice seguente:

    ```javascript
    mongoose.connect(database.localUrl);
    ```

    con questo codice:

    ```javascript
    mongoose.connect(process.env.MONGODB_URL || database.localUrl);
    ```

Se il codice viene digitato manualmente, anziché copia e incolla, quando si digita il punto dopo `process` , Visual Studio Code Visualizza i membri disponibili dell'API globale del processo Node.js.

![Variabili di ambiente di VS Code con l'ambiente del processo](../../media/node-howto-e2e/visual-studio-code-process-env.png)

Il completamento automatico funziona perché Visual Studio Code usa TypeScript in background, anche per JavaScript, per fornire informazioni sui tipi che possono quindi essere usate per l'elenco di completamento durante la digitazione. Visual Studio Code può rilevare che si tratta di un progetto di Node.js e quindi scarica automaticamente il file delle definizioni di tipi TypeScript per [Node.js da NPM](https://www.npmjs.com/package/@types/node). Il file delle definizioni di tipi consente di ottenere il completamento automatico per altre variabili globali Node.js, ad esempio `Buffer` e `setTimeout`, oltre a tutti i moduli predefiniti, ad esempio `fs` e `http`.

Oltre alle API Node.js predefinite, questa acquisizione automatica delle digitazioni funziona anche per più di 2.000 moduli di terze parti, come REACT, il carattere di sottolineatura ed Express. Ad esempio, per impedire a Mongoose di arrestare l'app di esempio in modo anomalo se non riesce a connettersi all'istanza di database MongoDB configurata, inserire la riga di codice seguente alla riga 13:

```javascript
mongoose.connection.on("error", () => { console.log("DB connection error"); });
```

Come con il codice precedente, si noterà che il completamento automatico funziona senza operazioni aggiuntive da parte dell'utente.

![Il completamento automatico visualizza automaticamente i membri di un'API](../../media/node-howto-e2e/visual-studio-code-autocomplete-mongoose.png)

È possibile vedere quali moduli supportano questa funzionalità di completamento automatico esplorando il progetto [DefinitelyTyped](https://github.com/DefinitelyTyped/DefinitelyTyped), che rappresenta la fonte gestita dalla community di tutte le definizioni di tipi TypeScript.

## <a name="running-the-local-nodejs-app"></a>Esecuzione dell'app Node.js locale

Dopo aver esaminato il codice, eseguire l'app. Per eseguire l'app da Visual Studio Code premere **F5**. Quando si esegue il codice tramite **F5** (modalità di debug), Visual Studio Code avvia l'app e visualizza la finestra **Console di debug** con il canale stdout per l'app.

![Monitoraggio di stdout di un'app tramite la console di debug](../../media/node-howto-e2e/visual-studio-code-debug-console.png)

Inoltre, la finestra **Console di debug** è collegata alla nuova app in esecuzione, per cui è possibile digitare espressioni JavaScript che verranno valutate nell'app. È anche disponibile il completamento automatico. Per vedere questo comportamento, digitare `process.env` nella console:

![Immissione del codice nella console di debug](../../media/node-howto-e2e/visual-studio-code-debug-console-autocomplete.png)

È stato possibile premere **F5** per eseguire l'app perché il file attualmente aperto è un file JavaScript (*server.js*). Di conseguenza, Visual Studio Code presuppone che il progetto sia un'app Node.js. Se si chiudono tutti i file JavaScript in Visual Studio Code e quindi si preme **F5**, Visual Studio Code chiederà di specificare l'ambiente:

![Indicazione dell'ambiente di runtime](../../media/node-howto-e2e/visual-studio-code-select-environment.png)

Aprire un browser e passare a `http://localhost:8080` per vedere l'app in esecuzione. Digitare un messaggio nella casella di testo e aggiungere o rimuovere alcune a-DOS per ottenere un'idea del funzionamento dell'app.

![Aggiungere o rimuovere elementi Todo con l'app](../../media/node-howto-e2e/add-remove-todos-app.png)

## <a name="debugging-the-local-nodejs-app"></a>Debug dell'app Node.js locale

Oltre a essere in grado di eseguire l'app e interagire con esso tramite la console integrata, è possibile impostare punti di interruzione direttamente all'interno del codice. Premere ad esempio **CTRL**+**P** per visualizzare la selezione file. Dopo aver visualizzato la selezione file, digitare `route` e selezionare il file *route.js*.

Impostare un punto di interruzione alla riga 28, che rappresenta la route Express che viene chiamata quando l'app prova ad aggiungere una voce Todo. Per impostare un punto di interruzione è sufficiente fare clic sull'area a sinistra del numero di riga nell'editor come illustrato nell'immagine seguente.

![Impostazione di un punto di interruzione in Visual Studio Code](../../media/node-howto-e2e/visual-studio-code-set-breakpoint.png)

> [!NOTE]
> Oltre ai punti di interruzione standard, Visual Studio Code supporta punti di interruzione condizionali che consentono di definire quando l'app deve sospendere l'esecuzione. Per impostare un punto di interruzione condizionale, fare clic con il pulsante destro del mouse a sinistra della riga in cui si vuole sospendere l'esecuzione, scegliere **Aggiungi punto di interruzione condizionale** e specificare un'espressione JavaScript (ad esempio `foo = "bar"`) o il conteggio esecuzioni che definisce la condizione in base a cui sospendere l'esecuzione.

Dopo aver impostato il punto di interruzione, tornare nell'app in esecuzione e aggiungere una voce Todo. Con l'aggiunta di una voce Todo, l'app sospende immediatamente l'esecuzione alla riga 28 in cui è stato impostato il punto di interruzione:

![Visual Studio Code sospende l'esecuzione nel punto di interruzione](../../media/node-howto-e2e/visual-studio-code-pause-breakpoint-execution.png)

Quando l'applicazione è stata sospesa, è possibile passare il puntatore del mouse sulle espressioni del codice per visualizzare il relativo valore corrente, controllare le variabili locali o le espressioni di controllo e lo stack di chiamate e usare la barra degli strumenti di debug per scorrere l'esecuzione del codice. Premere **F5** per riprendere l'esecuzione dell'app.

## <a name="local-full-stack-debugging-in-visual-studio-code"></a>Debug dello stack completo locale in Visual Studio Code

Come accennato in precedenza in questo argomento, l'app Todo è un'app MEAN, ovvero sia il front-end che il back-end sono scritti in JavaScript. Anche se quindi si esegue attualmente il debug del codice di back-end (Node/Express), a un certo punto potrebbe essere necessario eseguire il debug del codice di front-end (Angular). A tale scopo, Visual Studio Code mette a disposizione un vastissimo ecosistema di estensioni, incluso il debug di Chrome integrato.

Passare alla scheda **Estensioni** e digitare `chrome` nella casella di ricerca:

![Estensione di debug di Chrome per Visual Studio Code](../../media/node-howto-e2e/visual-studio-code-chrome-extension.png)

Selezionare l'estensione denominata **Debugger for Chrome** e selezionare **Installa**. Dopo aver installato l'estensione di debug di Chrome, selezionare **Ricarica** per chiudere e riaprire Visual Studio Code e attivare così l'estensione.

![Ricaricamento di Visual Studio Code dopo l'installazione dell'estensione di debug di Chrome](../../media/node-howto-e2e/visual-studio-code-reload-extension.png)

Anche se è stato possibile eseguire il codice Node.js ed effettuarne il debug senza una configurazione specifica di Visual Studio Code, per eseguire il debug di un'app Web front-end è necessario generare un file *launch.json* che indica a Visual Studio Code come eseguire l'app.

## <a name="create-a-full-stack-launchjson-file-for-visual-studio-code"></a>Creare un launch.jsdi stack completo sul file per Visual Studio Code

Per generare il *launch.jsnel* file, passare alla scheda **debug** , selezionare l'icona a forma di ingranaggio (che dovrebbe avere un piccolo punto rosso sopra di esso) e selezionare l'ambiente **node.js** .

![Opzione di Visual Studio Code per configurare il file launch.json](../../media/node-howto-e2e/visual-studio-code-debug-gear.png)

Dopo la creazione, il file *launch.json* sarà simile al seguente e indicherà a Visual Studio Code come avviare e/o collegarsi all'app per eseguirne il debug.

```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "type": "node",
            "request": "launch",
            "name": "Launch Program",
            "program": "${workspaceRoot}/server.js"
        },
        {
            "type": "node",
            "request": "attach",
            "name": "Attach to Port",
            "address": "localhost",
            "port": 5858
        }
    ]
}
```

Visual Studio Code è stato in grado di rilevare che lo script di avvio dell'app è *server.js*.

Con il file *launch.json* aperto, selezionare **Aggiungi configurazione** in basso a destra e selezionare **Chrome: Launch with userDataDir**.

![Aggiunta di una configurazione di Chrome a Visual Studio Code](../../media/node-howto-e2e/visual-studio-code-add-chrome-config.png)

L'aggiunta di una nuova configurazione di esecuzione per Chrome consente di eseguire il debug del codice JavaScript di front-end. 

È possibile passare il puntatore del mouse su una qualsiasi delle impostazioni specificate per visualizzare l'indicazione della funzione dell'impostazione. Si noti anche che Visual Studio Code rileva automaticamente l'URL dell'app. Modificare la proprietà **webRoot** in `${workspaceRoot}/public` in modo che il debugger di Chrome possa trovare gli asset front-end dell'app:

```json
{
   "type": "chrome",
   "request": "launch",
   "name": "Launch Chrome",
   "url": "http://localhost:8080",
   "webRoot": "${workspaceRoot}/public",
   "userDataDir": "${workspaceRoot}/.vscode/chrome"
}
```

Per avviare ed eseguire il debug contemporaneamente di front-end e back-end, è necessario creare una configurazione di esecuzione *composta* che indichi Visual Studio Code quale set di configurazioni eseguire in parallelo.

Aggiungere il frammento seguente come proprietà di primo livello nel file *launch.json*, ovvero come elemento di pari livello della proprietà **configurations** esistente.

```json
"compounds": [
   {
      "name": "Full-Stack",
      "configurations": ["Launch Program", "Launch Chrome"]
   }
]
```

I valori di stringa specificati nella matrice **compounds.configurations** si riferiscono al **nome** delle singole voci nell'elenco **configurations**. Se questi nomi sono stati modificati, è necessario apportare le modifiche appropriate nella matrice. Ad esempio, passare alla scheda Debug e impostare la configurazione su **Full-Stack** (il nome della configurazione composta), quindi premere **F5** per eseguirla.

![Esecuzione di una configurazione in Visual Studio Code](../../media/node-howto-e2e/visual-studio-code-full-stack-configuration.png)

Con l'esecuzione della configurazione vengono avviati l'app Node.js (come si può vedere nell'output della console di debug) e Chrome, che è configurato per passare all'app Node.js all'indirizzo `http://localhost:8080`.

Premere **CTRL**+**P**, quindi immettere o selezionare *todos.js*, ovvero il controller Angular principale per il front-end dell'app.

Impostare un punto di interruzione alla riga 11, ovvero nel punto di ingresso di una nuova voce Todo.

Tornare nell'app in esecuzione e aggiungere una nuova voce Todo. Si noti che Visual Studio Code ha ora sospeso l'esecuzione nel codice Angular.

![Debug del codice di front-end in Visual Studio Code](../../media/node-howto-e2e/visual-studio-code-chrome-pause.png)

Come per il debug di Node.js, è possibile posizionare il mouse su espressioni, visualizzare variabili locali/espressioni di controllo, valutare le espressioni nella console e così via. 

Esistono due aspetti importanti da notare:

1. Il riquadro **Stack di chiamate** visualizza due stack differenti, **Node** e **Chrome**, e indica quello attualmente sospeso.

1. È possibile passare tra il codice front-end e back-end: premere **F5** per raggiungere il punto di interruzione impostato precedentemente nella route Express.

Con questa configurazione è possibile eseguire in modo efficiente il debug del codice JavaScript di front-end, back-end o dello stack completo direttamente da Visual Studio Code.

Il concetto di debugger composto non è limitato a due processi di destinazione e non è limitato solo a JavaScript. Se quindi si usa un'app di microservizi, che è potenzialmente Polyglot, è possibile usare lo stesso flusso di lavoro dopo aver installato le estensioni appropriate per il linguaggio/framework.

## <a name="next-steps"></a>Passaggi successivi

* [Distribuire contenitori](../deploy-containers.md)
