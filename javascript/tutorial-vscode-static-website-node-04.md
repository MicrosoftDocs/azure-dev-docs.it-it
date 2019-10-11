---
title: Distribuire i file dell'app Node.js statica in Archiviazione di Azure da Visual Studio Code
description: Parte 4 dell'esercitazione, distribuire i file in Archiviazione di Azure
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/24/2019
ms.author: kraigb
ms.openlocfilehash: b1295fcb9a90ca26880715296e4214c2a1df6a07
ms.sourcegitcommit: c04984b6367e922dbc5973af44f8cd0ca81ce157
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/30/2019
ms.locfileid: "71685950"
---
# <a name="deploy-the-website-to-azure-storage"></a>Distribuire il sito Web in Archiviazione di Azure

[Passaggio precedente: Creare un account di archiviazione](tutorial-vscode-static-website-node-03.md)

In questo passaggio si usa Visual Studio Code per distribuire i file del sito Web statico creati nei passaggi precedenti in Archiviazione di Azure, dove verranno ospitati e gestiti.

1. In Visual Studio Code passare all'area **Archiviazione di Azure**, espandere la propria sottoscrizione, espandere il nodo relativo all'account di Archiviazione di Azure creato nel passaggio precedente, quindi espandere il nodo **Contenitori BLOB**. Il codice dell'app verrà distribuito nel contenitore `$web`.

    ![I nodi di Archiviazione di Azure nell'area Archiviazione di Azure](media/static-website/storage-nodes.png)

1. Selezionare l'area **File**, fare clic con il pulsante destro del mouse sulla cartella *build* e scegliere **Deploy to Static Website** (Distribuisci in sito Web statico):

    ![Comando Deploy to Static Website](media/static-website/deploy-build.png)

1. Quando richiesto, scegliere l'account di archiviazione creato in precedenza.

1. Al termine della distribuzione, viene visualizzato un messaggio con il pulsante **Browse to website** (Passa a sito Web). Selezionare il pulsante per aprire l'endpoint primario del codice dell'app distribuito.

    ![Messaggio di distribuzione completata](media/static-website/deployment-complete.png)

    ![Sito Web statico in esecuzione in Azure](media/static-website/azure-app.png)

> [!div class="nextstepaction"]
> [Il sito si trova in Azure](tutorial-vscode-static-website-node-05.md) [Si è verificato un problema](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-staticwebsite&step=create-storage)
