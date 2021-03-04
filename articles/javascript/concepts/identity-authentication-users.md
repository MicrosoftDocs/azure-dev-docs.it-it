---
title: Autenticazione e autorizzazione - JavaScript - Azure
description: Informazioni su come sviluppare app JavaScript con identità, autenticazione e utenti con Azure.
ms.topic: conceptual
ms.date: 12/09/2020
ms.custom: devx-track-js
ms.openlocfilehash: 68f53743d001b44aa1495ece36d0c2764ec749d4
ms.sourcegitcommit: 576c878c338d286060010646b96f3ad0fdbcb814
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/04/2021
ms.locfileid: "102118463"
---
# <a name="identity-authentication-and-users"></a>Identità, autenticazione e utenti

L'autenticazione e l'autorizzazione sono argomenti generali per un'applicazione Web che possono essere ridotti a specifiche attività a livello di codice e interazioni con l'utente. Questo articolo è incentrato sui concetti principali che uno sviluppatore JavaScript deve comprendere. 

## <a name="authentication-with-azure"></a>Autenticazione con Azure

L'autenticazione è la possibilità per un'identità di acquisire l'accesso a un oggetto. 

In genere, per uno sviluppatore JavaScript in Azure, questo significa la possibilità di:

* Consentire al programma di accedere alle risorse di Azure
* Consentire a un utente di accedere all'app, in genere dopo l'accesso

|Obbligatorio|Prospettiva|Descrizione|
|--|--|--|
|Sì|Sviluppatore|Il codice dell'applicazione deve passare le credenziali necessarie ad Azure per accedere alle risorse di Azure.|
|No|Utente|Per un utente di un'applicazione, l'autenticazione può essere anonima o richiedere un account utente. Per questo accesso con restrizioni è possibile usare qualsiasi provider di autenticazione comune, tra cui Microsoft, oppure creare un livello di autenticazione personalizzato per gli utenti.|

## <a name="authentication-for-developers-to-azure-services"></a>Autenticazione per sviluppatori nei servizi di Azure

L'autenticazione a livello di codice in Azure richiede credenziali valide per il servizio esatto usato dal codice. È necessario leggere la documentazione di avvio rapido per il servizio e identificare il tipo di credenziali previsto. 

### <a name="local-developer-environment-for-authenticating-to-azure"></a>Ambiente di sviluppo locale per l'autenticazione in Azure

Dopo aver capito come connettersi a un servizio, è necessario creare un'entità servizio e impostarla su una variabile di ambiente nel computer di sviluppo. Questo passaggio rimuove l'account personale dall'interazione diretta con Azure e il rischio che venga compromesso archiviando le credenziali con il codice sorgente. 

### <a name="cloud-apps-authenticating-to-azure"></a>Autenticazione delle app cloud in Azure

Per le app basate sul cloud, i servizi di hosting di Azure forniscono l'accesso alle [impostazioni dell'applicazione](../how-to/configure-web-app-settings.md), tra cui variabili di ambiente e segreti. Per aggiungere un altro livello di sicurezza all'app Web, archiviare i segreti in Azure [Key Vault](/azure/key-vault) e accedervi a livello di codice dall'app ospitata. 

## <a name="modern-programmatic-authentication-with-azureidentity"></a>Autenticazione moderna a livello di codice con @azure/identity

La libreria Azure SDK corrente usa un'entità servizio per l'autenticazione a livello di codice nei servizi di Azure con il pacchetto npm [@azure/identity](https://www.npmjs.com/package/@azure/identity). Questa autenticazione semplifica il processo ed è disponibile nei [pacchetti Azure SDK moderni](https://www.npmjs.com/package/@azure/identity#client-libraries-supporting-authentication-with-azure-identity). 

```javascript
// The default credential first checks environment variables for configuration.
// If environment configuration is incomplete, it will try managed identity.

// Azure service to use
const { KeyClient } = require("@azure/keyvault-keys");

// Azure authentication library to access Azure service
const { DefaultAzureCredential } = require("@azure/identity");

// Azure SDK clients accept the credential as a parameter
const credential = new DefaultAzureCredential();

// Create authenticated client
const client = new KeyClient(vaultUrl, credential);

// Use service from authenticated client
const getResult = await client.getKey("MyKeyName");
```

## <a name="classic-programmatic-authentication"></a>Autenticazione classica a livello di codice

Per la maggior parte delle altre librerie di Azure SDK gestite, usare uno dei pacchetti seguenti: 

* [@azure/ms-rest-js](https://www.npmjs.com/package/@azure/ms-rest-js): funziona nel browser e nell'ambiente Node.js
* [@azure/ms-rest-nodeauth](https://www.npmjs.com/package/@azure/ms-rest-nodeauth): fornisce diversi meccanismi di autenticazione, tra cui interattivo, entità servizio e utente/password
* [@azure/ms-rest-browserauth](https://www.npmjs.com/package/@azure/ms-rest-browserauth): richiede l'app Azure AD

L'esempio seguente illustra come eseguire l'autenticazione con una chiave e un endpoint forniti da un servizio.

```javascript
// Azure service to use
const { QnAMakerRuntimeClient } = require("@azure/cognitiveservices-qnamaker-runtime");

// Azure authentication library to access Azure service
const { CognitiveServicesCredentials } = require("@azure/ms-rest-azure-js");  
 
// QnA Maker runtime credentials
const QNAMAKER_KEY = process.env["QNAMAKER_KEY"];
const QNAMAKER_ENDPOINT = process.env["QNAMAKER_ENDPOINT"];
const KNOWLEDGEBASE_ID = process.env["QNAMAKER_KNOWLEDGE_BASE_ID"];

const cognitiveServicesCredentials = new CognitiveServicesCredentials(QNAMAKER_KEY);
const client = new QnAMakerRuntimeClient(cognitiveServicesCredentials, QNAMAKER_ENDPOINT);
const customHeaders = { Authorization: `EndpointKey ${QNAMAKER_KEY}` };

// A question you'd like to get a response for, from the knowledge base. For example
const question = "How are you?";

// Maximum number of answer to retreive
const top = 1;

// Find only answers that contain these metadata
const strictFilters = [{ name: "editorial", value: "chitchat" }];

client.runtime.generateAnswer( 
        KNOWLEDGEBASE_ID,
        { question, top, strictFilters },
        { customHeaders }
).then(result =>{
    console.log(JSON.stringify(result));

    // Sample Result
    // {
    //   answers: [
    //     {
    //       questions: [
    //         "How are you?",
    //         "How is your tuesday?"
    //       ],
    //       answer:
    //         ""I'm doing great, thanks for asking!",
    //       score: 100,
    //       id: 90,
    //       source:
    //         "qna_chitchat_Friendly.tsv",
    //       metadata: [{ name: "editorial", value: "chitchat" }],
    //       context: { isContextOnly: false, prompts: [] }
    //     }
    //   ],
    //   debugInfo: null,
    //   activeLearningEnabled: false
    // }

});

```

## <a name="user-authentication-with-an-app-registration"></a>Autenticazione utente con una registrazione app

MSAL (Microsoft Authentication Library) è la libreria consigliata per lo sviluppo Web a l'autenticazione utente. La libreria è disponibile in [diversi linguaggi e framework](/azure/active-directory/develop/msal-overview#languages-and-frameworks).

Per usare MSAL, per l'app Web è necessaria una [registrazione app](/azure/active-directory/develop/quickstart-register-app) con Microsoft. La registrazione app include informazioni di autenticazione comuni, come autorizzazioni per ambito utente, e l'URL di reindirizzamento. 

Per altre informazioni, vedere il progetto di esempio in questa [guida di avvio rapido su MSAL](/azure/active-directory/develop/quickstart-v2-javascript).

Un utente concede l'autorizzazione all'app al momento dell'accesso. Questa autorizzazione viene archiviata con l'utente, che la può gestire e revocare:

* Gestione delle autorizzazioni per app consumer: [https://account.live.com/consent/manage](https://account.live.com/consent/manage)
* Gestione delle autorizzazioni per app Active Directory: [https://myapplications.microsoft.com/](https://myapplications.microsoft.com/)

## <a name="next-steps"></a>Passaggi successivi

* [Configurare il servizio app di Azure](../how-to/configure-web-app-settings.md)