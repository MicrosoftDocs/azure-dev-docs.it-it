---
title: Pulire le risorse dopo la distribuzione di un app Node.js in contenitore da Visual Studio Code
description: Parte 8 dell'esercitazione su Docker, pulire le risorse
ms.topic: tutorial
ms.date: 09/20/2019
ms.custom: devx-track-js
ms.openlocfilehash: 47f258ef2b6bb31e7b3a9ee47749c9b0bdd22aaf
ms.sourcegitcommit: f723980ade4cbc13548a5d8ac3f3fa681b8a2dbd
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/16/2020
ms.locfileid: "97609307"
---
# <a name="part-8-clean-up-resources"></a>Parte 8: Pulire le risorse

[Passaggio precedente: Eseguire lo streaming dei log](tutorial-vscode-docker-node-07.md)

## <a name="delete-app-service-resource-and-resource-group"></a>Eliminare una risorsa e un gruppo di risorse del servizio app

Il Servizio app creato per il contenitore include un piano di Servizio app di supporto che può comportare costi. Per pulire le risorse, fare clic con il pulsante destro del mouse sul servizio app nell'area **Azure: App Service** (Azure: Servizio app) e selezionare **Delete** (Elimina).

È anche possibile visitare il [portale di Azure](https://portal.azure.com), selezionare **Gruppi di risorse** nel riquadro di spostamento sinistro, selezionare il gruppo di risorse creato nella procedura di questa esercitazione e quindi usare il comando **Elimina gruppo di risorse**.

### <a name="next-steps"></a>Passaggi successivi

[!INCLUDE [tutorial-next-steps](../../includes/tutorial-next-steps.md)]

> [!div class="nextstepaction"]
> [L'esercitazione è stata completata](../../how-to/deploy-containers.md) [Si è verificato un problema](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-docker-extension&step=clean-up-resources)