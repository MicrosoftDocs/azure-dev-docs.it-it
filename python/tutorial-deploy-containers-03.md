---
title: Ridistribuire un contenitore nel servizio app di Azure dopo aver apportato modifiche in Visual Studio Code
description: Passaggio 3 dell'esercitazione, la procedura semplice per ricompilare e ridistribuire un'immagine del contenitore.
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/12/2019
ms.author: kraigb
ms.openlocfilehash: 30b0d0863900c36232b69c23db0eae1c70a34396
ms.sourcegitcommit: 74e28a479c87a3a53592646420b78e69852dd86a
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/16/2019
ms.locfileid: "71019499"
---
# <a name="make-changes-and-redeploy"></a>Apportare modifiche e ripetere la distribuzione

[Passaggio precedente: Distribuire l'immagine in Azure](tutorial-deploy-containers-02.md)

Dal momento che inevitabilmente si apportano modifiche all'app, si finisce per ricompilare e ridistribuire il contenitore molte volte. Il processo è tuttavia semplice:

1. Apportare le modifiche all'app e testarla in locale. Questo passaggio e i due che seguono sono illustrati nell'esercitazione [Creare un contenitore Python in vs Code](https://code.visualstudio.com/docs/python/tutorial-create-container).

1. Ricompilare l'immagine Docker. Se si cambia solo il codice dell'app, la compilazione dovrebbe richiedere solo pochi secondi.

1. Eseguire il push dell'immagine nel registro. Anche in questo caso, se si cambia solo il codice dell'app, è necessario eseguire il push solo di un piccolo livello e il processo viene in genere completato in pochi secondi.

1. Nell'area **Azure: Servizio app** fare clic con il pulsante destro del mouse sul servizio app appropriato e scegliere **Restart** (Riavvia). Con il riavvio di un servizio app viene automaticamente eseguito il pull dell'immagine del contenitore più recente dal registro di sistema.

1. Dopo circa 15-20 secondi, visitare di nuovo l'URL del servizio app per controllare gli aggiornamenti.

> [!div class="nextstepaction"]
> [Sono state apportate modifiche ed è stata ripetuta la distribuzione](tutorial-deploy-containers-04.md)

[Si è verificato un problema](https://www.research.net/r/PWZWZ52?tutorial=vscode-appservice-containers&step=03-make-changes-redeploy)
