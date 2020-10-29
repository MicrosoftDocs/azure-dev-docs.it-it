---
title: Pulire le risorse dopo aver distribuito un'app Node.js in Azure tramite l'interfaccia della riga di comando di Azure
description: Parte 7 dell'esercitazione, pulire le risorse con l'interfaccia della riga di comando di Azure
ms.topic: tutorial
ms.date: 09/24/2019
ms.custom: devx-track-js, devx-track-azurecli
ms.openlocfilehash: 037bc51c8f11faaf5b0c9bd0051a6c28197dd17b
ms.sourcegitcommit: 1ddcb0f24d2ae3d1f813ec0f4369865a1c6ef322
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/27/2020
ms.locfileid: "92688615"
---
# <a name="part-7-clean-up-resources"></a>Parte 7: Pulire le risorse

[Passaggio precedente: Apportare modifiche e ripetere la distribuzione](tutorial-vscode-docker-node-06.md)

Il servizio app creato include un piano di servizio app sottostante che può comportare costi. Per pulire le risorse, eseguire il comando seguente in un terminale o al prompt dei comandi:

```azurecli
az group delete --name myResourceGroup
```

È anche possibile visitare il [portale di Azure](https://portal.azure.com), selezionare **Gruppi di risorse** nel riquadro di spostamento sinistro, selezionare il gruppo di risorse creato nella procedura di questa esercitazione e quindi usare il comando **Elimina gruppo di risorse** .

> [!div class="nextstepaction"]
> [L'esercitazione è stata completata](node-howto-deploy-web-app.md) [Si è verificato un problema](https://www.research.net/r/PWZWZ52?tutorial=node-deployment&step=clean-up-resources)
