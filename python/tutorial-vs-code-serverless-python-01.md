---
title: Creare e distribuire Funzioni di Azure serverless in Python con Visual Studio Code
description: "Passaggio 1 dell'esercitazione: introduzione e prerequisiti."
services: functions
author: kraigb
manager: barbkess
ms.service: azure-functions
ms.topic: conceptual
ms.date: 09/02/2019
ms.author: kraigb
ms.openlocfilehash: bf6dd180d5fabc04c8fe3bf09c8277bb7b22ea47
ms.sourcegitcommit: 86aec15e2d25b5b706092feb4271fb958c29d5fa
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/01/2019
ms.locfileid: "71710202"
---
# <a name="deploy-python-to-azure-functions-with-visual-studio-code"></a>Distribuire Python in Funzioni di Azure con Visual Studio Code

In questa esercitazione si usano Visual Studio Code e l'estensione Funzioni di Azure per creare un endpoint HTTP serverless con Python e aggiungere anche una connessione (o "binding") all'archiviazione. Funzioni di Azure consente di eseguire il codice in un ambiente serverless senza dover effettuare il provisioning di una macchina virtuale o pubblicare un'app Web. L'estensione Funzioni di Azure per Visual Studio Code semplifica notevolmente il processo di utilizzo di Funzioni perché gestisce automaticamente numerosi problemi di configurazione.

Se si riscontrano problemi con uno qualsiasi dei passaggi descritti in questa esercitazione, è possibile segnalarli. Per inviare feedback, usare il pulsante **Si è verificato un problema** alla fine di ogni articolo.

## <a name="prerequisites"></a>Prerequisiti

- Una [sottoscrizione di Azure](#azure-subscription).
- [Visual Studio Code con l'estensione Funzioni di Azure](#visual-studio-code-python-and-the-azure-functions-extension).
- [Azure Functions Core Tools](#azure-functions-core-tools).

### <a name="azure-subscription"></a>Sottoscrizione di Azure

Se non si ha una sottoscrizione di Azure, [iscriversi](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-functions-extension&mktingSource=vscode-tutorial-functions-extension) subito per ottenere un account gratuito valido per 30 giorni con 200 USD in crediti di Azure per provare qualsiasi combinazione di servizi.

### <a name="visual-studio-code-python-and-the-azure-functions-extension"></a>Visual Studio Code, Python e l'estensione Funzioni di Azure

Installare il software seguente:

- Python 3.6. x come richiesto da Funzioni di Azure. [Python 3.6.8](https://www.python.org/downloads/release/python-368/) è la versione 3.6. x più recente.
- [Visual Studio Code](https://code.visualstudio.com/).
- [Estensione Python](https://marketplace.visualstudio.com/items?itemName=ms-python.python) come descritto nella sezione dei [prerequisiti dell'esercitazione su Python in Visual Studio Code](https://code.visualstudio.com/docs/python/python-tutorial).
- [Estensione Funzioni di Azure](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions). Per informazioni di carattere generale, visitare il [repository GitHub vscode-azurefunctions](https://github.com/Microsoft/vscode-azurefunctions).

> [!NOTE]
> L'estensione Funzioni di Azure è inclusa nel [pacchetto di estensioni Strumenti di Azure](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-node-azure-pack).

### <a name="azure-functions-core-tools"></a>Azure Functions Core Tools

Seguire le istruzioni per il sistema operativo riportate in [Usare Azure Functions Core Tools](/azure/azure-functions/functions-run-local#v2). Gli strumenti stessi sono scritti in .NET Core e per una corretta installazione del pacchetto Core Tools è consigliabile usare npm, l'utilità di gestione pacchetti di Node.js. È per questo motivo che è attualmente necessario installare .NET Core e Node.js persino per il codice Python. È però possibile ignorare il requisito di .NET Core relativo all'uso di bundle di estensione, come descritto nella documentazione menzionata in precedenza. In qualunque caso è necessario installare questi componenti solo una volta, perché in seguito sarà Visual Studio Code a richiedere automaticamente l'installazione di eventuali aggiornamenti.

### <a name="sign-in-to-azure"></a>Accedere ad Azure

[!INCLUDE [azure-sign-in](includes/azure-sign-in.md)]

### <a name="verify-prerequisites"></a>Verificare i prerequisiti

Per verificare che siano installati tutti gli strumenti di Funzioni di Azure, aprire il riquadro comandi di Visual Studio Code (**F1**), selezionare il comando **Terminale: Crea nuovo terminale integrato** e all'apertura del terminale eseguire il comando `func`:

![Verifica dei prerequisiti di Azure Functions Core Tools](media/tutorial-vs-code-serverless-python/check-prereqs.png)

L'output che inizia con il logo di Funzioni di Azure (per visualizzarlo scorrere l'output verso l'alto) indica che Azure Functions Core Tools è presente.

Se il comando `func` non viene riconosciuto, verificare che la cartella in cui è stato installato Azure Functions Core Tools sia inclusa nella variabile di ambiente PATH.

> [!div class="nextstepaction"]
> [È stato eseguito l'accesso ad Azure](tutorial-vs-code-serverless-python-02.md)

[Si è verificato un problema](https://www.research.net/r/PWZWZ52?tutorial=vscode-functions-python&step=01-verify-prerequisites)
