---
title: Ridistribuire un contenitore nel servizio app di Azure dopo aver apportato modifiche in Visual Studio Code
description: Passaggio 6 dell'esercitazione su Docker, la procedura semplice per ricompilare e ridistribuire un'immagine del contenitore.
ms.topic: tutorial
ms.date: 09/20/2019
ms.custom: devx-track-js
ms.openlocfilehash: 29f07936d69d971c9ea139b019ac4ee272871342
ms.sourcegitcommit: 4dd392ea864be52421d0239e59198bc44b0a5a16
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/25/2020
ms.locfileid: "91365104"
---
# <a name="make-changes-and-redeploy-a-container-using-visual-studio-code"></a>Apportare modifiche e ridistribuire un contenitore con Visual Studio Code

[Passaggio precedente: Distribuire l'immagine dell'app](tutorial-vscode-docker-node-05.md)

Dal momento che inevitabilmente si apportano modifiche all'app, si finisce per ricompilare e ridistribuire il contenitore molte volte. Il processo è tuttavia semplice:

1. Apportare le modifiche all'app e testarla in locale.

1. In Visual Studio Code aprire il **riquadro comandi** (**F1**) ed eseguire **Docker Images: Build Image** (Immagini Docker: Compila immagine) per ricompilare l'immagine. Se si cambia solo il codice dell'app, la compilazione dovrebbe richiedere solo pochi secondi.

1. Per eseguire il push dell'immagine nel registro, aprire di nuovo il **riquadro comandi** (**F1**) ed eseguire **Docker Images: Push** (Immagini Docker: Esegui il push), scegliendo l'immagine appena compilata. Anche in questo caso, poiché si apporta solo una lieve modifica al codice dell'app, è necessario eseguire il push solo di questo livello e il processo viene in genere completato in pochi secondi.

1. Nell'area **Azure: Servizio app** fare clic con il pulsante destro del mouse sul servizio app appropriato e scegliere **Restart** (Riavvia). Con il riavvio di un servizio app viene automaticamente eseguito il pull dell'immagine del contenitore più recente dal registro di sistema.

1. Dopo circa 15-20 secondi, visitare di nuovo l'URL del servizio app per controllare gli aggiornamenti.

> [!div class="nextstepaction"]
> [Le modifiche vengono visualizzate](tutorial-vscode-docker-node-07.md) [Si è verificato un problema](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-docker-extension&step=deploy-changes)
