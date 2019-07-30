---
title: Distribuire una macchina virtuale di Azure da Go
description: Distribuire una macchina virtuale tramite Azure SDK per Go.
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 09/05/2018
ms.topic: quickstart
ms.devlang: go
ms.openlocfilehash: 01f6e40e80a4c5f29a6179869a2fd95f6cea0623
ms.sourcegitcommit: 2efdb9d8a8f8a2c1914bd545a8c22ae6fe0f463b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/15/2019
ms.locfileid: "68291956"
---
# <a name="quickstart-deploy-an-azure-virtual-machine-from-a-template-with-the-azure-sdk-for-go"></a>Guida introduttiva: Distribuire una macchina virtuale da un modello con Azure SDK per Go

Questa guida introduttiva illustra come distribuire risorse da un modello di Azure Resource Manager usando Azure SDK per Go. I modelli sono snapshot di tutte le risorse incluse in un [gruppo di risorse di Azure](/azure/azure-resource-manager/resource-group-overview). Sarà possibile acquisire familiarità con la funzionalità e le convenzioni dell'SDK.

Al termine della guida introduttiva sarà disponibile una VM in esecuzione a cui si accede con un nome utente e una password.

> [!NOTE]
> Per informazioni sulla creazione di una VM in Go senza il modello di Resource Manager, vedere l'[esempio imperativo](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute/vm.go) che illustra come creare e configurare tutte le risorse della VM con l'SDK. L'uso di un modello in questo esempio consente di concentrarsi sulle convenzioni dell'SDK, senza dover fornire informazioni troppo dettagliate sull'architettura dei servizi di Azure.

[!INCLUDE [quickstarts-free-trial-note](includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](includes/cloud-shell-try-it.md)]

Se si usa un'installazione locale dell'interfaccia della riga di comando di Azure, questa guida introduttiva richiede l'interfaccia della riga di comando versione __2.0.28__ o successiva. Eseguire `az --version` per assicurarsi che l'installazione dell'interfaccia della riga di comando rispetti questo requisito. Se è necessario eseguire l'installazione o l'aggiornamento, vedere [Installare l'interfaccia della riga di comando di Azure](/cli/azure/install-azure-cli).

## <a name="install-the-azure-sdk-for-go"></a>Installare Azure SDK per Go

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

## <a name="create-a-service-principal"></a>Creare un'entità servizio

Per accedere in modalità non interattiva ad Azure con un'applicazione, è necessaria un'entità servizio. Le entità servizio rientrano nel controllo degli accessi in base al ruolo, che crea un'identità univoca per l'utente. Per creare una nuova entità servizio con l'interfaccia della riga di comando, eseguire questo comando:

```azurecli-interactive
az ad sp create-for-rbac --sdk-auth > quickstart.auth
```

Impostare la variabile di ambiente `AZURE_AUTH_LOCATION` come percorso completo al file. L'SDK individua e legge le credenziali direttamente da questo file, senza che sia necessario apportare modifiche o registrare informazioni dall'entità servizio.

## <a name="get-the-code"></a>Ottenere il codice

Ottenere il codice della guida introduttiva e tutte le rispettive dipendenze con `go get`.

```bash
go get -u -d github.com/azure-samples/azure-sdk-for-go-samples/quickstarts/deploy-vm/...
```

Se la variabile `AZURE_AUTH_LOCATION` è impostata correttamente, non è necessario apportare modifiche al codice sorgente. Quando viene eseguito il programma, tutte le informazioni di autenticazione necessarie vengono caricati da esso.

## <a name="running-the-code"></a>Esecuzione del codice

Eseguire la guida introduttiva con il comando `go run`.

```bash
cd $GOPATH/src/github.com/azure-samples/azure-sdk-for-go-samples/quickstarts/deploy-vm
go run main.go
```

Se la distribuzione ha esito positivo, viene visualizzato un messaggio che indica nome utente, indirizzo IP e password per l'accesso alla macchina virtuale appena creata. Accedere tramite SSH alla macchina virtuale per verificare che sia attiva e in esecuzione. 

## <a name="cleaning-up"></a>Cleaning up

Pulire le risorse create durante la guida introduttiva eliminando il gruppo di risorse con l'interfaccia della riga di comando.

```azurecli-interactive
az group delete -n GoVMQuickstart
```

Eliminare anche l'entità servizio creata. Nel file `quickstart.auth` è disponibile una chiave JSON per `clientId`. Copiare questo valore nella variabile di ambiente `CLIENT_ID_VALUE` ed eseguire questo comando dell'interfaccia della riga di comando di Azure:

```azurecli-interactive
az ad sp delete --id ${CLIENT_ID_VALUE}
```

In questo comando si fornisce il valore per `CLIENT_ID_VALUE` da `quickstart.auth`.

> [!WARNING]
> Se non si elimina l'entità servizio per l'applicazione, l'applicazione risulterà attiva nel tenant di Azure Active Directory.
> Anche se il nome e la password per l'entità servizio vengono generati come UUID, è necessario assicurarsi di seguire procedure di sicurezza appropriate eliminando eventuali entità servizio e applicazioni di Azure Active Directory non usate.

## <a name="code-in-depth"></a>Informazioni dettagliate sul codice

Le operazioni eseguite dal codice della guida introduttiva vengono suddivise in un blocco di variabili e di alcune piccole funzioni, ognuna delle quali viene illustrata in questo articolo.

### <a name="variables-constants-and-types"></a>Variabili, costanti e tipi

Essendo indipendente, la guida introduttiva usa variabili e costanti globali.

```go
const (
    resourceGroupName     = "GoVMQuickstart"
    resourceGroupLocation = "eastus"

    deploymentName = "VMDeployQuickstart"
    templateFile   = "vm-quickstart-template.json"
    parametersFile = "vm-quickstart-params.json"
)

// Information loaded from the authorization file to identify the client
type clientInfo struct {
    SubscriptionID string
    VMPassword     string
}

var (
    ctx        = context.Background()
    clientData clientInfo
    authorizer autorest.Authorizer
)
```

Vengono dichiarati i valori che specificano i nomi delle risorse create. Viene specificata anche la posizione, che può essere modificata per verificare il comportamento delle distribuzioni in altri data center. Non tutte le risorse necessarie sono disponibili in tutti i data center.

Il tipo `clientInfo` include le informazioni caricate dal file di autenticazione per configurare i client nell'SDK e impostare la password della VM.

Le costanti `templateFile` e `parametersFile` fanno riferimento ai file necessari per la distribuzione. La variabile `authorizer` verrà configurata dall'SDK per Go per l'autenticazione, mentre la variabile `ctx` è un [contesto Go](https://blog.golang.org/context) per le operazioni di rete.

### <a name="authentication-and-initialization"></a>Autenticazione e inizializzazione

La funzione `init` configura l'autenticazione. Poiché l'autenticazione è una precondizione per qualsiasi operazione nella guida introduttiva, è consigliabile includerla nell'inizializzazione. Essa permette anche di caricare alcune informazioni necessarie dal file di autenticazione per configurare i client e la macchina virtuale.

```go
func init() {
    var err error
    authorizer, err = auth.NewAuthorizerFromFile(azure.PublicCloud.ResourceManagerEndpoint)
    if err != nil {
        log.Fatalf("Failed to get OAuth config: %v", err)
    }

    authInfo, err := readJSON(os.Getenv("AZURE_AUTH_LOCATION"))
    clientData.SubscriptionID = (*authInfo)["subscriptionId"].(string)
    clientData.VMPassword = (*authInfo)["clientSecret"].(string)
}
```

Prima di tutto viene chiamato il comando [auth.NewAuthorizerFromFile](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromFile) per caricare le informazioni di autenticazione dal file che si trova in `AZURE_AUTH_LOCATION`. Successivamente, questo file viene caricato manualmente dalla funzione `readJSON` (qui omessa) per eseguire il pull dei due valori necessari per eseguire il resto del programma: l'ID sottoscrizione del client e il segreto dell'entità servizio, che viene usato anche per la password della macchina virtuale.

> [!WARNING]
> Per semplicità, nella guida introduttiva viene riutilizzata la password dell'entità servizio. Nell'ambiente di produzione, prestare attenzione a non riutilizzare __mai__ una password che consente di accedere alle risorse di Azure.

### <a name="flow-of-operations-in-main"></a>Flusso di operazioni in main()

La funzione `main` è semplice e indica solo il flusso di operazioni e l'esecuzione del controllo degli errori.

```go
func main() {
    group, err := createGroup()
    if err != nil {
        log.Fatalf("failed to create group: %v", err)
    }
    log.Printf("Created group: %v", *group.Name)

    log.Printf("Starting deployment: %s", deploymentName)
    result, err := createDeployment()
    if err != nil {
        log.Fatalf("Failed to deploy: %v", err)
    }
    if result.Name != nil {
        log.Printf("Completed deployment %v: %v", deploymentName, *result.Properties.ProvisioningState)
    } else {
        log.Printf("Completed deployment %v (no data returned to SDK)", deploymentName)
    }
    getLogin()
}
```

Il codice esegue questi passaggi nell'ordine seguente:

* Creare il gruppo di risorse in cui eseguire la distribuzione (`createGroup`)
* Creare la distribuzione all'interno del gruppo (`createDeployment`)
* Ottenere e visualizzare le informazioni di accesso per la VM distribuita (`getLogin`)

### <a name="create-the-resource-group"></a>Creare il gruppo di risorse

La funzione `createGroup` crea il gruppo di risorse. È possibile esaminare il flusso di chiamate e di argomenti per verificare la struttura delle interazioni tra i servizi nell'SDK.

```go
func createGroup() (group resources.Group, err error) {
    groupsClient := resources.NewGroupsClient(clientData.SubscriptionID)
    groupsClient.Authorizer = authorizer

        return groupsClient.CreateOrUpdate(
                ctx,
                resourceGroupName,
                resources.Group{
                        Location: to.StringPtr(resourceGroupLocation)})
}
```

Questo è il flusso generale dell'interazione con un servizio di Azure:

* Creare il client con il metodo `service.New*Client()`, dove `*` è il tipo di risorsa di `service` con cui si vuole interagire. Questa funzione accetta sempre un ID sottoscrizione.
* Impostare il metodo di autorizzazione per il client, consentendo l'interazione con l'API remota.
* Effettuare la chiamata al metodo sul client corrispondente all'API remota. I metodi del client del servizio accettano in genere il nome della risorsa e un oggetto di metadati.

La funzione [`to.StringPtr`](https://godoc.org/github.com/Azure/go-autorest/autorest/to#StringPtr) viene usata qui per eseguire un tipo di conversione. I parametri per i metodi dell'SDK accettano quasi esclusivamente puntatori, quindi vengono forniti metodi utili per semplificare le conversioni dei tipi. Per un elenco completo e informazioni sul comportamento dei convertitori utili, vedere la documentazione per il modulo [autorest/to](https://godoc.org/github.com/Azure/go-autorest/autorest/to).

Il metodo `groupsClient.CreateOrUpdate` ha restituito un puntatore a un tipo di dati che rappresenta il gruppo di risorse. Un valore restituito diretto di questo tipo indica un'operazione a esecuzione ridotta che deve essere asincrona. Nella sezione successiva è disponibile un esempio di operazione a esecuzione prolungata e viene illustrato come interagire con essa.

### <a name="perform-the-deployment"></a>Eseguire la distribuzione

Dopo la creazione del gruppo di risorse, è possibile eseguire la distribuzione. Il codice viene suddiviso in sezioni di dimensioni minori per evidenziare le diverse parti della logica.

```go
func createDeployment() (deployment resources.DeploymentExtended, err error) {
    template, err := readJSON(templateFile)
    if err != nil {
        return
    }
    params, err := readJSON(parametersFile)
    if err != nil {
        return
    }
    (*params)["vm_password"] = map[string]string{
        "value": clientData.VMPassword,
    }
        // ...
```

I file della distribuzione vengono caricati da `readJSON`, per cui non vengono forniti dettagli. Questa funzione restituisce `*map[string]interface{}`, il tipo usato nella creazione dei metadati per la chiamata alla distribuzione delle risorse. La password della macchina virtuale viene impostata manualmente anche sui parametri della distribuzione.

```go
        // ...

    deploymentsClient := resources.NewDeploymentsClient(clientData.SubscriptionID)
    deploymentsClient.Authorizer = authorizer

    deploymentFuture, err := deploymentsClient.CreateOrUpdate(
        ctx,
        resourceGroupName,
        deploymentName,
        resources.Deployment{
            Properties: &resources.DeploymentProperties{
                Template:   template,
                Parameters: params,
                Mode:       resources.Incremental,
            },
        },
    )
    if err != nil {
        return
    }
```

Il codice segue lo stesso criterio della creazione del gruppo di risorse. Viene creato un nuovo client, a cui viene consentita l'autenticazione in Azure, e quindi viene chiamato un metodo.
Il metodo ha addirittura lo stesso nome (`CreateOrUpdate`) del metodo corrispondente per i gruppi di risorse. Questo criterio viene visualizzato in tutto l'SDK.
I metodi che eseguono operazioni simili hanno in genere lo stesso nome.

La differenza principale è costituita dal valore restituito del metodo `deploymentsClient.CreateOrUpdate`. Questo valore, di tipo [Future](https://godoc.org/github.com/Azure/go-autorest/autorest/azure#Future), segue lo [schema progettuale degli oggetti Future](https://en.wikipedia.org/wiki/Futures_and_promises). Gli oggetti Future rappresentano un'operazione con esecuzione prolungata in Azure per cui è possibile eseguire il polling, annullare o bloccare il loro completamento.

```go
        //...
    err = deploymentFuture.Future.WaitForCompletion(ctx, deploymentsClient.BaseClient.Client)
    if err != nil {
        return
    }
    return deploymentFuture.Result(deploymentsClient)
}
```

Per questo esempio è consigliabile attendere il completamento dell'operazione. Per rimanere in attesa di un oggetto Future sono necessari sia un [contesto di ambiente](https://blog.golang.org/context), sia il client che ha creato `Future`. Possono verificarsi due tipi di errore, ovvero un errore provocato sul lato client durante il tentativo di chiamata del metodo e una risposta di errore dal server. Quest'ultimo tipo di errore viene restituito come parte della chiamata `deploymentFuture.Result`.

### <a name="get-the-assigned-ip-address"></a>Ottenere l'indirizzo IP assegnato

Per eseguire qualsiasi operazione con la macchina virtuale appena creata, è necessario l'indirizzo IP assegnato. Gli indirizzi IP sono risorse di Azure distinte, associate a risorse della scheda di interfaccia di rete.

```go
func getLogin() {
    params, err := readJSON(parametersFile)
    if err != nil {
        log.Fatalf("Unable to read parameters. Get login information with `az network public-ip list -g %s", resourceGroupName)
    }

    addressClient := network.NewPublicIPAddressesClient(clientData.SubscriptionID)
    addressClient.Authorizer = authorizer
    ipName := (*params)["publicIPAddresses_QuickstartVM_ip_name"].(map[string]interface{})
    ipAddress, err := addressClient.Get(ctx, resourceGroupName, ipName["value"].(string), "")
    if err != nil {
        log.Fatalf("Unable to get IP information. Try using `az network public-ip list -g %s", resourceGroupName)
    }

    vmUser := (*params)["vm_user"].(map[string]interface{})

    log.Printf("Log in with ssh: %s@%s, password: %s",
        vmUser["value"].(string),
        *ipAddress.PublicIPAddressPropertiesFormat.IPAddress,
        clientData.VMPassword)
}
```

Questo metodo si basa su informazioni archiviate nel file dei parametri. Il codice può eseguire query direttamente sulla macchina virtuale per ottenere la rispettiva scheda di interfaccia di rete, può eseguire query sulla scheda di interfaccia di rete per ottenere la rispettiva risorsa IP e quindi può eseguire query direttamente sulla risorsa IP. È prevista una lunga catena di dipendenze e operazioni da risolvere, che risulta costosa. Poiché le informazioni JSON sono locali, è anche possibile caricarle.

Anche il valore per l'utente della macchina virtuale viene caricato dalle informazioni JSON. La password della macchina virtuale è stata caricata in precedenza dal file di autenticazione.

## <a name="next-steps"></a>Passaggi successivi

In questa guida introduttiva è stato selezionato un modello esistente e il modello è stato distribuito tramite Go. È stata quindi stabilita la connessione alla macchina virtuale appena creata tramite SSH.

Per continuare a ottenere informazioni sull'uso delle macchine virtuali nell'ambiente di Azure con Go, vedere gli [esempi di calcolo di Azure per Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute) o gli [esempi di gestione delle risorse di Azure per Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/resources).

Per altre informazioni sui metodi di autenticazione disponibili nell'SDK e sui tipi di autenticazione che supportano, vedere [Autenticazione con Azure SDK per Go](azure-sdk-go-authorization.md).
