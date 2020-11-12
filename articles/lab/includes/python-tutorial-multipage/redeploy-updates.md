---
title: file di inclusione azure-sign-in.md
description: file di inclusione azure-sign-in.md
ms.date: 10/13/2020
ms.topic: include
ms.custom: devx-track-javascript
ms.openlocfilehash: e500a760430d92fa6640d964320a3ef3f161b7cc
ms.sourcegitcommit: 5f64710b2b0822e789c7f15acba5a3a257c033f9
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/05/2020
ms.locfileid: "93405239"
---
In questa sezione apportare una piccola modifica al codice e quindi ridistribuirlo in Azure. La modifica del codice include un'istruzione `print` per generare l'output di registrazione da usare nella sezione successiva.

Aprire *app.py* in un editor e aggiornare la funzione `hello` in modo che corrisponda al codice seguente. 

```python
def hello():
    print("Handling request to home page.")
    return "Hello, Azure!"
```

    
Salvare le modifiche, quindi ridistribuire l'app usando di nuovo il comando `az webapp up`:

```azurecli
az webapp up
```

Questo comando usa i valori memorizzati nella cache in locale nel file *.azure/config* , inclusi il nome dell'app, il gruppo di risorse e il piano di servizio app.

Al termine della distribuzione, tornare alla finestra del browser aperta su `http://<app-name>.azurewebsites.net`. Aggiornare la pagina, che dovrebbe visualizzare il messaggio modificato:

![Eseguire un'app Python di esempio aggiornata in Azure](../../media/quickstart-python/run-updated-hello-world-sample-python-app-in-browser.png)

> [!TIP]
> Visual Studio Code offre estensioni potenti per Python e il Servizio app di Azure, al fine di semplificare il processo di distribuzione delle app Web Python nel Servizio app. Per altre informazioni, vedere [Distribuire app Python nel Servizio app di Azure in Linux da Visual Studio Code](/azure/python/tutorial-deploy-app-service-on-linux-01).