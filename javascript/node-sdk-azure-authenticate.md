---
title: Eseguire l'autenticazione con i moduli di gestione di Azure per Node.js
description: Eseguire l'autenticazione con un'entità servizio nei moduli di gestione di Azure per Node.js
author: kraigb
manager: barbkess
ms.author: kraigb
ms.date: 06/17/2017
ms.topic: article
ms.prod: azure
ms.devlang: nodejs
ms.openlocfilehash: 87a30973c8a295540924e41aee9c8e0af455b41f
ms.sourcegitcommit: 380300c283f3df8a87c7c02635eae3596732fb72
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/06/2019
ms.locfileid: "73661233"
---
# <a name="authenticate-with-the-azure-modules-for-nodejs"></a>Eseguire l'autenticazione con i moduli di Azure per Node.js 

Tutte le API di un servizio richiedono l'autenticazione tramite un oggetto `credentials` quando ne viene creata un'istanza. Esistono tre modi per autenticare e creare le credenziali necessarie tramite Azure SDK per Node.js: 

- Autenticazione di base
- Accesso interattivo
- Autenticazione di un'entità servizio

## <a name="basic-authentication"></a>Autenticazione di base

Per eseguire l'autenticazione a livello di codice usando le credenziali dell'account Azure, usare la funzione `loginWithUsernamePassword`. Il frammento di codice JavaScript seguente illustra come usare l'autenticazione di base usando le credenziali archiviate come variabili di ambiente. 

```javascript
const Azure = require('azure');
const MsRest = require('ms-rest-azure');

MsRest.loginWithUsernamePassword(process.env.AZURE_USER, 
                                 process.env.AZURE_PASS, 
                                 (err, credentials) => {
  if (err) throw err;

  let storageClient = Azure.createARMStorageManagementClient(credentials, 
                                                             '<azure-subscription-id>');

  // ..use the client instance to manage service resources.
});
```

## <a name="interactive-login"></a>Accesso interattivo

L'accesso interattivo fornisce un collegamento e un codice che consente all'utente di eseguire l'autenticazione da un browser. Usare questo metodo quando più account sono usati dallo stesso script o quando è preferibile l'intervento dell'utente.

```javascript
const Azure = require('azure');
const MsRest = require('ms-rest-azure');

MsRest.interactiveLogin((err, credentials) => {
  if (err) throw err;

  let storageClient = Azure.createARMStorageManagementClient(credentials, '<azure-subscription-id>');

  // ..use the client instance to manage service resources.
});
```

## <a name="service-principal-authentication"></a>Autenticazione di un'entità servizio

L'[accesso interattivo](#interactive-login) è il modo più semplice per eseguire l'autenticazione. Quando tuttavia si usa Node.js SDK, è consigliabile usare l'autenticazione basata su entità servizio invece di specificare le credenziali dell'account. L'argomento [Create an Azure service principal with Node.js](./node-sdk-azure-authenticate-principal.md) (Creare un'entità servizio di Azure con Node.js) illustra diverse tecniche per creare (e usare) un'entità servizio. 