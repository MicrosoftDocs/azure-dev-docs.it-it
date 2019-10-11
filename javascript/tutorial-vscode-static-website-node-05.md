---
title: Distribuire modifiche in un sito Web Node.js statico da Visual Studio Code
description: Parte 5 dell'esercitazione, apportare modifiche e ripetere la distribuzione
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/24/2019
ms.author: kraigb
ms.openlocfilehash: 986d2a0f8999d79dfd1d856ed20a053c495a3765
ms.sourcegitcommit: c04984b6367e922dbc5973af44f8cd0ca81ce157
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/30/2019
ms.locfileid: "71685938"
---
# <a name="make-changes-and-redeploy"></a>Apportare modifiche e ripetere la distribuzione

[Passaggio precedente: Eseguire la distribuzione in Archiviazione di Azure](tutorial-vscode-static-website-node-04.md)

In questo passaggio viene apportata una semplice modifica al codice sorgente dell'app, quindi il sito viene ridistribuito per verificare il flusso di lavoro di distribuzione end-to-end.

1. In Visual Studio Code aprire il file *src/app.js* e cambiare la riga 11 come indicato di seguito:

    ```js
    <h1 className="App-title">Welcome to Azure!</h1>
    ```

1. In un terminale o al prompt dei comandi eseguire `npm run build`.

1. In VS Code fare clic con il pulsante destro del mouse sulla cartella *build* aggiornata e scegliere di nuovo **Deploy to Static Website** (Distribuisci nel sito Web statico). Scegliere l'account di archiviazione e confermare di voler distribuire le modifiche. L'estensione di Azure elimina automaticamente i vecchi file prima di distribuire le modifiche per evitare problemi di memorizzazione nella cache.

1. Al termine della distribuzione, aggiornare il sito nel browser per osservare le modifiche:

    ![Modifiche nell'app dopo la ridistribuzione](media/static-website/updated-azure-app.png)

> [!div class="nextstepaction"]
> [Le modifiche sono state distribuite](tutorial-vscode-static-website-node-06.md) [Si è verificato un problema](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-staticwebsite&step=code-change)
