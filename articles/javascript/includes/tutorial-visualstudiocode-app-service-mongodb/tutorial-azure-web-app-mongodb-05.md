---
title: include file tutorial-azure-web-app-mongodb-05.md
description: include file tutorial-azure-web-app-mongodb-05.md
ms.date: 10/13/2020
ms.topic: include
ms.custom: devx-track-javascript
ms.openlocfilehash: 9b8a718ec921050237f911ee4b5c3a47b6bbb45e
ms.sourcegitcommit: 8a2a7df568c69fff2080ffab248409040efda1ac
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/19/2020
ms.locfileid: "92183865"
---
In questa sezione dell'esercitazione si creerà un database basato sul cloud e si connetterà l'app remota per l'uso del database cloud. 

## <a name="create-a-cosmos-database"></a>Creare un database Cosmos

Creare una risorsa Cosmos per ospitare un database MongoDB basato sul cloud. 

1. In Visual Studio Code selezionare l'icona **Azure** nel menu più a sinistra, quindi selezionare la sezione **Database** . 

    Se la sezione **Database** non è visibile, verificare di aver selezionato la sezione nel menu principale di Azure **...** . 

    :::image type="content" source="../../media/tutorial-end-to-end-app-cosmos/vscode-azure-extension-select-database-section.png" alt-text="Screenshot parziale dell'icona Remote Containers (Contenitori remoti) di Visual Studio Code"::: 

1. Nella sezione **Database** della finestra di esplorazione di Azure selezionare la sottoscrizione facendovi clic sopra con il tasto destro del mouse e quindi scegliere **Crea server** .
1. Nel riquadro comandi **Create new Azure Database Server** (Crea nuovo server di database Azure) selezionare **Azure Cosmos DB per l'API MongoDB** . 
1. Seguire le istruzioni riportate nella tabella seguente per informazioni su come vengono usati i valori. La creazione del database può richiedere fino a 15 minuti.

    |Proprietà|Valore|
    |--|--|
    |Immettere un nome univoco globale per la nuova risorsa in **Nome account** .| Immettere un valore, ad esempio `cosmos-mongodb-YOUR-NAME`, per la risorsa. Sostituire `YOUR-NAME` con il nome o l'ID univoco. Questo nome univoco viene usato anche come parte dell'URL per accedere alla risorsa in un browser.|
    |Selezionare o creare un gruppo di risorse.|Se è necessario creare un gruppo di risorse, usare una convenzione di denominazione che consenta di identificare il proprietario, lo scopo e l'area, ad esempio `westus-cosmostutorial-joesmith`.|
    |Località|Il percorso della risorsa. Per questa esercitazione, selezionare un'area nelle vicinanze.|

    > [!CAUTION]
    > La creazione della risorsa può richiedere fino a 15 minuti.     

1. Visualizzare la nuova risorsa Cosmos creata nella finestra di esplorazione. Non è ancora presente alcun database. 
1. Sempre nella finestra di esplorazione Database di Azure fare clic con il pulsante destro del mouse sul nome della risorsa, quindi scegliere **Copy Connection String** (Copia stringa di connessione) per copiare la stringa di connessione. Sarà necessario usarli più avanti nell'esercitazione.

    :::image type="content" source="../../media/tutorial-end-to-end-app-cosmos/vscode-databases-extenion-copy-connection-string.png" alt-text="Screenshot parziale dell'icona Remote Containers (Contenitori remoti) di Visual Studio Code":::

## <a name="optional-use-new-cloud-database-in-local-environment"></a>Facoltativo: usare il nuovo database cloud nell'ambiente locale

Per usare il nuovo database cloud, l'applicazione locale deve essere modificata per poter usare la nuova stringa di connessione. 

1. In Visual Studio Code aprire il file `.env` e aggiungere il valore di **DATABASE_URL** alla nuova stringa di connessione. 
1. Aggiungere `&retrywrites=false` alla fine della stringa di connessione in modo che il database possa essere creato la prima volta che viene eseguita l'app Web. 
1. Eseguire l'app Web in locale, senza usare il contenitore di sviluppo, per assicurarsi che l'app si connetta al database cloud. 

## <a name="use-new-cloud-database-in-remote-web-app"></a>Usare il nuovo database cloud nell'app Web remota

La connessione al database viene impostata con una variabile di ambiente denominata `DATABASE_URL`. Per configurare l'app Web remota in modo che usi questa variabile di ambiente, è necessario creare la variabile nell'app Web remota. 

1. Nella finestra di esplorazione del servizio app di Azure in Visual Studio Code selezionare ed espandere il nodo del servizio app Web.
1.  Fare clic con il pulsante destro del mouse sul nodo **Impostazioni applicazione** per aggiungere la proprietà `DATABASE_URL` con la stringa di connessione per Azure Cosmos DB per MongoDB. Aggiungere `&retrywrites=false` alla fine della stringa di connessione in modo che il database possa essere creato la prima volta che viene eseguita l'app Web. 

    :::image type="content" source="../../media/tutorial-end-to-end-app-cosmos/vscode-remote-web-app-application-settings.png" alt-text="Screenshot parziale dell'icona Remote Containers (Contenitori remoti) di Visual Studio Code"::: 

1. Aprire il sito Web remoto in un browser e usare il modulo per aggiungere ed eliminare i dati. 

## <a name="want-to-know-more"></a>Per saperne di più 

### <a name="mongodb-connection-strings"></a>Stringhe di connessione MongoDB
Poiché la creazione del database alla prima esecuzione del codice può richiedere diversi tentativi, la stringa di connessione deve includere l'impostazione `&retrywrites=false`. Per approfondire questo aspetto, iniziare da questo [problema pubblico n. 1296](https://github.com/microsoft/vscode-cosmosdb/issues/1296) su GitHub. 