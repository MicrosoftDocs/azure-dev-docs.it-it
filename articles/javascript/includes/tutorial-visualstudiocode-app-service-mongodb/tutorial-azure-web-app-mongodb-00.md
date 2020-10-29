---
title: file di inclusione tutorial-azure-web-app-mongodb-00.md
description: file di inclusione tutorial-azure-web-app-mongodb-00.md
ms.date: 10/13/2020
ms.topic: include
ms.custom: devx-track-javascript
ms.openlocfilehash: 9903ca1ae761cb2e16a7f72fcc4c0942f7fa5991
ms.sourcegitcommit: c3a1c9051b89870f6bfdb3176463564963b97ba4
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/22/2020
ms.locfileid: "92437319"
---
In questa esercitazione si usa un'app Node.js con un database MongoDB tramite l'API nativa MongoDB. Distribuire l'applicazione Node.js nel servizio app di Azure (in Linux), quindi verificare che l'app basata sul cloud funzioni. 

L'attività di programmazione viene eseguita automaticamente. Questa esercitazione è incentrata sull'uso corretto degli ambienti di Azure locali e remoti all'interno Visual Studio Code con le estensioni di Azure.

## <a name="top-tasks"></a>Attività principali

Questa esercitazione include diverse **attività principali di Azure** per sviluppatori JavaScript:

* Usare un database MongoDB locale
* Usare l'app con un contenitore
* Distribuire l'app nel cloud
* Configurare le impostazioni dell'app ospitata nel cloud 
* Connettere l'app locale a un database remoto

## <a name="sample-application"></a>Applicazione di esempio

La [semplice app Node.js](https://github.com/Azure-Samples/js-e2e-express-mongo), disponibile in GitHub, è costituita dagli elementi seguenti:

* **Server Express.js** ospitato sulla porta 8080
* Semplice motore di **visualizzazione lato server React.js**
* Funzioni delle **API native di MongoDB** per l'inserimento, l'eliminazione e la ricerca di dati

:::image type="content" source="../../media/tutorial-end-to-end-app-cosmos/nodejs-app-connected-mongodb-form.png" alt-text="Semplice app Node.js connessa al database MongoDB.":::

## <a name="the-mongodb-connection"></a>Connessione a MongoDB

Se non è possibile effettuare la connessione al database, l'app visualizza il messaggio `No database found.`. Questo sarà lo stato iniziale dell'app.

Quando viene effettuata la connessione al database, l'app è costituita da due campi di testo in un modulo con un pulsante di invio, sotto al quale è visualizzato il contenuto della raccolta Mongo.

## <a name="limited-time-to-work-on-the-tutorial"></a>Tempo limitato per lavorare all'esercitazione?

Nell'esercitazione si crea una risorsa di Azure per Cosmos DB, con cui Azure ospita i database MongoDB. Il processo di creazione della risorsa può richiedere fino a 20 minuti. È possibile [avviare il processo ora](../../tutorial/web-app-mongodb.yml?tutorial-step=5) se il tempo è limitato, quindi la risorsa sarà disponibile quando serve. 

## <a name="want-to-know-more"></a>Per saperne di più 

Ogni passaggio dell'esercitazione include una sezione **Per saperne di più** . Si tratta di _informazioni facoltative_ per consentire di approfondire l'argomento. È possibile leggerle durante l'esercitazione oppure tornare in un secondo momento. 