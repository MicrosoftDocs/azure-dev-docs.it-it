---
title: Configurare l'ambiente JavaScript locale per lo sviluppo di Azure
description: Informazioni su come configurare un ambiente di sviluppo JavaScript locale per l'uso con Azure, tra cui un editor, le librerie di Azure SDK, strumenti facoltativi e le credenziali necessarie per l'autenticazione delle librerie.
ms.date: 09/22/2020
ms.topic: conceptual
ms.custom: devx-track-js
ms.openlocfilehash: 6d9f86b026a7104e79228d78d5ac27649049a8a4
ms.sourcegitcommit: 4af22924a0eaf01e6902631c0714045c02557de4
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/24/2020
ms.locfileid: "91208682"
---
# <a name="configure-your-local-javascript-dev-environment-for-azure"></a>Configurare l'ambiente di sviluppo JavaScript locale per Azure

Gli sviluppatori di applicazioni cloud preferiscono in genere testare il codice nelle rispettive workstation locali prima di distribuirlo in un ambiente cloud come Azure. Quando si sviluppa in locale, è possibile accedere a una vasta gamma di strumenti e usare un ambiente di sviluppo familiare.

Questo articolo include le istruzioni di configurazione per creare e convalidare l'ambiente di sviluppo locale adatto JavaScript con Azure.

## <a name="one-time-subscription-creation"></a>Creazione della sottoscrizione una tantum

Le risorse di Azure vengono create all'interno di una sottoscrizione, che rappresenta l'unità di fatturazione per l'uso di Azure. Anche se è possibile creare risorse gratuite (ogni sottoscrizione offre una risorsa gratuita per la maggior parte dei servizi), è consigliabile creare risorse del livello a pagamento quando si prevede di distribuire la risorsa in un ambiente di produzione.

* Se si ha già una sottoscrizione, non è necessario crearne una nuova. Usare il [portale di Azure](https://portal.azure.com) per accedere alla sottoscrizione esistente.
* [Avviare la sottoscrizione di una versione di valutazione gratuita](https://azure.microsoft.com/free/cognitive-services)

## <a name="one-time-installation"></a>Installazione una tantum

Per sviluppare usando una risorsa di Azure con JavaScript nella workstation locale, è necessario installare una sola volta gli elementi seguenti:

|Nome/Programma di installazione|Descrizione|
|--|--|
|[Node.js](https://www.npmjs.com/)|Installare l'ambiente di runtime LTS (Long-Term Support) più recente per lo sviluppo in workstation locali. |
| npm (installato con le versioni moderne di Node.js) o [Yarn](https://yarnpkg.com/)|Gestione pacchetti per installare le librerie di Azure SDK.|
|[Visual Studio Code](https://code.visualstudio.com/)| Visual Studio Code offre un'ottima esperienza di integrazione e scrittura di codice per JavaScript, ma non è obbligatorio. È infatti possibile usare qualsiasi editor di codice. Se per questo documento si usa un editor diverso, verificare l'integrazione con Azure o usare l'interfaccia della riga di comando di Azure.|
|[Interfaccia della riga di comando di Azure](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest)|È possibile usare l'interfaccia della riga di comando di Azure per ricreare e gestire le risorse di Azure da una riga di comando, un terminale o una shell bash.|

> [!CAUTION]
> Se si prevede di usare una risorsa di Azure come ambiente di runtime per il codice, ad esempio un'app Web di Azure o un'istanza di contenitore di Azure, è opportuno verificare che l'ambiente di sviluppo Node.js locale corrisponda al runtime della risorsa di Azure che si intende usare.

### <a name="optional-local-installations"></a>Installazioni locali facoltative

Le installazioni comuni seguenti in workstation locali sono facoltative e consentono di semplificare le attività di sviluppo locali.

|Nome/Programma di installazione|Descrizione|
|--|--|
| [git](https://git-scm.com/downloads) | Strumenti da riga di comando per il controllo del codice sorgente. Se si preferisce, è possibile usare uno strumento diverso per il controllo del codice sorgente. |

## <a name="one-time-configuration-of-service-principal"></a>Configurazione una tantum dell'entità servizio

Ogni servizio di Azure prevede un meccanismo di autenticazione, che può includere chiavi ed endpoint, stringhe di connessione o altri meccanismi. Per la conformità alle procedure consigliate, usare un'entità servizio per creare risorse ed eseguire l'autenticazione a tali risorse. Un'entità servizio consente di definire in modo concreto l'ambito dell'accesso in base all'esigenza di sviluppo immediata.

Dal punto di vista teorico, i passaggi per creare e usare un'entità servizio includono:

* Accesso ad Azure con l'account utente singolo, ad esempio joe@microsoft.com.
* Creazione di un'entità servizio denominata con ambito specifico. Dal momento che per la maggior parte degli avvii rapidi è richiesta la creazione di una risorsa di Azure, l'entità servizio deve poter creare risorse.
* Disconnessione da Azure con l'account utente.
* Autenticazione ad Azure a livello di codice con l'entità servizio.
* L'entità servizio crea una risorsa di Azure e usa il servizio associato al servizio.

### <a name="create-service-principal"></a>Creare un'entità servizio

Informazioni su come [creare un'entità servizio](node-sdk-azure-authenticate-principal.md). Ricordarsi di salvare la risposta ottenuta nel passaggio di creazione. Per usare l'entità servizio, saranno necessari anche i valori di `appId`, `tenant` e `password`.

## <a name="steps-for-each-new-development-project-setup"></a>Procedura per ogni nuova installazione del progetto di sviluppo

Dal momento che le librerie di Azure SDK vengono fornite singolarmente per ogni servizio, non esiste un singolo pacchetto scaricabile per accedere a tutte le risorse di Azure. Ogni libreria viene installata in base al servizio di Azure che si vuole usare.

Ogni nuovo progetto che usa Azure deve:
- Creare le risorse di Azure o trovare le informazioni di autenticazione per le risorse di Azure esistenti
- Installare le librerie di Azure SDK da npm o Yarn. Informazioni sulle [versioni delle librerie](#library-versions).
- Gestire in modo sicuro le informazioni di autenticazione all'interno del progetto. Un metodo comune consiste nell'usare **[Dotenv](https://www.npmjs.com/package/dotenv)** per leggere le variabili di ambiente da un file `.env`. Assicurarsi di aggiungere il file `.env` al file `.gitignore` in modo che il file `.env` non venga archiviato nel controllo del codice sorgente.

### <a name="library-versions"></a>Versioni delle librerie

Le [librerie di Azure](azure-sdk-library-package-index.md) usano in genere l'ambito `@azure`.

Le librerie più recenti usano l'ambito `@azure`. I pacchetti meno recenti di Microsoft iniziano in genere con `azure-`. Molti pacchetti non prodotti da Microsoft sono contraddistinti da questo prefisso. Verificare che il proprietario del pacchetto sia Microsoft o Azure.

## <a name="create-azure-resource-with-service-principal"></a>Creare la risorsa di Azure con l'entità servizio

Usare l'interfaccia della riga di comando di Azure per [creare una risorsa di Azure usando l'entità servizio](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli?view=azure-cli-latest#create-a-resource-using-service-principal).

## <a name="use-service-principal-in-javascript"></a>Usare l'entità servizio in JavaScript

[Usare l'entità servizio](node-sdk-azure-authenticate-principal.md#using-the-service-principal) quando per l'autenticazione si usa una libreria client di Azure invece dell'account utente personale.

## <a name="create-environment-variables-for-the-azure-libraries"></a>Creare variabili di ambiente per le librerie di Azure

Per usare le impostazioni di Azure richieste dalle librerie di Azure SDK per accedere al cloud di Azure, impostare i valori più comuni per le variabili di ambiente. I comandi seguenti consentono di impostare le variabili di ambiente nella workstation locale. Un altro meccanismo comune consiste nell'usare il pacchetto npm `DOTENV` per creare un file `.env` per queste impostazioni. Se si prevede di usare un file `.env`, assicurarsi di non archiviare il file nel controllo del codice sorgente. Per assicurarsi che tali impostazioni vengano archiviate nel controllo del codice sorgente, il modo più semplice consiste nell'aggiungere il file `.env` al file `.ignore` di git.

Negli esempi seguenti l'ID client corrisponde all'ID e al segreto dell'entità servizio.

# <a name="bash"></a>[Bash](#tab/bash)

```bash
AZURE_SUBSCRIPTION_ID="aa11bb33-cc77-dd88-ee99-0918273645aa"
AZURE_TENANT_ID="00112233-7777-8888-9999-aabbccddeeff"
AZURE_CLIENT_ID="12345678-1111-2222-3333-1234567890ab"
AZURE_CLIENT_SECRET="abcdef00-4444-5555-6666-1234567890ab"
```

# <a name="cmd"></a>[Cmd](#tab/cmd)

```cmd
set AZURE_SUBSCRIPTION_ID="aa11bb33-cc77-dd88-ee99-0918273645aa"
set AZURE_TENANT_ID=00112233-7777-8888-9999-aabbccddeeff
set AZURE_CLIENT_ID=12345678-1111-2222-3333-1234567890ab
set AZURE_CLIENT_SECRET=abcdef00-4444-5555-6666-1234567890ab
```

---

Sostituire i valori mostrati in questi comandi con quelli dell'entità servizio specifica.

## <a name="install-npm-packages"></a>Installare i pacchetti npm

Per ogni progetto è consigliabile creare una cartella separata e un file `package.json` specifico seguendo questa procedura:

1. Aprire un terminale, un prompt dei comandi o una shell bash e creare una nuova cartella per il progetto. Passare quindi alla nuova cartella.

    ```console
    mkdir MY-NEW-PROJECT && cd MY-NEW-PROJECT
    ```

1. Inizializzare il file di pacchetto:

    ```console
    npm init -y
    ```

    Questo comando esegue il comando npm per creare il file package.json e inizializzare le proprietà minime. Quando si installano i pacchetti delle librerie di Azure SDK con npm o Yarn, il file package.json acquisisce le informazioni di installazione.

1. Installare le librerie di Azure SDK necessarie per l'avvio rapido. Il comando seguente è un esempio.

    ```console
    npm install @azure/ai-text-analytics@5.0.0
    ```

## <a name="use-source-control"></a>Usare il controllo del codice sorgente

È consigliabile creare un repository del controllo del codice sorgente ogni volta che si avvia un progetto. Se è installato git, eseguire il comando seguente:

```bash
git init
```

Da qui è possibile usare comandi come `git add` e `git commit` per eseguire il commit delle modifiche. Eseguendo regolarmente il commit delle modifiche, viene creata una cronologia di commit con cui è possibile ripristinare uno stato precedente.

Per eseguire un backup online del progetto, è anche consigliabile caricare il repository in [GitHub](https://github.com) o in [Azure DevOps](/azure/devops/user-guide/code-with-git?view=azure-devops). Se è già stato inizializzato un repository locale, usare `git remote add` per collegarlo a GitHub o ad Azure DevOps.

La documentazione per Git è disponibile all'indirizzo [git-scm.com/docs](https://git-scm.com/docs) e ovunque su Internet.

Visual Studio Code include una serie di funzionalità Git predefinite. Per altre informazioni, vedere [Uso del controllo della versione in VS Code](https://code.visualstudio.com/docs/editor/versioncontrol).

È anche possibile usare qualsiasi altro strumento di controllo del codice sorgente a scelta. Git è semplicemente uno dei più diffusi e supportati.

## <a name="next-steps"></a>Passaggi successivi

* [Creare e usare un'entità servizio](node-sdk-azure-authenticate-principal.md)
* [Eseguire l'autenticazione con i moduli di Azure per Node.js](node-sdk-azure-authenticate.md)
* [Distribuire un sito Web statico in Azure da Visual Studio Code](tutorial-vscode-static-website-node-01.md)