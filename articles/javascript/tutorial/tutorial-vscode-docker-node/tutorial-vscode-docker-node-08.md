---
title: Pulire le risorse dopo la distribuzione di un app Node.js in contenitore da Visual Studio Code
description: Parte 8 dell'esercitazione su Docker, pulire le risorse
ms.topic: tutorial
ms.date: 09/20/2019
ms.custom: devx-track-js
ms.openlocfilehash: 4a7ff0e9a84e10223bbe8b55f5309506593f6e07
ms.sourcegitcommit: 576c878c338d286060010646b96f3ad0fdbcb814
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/04/2021
ms.locfileid: "102117895"
---
# <a name="part-8-clean-up-resources"></a>Parte 8: Pulire le risorse

[Passaggio precedente: Eseguire lo streaming dei log](tutorial-vscode-docker-node-07.md)

## <a name="delete-resource-group"></a>Eliminare un gruppo di risorse

Il Servizio app creato per il contenitore include un piano di Servizio app di supporto che può comportare costi. Usare l'estensione Visual Studio Code, i gruppi di risorse di Azure per eliminare il gruppo di risorse e tutte le risorse all'interno del gruppo.

1. Trovare il nome del gruppo di risorse nell'elenco.
1. Fare clic con il pulsante destro del mouse sul nome del gruppo di risorse e scegliere **Elimina**.

    :::image type="content" source="../../media/visual-studio-code-azure-resources-extension-remove-resource-group.png" alt-text="Usare l'estensione Visual Studio Code, i gruppi di risorse di Azure per eliminare il gruppo di risorse e tutte le risorse all'interno del gruppo.":::

## <a name="next-steps"></a>Passaggi successivi

[!INCLUDE [tutorial-next-steps](../../includes/tutorial-next-steps.md)]

> [!div class="nextstepaction"]
> [L'esercitazione è stata completata](../../how-to/deploy-containers.md) [Si è verificato un problema](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-docker-extension&step=clean-up-resources)