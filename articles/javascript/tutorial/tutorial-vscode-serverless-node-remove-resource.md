---
title: Rimuovere le costose risorse remote di Azure dopo la distribuzione dell'applicazione di Funzioni di Azure
description: Rimuovere (pulire) le risorse remote di Azure per evitare di incorrere in costi. Per pulire le risorse, fare clic con il pulsante destro del mouse sull'app per le funzioni nell'area Funzioni di Azure, quindi scegliere **Delete Function App** (Elimina app per le funzioni).
ms.topic: tutorial
ms.date: 08/31/2020
ms.custom: devx-track-js, contperf-fy21q2
ms.openlocfilehash: cfb5941e6152a2919e3f2b7570d94df66e0c74f9
ms.sourcegitcommit: 576c878c338d286060010646b96f3ad0fdbcb814
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/04/2021
ms.locfileid: "102117785"
---
# <a name="5-clean-up-azure-resources-for-azure-functions-tutorial"></a>5. Pulire le risorse di Azure per l'esercitazione su Funzioni di Azure

[Passaggio precedente: Distribuire l'app per le funzioni](tutorial-vscode-serverless-node-deploy-hosting.md)

## <a name="remove-remote-azure-resources"></a>Rimuovere le risorse remote di Azure

L'app di Funzioni di Azure creata include risorse che possono comportare costi minimi (vedere [Prezzi di Funzioni di Azure](https://azure.microsoft.com/pricing/details/functions/)). Usare l'estensione Visual Studio Code, i gruppi di risorse di Azure per eliminare il gruppo di risorse e tutte le risorse all'interno del gruppo. 

1. Trovare il nome del gruppo di risorse nell'elenco.
1. Fare clic con il pulsante destro del mouse sul nome del gruppo di risorse e scegliere **Elimina**.

    :::image type="content" source="../media/visual-studio-code-azure-resources-extension-remove-resource-group.png" alt-text="Usare l'estensione Visual Studio Code, i gruppi di risorse di Azure per eliminare il gruppo di risorse e tutte le risorse all'interno del gruppo.":::

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
