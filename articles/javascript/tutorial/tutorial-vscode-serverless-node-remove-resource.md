---
title: Rimuovere le costose risorse remote di Azure dopo la distribuzione dell'applicazione di Funzioni di Azure
description: Rimuovere (pulire) le risorse remote di Azure per evitare di incorrere in costi. Per pulire le risorse, fare clic con il pulsante destro del mouse sull'app per le funzioni nell'area Funzioni di Azure, quindi scegliere **Delete Function App** (Elimina app per le funzioni).
ms.topic: tutorial
ms.date: 08/31/2020
ms.custom: devx-track-js, contperf-fy21q2
ms.openlocfilehash: 428330b34b3c315d01c2209840de62001caa747a
ms.sourcegitcommit: c8330128d5d6a71859933a890ecdf047cb950996
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/15/2020
ms.locfileid: "97522049"
---
# <a name="5-clean-up-azure-resources-for-azure-functions-tutorial"></a>5. Pulire le risorse di Azure per l'esercitazione su Funzioni di Azure

[Passaggio precedente: Distribuire l'app per le funzioni](tutorial-vscode-serverless-node-deploy-hosting.md)

## <a name="remove-remote-azure-resources"></a>Rimuovere le risorse remote di Azure

L'app di Funzioni di Azure creata include risorse che possono comportare costi minimi (vedere [Prezzi di Funzioni di Azure](https://azure.microsoft.com/pricing/details/functions/)). Per pulire le risorse, fare clic con il pulsante destro del mouse sull'app per le funzioni nell'area **Azure: Funzioni** e scegliere **Delete Function App** (Elimina app per le funzioni).

È anche possibile visitare il [portale di Azure](https://portal.azure.com), selezionare **Gruppi di risorse** nel riquadro di spostamento sinistro, selezionare il gruppo di risorse creato nella procedura di questa esercitazione e quindi usare il comando **Elimina gruppo di risorse**.

[!INCLUDE [Next steps for using VSCode extensions](../includes/tutorial-next-steps-vscode-extensions.md)]

[!INCLUDE [Next steps for using JavaScript on Azure](../includes/tutorial-next-steps-js-azure.md)]

## <a name="learn-more-about-azure-functions"></a>Altre informazioni su Funzioni di Azure

* [Guida per sviluppatori di Funzioni di Azure](/azure/azure-functions/functions-reference)
* [Guida per gli sviluppatori JavaScript di Funzioni di Azure](/azure/azure-functions/functions-reference-node)
* [Protezione di Funzioni di Azure](/azure/azure-functions/security-concepts)
* Considerazioni su [archiviazione](/azure/azure-functions/storage-considerations) e [prestazioni](/azure/azure-functions/functions-best-practices)

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"]
> [L'esercitazione è stata completata](../how-to/develop-serverless-apps.md) [Si è verificato un problema](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azurefunctions&step=clean-up-resources)
