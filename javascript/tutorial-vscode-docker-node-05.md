---
title: Distribuire un'immagine del contenitore per un'app Node.js da Visual Studio Code
description: Parte 5 dell'esercitazione, distribuire l'immagine nel Servizio app di Azure
ms.topic: conceptual
ms.date: 09/20/2019
ms.openlocfilehash: 487110258ed3302e781cfa24a5ae9f518ebb3bda
ms.sourcegitcommit: 756e4873f904db954a56c20ebb2f1f5116ee4596
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/05/2020
ms.locfileid: "80740682"
---
# <a name="deploy-the-image-to-azure-app-service"></a>Distribuire l'immagine nel Servizio app di Azure

[Passaggio precedente: Creare l'immagine dell'app](tutorial-vscode-docker-node-04.md)

In questo passaggio viene distribuita l'immagine di cui è stato eseguito il push in un registro del [Servizio app di Azure](https://azure.microsoft.com/services/app-service/) direttamente da Visual Studio Code.

## <a name="enable-admin-access-on-the-registry"></a>Abilitare l'accesso con privilegi di amministratore per il registro

Per distribuire l'immagine in un'applicazione Web, è necessario abilitare l'accesso con privilegi di amministratore per il registro nel portale di Azure.

1. Nell'area **Docker** fare clic con il pulsante destro del mouse sul nome del registro e scegliere "Apri nel portale". 

    ![Comando Apri nel portale in VS Code](media/deploy-containers/open-in-portal.png)

    Il registro verrà aperto nel portale di Azure.

1. Fare clic su "Chiavi di accesso" sulla barra laterale, quindi impostare l'opzione "Utente amministratore" su "Abilitato".  
    
    ![Abilitare l'impostazione dell'utente amministratore nel portale di Azure](media/deploy-containers/access-keys.png)

## <a name="deploy-image"></a>Distribuire l'immagine

1. Nell'area **DOCKER** espandere i nodi per l'immagine in **Registries** (Registri), fare clic con il pulsante destro del mouse su `:latest` e quindi scegliere **Deploy Image to Azure App Service** (Distribuisci l'immagine nel Servizio app di Azure).

    ![Eseguire la distribuzione dallo strumento di esplorazione](media/deploy-containers/deploy-image-command.png)

1. Quando richiesto, specificare i valori per il Servizio app:

    - Il nome deve essere univoco in Azure.
    - Selezionare un gruppo di risorse esistente oppure crearne uno nuovo. Un **gruppo di risorse** è essenzialmente una raccolta denominata delle risorse di un'applicazione in Azure.
    - Selezionare un piano di Servizio app esistente oppure crearne uno nuovo. Un **piano di Servizio app** definisce le risorse fisiche che ospitano il sito Web. Per questa esercitazione, è possibile usare il piano di livello gratuito o Basic.

1. Al termine della distribuzione, Visual Studio Code visualizza una notifica con l'URL del sito Web:

    ![Messaggio di distribuzione completata](media/deploy-containers/deploy-successful.png)

1. È anche possibile visualizzare i risultati nel pannello **Output** di Visual Studio Code, nella sezione **Docker**:

    ![Output di distribuzione completata](media/deploy-containers/deploy-output.png)

1. Per esplorare il sito Web distribuito, è possibile premere **CTRL**+**clic** sull'URL nel pannello **Output**. Il nuovo Servizio app viene visualizzato anche nell'area **AZURE** di Visual Studio Code, nella sezione **SERVIZIO APP**, dove è possibile fare clic con il pulsante destro del mouse sul sito Web e scegliere **Esplora sito Web**.

> [!div class="nextstepaction"]
> [Il sito si trova in Azure](tutorial-vscode-docker-node-06.md) [Si è verificato un problema](https://www.research.net/r/PWZWZ52?tutorial=docker-extension&step=deploy-app)
