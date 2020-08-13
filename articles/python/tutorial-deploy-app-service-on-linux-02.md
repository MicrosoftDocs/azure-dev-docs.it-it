---
title: "Passaggio 2: Preparare un'app per la distribuzione nel Servizio app Azure in Linux da Visual Studio Code"
description: Passaggio 2 dell'esercitazione, configurare l'applicazione
ms.topic: conceptual
ms.date: 09/12/2019
ms.custom: devx-track-python, seo-python-october2019
ms.openlocfilehash: 0f85ea4e7da74b67ce2a20168ae751444b63b292
ms.sourcegitcommit: 980efe813d1f86e7e00929a0a3e1de83514ad7eb
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/07/2020
ms.locfileid: "87983586"
---
# <a name="2-prepare-your-app-for-deployment-to-azure-app-service"></a>2: Preparare l'app per la distribuzione nel Servizio app di Azure

[Passaggio precedente: Prerequisiti](tutorial-deploy-app-service-on-linux-01.md)

In questo articolo viene preparata un'app da distribuire nel servizio app di Azure per questa esercitazione. È possibile usare un'app esistente oppure creare o scaricare un'app.

Se si ha già un'app che si vuole usare, verificare di avere un file *requirements.txt* che descrive le dipendenze, inclusi i framework come Flask o Django. È possibile usare il framework che si preferisce.

Se non si ha già un'app, usare una delle opzioni seguenti. Assicurarsi che l'app venga eseguita in locale.

## <a name="minimal-flask-app"></a>App Flask minima

Questa sezione descrive l'app Flask minima usata in questa procedura dettagliata.

1. Creare una nuova cartella, aprirla in VS Code e aggiungere un file denominato *hello.py* con il contenuto seguente. L'oggetto app è denominato appositamente `myapp` per dimostrare come vengono usati i nomi nel comando di avvio per il servizio app, come illustrato di seguito.

    ```python
    from flask import Flask
    myapp = Flask(__name__)

    @myapp.route("/")
    def hello():
        return "Hello Flask, on Azure App Service for Linux"
    ```

1. Creare un file denominato *requirements.txt* con il contenuto seguente:

    ```text
    Flask==1.1.1
    ```

1. Seguire le istruzioni riportate in [Esercitazione su Flask - Creare un ambiente di progetto per Flask](https://code.visualstudio.com/docs/python/tutorial-flask#create-a-project-environment-for-flask) per creare un ambiente virtuale con Flask installato, all'interno del quale è possibile eseguire l'app in locale.

1. Impostare la variabile di ambiente FLASK_APP per indicare a Flask dove trovare l'oggetto app:

    # <a name="cmd"></a>[cmd](#tab/cmd)

    ```cmd
    set FLASK_APP=hello:myapp
    ```

    # <a name="powershell"></a>[PowerShell](#tab/powershell)

    ```ps
    $env:FLASK_APP = "hello:myapp"
    ```

   # <a name="bash"></a>[Bash](#tab/bash)

    ```bash
    export FLASK_APP=hello:myapp
    ```

    ---

1. Eseguire l'app:

    ```cmd
    flask run
    ```

1. È quindi possibile aprire l'app in un browser usando l'URL `http://127.0.0.1:5000/`.

## <a name="vs-code-flask-tutorial-sample"></a>Esempio di esercitazione su Flask in VS Code

Scaricare o clonare [python-sample-vscode-flask-tutorial](https://github.com/Microsoft/python-sample-vscode-flask-tutorial), che è il risultato dell'[esercitazione su Flask](https://code.visualstudio.com/docs/python/tutorial-flask).

## <a name="vs-code-django-tutorial-sample"></a>Esempio di esercitazione su Django in VS Code

Scaricare o clonare [python-sample-vscode-django-tutorial](https://github.com/Microsoft/python-sample-vscode-django-tutorial), che è il risultato dell'[esercitazione su Django](https://code.visualstudio.com/docs/python/tutorial-django).

Se l'app Django usa un database SQLite locale come questo esempio, è necessario includere una copia pre-inizializzata e precompilata del file *db.sqlite3* nel repository. Il motivo è che, al momento, il servizio app per Linux non consente di eseguire il comando `migrate` di Django come parte della distribuzione, quindi è necessario distribuire un database creato in precedenza. Anche in questo caso, il database è effettivamente di sola lettura e le operazioni di scrittura generano errori.

In ogni caso, l'opzione migliore prevede l'uso di un database separato distribuito e inizializzato in modo indipendente dal codice dell'app.

> [!div class="nextstepaction"]
> [L'app è pronta: procedere con il passaggio 3 >>>](tutorial-deploy-app-service-on-linux-03.md)

[Si è verificato un problema](https://www.research.net/r/PWZWZ52?tutorial=vscode-appservice-python&step=02-prepare-app)
