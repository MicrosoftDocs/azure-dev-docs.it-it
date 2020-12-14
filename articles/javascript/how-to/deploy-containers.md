---
title: Distribuire contenitori Node.js in Azure
description: Usare contenitori Docker per distribuire app Node.js in Azure
ms.topic: how-to
ms.date: 12/07/2020
ms.custom: seo-javascript-september2019, devx-track-js
ms.openlocfilehash: 6bf8acb66a708433966bdfe90cc358a56d2d3b42
ms.sourcegitcommit: ae2fa266a36958c04625bb0ab212e6f2db98e026
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/08/2020
ms.locfileid: "96857809"
---
# <a name="deploy-nodejs-container-to-azure"></a>Distribuire un contenitore Node.js in Azure 

La creazione di un'app con contenitori è un approccio comune per la scalabilità. Azure offre alcune opzioni per la distribuzione dei contenitori.

## <a name="host-your-container-app-on-azure"></a>Ospitare l'app contenitore in Azure

Le opzioni di hosting seguenti consentono di distribuire le applicazioni in contenitori.

| Servizio | Suggerito per |
|--|--|
|[Servizio app](/azure/app-service/quickstart-custom-container?pivots=container-linux)|Distribuire ed eseguire un contenitore personalizzato nel Servizio app di Azure.|
|[Istanze di Container](/azure/container-instances/)|Configurare rapidamente un singolo contenitore.|
|[Registro Container](/azure/container-registry/)|Creare, archiviare e gestire immagini personalizzate o private di contenitori.|
|[Servizio Kubernetes](/azure/aks/)|Orchestrazioni di più contenitori.|
|[Macchine virtuali](/azure/virtual-machines) (VM)|Controllo completo di una VM Windows o Linux. [Trovare una distribuzione Linux approvata](/azure/virtual-machines/linux/endorsed-distros?toc=/azure/virtual-machines/linux/toc.json) o [informazioni su come trovare](/azure/virtual-machines/linux/cli-ps-findimage) immagini di macchine virtuali in Azure Marketplace.|

## <a name="build-containerize-and-deploy-app-to-azure"></a>Creare, inserire in contenitori e distribuire app in Azure

Per iniziare, seguire questa [esercitazione](develop-nodejs-on-azure.md) per imparare a eseguire queste operazioni:

* Scaricare il codice di esempio
* Eseguire l'app Node.js
* Eseguire il debug dell'app in Visual Studio Code
* Inserire l'app MEAN Node.js in contenitori
* Distribuire l'app usando comandi dell'interfaccia della riga di comando di Azure
* Creare un server MongoDB in una risorsa di CosmosDB
* Aggiungere l'immagine del contenitore al registro contenitori privato
* Aggiungere nomi di dominio personalizzati all'app Web
* Specificare dimensioni superiori per l'app Web
* Creare ed eliminare un gruppo di risorse per tutte le risorse

## <a name="next-steps"></a>Passaggi successivi

Moduli di Microsoft Learn:

- [Eseguire i contenitori Docker con Istanze di Azure Container](/learn/modules/run-docker-with-azure-container-instances/)

- [Compilare e archiviare immagini del contenitore con Registro Azure Container](/learn/modules/build-and-store-container-images/)
