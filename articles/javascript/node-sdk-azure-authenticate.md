---
title: Eseguire l'autenticazione con i moduli di gestione di Azure per Node.js
description: Eseguire l'autenticazione con un'entità servizio nei moduli di gestione di Azure per Node.js
ms.topic: how-to
ms.date: 09/29/2020
ms.custom: devx-track-js
ms.openlocfilehash: 2d7b4047226b28ab71597e523243adf7fc0d4c0d
ms.sourcegitcommit: 0b1c751c5a4a837977fec1c777bca5ad15cf2fc7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/01/2020
ms.locfileid: "91621667"
---
# <a name="authenticate-with-the-azure-management-modules-for-javascript"></a>Eseguire l'autenticazione con i moduli di gestione di Azure per JavaScript

Tutte le [librerie del client SDK](azure-sdk-library-package-index.md) richiedono l'autenticazione tramite un oggetto `credentials` durante la creazione di un'istanza. Esistono più modi per autenticare e creare le credenziali necessarie.

I metodi comuni per creare le credenziali necessarie sono i seguenti:

- L'autenticazione basata su **entità servizio** è il _metodo consigliato_. Informazioni su come [creare un'entità servizio di Azure](node-sdk-azure-authenticate-principal.md). 
- L'**accesso interattivo** è il modo più semplice per eseguire l'autenticazione, ma richiede l'accesso con un account utente e un browser.
- Autenticazione **di base** con il nome utente e la password. Questo è il metodo meno sicuro. 

## <a name="samples"></a>Esempi

|Pacchetto di autenticazione|Script di autenticazione di esempio|
|--|--|
|[@azure/ms-rest-nodeauth](https://www.npmjs.com/package/@azure/ms-rest-nodeauth) <br>(scelta consigliata)|[Entità servizio con certificato](https://github.com/Azure/ms-rest-nodeauth/blob/master/samples/authFileWithSpCert.ts)<br>[Entità servizio dal file](https://github.com/Azure/ms-rest-nodeauth/blob/master/samples/authFileWithSpSecret.ts)<br>[Interattivo](https://github.com/Azure/ms-rest-nodeauth/blob/master/samples/interactivePersonalAccount.ts)<br>[Base](https://github.com/Azure/ms-rest-nodeauth/blob/master/samples/usernamePassword.ts)|
|[@azure/ms-rest-browserauth](https://www.npmjs.com/package/@azure/ms-rest-browserauth)<br>(scelta consigliata)|[Autenticazione con popup (create-react-app)](https://github.com/Azure/ms-rest-browserauth/tree/master/samples/authentication-with-popup)<br>[React senza popup](https://github.com/Azure/ms-rest-browserauth/tree/master/samples/react-app)<br>[HTML con pulsante di accesso](https://github.com/Azure/ms-rest-browserauth/tree/master/samples/vanilla)|
|[ms-rest-azure](https://www.npmjs.com/package/ms-rest-azure)|[Entità servizio](https://github.com/Azure/azure-sdk-for-node/blob/master/Documentation/Authentication.md#service-principal-authentication)<br>[Interattivo](https://github.com/Azure/azure-sdk-for-node/blob/master/Documentation/Authentication.md#interactive-login)<br>[Base](https://github.com/Azure/azure-sdk-for-node/blob/master/Documentation/Authentication.md#basic-authentication)|

[!INCLUDE [chrome-note](includes/chrome-note.md)]

## <a name="next-steps"></a>Passaggi successivi   

* [Distribuire un sito Web statico in Azure da Visual Studio Code](tutorial-vscode-static-website-node-01.md)