---
title: 'Passaggio 3: Ridistribuire un contenitore nel servizio app di Azure dopo aver apportato modifiche in Visual Studio Code'
description: Passaggio 3 dell'esercitazione, la procedura semplice per ricompilare e ridistribuire un'immagine del contenitore.
ms.topic: conceptual
ms.date: 09/12/2019
ms.custom: seo-python-october2019
ms.openlocfilehash: f0e6eb808e25f3bb48eb2c849310747945e0d7f9
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/05/2020
ms.locfileid: "80441866"
---
# <a name="2-redeploy-a-container-to-azure-app-service-after-making-changes"></a>2: Ridistribuire un contenitore nel Servizio app di Azure dopo aver apportato modifiche

[Passaggio precedente: Distribuire l'immagine in Azure](tutorial-deploy-containers-02.md)

Questo articolo spiega come ridistribuire un contenitore nel servizio app di Azure dopo aver apportato modifiche in Visual Studio Code.

Dal momento che inevitabilmente si apportano modifiche all'app, si finisce per ricompilare e ridistribuire il contenitore molte volte. Il processo è tuttavia semplice:

1. Apportare le modifiche all'app e testarla in locale. Questo passaggio e i due che seguono sono illustrati nell'esercitazione [Creare un contenitore Python in vs Code](https://code.visualstudio.com/docs/python/tutorial-create-containers).

1. Ricompilare l'immagine Docker. Se si cambia solo il codice dell'app, la compilazione dovrebbe richiedere solo pochi secondi.

1. Eseguire il push dell'immagine nel registro. Anche in questo caso, se si cambia solo il codice dell'app, è necessario eseguire il push solo di un piccolo livello e il processo viene in genere completato in pochi secondi.

1. Nell'area **Azure: Servizio app** fare clic con il pulsante destro del mouse sul servizio app appropriato e scegliere **Restart** (Riavvia). Con il riavvio di un servizio app viene automaticamente eseguito il pull dell'immagine del contenitore più recente dal registro di sistema.

1. Dopo circa 15-20 secondi, visitare di nuovo l'URL del servizio app per controllare gli aggiornamenti.

> [!div class="nextstepaction"]
> [Sono state apportate modifiche ed è stata ripetuta la distribuzione: procedere con il passaggio 4 >>>](tutorial-deploy-containers-04.md)

[Si è verificato un problema](https://www.research.net/r/PWZWZ52?tutorial=vscode-appservice-containers&step=03-make-changes-redeploy)
