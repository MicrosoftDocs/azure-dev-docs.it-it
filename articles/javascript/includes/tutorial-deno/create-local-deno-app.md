---
title: file di inclusione 2
description: file di inclusione 2
ms.topic: include
ms.date: 06/01/2020
ms.custom: devx-track-js
ms.openlocfilehash: 05f3e5fb847e7589e41d041736c986dce0bc5b29
ms.sourcegitcommit: 5541f993c01ce356e1b0eaa8f95aea9051c3c21e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/03/2020
ms.locfileid: "93278679"
---
In questo passaggio si crea una semplice API Deno usando il server Web predefinito di Deno. L'app verrà quindi eseguita in locale.

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

    ![Esecuzione del server demo](../../media/deploy-azure/deno-hello-world.png)

    È anche possibile eseguire questo codice digitando `deno run --allow-net https://gist.githubusercontent.com/khaosdoctor/cd2bbb28e682feb8d20a7aba47fc1e17/raw/92de998fd11f2a24ae40bbcb84f5262cfe9389b2/deno-demo.ts`

1. Premere **CTRL**+**C** nel terminale per arrestare il server.
