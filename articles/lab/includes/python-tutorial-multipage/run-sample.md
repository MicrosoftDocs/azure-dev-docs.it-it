---
title: file di inclusione azure-sign-in.md
description: file di inclusione azure-sign-in.md
ms.date: 10/13/2020
ms.topic: include
ms.custom: devx-track-javascript
ms.openlocfilehash: 6f9e5e8f30c5108996f14bef9dc4c90a668c60c1
ms.sourcegitcommit: 5f64710b2b0822e789c7f15acba5a3a257c033f9
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/05/2020
ms.locfileid: "93405298"
---
1. Assicurarsi di trovarsi nella cartella *python-docs-hello-world*. 

1. Creare un ambiente virtuale e installare le dipendenze:

    [!include [virtual environment setup](../app-service-quickstart-python-venv.md)]

    Se si verifica l'errore "[Errno 2] Impossibile trovare il file o la directory: 'requirements.txt'", assicurarsi di trovarsi nella cartella *python-docs-hello-world*.

1. Eseguire il server di sviluppo.

    ```terminal  
    flask run
    ```
    
    Per impostazione predefinita, il server presuppone che il modulo di ingresso dell'app si trovi in *app.py* , come usato nell'esempio. Se si usa un nome di modulo diverso, impostare la variabile di ambiente `FLASK_APP` su tale nome.

1. Aprire un Web browser e passare all'app di esempio all'indirizzo `http://localhost:5000/`. L'app visualizza il messaggio **Hello World!** .

    ![Eseguire un'app Python di esempio in locale](../../media/quickstart-python/run-hello-world-sample-python-app-in-browser-localhost.png)
    
1. Nella finestra del terminale premere **CTRL**+**C** per uscire dal server di sviluppo.
