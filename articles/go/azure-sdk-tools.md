---
title: Strumenti per gli sviluppatori che usano Azure SDK per Go
description: Strumenti per l'uso di Azure SDK per Go e dei servizi di Azure
ms.date: 09/05/2018
ms.topic: conceptual
ms.openlocfilehash: b08a578c3e5ceb7381cc1643b2967405b292376e
ms.sourcegitcommit: 31f6d047f244f1e447faed6d503afcbc529bd28c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80319438"
---
# <a name="tools-for-developers-using-the-azure-sdk-for-go"></a>Strumenti per gli sviluppatori che usano Azure SDK per Go

Per scrivere in modo efficiente codice di Go e assicurarsi che funzioni correttamente con i servizi di Azure sono disponibili alcuni strumenti consigliati.

## <a name="azure-cli"></a>Interfaccia della riga di comando di Azure

L'interfaccia della riga di comando di Azure permette la creazione e la configurazione delle risorse di Azure nelle sottoscrizioni. L'interfaccia della riga di comando può semplificare le operazioni iniziali per la creazione rapida di risorse comuni e condivise di Azure, per consentire agli sviluppatori di concentrarsi sull'utilizzo più complesso dei servizi. L'interfaccia della riga di comando è multipiattaforma e include funzionalità per query e filtri, che consentono di inviare tramite pipe l'output direttamente agli strumenti preferiti della riga di comando. L'interfaccia della riga di comando è disponibile per l'installazione nel sistema locale, come immagine Docker, o tramite [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).

> [!div class="nextstepaction"]
> [Installare l'interfaccia della riga di comando di Azure](/cli/azure/install-azure-cli)

## <a name="visual-studio-code"></a>Visual Studio Code

Visual Studio Code è un editor leggero che offre supporto per Go. Questa estensione offre funzionalità quali il completamento automatico, i modelli di `impl`, il refactoring e il debug. Visual Studio Code offre anche il supporto per l'accesso nell'editor al controllo del codice sorgente ed estensioni per l'uso dei servizi di Azure.

* [Installare Visual Studio Code](https://code.visualstudio.com/Download)
* [Ottenere l'estensione di Visual Studio Code per Go](https://code.visualstudio.com/docs/languages/go)
* [Ottenere l'estensione Visual Studio Code per strumenti di Azure](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-azureextensionpack)

## <a name="cicd-with-azure-devops-project"></a>CI/CD con il progetto DevOps di Azure

Le pipeline del progetto Azure DevOps consentono di configurare un sistema di integrazione continua per le applicazioni Go. Per eseguire la distribuzione e i test direttamente in Azure, è sufficiente avere un repository Git.

> [!div class="nextstepaction"]
> [Informazioni su come creare una pipeline CI/CD con i progetti DevOps di Azure](/azure/devops-project/azure-devops-project-go)

## <a name="dependency-management-with-dep"></a>Gestione delle dipendenze con dep

Azure SDK per Go usa il comando dep per la gestione delle dipendenze. Il comando dep consente di eseguire il pull dei requisiti del fornitore per l'applicazione Go, evitando conflitti di versione e assicurando il funzionamento corretto del progetto.

> [!div class="nextstepaction"]
> [Ottenere la gestione dipendenze di dep](https://github.com/golang/dep)
