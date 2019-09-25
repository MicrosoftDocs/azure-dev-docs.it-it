---
title: Distribuire un'immagine del contenitore nel servizio app di Azure con Visual Studio Code
description: Passaggio 2 dell'esercitazione, distribuzione dell'immagine Docker effettiva al servizio app Azure da un registro contenitori.
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/12/2019
ms.author: kraigb
ms.openlocfilehash: 27cc6e68892821170c1e438378f8635bd320afe5
ms.sourcegitcommit: 74e28a479c87a3a53592646420b78e69852dd86a
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/16/2019
ms.locfileid: "71020089"
---
# <a name="deploy-the-image-to-azure"></a>Distribuire l'immagine in Azure

[Passaggio precedente: Prerequisiti](tutorial-deploy-containers-01.md)

Con un'immagine del contenitore in un registro, è possibile usare l'estensione Docker in VS Code per configurare facilmente un servizio app di Azure che esegue il contenitore.

1. Nell'area **Docker** espandere **Registries** (Registri), espandere il nodo del registro, ad esempio **Azure**, quindi espandere il nodo relativo al nome dell'immagine fino a visualizzare l'immagine con il tag `:latest`.

    ![Individuazione di un'immagine nell'area Docker](media/deploy-containers/deploy-find-image.png)

1. Fare clic con il pulsante destro del mouse sull'immagine e scegliere **Deploy Image to Azure App Service** (Distribuisci immagine nel servizio app di Azure).

    ![Selezione del comando di menu per la distribuzione](media/deploy-containers/deploy-menu.png)

1. Seguire le istruzioni per selezionare una sottoscrizione di Azure, selezionare o specificare un gruppo di risorse, specificare un'area, configurare un piano del servizio app (B1 è il meno costoso) e specificare un nome per il sito. L'animazione seguente illustra il processo.

    ![Animazione sul processo di creazione e distribuzione](media/deploy-containers/deploy-to-app-service.gif)

    Un **gruppo di risorse** è una raccolta denominata delle varie risorse che costituiscono un'app. Assegnando tutte le risorse dell'app a un singolo gruppo, è possibile gestirle facilmente come singola unità. Per altre informazioni, vedere [Panoramica di Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) nella documentazione di Azure.

    Un **piano del servizio app** definisce le risorse fisiche (una macchina virtuale sottostante) che ospitano il contenitore in esecuzione. Per questa esercitazione, B1 è il piano meno costoso che supporta i contenitori Docker. Per altre informazioni, vedere [Panoramica del piano del servizio app](https://docs.microsoft.com/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview) nella documentazione di Azure.

    Il nome del servizio app deve essere univoco in Azure, quindi in genere si usa il nome personale o della società. Per i siti di produzione, il servizio app viene in genere configurato con un nome di dominio registrato separatamente.

1. La creazione del servizio app richiede alcuni minuti e lo stato di avanzamento viene visualizzato nel pannello Output di VS Code.

1. Al termine, è **necessario** aggiungere anche un'impostazione denominata `WEBSITES_PORT` al servizio app per specificare la porta su cui il contenitore è in ascolto. Se si usa un'immagine dell'esercitazione [Creare un contenitore Python in VS Code](https://code.visualstudio.com/docs/python/tutorial-create-container), ad esempio, la porta è 5000 per Flask e 8000 per Django. Per impostare `WEBSITES_PORT`, passare all'area **Azure: Servizio app**, espandere il nodo del nuovo servizio app (aggiornare se necessario), quindi fare clic con il pulsante destro del mouse su **Applications Settings** (Impostazioni applicazioni) e scegliere **Add New Setting** (Aggiungi nuova impostazione). Quando richiesto, immettere `WEBSITES_PORT` come chiave e il numero di porta per il valore.

    ![Comando del menu di scelta rapida in un servizio app per l'aggiunta di una nuova impostazione](media/deploy-containers/add-app-service-setting.png)

1. Il servizio app viene riavviato automaticamente quando si cambiano le impostazioni. È anche possibile fare clic con il pulsante destro del mouse sul servizio app e scegliere **Restart** (Riavvia) in qualsiasi momento.

1. Dopo il riavvio del servizio, esplorare il sito all'indirizzo `http://<name>.azurewebsites.net`. È possibile usare **CTRL+clic** (oppure **CMD+clic** in MacOS) sull'URL nel pannello Output oppure fare clic con il pulsante destro del mouse sul servizio app nell'area **Azure: Servizio app** e scegliere **Esplora sito Web**.

> [!div class="nextstepaction"]
> [L'immagine è stata distribuita](tutorial-deploy-containers-03.md)

[Si è verificato un problema](https://www.research.net/r/PWZWZ52?tutorial=vscode-appservice-containers&step=02-deploy-container)
