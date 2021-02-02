---
title: Eseguire l'autenticazione con i moduli di gestione di Azure per Node.js
description: Eseguire l'autenticazione con un'entità servizio nei moduli di gestione di Azure per Node.js
ms.topic: how-to
ms.date: 01/04/2021
ms.custom: devx-track-js
ms.openlocfilehash: e5774f0453960b41679a01170882fad1d9f50bad
ms.sourcegitcommit: b09d3aa79113af04a245b05cec2f810e43062152
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/02/2021
ms.locfileid: "99476437"
---
# <a name="authenticate-with-the-azure-management-modules-for-javascript"></a>Eseguire l'autenticazione con i moduli di gestione di Azure per JavaScript

Tutte le [librerie client SDK](../azure-sdk-library-package-index.md) richiedono l'autenticazione tramite un oggetto `credentials`. Esistono più modi per autenticare e creare le credenziali necessarie.

L'autenticazione, come tutti i software e i servizi, è stata migliorata nel corso degli anni. È importante stabilire quale libreria di autenticazione sia usata dal servizio o dai servizi. 

Le librerie di autenticazione includono quanto segue:

* @azure/identity: pacchetto di autenticazione più recente
* @azure/ms-rest-nodeauth
* @azure/ms-rest-browserauth

Sono in uso pacchetti di autenticazione precedenti. Se si usano questi pacchetti, è consigliabile abbandonare i metodi di autenticazione meno recenti per un'esperienza più sicura e affidabile. 

## <a name="best-practices-with-azure-sdk-client-library-authentication"></a>Procedure consigliate per l'autenticazione della libreria client di Azure SDK

Ogni pacchetto npm mostrerà l'autenticazione per tale esatta libreria client. Non combinare i pacchetti di autenticazione e il codice a meno che tutti i pacchetti usino lo stesso pacchetto di autenticazione nella pagina del pacchetto npm. 

## <a name="azure-identity-library"></a>Libreria di identità di Azure

La libreria di identità di Azure è il pacchetto di autenticazione più recente per Azure. Controllare il file Leggimi della libreria client utilizzata per verificare se supporta l'utilizzo della nuova libreria.

La libreria [@azure/identity](https://www.npmjs.com/package/@azure/identity) semplifica l'autenticazione rispetto ad Azure Active Directory per le librerie di Azure SDK. Fornisce un set di implementazioni di TokenCredential, che possono essere passate nelle librerie SDK per autenticare le richieste API. Supporta l'autenticazione token usando un'entità servizio o un'identità gestita di Azure Active Directory.

```javascript
const { DefaultAzureCredential } = require("@azure/identity");
const { BlobServiceClient } = require("@azure/storage-blob");
 
// Enter your storage account name
const account = "<account>";
const defaultAzureCredential = new DefaultAzureCredential();
 
const blobServiceClient = new BlobServiceClient(
  `https://${account}.blob.core.windows.net`,
  defaultAzureCredential
);
```

Il codice di esempio JavaScript precedente illustra come usare la libreria di identità di Azure per creare credenziali di Azure predefinite, da usare quindi per accedere a una risorsa di Archiviazione di Azure.

## <a name="azure-ms-rest--libraries"></a>Librerie ms-rest-* di Azure
Con le [librerie client](../azure-sdk-library-package-index.md#modern-javascripttypescript-libraries) moderne con ambito `@azure`, è necessario un token per usare un servizio. Il token si ottiene usando un metodo di autenticazione client di Azure SDK, che restituisce una credenziale. 

```javascript
const msRestNodeAuth = require("@azure/ms-rest-nodeauth");
msRestNodeAuth.interactiveLogin().then((credential) => {
    // service code goes here
}).catch((err) => {
    // error code goes here
    console.error(err);
});
```

Il codice di esempio JavaScript precedente illustra come usare la moderna libreria di autenticazione di Azure con un account di accesso interattivo per ottenere le credenziali.

```javascript
// service code - this is an example only and not best practices for code flow
const { BlobServiceClient } = require('@azure/storage-blob');
const billingManagementClient = new billing.BillingManagementClient(credential, subscriptionId);
billingManagementClient.enrollmentAccounts.list().then((enrollmentList) => {
    console.log("The result is:");
    console.log(result);
})
```

Il codice di esempio JavaScript recedente illustra come passare le credenziali a una libreria client di uno specifico servizio di Azure, ad esempio il servizio di archiviazione usato nell'esempio di codice seguente. La libreria client accetta le credenziali e genera un token per l'utente. Il servizio usa il token per convalidare l'autenticazione a livello di servizio per le richieste. 

La libreria client gestisce il token e sa quando aggiornare il token. Lo sviluppatore con una codebase personalizzata non deve necessariamente gestirlo.

## <a name="older-azure-sdk-client-authentication"></a>Autenticazione client Azure SDK precedente 

I client Azure SDK precedenti verranno infine migrati alla nuova autenticazione moderna usata nell'esempio precedente. Fino a quel momento, le librerie client precedenti useranno client di autenticazione diversi o potranno eseguire l'autenticazione con un meccanismo completamente separato, come le chiavi di risorsa. 

Per ottenere risultati ottimali con le librerie client precedenti: 
* Ogni pacchetto npm mostrerà l'autenticazione per tale esatta libreria client.  
* Se il codice corrente usa le moderne librerie `@azure/ms-*` e le librerie di autenticazione meno recenti nella stessa codebase:
    * Assicurarsi che la libreria con ambito non Azure sia la più recente per il servizio. Questa informazione è indicata nella documentazione del servizio. 
    * Se è necessario continuare a usare una combinazione di librerie di autenticazione moderne e meno recenti, può essere necessario fornire la scadenza e l'aggiornamento delle credenziali per la libreria precedente in base alla logica dell'applicazione nella codebase. 

## <a name="authentication-with-azure-services-while-developing"></a>Autenticazione con i servizi di Azure durante lo sviluppo

Metodi comuni per creare le credenziali necessarie durante lo sviluppo:

| Tipo di autenticazione di Azure|Scopo|
|--|--|
|**Entità servizio**|Questo è il _metodo di autenticazione consigliato_. Informazioni su come [creare un'entità servizio di Azure](node-sdk-azure-authenticate-principal.md). Un'entità servizio consente di avere una connessione ad Azure separata dall'account Azure personale. Può trattarsi di un account temporaneo oppure un account esistente che sostituisce l'account personale.|
| **Interattivo**| Questo è il modo più semplice per eseguire l'autenticazione quando si provano i servizi di Azure. Richiede l'accesso con l'account personale con un browser. |
|**Basic**|Per questa autenticazione è necessario immettere il nome utente e la password personali. Questo è il metodo meno sicuro e non è consigliato.| 

## <a name="authentication-with-azure-services-and-production-code"></a>Autenticazione con i servizi di Azure e il codice di produzione

Metodi comuni per creare le credenziali necessarie nel codice di produzione:

|Tipo di autenticazione di Azure|Scopo|
|--|--|
|**Identità del servizio gestito**|L'[autenticazione MSI](/azure/active-directory/managed-identities-azure-resources/overview) è il metodo migliore per gli scenari di produzione. Non verrà usato nell'ambiente di sviluppo locale. [Servizi](/azure/active-directory/managed-identities-azure-resources/services-support-managed-identities) che supportano MSI.|
|**Certificati**|I [certificati](/azure/cloud-services/cloud-services-certs-create) devono essere caricati in Azure usando il [portale](/azure/cloud-services/cloud-services-configure-ssl-certificate-portal).|

## <a name="javascript-authentication-samples-for-azure"></a>Esempi di autenticazione JavaScript per Azure

|Pacchetto di autenticazione|Script di autenticazione di esempio|
|--|--|
|[@azure/ms-rest-nodeauth](https://www.npmjs.com/package/@azure/ms-rest-nodeauth) <br>(scelta consigliata)|[Entità servizio con certificato](https://github.com/Azure/ms-rest-nodeauth/blob/master/samples/authFileWithSpCert.ts)<br>[Entità servizio dal file](https://github.com/Azure/ms-rest-nodeauth/blob/master/samples/authFileWithSpSecret.ts)<br>[Interattivo](https://github.com/Azure/ms-rest-nodeauth/blob/master/samples/interactivePersonalAccount.ts)<br>[Base](https://github.com/Azure/ms-rest-nodeauth/blob/master/samples/usernamePassword.ts)|
|[@azure/ms-rest-browserauth](https://www.npmjs.com/package/@azure/ms-rest-browserauth)<br>(scelta consigliata)|[Autenticazione con popup (create-react-app)](https://github.com/Azure/ms-rest-browserauth/tree/master/samples/authentication-with-popup)<br>[React senza popup](https://github.com/Azure/ms-rest-browserauth/tree/master/samples/react-app)<br>[HTML con pulsante di accesso](https://github.com/Azure/ms-rest-browserauth/tree/master/samples/vanilla)|
|[ms-rest-azure](https://www.npmjs.com/package/ms-rest-azure)|[Entità servizio](https://github.com/Azure/azure-sdk-for-node/blob/master/Documentation/Authentication.md#service-principal-authentication)<br>[Interattivo](https://github.com/Azure/azure-sdk-for-node/blob/master/Documentation/Authentication.md#interactive-login)<br>[Base](https://github.com/Azure/azure-sdk-for-node/blob/master/Documentation/Authentication.md#basic-authentication)|

## <a name="next-steps"></a>Passaggi successivi   

* [Pacchetti npm di Azure](../azure-sdk-library-package-index.md)
* [Documentazione dei pacchetti npm di Azure](/javascript/api/overview/azure/)
