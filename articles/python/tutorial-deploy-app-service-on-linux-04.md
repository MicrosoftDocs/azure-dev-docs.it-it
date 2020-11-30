---
title: 'Passaggio 4: Configurare un file di avvio personalizzato per le app Python nel Servizio app di Azure in Linux'
description: Passaggio 4 dell'esercitazione, che indica al servizio app come avviare l'app Web e include istruzioni specifiche per Django, Flask e altri framework.
ms.topic: conceptual
ms.date: 11/20/2020
ms.custom: devx-track-python, seo-python-october2019
ms.openlocfilehash: 0c7fd378fa1166113812c4748c10676030941e13
ms.sourcegitcommit: 29930f1593563c5e968b86117945c3452bdefac1
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/23/2020
ms.locfileid: "95485650"
---
# <a name="4-configure-a-custom-startup-file-for-python-apps-on-azure-app-service"></a>4: Configurare un file di avvio personalizzato per le app Python nel Servizio app di Azure

[Passaggio precedente: Creare il servizio app](tutorial-deploy-app-service-on-linux-03.md)

In questo passaggio si configura un file di avvio personalizzato, se necessario, per un'app Python in un servizio app di Azure.

Il file di avvio personalizzato è necessario nei casi seguenti:

- Si vuole avviare il server Web Gunicorn con argomenti aggiuntivi diversi dalle impostazioni predefinite, ovvero `--bind=0.0.0.0 --timeout 600`.

- L'app è compilata con un framework diverso da Flask o Django oppure si vuole usare un server Web diverso.

- È presente un'app Flask il cui file di codice principale ha un nome **diverso** da *app.py* o *application.py** oppure l'oggetto app ha un nome **diverso** da `app`.

    In altre parole, a meno che non sia presente un file *app.py* o *application.py* nella cartella radice del progetto *e* il nome dell'oggetto app Flask non sia `app`, è necessario un comando di avvio personalizzato.

    Ad esempio, il file principale nell'esempio di codice del [passaggio 2](tutorial-deploy-app-service-on-linux-02.md) è denominato *hello.py* e l'oggetto app è denominato `myapp`, quindi è necessario un file di avvio personalizzato, come illustrato in questo articolo.

Per altre informazioni, vedere [Configurare le app Python: processo di avvio del contenitore](/azure/app-service/configure-language-python#container-startup-process).

## <a name="create-a-startup-file"></a>Creare un file di avvio

Se è necessario un file di avvio personalizzato, seguire questa procedura:

1. Creare nel progetto un file denominato *startup.txt*, *startup.sh* o un altro nome a scelta che contiene il comando o i comandi di avvio. Vedere le sezioni successive di questo articolo per informazioni specifiche su Django, Flask e altri framework.

    Se necessario, un file di avvio può includere più comandi.

1. Eseguire il commit del file nel repository del codice in modo che possa essere distribuito con il resto dell'app.

1. Nell'area **Azure: App Service** (Azure: Servizio app) espandere il servizio app, fare clic con il pulsante destro del mouse su **Application Settings** (Impostazioni applicazione) e selezionare **Open in Portal** (Apri nel portale):

    ![Comando Open in Portal (Apri nel portale) in Application Settings (Impostazioni applicazione) nell'area App Service (Servizio app)](media/deploy-azure/open-application-settings-in-portal-for-app-service.png)

1. Nel portale di Azure eseguire l'accesso se necessario e quindi nella pagina **Configurazione** selezionare **Impostazioni generali**, immettere il nome del file di avvio, ad esempio *startup.txt* o *startup.sh*, in **Impostazioni stack** > **Comando di avvio** e infine selezionare **Salva**.

    ![Impostazione del nome file in Comando di avvio nel portale di Azure](media/deploy-azure/enter-startup-file-for-app-service-in-the-azure-portal.png)

    > [!NOTE]
    > Invece di usare un file di comandi di avvio, è possibile inserire il comando di avvio stesso direttamente nel campo **Comando di avvio** del portale di Azure. L'uso di un file di comandi è tuttavia preferibile, perché consente di mantenere queste impostazioni di configurazione nel repository in cui è possibile controllare le modifiche e ridistribuirle in una diversa istanza del servizio app.

1. Il Servizio app viene riavviato quando si salvano le modifiche.

    Dal momento che il codice dell'app non è stato ancora distribuito, però, se si visita il sito a questo punto viene visualizzato un messaggio di errore di applicazione. Questo messaggio indica che il server Gunicorn è stato avviato ma non è riuscito a trovare l'app e quindi non risponde alle richieste HTTP. Il codice dell'app verrà distribuito nel passaggio successivo.

## <a name="django-startup-commands"></a>Comandi di avvio di Django

Per impostazione predefinita, il Servizio app individua automaticamente la cartella che contiene il file *wsgi.py* e avvia Gunicorn con il comando seguente:

```sh
# <module> is the folder that contains wsgi.py. If you need to use a subfolder,
# specify the parent of <module> using --chdir.
gunicorn --bind=0.0.0.0 --timeout 600 <module>.wsgi
```

Se si intende modificare uno degli argomenti Gunicorn, ad esempio usando `--timeout 1200`, creare un file di comando con tali modifiche.

## <a name="flask-startup-commands"></a>Comandi di avvio di Flask

Per impostazione predefinita, il contenitore del Servizio app in Linux presuppone che l'interfaccia WSGI chiamabile dell'app Flask sia denominata `app`, sia contenuta in un file denominato *application.py* o *app.py* e risieda nella cartella radice dell'app.

Se si usa una delle varianti seguenti, il comando di avvio personalizzato deve identificare la posizione dell'oggetto app nel formato *file:ogetto_app*:

- **Il nome file e/o il nome dell'oggetto app sono diversi**: ad esempio, se il nome del file di codice principale dell'app è *hello.py* e quello dell'oggetto app è `myapp`, il comando di avvio sarà il seguente:

    ```text
    gunicorn --bind=0.0.0.0 --timeout 600 hello:myapp
    ```

- **Il file di avvio si trova in una sottocartella**: ad esempio, se il nome del file di avvio è *myapp/website.py* e quello dell'oggetto app è `app`, usare l'argomento `--chdir` di Gunicorn per specificare la cartella e quindi assegnare un nome al file di avvio e all'oggetto app come di consueto:

    ```text
    gunicorn --bind=0.0.0.0 --timeout 600 --chdir myapp website:app
    ```

- **Il file di avvio si trova in un modulo**: nel codice [python-sample-vscode-flask-tutorial](https://github.com/Microsoft/python-sample-vscode-flask-tutorial) il file di avvio *webapp.py* è contenuto nella cartella *hello_app*, che è di per sé un modulo contenente un file *\_\_init\_\_.py*. Il nome dell'oggetto app è `app` e l'oggetto è definito in *\_\_init\_\_.py*, mentre *webapp.py* usa un'importazione relativa.

    In una situazione di questo tipo se si imposta `webapp:app` come destinazione di Gunicorn viene generato l'errore "Attempted relative import in non-package" (Tentativo di importazione relativa in un non pacchetto) e l'avvio dell'app non riesce.

    In questo caso creare un file shim semplice che importa l'oggetto app dal modulo e quindi fare in modo che Gunicorn avvii l'app usando lo shim. Il codice [python-sample-vscode-flask-tutorial](https://github.com/Microsoft/python-sample-vscode-flask-tutorial), ad esempio, contiene *startup.py* con il contenuto seguente:

    ```python
    # startup.py shim
    from hello_app.webapp import app
    ```

    Il comando di avvio è quindi il seguente:

    ```text
    gunicorn --bind=0.0.0.0 --timeout 600 startup:app
    ```


## <a name="startup-commands-for-other-frameworks-and-web-servers"></a>Comandi di avvio per altri framework e server Web

Django e Flask, unitamente al server Web gunicorn, sono installati per impostazione predefinita nel contenitore del servizio app che esegue le app Python.

Per usare un framework diverso da Django o Flask (ad esempio Falcon, FastAPI e così via) o per usare un server Web diverso, seguire questa procedura:

- Includere il framework e/o il server Web nel file *requirements.txt*.

- Nel comando di avvio identificare l'interfaccia WSGI chiamabile come descritto nella [sezione precedente relativa a Flask](#flask-startup-commands).

- Per avviare un server Web diverso da gunicorn, usare un comando `python -m` invece di richiamare direttamente il server. Ad esempio, il comando seguente avvia il server uvicorn, supponendo che l'interfaccia WSGI chiamabile sia denominata `app` e si trovi in *application.py*:

    ```sh
    python -m uvicorn application:app --host 0.0.0.0
    ```

    È necessario usare `python -m` perché i server Web installati tramite *requirements.txt* non vengono aggiunti all'ambiente globale Python e quindi non possono essere richiamati direttamente. Il comando `python -m` richiama il server dall'interno dell'ambiente virtuale corrente.

> [!div class="nextstepaction"]
> [Il file di avvio è stato configurato: procedere con il passaggio 5 >>>](tutorial-deploy-app-service-on-linux-05.md)

[Problemi? Segnalarli](https://aka.ms/FlaskVSCQuickstartHelp).
