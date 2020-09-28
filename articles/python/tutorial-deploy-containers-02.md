---
title: "Passaggio 2: Distribuire un'immagine del contenitore nel servizio app di Azure con Visual Studio Code"
description: Passaggio 2 dell'esercitazione, distribuzione dell'immagine Docker effettiva al servizio app Azure da un registro contenitori.
ms.topic: conceptual
ms.date: 09/17/2020
ms.custom: devx-track-python, seo-python-october2019
ms.openlocfilehash: 5255e0d65fda839fbbe86c1743d424ab5801774f
ms.sourcegitcommit: 39f3f69e3be39e30df28421a30747f6711c37a7b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/21/2020
ms.locfileid: "90830897"
---
# <a name="2-deploy-a-container-image-to-azure-app-service"></a>2: Distribuire un'immagine del contenitore nel Servizio app di Azure

[Passaggio precedente: Configurare l'ambiente](tutorial-deploy-containers-01.md)

Con un'immagine del contenitore in un registro, è possibile usare l'estensione Docker in VS Code per configurare facilmente un servizio app di Azure che esegue il contenitore.

1. Nell'area **Docker** espandere **Registries** (Registri), espandere il nodo del registro, ad esempio **Azure**, quindi espandere il nodo relativo al nome dell'immagine fino a visualizzare l'immagine con il tag `:latest`.

    ![Individuare un'immagine nell'area Docker](media/deploy-containers/find-image-to-deploy-in-docker-explorer.png)

1. Fare clic con il pulsante destro del mouse sull'immagine e scegliere **Deploy Image to Azure App Service** (Distribuisci immagine nel servizio app di Azure).

    ![Selezionare la voce di menu Deploy Image to Azure App Service](media/deploy-containers/deploy-image-to-azure-app-service-with-docker-explorer.png)

1. Seguire le istruzioni per selezionare una sottoscrizione di Azure, selezionare o specificare un gruppo di risorse, specificare un'area, configurare un piano del servizio app (B1 è il meno costoso) e specificare un nome per il sito. L'animazione seguente illustra il processo.

    ![Creare e distribuire l'immagine nel Servizio app di Azure](media/deploy-containers/deploy-image-to-azure-app-service.gif)

    Un **gruppo di risorse** è una raccolta denominata delle varie risorse che costituiscono un'app. Assegnando tutte le risorse dell'app a un singolo gruppo, è possibile gestirle facilmente come singola unità. Per altre informazioni, vedere [Panoramica di Azure Resource Manager](/azure/azure-resource-manager/resource-group-overview) nella documentazione di Azure.

    Un **piano del servizio app** definisce le risorse fisiche (una macchina virtuale sottostante) che ospitano il contenitore in esecuzione. Per questa esercitazione, B1 è il piano meno costoso che supporta i contenitori Docker. Per altre informazioni, vedere [Panoramica del piano del servizio app](/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview) nella documentazione di Azure.

    Il nome del servizio app deve essere univoco in Azure, quindi in genere si usa il nome personale o della società. Per i siti di produzione, il servizio app viene in genere configurato con un nome di dominio registrato separatamente.

1. La creazione del servizio app richiede alcuni minuti e lo stato di avanzamento viene visualizzato nel pannello Output di VS Code.

1. Al termine, è **necessario** aggiungere anche un'impostazione denominata `WEBSITES_PORT` al servizio app per specificare la porta su cui il contenitore è in ascolto. Se si usa un'immagine dell'esercitazione [Creare un contenitore Python in VS Code](https://code.visualstudio.com/docs/python/tutorial-create-containers), ad esempio, la porta è 5000 per Flask e 8000 per Django. Per impostare `WEBSITES_PORT`, passare all'area **Azure: Servizio app**, espandere il nodo del nuovo servizio app (aggiornare se necessario), quindi fare clic con il pulsante destro del mouse su **Applications Settings** (Impostazioni applicazioni) e scegliere **Add New Setting** (Aggiungi nuova impostazione). Quando richiesto, immettere `WEBSITES_PORT` come chiave e il numero di porta per il valore.

    ![Aggiunta di una nuova impostazione a un servizio app che specifica una porta](media/deploy-containers/add-new-setting-in-app-service-settings-explorer.png)

1. Il servizio app viene riavviato automaticamente quando si cambiano le impostazioni. È anche possibile fare clic con il pulsante destro del mouse sul servizio app e scegliere **Restart** (Riavvia) in qualsiasi momento.

1. Dopo il riavvio del servizio, esplorare il sito all'indirizzo `http://<name>.azurewebsites.net`. È possibile usare **CTRL+clic** (oppure **CMD+clic** in MacOS) sull'URL nel pannello Output oppure fare clic con il pulsante destro del mouse sul servizio app nell'area **Azure: Servizio app** e scegliere **Esplora sito Web**.

> [!div class="nextstepaction"]
> [L'immagine è stata distribuita: procedere con il passaggio 3 >>>](tutorial-deploy-containers-03.md)