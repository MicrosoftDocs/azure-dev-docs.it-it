---
title: include file tutorial-azure-web-app-mongodb-03.md
description: include file tutorial-azure-web-app-mongodb-03.md
ms.date: 10/13/2020
ms.topic: include
ms.custom: devx-track-javascript
ms.openlocfilehash: adea87271b1332f77ab254530410787d1a9baa3c
ms.sourcegitcommit: 8a2a7df568c69fff2080ffab248409040efda1ac
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/19/2020
ms.locfileid: "92183864"
---
In questa sezione dell'esercitazione viene distribuita l'applicazione di esempio in Azure. È quindi possibile visualizzare l'app in esecuzione in remoto nel browser. 

## <a name="sign-in-to-azure"></a>Accedere ad Azure

[!INCLUDE [azure-sign-in](../azure-sign-in.md)]

## <a name="create-web-app-resource"></a>Creare la risorsa app Web

Usare l'estensione per Visual Studio Code per creare una risorsa del servizio app e distribuire l'app Web nella risorsa.

1. Passare alla finestra di esplorazione di Azure. Fare clic con il pulsante destro del mouse sulla sottoscrizione e quindi scegliere `Create new web app...`.

    :::image type="content" source="../../media/tutorial-end-to-end-app-cosmos/create-web-app-with-extension.png" alt-text="Screenshot parziale di Visual Studio Code con l'estensione del servizio app di Azure per creare un'app Web.":::

1. Seguire le istruzioni riportate nella tabella seguente per informazioni su come vengono usati i valori.

    |Proprietà|Valore|
    |--|--|
    |Immettere un nome univoco globale per la nuova app Web.| Immettere un valore, ad esempio `web-app-with-mongodb-YOUR-NAME`, per la risorsa del servizio app. Sostituire `<YOUR-NAME>` con il nome o l'ID univoco. Questo nome univoco viene usato anche come parte dell'URL per accedere alla risorsa in un browser.|
    |Selezionare un runtime per l'app Linux.|Selezionare `Node 12 LTS`.|

1. Al termine del processo di creazione dell'app, viene visualizzato un messaggio di stato nell'angolo in basso a destra di Visual Studio Code con la possibilità di scegliere tra `Deploy` e `View output`. Selezionare `Deploy`.

    :::image type="content" source="../../media/tutorial-end-to-end-app-cosmos/vscode-app-extension-create-web-app-deploy-web-app.png" alt-text="Screenshot parziale di Visual Studio Code con l'estensione del servizio app di Azure per creare un'app Web.":::

    Se il messaggio di stato non è più visibile, è possibile avviare la distribuzione selezionando la finestra di esplorazione di Azure, facendo clic con il pulsante destro del mouse sul nome della risorsa e quindi scegliendo **Distribuisci nell'app Web...** .

1. Durante il processo di distribuzione una notifica consente di selezionare un'opzione per visualizzare la **finestra di output** .  Questa consente di visualizzare lo stato in sequenza della distribuzione. 

1. Al termine della distribuzione, viene visualizzata una notifica. Selezionare **Log in streaming** per visualizzare i log in sequenza. 

    :::image type="content" source="../../media/tutorial-end-to-end-app-cosmos/vscode-app-service-deployed.png" alt-text="Screenshot parziale di Visual Studio Code con l'estensione del servizio app di Azure per creare un'app Web.":::

    :::image type="content" source="../../media/tutorial-end-to-end-app-cosmos/vscode-app-service-stream-logs.png" alt-text="Screenshot parziale di Visual Studio Code con l'estensione del servizio app di Azure per creare un'app Web.":::    

1. Aprire il sito Web in un browser, sostituire il testo `YOUR-RESOURCE_NAME` con il nome della risorsa: `https://YOUR-RESOURCE_NAME.azurewebsites.net`.
    
    Il sito Web può ora essere eseguito in locale e in remoto, ma non si connette ancora al database. 

## <a name="want-to-know-more"></a>Per saperne di più

Il servizio Web iniziale è configurato per l'esecuzione sulla porta 8080 ed è disponibile pubblicamente. Questi tipi di impostazioni del sito Web sono configurabili.
* [Impostazioni app](/azure/app-service/configure-common)
* [autenticazione](/azure/app-service/configure-authentication-provider-microsoft)
* [Limitare l'accesso in base alla rete](/azure/azure/app-service/app-service-ip-restrictions)

Quando si usa questa estensione del servizio app per distribuire il sito Web nel cloud di Azure, può essere necessario saper [configurare la distribuzione](https://github.com/microsoft/vscode-azureappservice/wiki/Configuring-Zip-Deployment#additional-zip-deploy-configuration-settings)