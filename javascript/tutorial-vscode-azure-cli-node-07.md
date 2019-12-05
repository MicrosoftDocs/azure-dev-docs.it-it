---
title: Pulire le risorse dopo aver distribuito un'app Node.js in Azure tramite l'interfaccia della riga di comando di Azure
description: Parte 7 dell'esercitazione, pulire le risorse
ms.topic: conceptual
ms.date: 09/24/2019
ms.openlocfilehash: 7998eb641090b252455613a46ae41e45e5cd1c1d
ms.sourcegitcommit: e77f8f652128b798dbf972078a7b460ed21fb5f8
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/25/2019
ms.locfileid: "74466752"
---
# <a name="clean-up-resources"></a>Pulire le risorse

[Passaggio precedente: Apportare modifiche e ripetere la distribuzione](tutorial-vscode-docker-node-06.md)

Il servizio app creato include un piano di servizio app sottostante che può comportare costi. Per pulire le risorse, eseguire il comando seguente in un terminale o al prompt dei comandi:

```bash
az group delete --name myResourceGroup
```

È anche possibile visitare il [portale di Azure](https://portal.azure.com), selezionare **Gruppi di risorse** nel riquadro di spostamento sinistro, selezionare il gruppo di risorse creato nella procedura di questa esercitazione e quindi usare il comando **Elimina gruppo di risorse**.

> [!div class="nextstepaction"]
> [L'esercitazione è stata completata](node-howto-deploy-web-app.md) [Si è verificato un problema](https://www.research.net/r/PWZWZ52?tutorial=node-deployment&step=clean-up-resources)
