---
title: 'Esercitazione: Creare e distribuire Funzioni di Azure serverless in Python con VS Code'
description: "Passaggio 1 dell'esercitazione: introduzione e prerequisiti."
ms.topic: conceptual
ms.date: 09/02/2019
ms.custom: seo-python-october2019
ms.openlocfilehash: 388c49767e08d4f86ad02439ece58610b7c2cf09
ms.sourcegitcommit: 68a4044b9fa3291c9e7e2f68ae0049328f9c01bb
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/10/2019
ms.locfileid: "74992542"
---
# <a name="tutorial-create-and-deploy-serverless-azure-functions-in-python-with-visual-studio-code"></a>Esercitazione: Creare e distribuire Funzioni di Azure serverless in Python con Visual Studio Code

In questo articolo si usano Visual Studio Code e l'estensione Funzioni di Azure per creare un endpoint HTTP serverless con Python e anche per aggiungere una connessione (o "binding") all'archiviazione.

Funzioni di Azure consente di eseguire il codice in un ambiente serverless senza dover effettuare il provisioning di una macchina virtuale o pubblicare un'app Web. L'estensione Funzioni di Azure per Visual Studio Code semplifica notevolmente il processo di utilizzo di Funzioni perché gestisce automaticamente numerosi problemi di configurazione.

Se si riscontrano problemi con uno qualsiasi dei passaggi descritti in questa esercitazione, è possibile segnalarli. Per inviare feedback, usare il pulsante **Si è verificato un problema** alla fine di ogni articolo.

## <a name="prerequisites"></a>Prerequisiti

- Una [sottoscrizione di Azure](#azure-subscription).
- [Visual Studio Code con l'estensione Funzioni di Azure](#visual-studio-code-python-and-the-azure-functions-extension).
- [Azure Functions Core Tools](#azure-functions-core-tools).

### <a name="azure-subscription"></a>Sottoscrizione di Azure

Se non si ha una sottoscrizione di Azure, [iscriversi](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-functions-extension&mktingSource=vscode-tutorial-functions-extension) subito per ottenere un account gratuito valido per 30 giorni con 200 USD in crediti di Azure per provare qualsiasi combinazione di servizi.

### <a name="visual-studio-code-python-and-the-azure-functions-extension"></a>Visual Studio Code, Python e l'estensione Funzioni di Azure

Installare il software seguente:

- Python 3.7 o Python 3.6 come richiesto da Funzioni di Azure. [Python 3.7.5](https://www.python.org/downloads/release/python-375/) e [Python 3.6.8](https://www.python.org/downloads/release/python-368/) sono le versioni compatibili più recenti.
- [Visual Studio Code](https://code.visualstudio.com/).
- [Estensione Python](https://marketplace.visualstudio.com/items?itemName=ms-python.python) come descritto nella sezione dei [prerequisiti dell'esercitazione su Python in Visual Studio Code](https://code.visualstudio.com/docs/python/python-tutorial).
- [Estensione Funzioni di Azure](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions). Per informazioni di carattere generale, visitare il [repository GitHub vscode-azurefunctions](https://github.com/Microsoft/vscode-azurefunctions).

> [!NOTE]
> L'estensione Funzioni di Azure è inclusa nel [pacchetto di estensioni Strumenti di Azure](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-node-azure-pack).

### <a name="azure-functions-core-tools"></a>Azure Functions Core Tools

Seguire le istruzioni per il sistema operativo riportate in [Usare Azure Functions Core Tools](/azure/azure-functions/functions-run-local#v2).

Gli strumenti sono scritti in .NET Core e per una corretta installazione del pacchetto Core Tools è consigliabile usare npm, l'utilità di gestione pacchetti di Node.js. È per questo motivo che è attualmente necessario installare .NET Core e Node.js anche per usare Funzioni di Azure in Python. È però possibile ignorare il requisito di .NET Core relativo all'uso di bundle di estensione, come descritto nella documentazione menzionata in precedenza. In qualunque caso è necessario installare questi componenti solo una volta, perché in seguito sarà Visual Studio Code a richiedere automaticamente l'installazione di eventuali aggiornamenti.

### <a name="sign-in-to-azure"></a>Accedere ad Azure

[!INCLUDE [azure-sign-in](includes/azure-sign-in.md)]

### <a name="verify-prerequisites"></a>Verificare i prerequisiti

Per verificare che siano installati tutti gli strumenti di Funzioni di Azure, aprire il riquadro comandi di Visual Studio Code (**F1**), selezionare il comando **Terminale: Crea nuovo terminale integrato** e all'apertura del terminale eseguire il comando `func`:

![Verificare i prerequisiti di Azure Functions Core Tools](media/tutorial-vs-code-serverless-python/check-azure-functions-tools-prerequisites-in-visual-studio-code.png)

L'output che inizia con il logo di Funzioni di Azure (per visualizzarlo scorrere l'output verso l'alto) indica che Azure Functions Core Tools è presente.

Se il comando `func` non viene riconosciuto, eseguire nuovamente `npm install -g azure-functions-core-tools` e verificare che l'installazione abbia esito positivo. Assicurarsi anche di usare l'opzione `-g` con il comando install; in caso contrario, npm installa il pacchetto solo nella cartella corrente.

Il comando `func` funziona tramite il file *func.cmd* che viene installato nella cartella globale di Node.js. Per vedere la posizione di questa cartella, eseguire `npm -l` ed esaminare il percorso alla fine dell'output.

> [!div class="nextstepaction"]
> [L'accesso ad Azure è stato effettuato](tutorial-vs-code-serverless-python-02.md)

[Si è verificato un problema](https://www.research.net/r/PWZWZ52?tutorial=vscode-functions-python&step=01-verify-prerequisites)
