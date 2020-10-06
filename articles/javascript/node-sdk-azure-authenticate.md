---
title: Eseguire l'autenticazione con i moduli di gestione di Azure per Node.js
description: Eseguire l'autenticazione con un'entità servizio nei moduli di gestione di Azure per Node.js
ms.topic: how-to
ms.date: 06/17/2017
ms.custom: devx-track-js
ms.openlocfilehash: 150b00c4dbb21d0514d1d7c7d34813272bbf06e1
ms.sourcegitcommit: 717e32b68fc5f4c986f16b2790f4211967c0524b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/30/2020
ms.locfileid: "91586119"
---
# <a name="authenticate-with-the-azure-management-modules-for-javascript"></a>Eseguire l'autenticazione con i moduli di gestione di Azure per JavaScript

Sono disponibili due set di pacchetti di gestione per i servizi di Azure che semplificano la gestione delle risorse.
- Azure SDK per Node.js
- Azure SDK per JavaScript

Azure SDK per Node.js è il set di pacchetti di gestione meno recente per i servizi di Azure 
- utilizzabili solo in Node.js e non nei browser
- sono scritti in JavaScript con file di dichiarazione di tipo scritti manualmente
- non in fase di sviluppo attivo e deprecati a favore dei pacchetti di Azure SDK per JavaScript
- hanno nomi di pacchetto che iniziano con `azure-arm-`
- richiedono il pacchetto [ms-rest-azure](https://www.npmjs.com/package/ms-rest-azure) per creare credenziali che possono quindi essere passate alle classi client nei pacchetti per eseguire l'autenticazione usando Azure Active Directory.
- si trovano nel repository https://github.com/Azure/azure-sdk-for-node

Azure SDK per JavaScript è il set di pacchetti di gestione più recente per i servizi di Azure
- utilizzabili sia in Node.js che nei browser
- scritto in TypeScript, può essere usato nei progetti sia JavaScript che TypeScript
- in fase di sviluppo attivo e ricevono gli aggiornamenti non appena i servizi di Azure aggiornano le API di gestione delle risorse
- hanno nomi di pacchetto che iniziano con `@azure/arm-`
- richiedono il pacchetto [@azure/ms-rest-nodeauth](https://www.npmjs.com/package/@azure/ms-rest-nodeauth) per creare credenziali che possono quindi essere passate alle classi client nei pacchetti per eseguire l'autenticazione usando Azure Active Directory. Se l'applicazione viene eseguita nel browser, usare invece [@azure/ms-rest-browserauth](https://www.npmjs.com/package/@azure/ms-rest-browserauth).
- si trovano nel repository https://github.com/Azure/azure-sdk-for-js

I due set di pacchetti possono essere facilmente distinti esaminando i nomi dei pacchetti.

Tutte le API di un servizio richiedono l'autenticazione tramite un oggetto `credentials` quando ne viene creata un'istanza. Esistono diversi modi per autenticare e creare le credenziali necessarie per i pacchetti sia in Azure SDK per Node.js che in Azure SDK per JavaScript.

Alcuni dei metodi comuni sono:

- Autenticazione di base che usa nome utente e password
- Accesso interattivo, che è il modo più semplice per eseguire l'autenticazione, ma richiede l'accesso con un account utente.
- Autenticazione tramite entità servizio. L'argomento [Creare un'entità servizio di Azure con Node.js](./node-sdk-azure-authenticate-principal.md) illustra diverse tecniche per creare un'entità servizio. 

Il file Leggimi di ognuno dei pacchetti qui sotto illustra in dettaglio i diversi modi per ottenere un oggetto credenziale.
- [@azure/ms-rest-nodeauth](https://www.npmjs.com/package/@azure/ms-rest-nodeauth) quando si usa un pacchetto di gestione in Azure SDK per JavaScript in Node.js
- [@azure/ms-rest-browserauth](https://www.npmjs.com/package/@azure/ms-rest-browserauth) quando si usa un pacchetto di gestione in Azure SDK per JavaScript in un browser
- [ms-rest-azure](https://www.npmjs.com/package/ms-rest-azure) quando si usa un pacchetto di gestione in Azure SDK meno recente per Node.js

[!INCLUDE [chrome-note](includes/chrome-note.md)]

## <a name="next-steps"></a>Passaggi successivi   

* [Distribuire un sito Web statico in Azure da Visual Studio Code](tutorial-vscode-static-website-node-01.md)