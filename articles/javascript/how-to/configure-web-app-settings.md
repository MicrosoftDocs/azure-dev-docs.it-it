---
title: Ospitare app Web - Impostazioni di configurazione
description: Informazioni su come impostare configurazioni comuni per l'app Web.
ms.topic: conceptual
ms.date: 01/11/2021
ms.custom: devx-track-js
ms.openlocfilehash: ec183d08460b932cec1dea8d301f2877d7bc9ece
ms.sourcegitcommit: 593d177cfb5f56f236ea59389e43a984da30f104
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/18/2021
ms.locfileid: "98561697"
---
# <a name="hosting-web-apps-on-azure"></a>Hosting di app Web in Azure

Informazioni su come impostare configurazioni comuni per l'app Web. Se un'impostazione comune non è presente, creare un problema nel feedback e segnalarlo. 

Eventuali **impostazioni obbligatorie** sono richieste quando si crea la risorsa. Se un'impostazione non viene richiesta in tale momento, ha un valore predefinito che può essere modificato dopo la creazione della risorsa. 

## <a name="what-is-a-web-app"></a>Informazioni sull'app Web

Un'app Web è qualsiasi elemento raggiunto tramite un URL Internet. Molti servizi di Azure possono essere considerati come un'app Web. I servizi principali usati in genere per un'app Web sono:

* Servizio app, che include anche
    * [App Web statiche](/azure/static-web-apps/)
    * [Funzioni](/azure/azure-functions/)
    * [App Web](/azure/app-service/)
    * [Contenitori](/azure/app-service/configure-custom-container?pivots=container-linux)
* Contenitori - [Kubernetes](/azure/aks/) e [contenitori](/azure/container-instances/) singoli
* Macchine virtuali - [Windows](/azure/virtual-machines/windows) e [Linux](/azure/virtual-machines/linux)

## <a name="how-to-configure-web-app-settings"></a>Come configurare le impostazioni dell'app Web

La maggior parte dei servizi di Azure offre quattro modalità di configurazione delle impostazioni:

* [Azure portal](https://portal.azure.com)
* [Azure SDK](https://github.com/Azure/azure-sdk) per il servizio, indicato in genere come gestione
* [Interfaccia della riga di comando di Azure](/cli/azure/)
* [Azure PowerShell](/powershell/azure/)

Molte impostazioni possono essere configurate anche in Visual Studio Code con le [estensioni](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice). 

## <a name="use-default-domain-name-provided-by-azure"></a>Usare il nome di dominio predefinito fornito da Azure

La maggior parte dei servizi di Azure fornisce un URL per la risorsa. Il nome del servizio determina il sottodominio, mentre il resto del dominio proviene da Azure. 

Ad esempio:

* Funzioni di Azure - `https://my-function-app.azurewebsites.net`
* App Web di Azure - `https://my-web-app.azurewebsites.net`
* BLOB del servizio di archiviazione di Azure - `https://mystorage.blob.core.windows.net/`

Alcuni servizi, ad esempio le app Web statiche, forniscono all'utente un sottodominio relativamente univoco, consentendone l'uso immediato in produzione:

* App Web statiche di Azure = `https://gentle-tree-0b08aaf12.azurestaticapps.net`

## <a name="configure-custom-domain-name"></a>Configurare un nome di dominio personalizzato 

Ogni servizio fornisce un meccanismo specifico per aggiungere un dominio personalizzato. 

* [App Web statiche di Azure](/azure/static-web-apps/custom-domain)
* [Funzioni di Azure](/azure/app-service/app-service-web-tutorial-custom-domain) & [App Web di Azure](/azure/app-service/app-service-web-tutorial-custom-domain) - Le funzioni sono basate sulle app Web, quindi usano lo stesso meccanismo
* [BLOB di archiviazione di Azure:](/azure/storage/blobs/storage-custom-domain-name?tabs=azure-portal)

## <a name="configure-port-forwarding"></a>Configurare il port forwarding

È necessario [eseguire il mapping del numero di porta dell'app](/azure/app-service/configure-language-nodejs?pivots=platform-windows#get-port-number) se non è la porta predefinita, `8080`. Ciò consente al Servizio app di inoltrare richieste alla porta corretta. 

## <a name="configure-browser-for-cors-to-connect-with-server"></a>Configurare il browser per CORS per connettersi al server

Se è necessario connettersi al proprio server e occorre ignorare la sicurezza CORS durante l'esecuzione e il debug con il client in locale, la soluzione consigliata consiste nel configurare questa impostazione nel file di debug di Visual Studio Code, `launch.json`, per passare le impostazioni al browser in modo da disabilitare la sicurezza. 

Poiché questo file viene usato strettamente per avviare una sessione di debug, è possibile archiviarlo nel controllo del codice sorgente in modo sicuro. 

### <a name="configure-edge-browser-to-disable-cors-for-debugging"></a>Configurare il browser Microsoft Edge per disabilitare CORS per il debug

Il file `launch.json` seguente configura l'impostazione **Browser Microsoft Edge** in modo da disattivare la sicurezza CORS per la sessione di debug: `--disable-web-security`. 

```json
{
    // Debug client, with requests to server, w/o 
    // changes to client or server
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Launch Edge against localhost",
            "request": "launch",
            "type": "pwa-msedge",
            "url": "http://localhost:3000",
            "webRoot": "${workspaceFolder}",
            "runtimeArgs": [
                "--disable-web-security"
            ],
        },
    ]
}
```

### <a name="configure-chrome-browser-to-disable-cors-for-debugging"></a>Configurare il browser Chrome per disabilitare CORS per il debug

Il file `launch.json` seguente configura l'impostazione **Browser Chrome** in modo da disattivare la sicurezza CORS per la sessione di debug: `--disable-web-security`. 

```json
{
    // Debug client, with requests to server, w/o 
    // changes to client or server
    "version": "0.2.0",
    "configurations": [
        {
            "type": "pwa-chrome",
            "request": "launch",
            "name": "Launch Chrome against localhost",
            "url": "http://localhost:3000",
            "webRoot": "${workspaceFolder}",
            "runtimeArgs": [
                "--disable-web-security"
            ],
        }
    ]
}
```


## <a name="configure-certificates"></a>Configurare i certificati

Se l'app richiede immediatamente i certificati, sono disponibili diverse opzioni per [fornire certificati](/azure/app-service/configure-ssl-certificate#import-an-app-service-certificate):

* Caricare un certificato personale
* Gestire i certificati nel Servizio app
* Importare il certificato da Azure Key Vault
* Fornire il certificato [nel codice](/azure/app-service/configure-ssl-certificate-in-code)

## <a name="configure-secrets"></a>Configurare segreti

I segreti vengono in genere forniti nei modi seguenti:

* Azure Key Vault - Creare una risorsa per questo servizio, che fornisce [segreti dell'app](/azure/app-service/app-service-key-vault-references). 
* Impostazioni dell'app - Per una soluzione più leggera, è possibile fornire i segreti come impostazioni dell'app e farvi riferimento con le tipiche [variabili di ambiente](/azure/app-service/configure-language-nodejs?pivots=platform-windows), `process.env.VARNAME`. 

## <a name="configure-logging"></a>Configurare la registrazione

La registrazione include:

* Registrazione della piattaforma: eventi che si verificano all'esterno dell'app
* Registrazione dell'app: eventi che si verificano nell'app

I log della piattaforma vengono forniti all'utente per:
* Fornire informazioni sull'integrità dell'ambiente.
* Consentire la scalabilità a un piano tariffario diverso o tra diverse aree. 

I registri applicazioni non vengono forniti all'utente. È possibile aggiungere una registrazione personalizzata per il comportamento interno dell'app:
* [Monitoraggio di Azure](/azure/azure-monitor/overview) offre librerie npm per [Application Insights](/azure/azure-monitor/app/app-insights-overview) per fornire la registrazione e le risorse di archiviazione in cui vengono archiviati i log. 

## <a name="configure-database-and-storage"></a>Configurare il database e l'archiviazione

Una connessione a un database o a una risorsa di archiviazione dei dati inizia in genere con una stringa di connessione. 

Considerazioni sulle connessioni dati:
* È possibile usare la connessione corrente
* Nuova risorsa di archiviazione dei dati: se l'app necessita di un nuovo meccanismo di archiviazione, Azure offre [molte opzioni diverse per i database](integrate-database.md). La connessione deve essere archiviata in modo sicuro. 

## <a name="missing-something"></a>Mancano informazioni? 

Se alcune informazioni non sono presenti nell'elenco, compilare il feedback per segnalare il problema. 

## <a name="next-steps"></a>Passaggi successivi

* Vedere molti di questi passaggi in un flusso di sviluppo di un'[app Node.js end-to-end](./develop-nodejs-on-azure.md).