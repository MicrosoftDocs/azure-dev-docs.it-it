---
title: Distribuire contenitori Node.js in Azure
description: Usare contenitori Docker per distribuire app Node.js in Azure
ms.topic: how-to
ms.date: 12/07/2020
ms.custom: seo-javascript-september2019, devx-track-js
ms.openlocfilehash: 50c26c26fffc0c9377b661d79858dd97318bd3c2
ms.sourcegitcommit: 3f8aa923e4626b31cc533584fe3b66940d384351
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/01/2021
ms.locfileid: "99224785"
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

Per iniziare, selezionare nell'elenco:
* Esercitazione: [Express.js](../tutorial/tutorial-vscode-docker-node/tutorial-vscode-docker-node-01.md)
* Esercitazione: [deno](../tutorial/deploy-deno-app-azure-app-service-azure-cli.md)

## <a name="next-steps"></a>Passaggi successivi

Moduli di Microsoft Learn:

- [Eseguire i contenitori Docker con Istanze di Azure Container](/learn/modules/run-docker-with-azure-container-instances/)

- [Compilare e archiviare immagini del contenitore con Registro Azure Container](/learn/modules/build-and-store-container-images/)
