---
title: Che cos'è Azure per sviluppatori JavaScript?
description: Concetti di Azure per sviluppatori JavaScript, TypeScript e Node.js.
ms.topic: conceptual
ms.date: 12/15/2020
ms.custom: devx-track-js
ms.openlocfilehash: b6d0e54bdaf1b0ea9adba9800de58fe664df8324
ms.sourcegitcommit: 3d906f265b748fbc0a070fce252098675674c8d9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/22/2021
ms.locfileid: "98699799"
---
# <a name="what-is-azure-for-javascript-developers"></a>Che cos'è Azure per sviluppatori JavaScript?

Azure è una piattaforma cloud che offre una gamma completa di opzioni di hosting e servizi basati sul cloud. Se non si ha familiarità con lo sviluppo cloud, vedere altre informazioni su Azure:

* [Centro architetture di Azure](/azure/architecture/) 
* [Terminologia di Azure](/azure/cloud-adoption-framework/ready/considerations/fundamental-concepts)
* [Dieci principi di progettazione per le applicazioni Azure](/azure/architecture/guide/design-principles/)
* [Modelli di progettazione cloud](/azure/architecture/patterns/)

## <a name="javascript-typescript-and-other-languages"></a>JavaScript, TypeScript e altri linguaggi

Il supporto del runtime di Azure per JavaScript include anche TypeScript o qualsiasi altra versione con transpile a JavaScript. 

## <a name="run-javascript-with-hosted-runtime"></a>Eseguire JavaScript con runtime ospitato 

* Il [servizio app](/azure/app-service/) di Azure usa il motore di runtime Node.js. Per visualizzare tutte le versioni di Node.js supportate, eseguire il comando seguente in [Cloud Shell](https://shell.azure.com):

    ```azurecli-interactive
    az webapp list-runtimes | grep node
    ```

* Le [versioni di Node.js supportate da Funzioni di Azure](/azure/azure-functions/functions-reference-node?tabs=v2#node-version) si basano sulla versione di Funzioni in uso. 

* Runtime personalizzati: un runtime personalizzato è supportato nei modi seguenti:

    * [Macchine virtuali](/azure/virtual-machines/)
    * Contenitori: [singolo](/azure/container-instances/), [app Web](/azure/app-service/), [Kubernetes](/azure/aks/)
    * Funzioni (serverless): usare [gestori personalizzati](/azure/azure-functions/functions-custom-handlers)

## <a name="run-javascript-with-azure-sdks"></a>Eseguire JavaScript con gli SDK di Azure

Tutti gli SDK di Azure vengono eseguiti con JavaScript senza altri strumenti. Anche se la maggior parte dei moderni SDK sono scritti in TypeScript e includono il file `*.d.ts` per il controllo dei tipi, TypeScript non è un requisito per usare gli SDK di Azure o i servizi cloud di Azure. 

Il codice JavaScript può usare i servizi di Azure, indipendentemente dalla posizione in cui sono ospitati (ambiente locale, ibrido, cloud). Per usare i servizi di Azure a livello di codice con JavaScript, è consigliabile usare gli SDK di Azure. Questi SDK richiedono almeno la versione 8 e successive di Node.js. 

## <a name="azure-concepts-and-terminology"></a>Concetti e terminologia di Azure

Per iniziare a usare Azure, è necessario:
* Creare una [sottoscrizione gratuita](https://azure.microsoft.com/en-us/free/)
* Creare una risorsa, per cui è necessario:
    * Accedere al [portale di Azure](https://portal.azure.com/)
    * Creare un gruppo di risorse in cui contenere una raccolta logica delle risorse di Azure
    * Creare una risorsa nel gruppo di risorse
* Usare la risorsa, a seconda del tipo, nei modi seguenti:
    * Con il portale di Azure
    * Con l'interfaccia della riga di comando di Azure
    * Con PowerShell
    * Con gli SDK di Azure

Vedere altre informazioni sui [concetti introduttivi e sulla terminologia di Azure](/azure/cloud-adoption-framework/ready/considerations/fundamental-concepts). 

Azure fornisce indicazioni su [convenzioni di denominazione e assegnazione di tag alle risorse](/azure/cloud-adoption-framework/ready/azure-best-practices/naming-and-tagging) per facilitare l'individuazione delle risorse. 

## <a name="select-development-ide-and-set-up-environment"></a>Selezionare l'IDE di sviluppo e configurare l'ambiente

Sviluppare l'applicazione JavaScript con un ambiente di sviluppo integrato. È consigliabile usare **Visual Studio Code** per lo sviluppo JavaScript. 

Vedere altre informazioni sugli [strumenti consigliati per gli sviluppatori JavaScript](../node-azure-tools.md). 

## <a name="select-azure-account-subscription-resource-and-tag"></a>Selezionare l'account Azure, la sottoscrizione, la risorsa e il tag

* L'**account Azure** è associato a una sottoscrizione tramite l'indirizzo di posta elettronica. È possibile avere uno o più account Azure.

* Una **sottoscrizione** è un'unità organizzativa e di fatturazione per le risorse. È possibile avere una o più sottoscrizioni. Vedere altre informazioni su come [creare le sottoscrizioni di produzione iniziali](/azure/cloud-adoption-framework/ready/azure-best-practices/initial-subscriptions).

* Il **gruppo di risorse** è il livello inferiore successivo dell'organizzazione, all'interno di una sottoscrizione. Creare un nuovo gruppo di risorse per ogni progetto o gruppo di servizi di Azure che si prevede di usare insieme. 

* Il nome selezionato per una **risorsa** verrà usato per l'endpoint URL della risorsa. È anche possibile assegnare un nome di dominio personalizzato a molte risorse dopo averle create. 

* Un raggruppamento più fluido di risorse consiste nel **contrassegnarle con un tag**, con un nome e un valore di proprietà. In genere il proprietario di progetto assegna a ogni risorsa che crea il proprio nome o indirizzo di posta elettronica. Un amministratore della sottoscrizione può facilmente trovare la risorsa di cui si è proprietari o determinarne la proprietà quando vengono rilevati problemi di utilizzo o di fatturazione. 

Una [convenzione di denominazione sistematica](/azure/cloud-adoption-framework/ready/azure-best-practices/resource-naming) è particolarmente importante per l'utilizzo di Azure a livello di produzione.

## <a name="create-azure-resources"></a>Creare le risorse di Azure

Il modo più comune per creare risorse per un nuovo utente di Azure consiste nell'usare il portale di Azure. La procedura di creazione mostra ogni selezione che è necessario effettuare insieme alla scelta consigliata.

### <a name="pricing-tiers"></a>Piani tariffari

I piani tariffari rappresentano il modo in cui viene fatturata la risorsa. Usare il [calcolatore dei prezzi di Azure](https://azure.microsoft.com/en-us/pricing/calculator) per capire come verrà fatturata la risorsa. 

### <a name="free-tier-resources"></a>Risorse del livello gratuito

Quando si seleziona il piano tariffario gratuito (F0), è importante conoscerne le limitazioni.

Quando è disponibile un livello gratuito:

* Una sottoscrizione può essere limitata a una sola risorsa gratuita del servizio. Se non è possibile creare una risorsa gratuita, significa che esiste già nella sottoscrizione.
* Se si supera la quota del piano tariffario, in numero di transazioni al secondo o al mese, l'applicazione riceverà un errore HTTP con un messaggio che indica che la quota è stata superata. 

### <a name="resource-creation-time"></a>Tempo necessario per la creazione di risorse

La creazione della maggior parte delle risorse richiede alcuni minuti, ma per alcune può essere necessario più tempo. La finestra di notifiche del portale di Azure segnala quando la risorsa è disponibile per l'uso fornendo anche un collegamento corrispondente. 

## <a name="install-azure-sdk-for-resources"></a>Installare gli SDK di Azure per le risorse

Gli [**SDK di Azure**](../azure-sdk-library-package-index.md) costituiscono la scelta consigliata per gli sviluppatori JavaScript per usare le risorse a livello di codice. 

Le risorse di Azure sono anche disponibili in:
* [Interfaccia della riga di comando di Azure](/cli/azure/install-azure-cli) e [Azure Cloud Shell](https://shell.azure.com/)
* [Azure PowerShell](/powershell/azure/)
* [API REST](/rest/api/azure/)

## <a name="deploy-web-apps-to-hosting-options"></a>Distribuire app Web in opzioni di hosting

Le opzioni di hosting consentono di usare rapidamente Azure per l'applicazione. Le guide di avvio rapido e le esercitazioni seguenti sull'hosting illustrano l'esperienza più comune del primo utilizzo di Azure:

* Applicazione client/statica
    * [Vanilla JS](/azure/static-web-apps/getting-started?tabs=vanilla-javascript)
    * [React](/azure/static-web-apps/getting-started?tabs=react)
    * [Angular](/azure/static-web-apps/getting-started?tabs=angular)
    * [Vue](/azure/static-web-apps/getting-started?tabs=vue)
* Applicazione server 
    * [Distribuire l'app MongoDB Express.js nel servizio app da Visual Studio Code](../tutorial/deploy-nodejs-mongodb-app-service-from-visual-studio-code.md)
* Applicazione contenitore 
    * [Distribuire l'app in contenitori Express.js nel servizio app da un registro contenitori privato tramite Visual Studio Code](../tutorial/tutorial-vscode-docker-node/tutorial-vscode-docker-node-01.md?tabs=bash)
* Applicazione macchina virtuale
    * [Creare e distribuire una macchina virtuale Linux con l'app Express.js usando l'interfaccia della riga di comando di Azure e GitHub Actions](../tutorial/nodejs-virtual-machine-vm/create-linux-virtual-machine-azure-cli.md)

Vedere altre informazioni sulle [opzioni di hosting](../how-to/deploy-web-app.md).

## <a name="next-steps"></a>Passaggi successivi

* [Installare Node.js](install-nodejs-develop-azure-sdk-project.md)
* [Vedere gli strumenti consigliati per sviluppatori JavaScript](../node-azure-tools.md)
