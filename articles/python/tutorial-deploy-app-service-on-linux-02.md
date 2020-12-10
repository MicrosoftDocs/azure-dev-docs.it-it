---
title: "Passaggio 2: Preparare un'app per la distribuzione nel Servizio app Azure in Linux da Visual Studio Code"
description: Passaggio 2 dell'esercitazione, configurare l'applicazione
ms.topic: conceptual
ms.date: 11/20/2020
ms.custom: devx-track-python, seo-python-october2019
ms.openlocfilehash: 7197f8afc28bd62e7247c3955c888199ee69c509
ms.sourcegitcommit: 09b4a2dbe13601fdf16fcc4082a5075b46ad3459
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/03/2020
ms.locfileid: "96559195"
---
# <a name="2-prepare-your-app-for-deployment-to-azure-app-service"></a>2: Preparare l'app per la distribuzione nel Servizio app di Azure

[Passaggio precedente: Configurare l'ambiente](tutorial-deploy-app-service-on-linux-01.md)

In questo articolo viene preparata un'app da distribuire nel servizio app di Azure per questa esercitazione. È possibile usare un'app esistente oppure creare o scaricare un'app.

## <a name="if-you-already-have-an-app"></a>Se l'app è già esistente

Se si vuole usare un'app esistente, verificare la presenza di un file *requirements.txt* nel progetto che elenchi le dipendenze, inclusi i framework come Flask o Django. È possibile usare il framework che si preferisce.

> [!div class="nextstepaction"]
> [L'app è pronta: procedere con il passaggio 3 >>>](tutorial-deploy-app-service-on-linux-03.md)

## <a name="if-you-dont-already-have-an-app"></a>Se non si ha già un'app

Se non si ha già un'app, usare *una* delle opzioni seguenti. Assicurarsi che l'app venga eseguita in locale.

La parte restante di questa esercitazione usa il codice illustrato nell'[opzione 3](#option-3-create-a-minimal-flask-app).

### <a name="option-1-use-the-vs-code-flask-tutorial-sample"></a>Opzione 1: Usare l'esempio di esercitazione su Flask in VS Code

Scaricare o clonare [https://github.com/Microsoft/python-sample-vscode-flask-tutorial](https://github.com/Microsoft/python-sample-vscode-flask-tutorial), ovvero il risultato dell'[esercitazione su Flask](https://code.visualstudio.com/docs/python/tutorial-flask). Si noti che il codice dell'app si trova specificamente nella cartella *hello_app*. Per istruzioni sull'esecuzione dell'app in locale, vedere il file *readme.md* dell'esempio.

### <a name="option-2-use-the-vs-code-django-tutorial-sample"></a>Opzione 2: Usare l'esempio di esercitazione su Django in VS Code

Scaricare o clonare [https://github.com/Microsoft/python-sample-vscode-django-tutorial](https://github.com/Microsoft/python-sample-vscode-django-tutorial), ovvero il risultato dell'[esercitazione su Django](https://code.visualstudio.com/docs/python/tutorial-django).

Idealmente, anche le app Django distribuite nel cloud usano un database basato sul cloud, ad esempio PostgreSQL per Azure. Per altre informazioni, vedere [Esercitazione: Distribuire un'app Web Django con PostgreSQL tramite il portale di Azure](tutorial-python-postgresql-app-portal.md).

Se l'app Django usa un database SQLite locale come questo esempio, ai fini di questa esercitazione è più semplice includere una copia pre-inizializzata e precompilata del file *db.sqlite3* nel repository. In caso contrario, è necessario configurare un comando post-compilazione per eseguire il comando `migrate` di Django nel contenitore in cui viene distribuita l'app. Per altre informazioni, vedere [Configurazione del servizio app - Personalizzare l'automazione della compilazione](/azure/app-service/configure-language-python#customize-build-automation).

### <a name="option-3-create-a-minimal-flask-app"></a>Opzione 3: Creare un'app Flask minima

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
    Flask
    ```

1. Aprire una finestra del terminale usando il comando di menu **Terminale** > **Nuovo terminale**.

1. Nel terminale creare e attivare un ambiente virtuale denominato `.venv`. 

    # <a name="macoslinux"></a>[macOS/Linux](#tab/linux)

    ```bash
    sudo apt-get install python3-venv    # If needed
    python3 -m venv .venv
    source .venv/bin/activate
    ```

    # <a name="windows"></a>[Windows](#tab/windows)

    ```cmd
    py -3 -m venv .venv
    .venv\scripts\activate
    ```

    ---

1. Quando VS Code richiede l'attivazione dell'ambiente appena creato, rispondere **Sì**.

1. Installare le dipendenze dell'app:

    ```cmd
    pip install -r requirements.txt
    ```

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

1. Aprire l'app in un browser usando l'URL `http://127.0.0.1:5000/`. Verrà visualizzato il messaggio "Hello Flask, on Azure App Service for Linux".

1. Arrestare il server Flask premendo **CTRL**+**C** nel terminale.

> [!div class="nextstepaction"]
> [L'app è pronta: procedere con il passaggio 3 >>>](tutorial-deploy-app-service-on-linux-03.md)

[Problemi? Segnalarli](https://aka.ms/FlaskVSCQuickstartHelp).
