---
title: Creare il Servizio app di Azure Deno da Visual Studio Code
description: Parte 2 dell'esercitazione su Deno, creare l'app Deno ed eseguirla in locale
ms.topic: tutorial
ms.date: 06/01/2020
ms.custom: devx-track-js
ms.openlocfilehash: da5ed76459bdd9f00379e9f9c96873589886f8ed
ms.sourcegitcommit: e1175aa94709b14b283645986a34a385999fb3f7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/03/2020
ms.locfileid: "93233417"
---
# <a name="test-local-deno-apps"></a>Testare app Deno locali

[Passaggio precedente: Introduzione e prerequisiti](tutorial-visual-studio-code-azure-app-service-deno-01.md)

In questo passaggio si crea una semplice API Deno usando il server Web predefinito di Deno. L'app verrà quindi eseguita in locale.

## <a name="create-and-run-a-local-deno-app"></a>Creare ed eseguire un'app Deno locale

1. In un terminale o un prompt dei comandi passare alla posizione in cui creare una nuova cartella dell'app denominata`deno-demo`.

1. Creare un nuovo file denominato `demo.ts`.
1. Deno accetta il codice in esecuzione direttamente dagli URL. Scrivere un server HTTP che risponde a tutte le richieste con "Hello World". Usare il codice seguente:

    ```typescript
    import { serve } from "https://deno.land/std@0.54.0/http/server.ts"
    const handler = serve({ port: 80 })

    console.log("Serving at 80")

    for await (const req of handler) {
     req.respond({ body: "Hello World!\n" })
    }
    ```

1. Eseguire l'app con lo script seguente:

    ```bash
    deno run --allow-net ./demo.ts
    ```

1. Testare l'app aprendo un browser all'indirizzo `http://localhost:80`. Il sito dovrebbe essere simile al seguente:

    ![Esecuzione del server demo](media/deploy-azure/deno-hello-world.png)

    È anche possibile eseguire questo codice digitando `deno run --allow-net https://gist.githubusercontent.com/khaosdoctor/cd2bbb28e682feb8d20a7aba47fc1e17/raw/92de998fd11f2a24ae40bbcb84f5262cfe9389b2/deno-demo.ts`

1. Premere **CTRL**+**C** nel terminale per arrestare il server.

> [!div class="nextstepaction"]
> [L'app Deno è stata creata](tutorial-visual-studio-code-azure-app-service-deno-03.md) [Si è verificato un problema](https://www.research.net/r/PWZWZ52?tutorial=deno-deployment-azureappservice&step=create-app)

## <a name="next-steps"></a>Passaggi successivi

[!INCLUDE [tutorial-next-steps](includes/tutorial-next-steps.md)]

> [!div class="nextstepaction"]
> [L'esercitazione è stata completata](node-howto-deploy-web-app.md) [Si è verificato un problema](https://www.research.net/r/PWZWZ52?tutorial=deno-deployment-azureappservice&step=clean-up-resources)
