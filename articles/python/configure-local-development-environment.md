---
title: Configurare l'ambiente Python locale per lo sviluppo di Azure
description: Come configurare un ambiente di sviluppo Python locale per l'uso con Azure, tra cui Visual Studio Code, le librerie di Azure SDK e le credenziali necessarie per l'autenticazione delle librerie.
ms.date: 05/29/2020
ms.topic: conceptual
ms.openlocfilehash: cf87c90bd36594ffa4e1f3837133238f89a77836
ms.sourcegitcommit: 43e4b50f6f6f5806b2f162ca39367face0779ff6
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/04/2020
ms.locfileid: "84421497"
---
# <a name="configure-your-local-python-dev-environment-for-azure"></a>Configurare l'ambiente di sviluppo Python locale per Azure

Gli sviluppatori di applicazioni cloud preferiscono in genere testare il codice nelle rispettive workstation locali prima di distribuirlo in un ambiente cloud come Azure. Lo sviluppo locale offre il vantaggio della velocità, oltre a un'ampia varietà di strumenti di debug.

Questo articolo include le istruzioni di installazione da seguire una sola volta per creare e convalidare l'ambiente di sviluppo locale adatto per Python in Azure:

- [Installare i componenti necessari](#required-components), ovvero un account Azure, Python e l'interfaccia della riga di comando di Azure.
- [Configurare l'autenticazione](#configure-authentication) per l'uso delle librerie di Azure per effettuare il provisioning, gestire e accedere alle risorse di Azure.
- Esaminare il processo associato all'[uso di ambienti virtuali Python](#use-python-virtual-environments) per ogni progetto.

Una volta configurata la workstation, sarà necessario aggiungere solo una configurazione minima per completare varie guide di avvio rapido ed esercitazioni altrove in questo centro per sviluppatori e nella documentazione di Azure.

Questa configurazione per lo sviluppo locale è una questione diversa rispetto alle [risorse di provisioning](cloud-development-flow.md) che costituiscono l'*ambiente cloud* dell'applicazione in Azure. Nel processo di sviluppo viene eseguito codice nell'ambiente di sviluppo locale in grado di accedere a tali risorse nel cloud, ma il codice non è ancora distribuito in un [servizio di hosting appropriato](quickstarts-app-hosting.md) nel cloud. Il passaggio di distribuzione avviene in seguito, come descritto nell'articolo sul [flusso di sviluppo di Azure](cloud-development-flow.md).

## <a name="install-components"></a>Installazione dei componenti

### <a name="required-components"></a>Componenti richiesti

| Nome/Programma di installazione | Descrizione |
| --- | --- |
| [Account Azure con una sottoscrizione attiva](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=python-dev-center&mktingSource=environment-setup) | Gli account e le sottoscrizioni sono gratuiti e includono molti servizi gratuiti. |
| [Python 2.7 o 3.5.3 e versioni successive](https://www.python.org/downloads) | Runtime del linguaggio Python. È consigliabile usare l'ultima versione di Python 3.x a meno che non siano previsti requisiti specifici per la versione. |
| [Interfaccia della riga di comando di Azure](/cli/azure/install-azure-cli) | Fornisce una serie completa di comandi dell'interfaccia della riga di comando per il provisioning e la gestione delle risorse di Azure. Per usare le librerie di gestione di Azure, gli sviluppatori Python solitamente usano l'interfaccia della riga di comando di Azure con script Python personalizzati. |

Note:

- I singoli pacchetti di librerie di Azure si installano per ogni progetto in base alle esigenze. È consigliabile [usare ambienti virtuali Python](#use-python-virtual-environments) per ogni progetto. Non esiste alcun programma di installazione "SDK" autonomo per Python.
- Anche se Azure PowerShell è in generale equivalente all'interfaccia della riga di comando di Azure, è consigliabile usare l'interfaccia della riga di comando di Azure quando si lavora con Python.

### <a name="recommended-components"></a>Componenti consigliati

| Nome/Programma di installazione | Descrizione |
| --- | --- |
| [Visual Studio Code](https://code.visualstudio.com) | Anche se è possibile usare qualsiasi editor o IDE, l'ambiente IDE leggero e gratuito di Microsoft è ampiamente diffuso tra gli sviluppatori Python. Per un'introduzione, vedere [Python in VS Code](https://code.visualstudio.com/docs/python/python-tutorial). |
| [Estensione di Python per VS Code](https://marketplace.visualstudio.com/items?itemName=ms-python.python) | Aggiunge il supporto per Python a VS Code. |
| [Estensione di Azure per VS Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-node-azure-pack) | Aggiunge il supporto per un'ampia gamma di servizi di Azure a VS Code. È anche possibile installare singolarmente il supporto per servizi specifici. |
| [git](https://git-scm.com/downloads) | Strumenti da riga di comando per il controllo del codice sorgente. Se si preferisce, è possibile usare strumenti diversi per il controllo del codice sorgente. |

### <a name="optional-components"></a>Componenti facoltativi

| Nome/Programma di installazione | Descrizione |
| --- | --- |
| [Estensione di Docker per VS Code](https://marketplace.visualstudio.com/items?itemName=ms-python.python) | Aggiunge il supporto per Docker a VS Code, che risulta utile se si lavora regolarmente con i contenitori. |

### <a name="verify-components"></a>Verificare i componenti

1. Aprire un terminale o un prompt dei comandi.
1. Verificare la versione di Python eseguendo il comando `python --version`.
1. Verificare la versione dell'interfaccia della riga di comando di Azure eseguendo `az --version`.
1. Verificare l'installazione di VS Code:
    1. Eseguire `code .` per aprire VS Code nella cartella corrente.
    1. In VS Code selezionare il comando **Visualizza** > **Estensioni** per aprire la visualizzazione delle estensioni, quindi verificare che l'elenco includa "Python" e "Account Azure" (tra altre estensioni "Azure" e "Docker", se è stata installata anche questa estensione).

## <a name="sign-in-to-azure-from-the-cli"></a>Accedere ad Azure dall'interfaccia della riga di comando di Azure

In un terminale o da un prompt dei comandi accedere alla sottoscrizione di Azure:

```azurecli
az login
```

`az` è il comando radice dell'interfaccia della riga di comando di Azure. `az` è seguito da uno o più comandi specifici, ad esempio `login`. Vedere le informazioni di riferimento del comando [az login](/cli/azure/authenticate-azure-cli).

L'interfaccia della riga di comando di Azure mantiene in genere l'accesso tra le sessioni, ma è consigliabile eseguire `az login` ogni volta che si apre un nuovo terminale o un nuovo prompt dei comandi.

## <a name="configure-authentication"></a>Configurare l'autenticazione

Come descritto in [Come gestire le entità servizio - Informazioni di base sull'autorizzazione](how-to-manage-service-principals.md#basics-of-azure-authorization), ogni sviluppatore deve usare un'entità servizio come identità dell'applicazione durante i test del codice dell'app in locale.

Le sezioni seguenti descrivono come creare un'entità servizio e le variabili di ambiente che forniscono le relative proprietà alle librerie di Azure quando è necessario.

Ogni sviluppatore dell'organizzazione dovrà eseguire questi passaggi singolarmente.

### <a name="create-a-service-principal-and-environment-variables-for-development"></a>Creare un'entità servizio e le variabili di ambiente per lo sviluppo

1. Aprire un terminale o un prompt dei comandi in cui è stato eseguito l'accesso all'interfaccia della riga di comando di Azure (`az login`).

1. Creare l'entità servizio:

    ```azurecli
    az ad sp create-for-rbac --name localtest-sp-rbac --skip-assignment --sdk-auth > local-sp.json
    ```

    Questo comando salva l'output in *local-sp.json*. Per altri dettagli sul comando e sui relativi argomenti, vedere [Funzioni del comando create-for-rbac](#what-the-create-for-rbac-command-does).

    Se si fa parte di un'organizzazione, è possibile che non si abbia l'autorizzazione nella sottoscrizione per eseguire questo comando. In tal caso, chiedere ai proprietari della sottoscrizione di creare l'entità servizio.

1. Creare le variabili di ambiente necessarie per le librerie di Azure. L'oggetto `DefaultAzureCredential` della libreria azure-identity cerca queste variabili.

    # <a name="cmd"></a>[cmd](#tab/cmd)

    ```cmd
    set AZURE_SUBSCRIPTION_ID="aa11bb33-cc77-dd88-ee99-0918273645aa"
    set AZURE_TENANT_ID=00112233-7777-8888-9999-aabbccddeeff
    set AZURE_CLIENT_ID=12345678-1111-2222-3333-1234567890ab
    set AZURE_CLIENT_SECRET=abcdef00-4444-5555-6666-1234567890ab
    ```

    # <a name="bash"></a>[Bash](#tab/bash)

    ```bash
    AZURE_SUBSCRIPTION_ID="aa11bb33-cc77-dd88-ee99-0918273645aa"
    AZURE_TENANT_ID="00112233-7777-8888-9999-aabbccddeeff"
    AZURE_CLIENT_ID="12345678-1111-2222-3333-1234567890ab"
    AZURE_CLIENT_SECRET="abcdef00-4444-5555-6666-1234567890ab"
    ```

    ---

    Sostituire i valori mostrati in questi comandi con quelli dell'entità servizio specifica.

    Per recuperare l'ID sottoscrizione, eseguire il comando [`az account show`](/cli/azure/account?view=azure-cli-latest#az-account-show) e cercare la proprietà `id` nell'output.

    Per praticità, creare un file con estensione *sh* o *cmd* con questi comandi, che è possibile eseguire ogni volta che si apre un terminale o un prompt dei comandi per i test locali. Anche in questo caso, non aggiungere il file al controllo del codice sorgente in modo che rimanga solo nell'account utente.

1. Proteggere l'ID client e il segreto client (e tutti i file in cui sono archiviati) in modo che rimangano sempre all'interno di un account utente specifico in una workstation. Non salvare mai queste proprietà nel controllo del codice sorgente né condividerle con altri sviluppatori. Se necessario, è possibile eliminare l'entità servizio e crearne una nuova.

    Per un ulteriore livello di sicurezza, è possibile creare un criterio per eliminare e ricreare le entità servizio in base a una pianificazione regolare, invalidando di conseguenza gli ID e i segreti precedenti.

    Inoltre, un'entità servizio di sviluppo è idealmente autorizzata solo per risorse non di produzione o viene creata all'interno di una sottoscrizione di Azure usata solo a scopo di sviluppo. L'applicazione di produzione userebbe una sottoscrizione diversa e risorse di produzione distinte, autorizzate solo per l'applicazione cloud distribuita.

1. Per modificare o eliminare le entità servizio in un secondo momento, vedere [Come gestire le entità servizio](how-to-manage-service-principals.md).

#### <a name="what-the-create-for-rbac-command-does"></a>Funzioni del comando create-for-rbac

Il `az ad create-for-rbac` comando crea un'entità servizio per l'autenticazione in base al ruolo (Controllo degli accessi in base al ruolo).

- `ad` indica Azure Active Directory, `sp` significa "entità servizio" e `create-for-rbac` significa "crea per il controllo degli accessi in base al ruolo", il tipo principale di autorizzazione di Azure. Vedere le informazioni di riferimento del comando [az ad sp create-for-rbac](/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac).

- L'argomento `--name` deve essere univoco all'interno dell'organizzazione e in genere corrisponde al nome dello sviluppatore che usa l'entità servizio. Se si omette questo argomento, l'interfaccia della riga di comando di Azure usa un nome generico in formato `azure-cli-<timestamp>`. Se si preferisce, è possibile rinominare l'entità servizio nel portale di Azure.

- L'argomento `--skip-assignment` crea un'entità servizio senza autorizzazioni predefinite. È quindi necessario assegnare autorizzazioni specifiche all'entità servizio per consentire al codice in esecuzione in locale di accedere alle risorse. Diverse guide di avvio rapido ed esercitazioni forniscono informazioni dettagliate su come autorizzare un'entità servizio per le risorse necessarie.

- Il comando genera un output JSON, che viene salvato in un file denominato *local-sp.json*.

- L'argomento `--sdk-auth` genera un output JSON simile ai valori seguenti. I valori di ID e il segreto saranno tutti diversi:

    <pre>
    {
      "clientId": "12345678-1111-2222-3333-1234567890ab",
      "clientSecret": "abcdef00-4444-5555-6666-1234567890ab",
      "subscriptionId": "00000000-0000-0000-0000-000000000000",
      "tenantId": "00112233-7777-8888-9999-aabbccddeeff",
      "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
      "resourceManagerEndpointUrl": "https://management.azure.com/",
      "activeDirectoryGraphResourceId": "https://graph.windows.net/",
      "sqlManagementEndpointUrl": "https://management.core.windows.net:8443/",
      "galleryEndpointUrl": "https://gallery.azure.com/",
      "managementEndpointUrl": "https://management.core.windows.net/"
    }
    </pre>

    Senza l'argomento `--sdk-auth`, il comando genera un output più semplice:

    <pre>
    {
      "appId": "12345678-1111-2222-3333-1234567890ab",
      "displayName": "localtest-sp-rbac",
      "name": "http://localtest-sp-rbac",
      "password": "abcdef00-4444-5555-6666-1234567890ab",
      "tenant": "00112233-7777-8888-9999-aabbccddeeff"
    }
    </pre>

    In questo caso, `tenant` è l'ID tenant, `appId` è l'ID client e `password` è il segreto client.

    > [!IMPORTANT]
    > L'output di questo comando è l'unico punto in cui sarà visibile la password o il segreto client. Non è possibile recuperare la password o il segreto in un secondo momento. È tuttavia possibile aggiungere un nuovo segreto, se necessario, senza invalidare l'entità servizio o i segreti esistenti.

## <a name="use-python-virtual-environments"></a>Usare gli ambienti virtuali Python

Per ogni progetto, è consigliabile creare e attivare sempre un *ambiente virtuale* seguendo questa procedura:

1. Aprire un terminale o un prompt dei comandi.

1. Creare una cartella per il progetto.

1. Creare l'ambiente virtuale:

    # <a name="cmd"></a>[cmd](#tab/cmd)

    ```bash
    python -m venv .venv
    ```

    # <a name="bash"></a>[Bash](#tab/bash)

    ```bash
    python -m venv .venv
    ```

    ---

    Questo comando esegue il modulo `venv` di Python e crea un ambiente virtuale in una cartella denominata `.venv`.

1. Attivare l'ambiente virtuale:

    # <a name="cmd"></a>[cmd](#tab/cmd)

    ```bash
    .venv\scripts\activate
    ```

    # <a name="bash"></a>[Bash](#tab/bash)

    ```bash
    source .venv/scripts/activate
    ```

    ---

Un ambiente virtuale è una cartella all'interno di un progetto che isola una copia di un interprete Python specifico. Una volta attivato l'ambiente (cosa che Visual Studio Code fa automaticamente), l'esecuzione di `pip install` installa una libreria solo al suo interno. Quindi il codice Python verrà eseguito nel contesto esatto dell'ambiente con le versioni specifiche di ogni libreria. Eseguendo `pip freeze`, si ottiene l'elenco esatto di tali librerie. In molti esempi di questa documentazione si crea un file *requirements.txt* per le librerie necessarie, quindi si usa `pip install -r requirements.txt`. Un file dei requisiti è in genere necessario quando si distribuisce il codice in Azure.

Se non si usa un ambiente virtuale, Python viene eseguito nell'*ambiente globale*. Pur essendo rapido e pratico da usare, l'ambiente globale tende a riempirsi a dismisura nel corso del tempo con tutte le librerie installate per qualsiasi progetto o esperimento. Inoltre, se si aggiorna una raccolta per un progetto, si potrebbero danneggiare altri progetti che dipendono da versioni diverse della libreria. E poiché l'ambiente è condiviso da un numero qualsiasi di progetti, non è possibile usare `pip freeze` per recuperare un elenco di dipendenze di un progetto specifico.

Nell'ambiente globale è consigliabile installare i pacchetti di strumenti da usare in più progetti. Ad esempio, è possibile eseguire `pip install gunicorn` nell'ambiente globale per rendere disponibile il server Web gunicorn ovunque.

## <a name="use-source-control"></a>Usare il controllo del codice sorgente

È consigliabile creare un repository del controllo del codice sorgente ogni volta che si avvia un progetto. Se è installato Git, è sufficiente eseguire il comando seguente:

```cmd
git init
```

Da qui è possibile usare comandi come `git add` e `git commit` per eseguire il commit delle modifiche. Eseguendo regolarmente il commit delle modifiche, viene creata una cronologia di commit con cui è possibile ripristinare uno stato precedente.

Per eseguire un backup online del progetto, è anche consigliabile caricare il repository in [GitHub](https://github.com) o in [Azure DevOps](/azure/devops/user-guide/code-with-git?view=azure-devops). Se è già stato inizializzato un repository locale, usare `git remote add` per collegarlo a GitHub o ad Azure DevOps.

La documentazione per Git è disponibile all'indirizzo [git-scm.com/docs](https://git-scm.com/docs) e ovunque su Internet.

Visual Studio Code include una serie di funzionalità Git predefinite. Per altre informazioni, vedere [Uso del controllo della versione in VS Code](https://code.visualstudio.com/docs/editor/versioncontrol).

È anche possibile usare qualsiasi altro strumento di controllo del codice sorgente a scelta. Git è semplicemente uno dei più diffusi e supportati.

## <a name="next-step"></a>Passaggio successivo

Una volta implementato l'ambiente di sviluppo locale, vedere una rapida panoramica sui modelli di utilizzo comuni delle librerie di Azure:

> [!div class="nextstepaction"]
> [Esaminare i modelli di utilizzo comuni >>>](azure-sdk-library-usage-patterns.md)
