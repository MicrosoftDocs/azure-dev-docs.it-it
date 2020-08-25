---
title: Pulire le risorse dopo aver distribuito un'app Node.js in Azure tramite l'interfaccia della riga di comando di Azure
description: Parte 7 dell'esercitazione, pulire le risorse
ms.topic: conceptual
ms.date: 09/24/2019
ms.custom: devx-track-javascript
ms.openlocfilehash: fb78f369d80ff40fef1d4a21df68ea79fafa2dbc
ms.sourcegitcommit: 0699b984b85782b1c441289fa756f285eae853c3
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/14/2020
ms.locfileid: "88218535"
---
# <a name="part-7-clean-up-resources"></a>Parte 7: Pulire le risorse

[Passaggio precedente: Apportare modifiche e ripetere la distribuzione](tutorial-vscode-docker-node-06.md)

Il servizio app creato include un piano di servizio app sottostante che può comportare costi. Per pulire le risorse, eseguire il comando seguente in un terminale o al prompt dei comandi:

```azurecli
az group delete --name myResourceGroup
```

È anche possibile visitare il [portale di Azure](https://portal.azure.com), selezionare **Gruppi di risorse** nel riquadro di spostamento sinistro, selezionare il gruppo di risorse creato nella procedura di questa esercitazione e quindi usare il comando **Elimina gruppo di risorse**.

> [!div class="nextstepaction"]
> [L'esercitazione è stata completata](node-howto-deploy-web-app.md) [Si è verificato un problema](https://www.research.net/r/PWZWZ52?tutorial=node-deployment&step=clean-up-resources)
