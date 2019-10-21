---
title: 'Esercitazione: Configurare un file di avvio personalizzato per le app Python nel Servizio app di Azure in Linux'
description: "Passaggio 4 dell'esercitazione: indicare al servizio app come avviare l'app Web."
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/12/2019
ms.author: kraigb
ms.custom: seo-python-october2019
ms.openlocfilehash: 7c3c863ed333528c675cda939f52b86f53bc8380
ms.sourcegitcommit: 6012460ad8d6ff112226b8f9ea6da397ef77712d
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/11/2019
ms.locfileid: "72278935"
---
# <a name="tutorial-configure-a-custom-startup-file-for-python-apps-on-azure-app-service"></a>Esercitazione: Configurare un file di avvio personalizzato per le app Python nel Servizio app di Azure

[Passaggio precedente: Creare il servizio app](tutorial-deploy-app-service-on-linux-03.md)

Questo articolo illustra come configurare un file di avvio personalizzato per un'app Python in un servizio app di Azure.

A seconda di come è stata strutturata l'app, può essere necessario creare un file di comando di avvio personalizzato per l'app, come descritto in [Configurare le app Python per il Servizio app in Linux](https://docs.microsoft.com/azure/app-service/containers/how-to-configure-python) nella documentazione di Azure.

I casi d'uso specifici di un comando di avvio personalizzato sono i seguenti:

- Si usa un'app **Flask** in cui il nome del file di avvio e dell'oggetto app è **diverso** da *application.py* e `app`. In altre parole, a meno che non sia presente un file *application.py* nella cartella radice del progetto *e* il nome dell'oggetto app Flask non sia `app`, è necessario un comando di avvio personalizzato.
- Si vuole avviare il server Web Gunicorn con argomenti aggiuntivi diversi dalle impostazioni predefinite, ovvero `--bind=0.0.0.0 --timeout 600`.

## <a name="create-a-startup-file"></a>Creare un file di avvio

Se è necessario un file di avvio personalizzato, attenersi alla procedura seguente:

1. Creare nel progetto un file denominato *startup.txt* (o un altro nome a scelta) che contiene il comando di avvio. Per Flask, vedere [Comandi di avvio di Flask](#flask-startup-commands) nella sezione successiva. Le app Django non richiedono in genere alcuna personalizzazione.

1. Eseguire il commit del file nel repository del codice in modo che possa essere distribuito con il resto dell'app.

1. Nell'area **Azure: App Service** (Azure: Servizio app) espandere il servizio app, fare clic con il pulsante destro del mouse su **Application Settings** (Impostazioni applicazione) e selezionare **Open in Portal** (Apri nel portale):

    ![Comando Open in Portal (Apri nel portale) in Application Settings (Impostazioni applicazione) nell'area App Service (Servizio app)](media/deploy-azure/open-application-settings-in-portal-for-app-service.png)

1. Nel portale di Azure eseguire l'accesso se necessario e quindi nella pagina **Configurazione** selezionare **Impostazioni generali**, immettere il nome del file di avvio, ad esempio *startup.txt*, in **Impostazioni stack** >  **Comando di avvio** e infine selezionare **Salva**.

    ![Impostazione del nome file in Comando di avvio nel portale di Azure](media/deploy-azure/enter-startup-file-for-app-service-in-the-azure-portal.png)

    > [!NOTE]
    > Invece di usare un file di comando di avvio, è anche possibile inserire il comando di avvio direttamente nel campo **Comando di avvio** nel portale di Azure. L'uso di un file è tuttavia preferibile in genere, perché consente di mantenere queste impostazioni di configurazione nel repository in cui è possibile controllare le modifiche e ridistribuirle in una diversa istanza del Servizio app.

1. Il Servizio app viene riavviato quando si salvano le modifiche. Dal momento che il codice dell'app non è stato ancora distribuito, però, se si visita il sito a questo punto viene visualizzato un messaggio di errore di applicazione. Questo messaggio indica che il server Gunicorn è stato avviato ma non è riuscito a trovare l'app e quindi non risponde alle richieste HTTP. Il codice dell'app verrà distribuito nel passaggio successivo.

## <a name="django-startup-commands"></a>Comandi di avvio di Django

Per impostazione predefinita, il Servizio app individua automaticamente la cartella che contiene il file *wsgi.py* e avvia Gunicorn con il comando seguente:

```bash
# <module> is the path to the folder that contains wsgi.py
gunicorn --bind=0.0.0.0 --timeout 600 <module>.wsgi
```

Se si intende modificare uno degli argomenti Gunicorn, ad esempio usando `--timeout 1200`, creare un file di comando con tali modifiche.

## <a name="flask-startup-commands"></a>Comandi di avvio di Flask

Per impostazione predefinita, il contenitore del Servizio app in Linux presuppone che il file di avvio di un'app Flask sia denominato *application.py* e che risieda nella cartella radice dell'app. Presuppone inoltre che l'oggetto app Flask definito all'interno del file sia denominato `app`. Se l'app non è strutturata esattamente in questo modo, il comando di avvio personalizzato deve identificare il percorso dell'oggetto app:

1. **Il nome file e/o il nome dell'oggetto app sono diversi**: ad esempio, se il nome del file di avvio dell'app è *hello.py* e quello dell'oggetto app è `myapp`, il comando di avvio è il seguente:

    ```text
    gunicorn --bind=0.0.0.0 --timeout 600 hello:myapp
    ```

1. **Il file di avvio si trova in una sottocartella**: ad esempio, se il nome del file di avvio è *myapp/website.py* e quello dell'oggetto app è `app`, usare l'argomento `--chdir` di Gunicorn per specificare la cartella e quindi assegnare un nome al file di avvio e all'oggetto app come di consueto:

    ```text
    gunicorn --bind=0.0.0.0 --timeout 600 --chdir myapp website:app
    ```

1. **Il file di avvio si trova in un modulo**: nel codice [python-sample-vscode-flask-tutorial](https://github.com/Microsoft/python-sample-vscode-flask-tutorial) il file di avvio *webapp.py* è contenuto nella cartella *hello_app*, che è di per sé un modulo contenente un file *\_\_init\_\_.py*. Il nome dell'oggetto app è `app` e l'oggetto è definito in *\_\_init\_\_.py*, mentre *webapp.py* usa un'importazione relativa. In una situazione di questo tipo se si imposta `webapp:app` come destinazione di Gunicorn viene generato l'errore "Attempted relative import in non-package" (Tentativo di importazione relativa in un non pacchetto) e l'avvio dell'app non riesce.

    In questo caso creare un file shim semplice che importa l'oggetto app dal modulo e quindi fare in modo che Gunicorn avvii l'app usando lo shim. Il codice [python-sample-vscode-flask-tutorial](https://github.com/Microsoft/python-sample-vscode-flask-tutorial), ad esempio, contiene *startup.py* con il contenuto seguente:

    ```python
    # startup.py shim
    from hello_app.webapp import app
    ```

    Il comando di avvio è quindi il seguente:

    ```text
    gunicorn --bind=0.0.0.0 --timeout 600 startup:app
    ```

> [!div class="nextstepaction"]
> [Il file di avvio è stato configurato](tutorial-deploy-app-service-on-linux-05.md)

[Si è verificato un problema](https://www.research.net/r/PWZWZ52?tutorial=vscode-appservice-python&step=04-startup-command)