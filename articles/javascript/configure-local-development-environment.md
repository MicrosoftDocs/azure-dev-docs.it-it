---
title: Configurare l'ambiente JavaScript locale per lo sviluppo di Azure
description: Informazioni su come configurare un ambiente di sviluppo JavaScript locale per l'uso con Azure, tra cui un editor, le librerie di Azure SDK, strumenti facoltativi e le credenziali necessarie per l'autenticazione delle librerie.
ms.date: 07/01/2020
ms.topic: conceptual
ms.openlocfilehash: 2285e79ea62d2de961fd7d4dc7647fec2312b83c
ms.sourcegitcommit: 7be67fb768fb5e19f7de573068cc1376b3d90d1f
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/02/2020
ms.locfileid: "85911190"
---
# <a name="configure-your-local-javascript-dev-environment-for-azure"></a>Configurare l'ambiente di sviluppo JavaScript locale per Azure

Gli sviluppatori di applicazioni cloud preferiscono in genere testare il codice nelle rispettive workstation locali prima di distribuirlo in un ambiente cloud come Azure. Quando si sviluppa in locale, è possibile accedere a una vasta gamma di strumenti e usare un ambiente di sviluppo familiare.

Questo articolo include le istruzioni di configurazione per creare e convalidare l'ambiente di sviluppo locale adatto JavaScript con Azure.

## <a name="one-time-subscription-creation"></a>Creazione della sottoscrizione una tantum

Le risorse di Azure vengono create all'interno di una sottoscrizione, che rappresenta l'unità di fatturazione per l'uso di Azure. Anche se è possibile creare risorse gratuite (ogni sottoscrizione offre una risorsa gratuita per la maggior parte dei servizi), è consigliabile creare risorse del livello a pagamento quando si prevede di distribuire la risorsa in un ambiente di produzione.

* Se si ha già una sottoscrizione, non è necessario crearne una nuova. Usare il [portale di Azure](https://portal.azure.com) per accedere alla sottoscrizione esistente.
* [Avviare la sottoscrizione di una versione di valutazione gratuita]()

## <a name="one-time-installation"></a>Installazione una tantum

Per sviluppare usando una risorsa di Azure con JavaScript nella workstation locale, è necessario installare una sola volta gli elementi seguenti:

|Nome/Programma di installazione|Descrizione|
|--|--|
|[Node.js]()|Installare l'ambiente di runtime LTS (Long-Term Support) più recente per lo sviluppo in workstation locali. |
| npm (installato con le versioni moderne di Node.js) o [Yarn]()|Gestione pacchetti per installare le librerie di Azure SDK.|
|[VSCode](https://aka.ms/vscode-deploy)| VSCode offre un'ottima esperienza di integrazione e scrittura di codice per JavaScript, ma non è obbligatorio. È infatti possibile usare qualsiasi editor di codice. Se per questo documento si usa un editor diverso, verificare l'integrazione con Azure o usare l'interfaccia della riga di comando di Azure.|
|[Interfaccia della riga di comando di Azure]()|È possibile usare l'interfaccia della riga di comando di Azure per ricreare e gestire le risorse di Azure da una riga di comando, un terminale o una shell bash.|

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

Per semplificare la creazione dell'entità servizio, usare la procedura seguente e lo script fornito per creare l'entità servizio da usare con gli avvii rapidi di Azure. Nei passaggi seguenti si usa `JOE` come nome utente di esempio. Sostituirlo con il nome o l'alias di posta elettronica personale.

1. Aprire VSCode e installare l'estensione [Strumenti dell'interfaccia della riga di comando di Azure](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azurecli). Questa estensione consente di eseguire i comandi dell'interfaccia della riga di comando di Azure dal file script, riga per riga. Per ogni comando eseguito in VSCode viene aperto un documento adiacente per visualizzare i risultati.

1. Creare un nuovo file denominato `create-service-principal.sh` e copiarvi i comandi di Azure seguenti:

    ```azurecli
    # Replace ALL-CAPS variables with your own values

    ####################################
    # Login as you
    ####################################

    # Login - command opens browser, select your account to finish authentication, then close browser
    az login

    ####################################
    # Optional, set default subscription
    ####################################

    # If you have more than 1 subscription, use the `list` command to find the subscription, then use the `set` command to set the default by name or id
    az account list
    az account set --subscription MYCOMPANYSUBSCRIPTION

    ####################################
    # Create service principal
    ####################################

    # Create a service principal with a name that indicates its purpose and owner - the response includes the `appId` which is necessary in some of the remaining commands
    az ad sp create-for-rbac --name JOE-SERVICEPRINCIPAL-DOCUMENT-QUICKSTARTS --skip-assignment

    ####################################
    # Add role of contributor
    ####################################

    # Add contributor role to service principal so it can create Azure resources
    az role assignment create --assignee APP-ID --role CONTRIBUTOR

    ####################################
    # Optional, verify role assignment
    ####################################

    # Verify role assignment for service principal
    az role assignment list --assignee APP-ID

    ####################################
    # Logout
    ####################################

    # Logout off Azure CLI
    az logout
    ```

    Nei passaggi rimanenti di questa procedura, per ogni riga del file che **non** inizia con `#`, posizionare il cursore di VSCode sulla riga e quindi **farvi clic sopra con il pulsante destro del mouse** per selezionare **Esegui riga nell'editor**.

    :::image type="content" source="media/development-setup/vscode-rightclick-run-line-in-editor.png" alt-text="Nei passaggi rimanenti di questa procedura, per ogni riga del file che non inizia con `#`, posizionare il cursore di VSCode sulla riga e quindi farvi clic sopra con il pulsante destro del mouse per selezionare `Esegui riga nell'editor`.":::

1. Usare il pulsante destro del mouse/Esegui riga nell'editor nella riga seguente per eseguire l'autenticazione ad Azure con l'account utente personale usando l'interfaccia della riga di comando di Azure. Con questo comando viene aperto un browser Internet. Selezionare l'account Azure. Dopo l'autenticazione dell'account, chiudere la finestra del browser, perché non sarà necessaria con le attività rimanenti.

    ```azurecli
    az login
    ```

    La risposta include tutte le sottoscrizioni a cui si ha accesso, visualizzate come matrice JSON in un'altra finestra del documento VSCode. Trovare la proprietà `name` o `id`. Uno di questi valori è necessario per i comandi rimanenti.

    ```json
    [  {
    "cloudName": "AzureCloud",
    "id": "320d9379-aaaa-bbbb-cccc-52f2b0fc40ac",
    "isDefault": false,
    "name": "contoso-development-team",
    "state": "Enabled",
    "tenantId": "72f988bf-aaaa-bbbb-cccc-2d7cd011db47",
    "user": {
      "name": "joe@contoso.com",
      "type": "user"
    }
    }]
    ```

    La sottoscrizione contrassegnata con `isDefault: true` è quella che riceve i comandi rimanenti. Se è necessario cambiare la sottoscrizione predefinita, usare il comando `az account set --subscription <name or id>`.


<a name='create-service-principal-command'></a>

1. Usare il pulsante destro del mouse/Esegui riga nell'editor nella riga seguente per creare l'entità servizio collegata all'account utente. Questa entità servizio non ha ancora autorizzazioni con ambito, perché è presente il parametro `--skip-assignment`.


    ```azurecli
    az ad sp create-for-rbac --name JOE-SERVICEPRINCIPAL-DOCUMENT-QUICKSTARTS --skip-assignment
    ```

    Il nome dell'entità servizio è `JOE-SERVICEPRINCIPAL-DOCUMENT-QUICKSTARTS`. È possibile visualizzare un elenco di tutte le entità servizio associate all'account utente di Azure nel portale di Azure, nell'elenco di applicazioni del servizio Active Directory.

    Il risultato include le informazioni necessarie, ovvero `appId` e `password`. Salvare il file con il nome `create-service-principal.json`

    ```json
    {
      "appId": "93453d56-aaaa-bbbb-cccc-db600ecc4f6a",
      "displayName": "JOE-SERVICEPRINCIPAL-DOCUMENT-QUICKSTARTS",
      "name": "http://JOE-SERVICEPRINCIPAL-DOCUMENT-QUICKSTARTS",
      "password": "d88b21e0-aaaa-bbbb-cccc-e1e9b06d50f6",
      "tenant": "72f988bf-aaaa-bbbb-cccc-2d7cd011db47"
    }
    ```

1. Usare il pulsante destro del mouse/Esegui riga nell'editor nella riga seguente per assegnare l'autorizzazione con ambito per la creazione delle risorse di Azure. L'ambito `CONTRIBUTOR` consente all'entità servizio di creare risorse di Azure.

    ```azurecli
    az role assignment create --assignee APP-ID --role CONTRIBUTOR
    ```

    Il risultato è simile al seguente:

    ```json
    {
      "canDelegate": null,
      "id": "/subscriptions/a5b1ca8b-aaaa-bbbb-cccc-4cf7ec4791a0/providers/Microsoft.Authorization/roleAssignments/3a155db5-aaaa-bbbb-cccc-0cbfebf75464",
      "name": "3a155db5-aaaa-bbbb-cccc-0cbfebf75464",
      "principalId": "c05d56c9-aaaa-bbbb-cccc-0535d6167ed4",
      "principalType": "ServicePrincipal",
      "roleDefinitionId": "/subscriptions/a5b1ca8b-aaaa-bbbb-cccc-4cf7ec4791a0/providers/Microsoft.Authorization/roleDefinitions/b24988ac-aaaa-bbbb-cccc-20f7382dd24c",
      "scope": "/subscriptions/a5b1ca8b-aaaa-bbbb-cccc-4cf7ec4791a0",
      "type": "Microsoft.Authorization/roleAssignments"
    }
    ```

    A questo punto, l'entità servizio è pronta per l'uso.

1. Usare il pulsante destro del mouse/Esegui riga nell'editor nella riga seguente per disconnettersi dall'interfaccia della riga di comando di Azure con il comando seguente:

    ```azurecli
    az logout
    ```

## <a name="steps-for-each-new-development-project-setup"></a>Procedura per ogni nuova installazione del progetto di sviluppo

Dal momento che le librerie di Azure SDK vengono fornite singolarmente per ogni servizio, non esiste un singolo pacchetto scaricabile per accedere a tutte le risorse di Azure. Ogni libreria viene installata in base al servizio di Azure che si vuole usare.

Ogni nuovo progetto che usa Azure deve:
- Creare le risorse di Azure o trovare le informazioni di autenticazione per le risorse di Azure esistenti
- Installare le librerie di Azure SDK da npm o Yarn. Informazioni sulle [versioni delle librerie](#library-versions).
- Gestire in modo sicuro le informazioni di autenticazione all'interno del progetto. Un metodo comune consiste nell'usare **[Dotenv](https://www.npmjs.com/package/dotenv)** per leggere le variabili di ambiente da un file `.env`. Assicurarsi di aggiungere il file `.env` al file `.gitignore` in modo che il file `.env` non venga archiviato nel controllo del codice sorgente.

### <a name="library-versions"></a>Versioni delle librerie

Tutte le librerie di Azure verranno spostate nell'ambito `@azure`.

| Tipo di libreria | Descrizione|
|--|--|
|Moderna|L'ambito di tali librerie è `@azure`, ad esempio [@azure/storage-blob](https://www.npmjs.com/package/@azure/storage-blob) e [@azure/cosmos](https://www.npmjs.com/package/@azure/cosmos), e includono i tipi TypeScript.|
|Pacchetti meno recenti|Iniziano in genere con `azure-`. Molti pacchetti non prodotti da Microsoft sono contraddistinti da questo prefisso. Verificare che il proprietario del pacchetto sia Microsoft o Azure.|

### <a name="create-resource-using-service-principal"></a>Creare la risorsa usando l'entità servizio

La sezione seguente include un esempio relativo alla creazione di una risorsa di servizio di Azure con un'entità servizio. Per accedere con un'entità servizio, sono necessari i valori di `appId`, `tenant` e `password` salvati dalla procedura [Creare l'entità servizio](#create-service-principal) presenti nel file `create-service-principal.json`.

1. Aprire VSCode e usare l'estensione [Strumenti dell'interfaccia della riga di comando di Azure](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azurecli) installata in precedenza. Questa estensione consente di eseguire i comandi dell'interfaccia della riga di comando di Azure dal file script, riga per riga. Per ogni comando eseguito in VSCode viene aperto un documento adiacente per visualizzare i risultati.

1. Creare un nuovo file denominato `create-service-resource.sh` e copiarvi i comandi di Azure seguenti:

    ```azurecli
    ####################################
    # Login as service principal
    ####################################
    # User name for command is the app id
    az login --service-principal --username APP_ID --password PASSWORD --tenant TENANT_ID

    ####################################
    # Create resource group
    ####################################

    # Create resource group in westus region - check your quickstart if it requires a specific region, then change this value to the appropriate region
    # Common naming convention for resource group is `USERNAME-REGION-PURPOSE`
    az group create --location WESTUS --name JOE-WESTUS-QUICKSTARTS-RESOURCEGROUP

    ####################################
    # Create specific service resource
    ####################################

    # Create resource in westus
    # This is an example of creating a Cognitive Services LUIS resource
    # Review your quickstart to find the exact command
    az SERVICENAME account create --name JOE-WESTUS-COGNITIVESERVICES-LUIS --resource-group JOE-WESTUS-QUICKSTARTS-RESOURCEGROUP --kind LUIS --sku F0 --location WESTUS --yes

    ####################################
    # Get resource keys
    ####################################

    # Get resource keys
    az cognitiveservices account keys list --name JOE-WESTUS-COGNITIVESERVICES-LUIS --resource-group JOE-WESTUS-QUICKSTARTS-RESOURCEGROUP
    ```

1. Usare il pulsante destro del mouse/Esegui riga nell'editor nella riga seguente per accedere con l'entità servizio. Le variabili tutte in maiuscolo sono state restituite nella risposta dal [comando usato in precedenza per creare l'entità servizio](#create-service-principal-command).

    ```azurecli
    az login --service-principal --username APP_ID --password PASSWORD --tenant TENANT_ID
    ```

1. Usare il pulsante destro del mouse/Esegui riga nell'editor nella riga seguente per creare un gruppo di risorse per tutte le risorse che è necessario creare per l'avvio rapido. L'area del gruppo di risorse può contenere solo risorse di tale area.

    ```azurecli
    az group create --location WESTUS --name JOE-WESTUS-QUICKSTARTS-RESOURCEGROUP
    ```

    Dopo aver eseguito le operazioni necessarie con le risorse di avvio rapido, è possibile eliminare il gruppo di risorse, in modo da eliminare tutte le risorse contemporaneamente.

1. Usare il pulsante destro del mouse/Esegui riga nell'editor nella riga seguente per creare una risorsa LUIS di Servizi cognitivi. Questo è un esempio, di conseguenza il comando per la risorsa personalizzata sarà diverso.

    ```azurecli
    az SERVICENAME account create --name JOE-WESTUS-COGNITIVESERVICES-LUIS --resource-group JOE-WESTUS-QUICKSTARTS-RESOURCEGROUP --kind LUIS --sku F0 --location WESTUS --yes
    ```

    La risorsa LUIS usa una chiave e un endpoint, che sono necessari per usare gli avvii rapidi per LUIS.

1. Usare il pulsante destro del mouse/Esegui riga nell'editor nella riga seguente per ottenere la chiave e l'endpoint LUIS. Per l'autenticazione al servizio LUIS si usano la chiave e l'endpoint.

    ```azurecli
    az cognitiveservices account keys list --name JOE-WESTUS-COGNITIVESERVICES-LUIS --resource-group JOE-WESTUS-QUICKSTARTS-RESOURCEGROUP
    ```

### <a name="create-environment-variables-for-the-azure-libraries"></a>Creare variabili di ambiente per le librerie di Azure

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

### <a name="install-npm-packages"></a>Installare i pacchetti npm

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
    npm install @azure/cognitiveservices-luis-runtime
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

* [Distribuire un sito Web statico in Azure da Visual Studio Code](tutorial-vscode-static-website-node-01.md)