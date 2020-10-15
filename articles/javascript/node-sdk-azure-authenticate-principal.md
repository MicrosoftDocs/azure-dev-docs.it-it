---
title: Creare un'entità servizio di Azure con Node.js
description: Informazioni su come usare l'autenticazione basata su entità servizio in Azure con Node.js e JavaScript
ms.topic: how-to
ms.date: 09/30/2020
ms.custom: devx-track-js
ms.openlocfilehash: 4384582d0c24209927bef971a1bc76d8322c18c0
ms.sourcegitcommit: 0b1c751c5a4a837977fec1c777bca5ad15cf2fc7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/01/2020
ms.locfileid: "91621657"
---
# <a name="create-an-azure-service-principal-for-nodejs"></a>Creare un'entità servizio di Azure per Node.js

Quando un'app deve accedere alle risorse, è possibile configurare un'identità per l'app ed eseguirne l'autenticazione con credenziali specifiche. Questa identità è nota come *entità servizio*. Essenzialmente, si creano le chiavi per l'account Azure Active Directory da fornire all'SDK per l'autenticazione invece di richiedere l'intervento dell'utente o il nome utente e la password.

L'approccio basato sull'entità servizio consente di:
- Assegnare all'identità dell'app autorizzazioni diverse rispetto a quelle dell'utente. Tali autorizzazioni sono in genere limitate alle specifiche operazioni che devono essere eseguite dall'app.
- Usare un certificato per l'autenticazione in caso di esecuzione di uno script automatico.

Questo argomento illustra tre tecniche per creare un'entità servizio.

- Portale di Azure
- Interfaccia della riga di comando di Azure 2.0

[!INCLUDE [chrome-note](includes/chrome-note.md)]

## <a name="create-a-service-principal-using-the-azure-portal"></a>Creare un'entità servizio usando il portale di Azure

Seguire i passaggi illustrati nell'argomento [Usare il portale per creare un'applicazione Azure Active Directory e un'entità servizio che possano accedere alle risorse](/azure/active-directory/develop/howto-create-service-principal-portal) per generare l'entità servizio.

## <a name="create-a-service-principal-using-the-azure-cli-20"></a>Creare un'entità servizio usando l'interfaccia della riga di comando di Azure 2.0

Per creare un'entità servizio usando l'[interfaccia della riga di comando di Azure 2.0](/cli/azure/install-az-cli2), seguire questa procedura:

1. Scaricare l'[interfaccia della riga di comando di Azure 2.0](/cli/azure/install-az-cli2).

2. Aprire una finestra del terminale e digitare il comando `az login` per avviare il processo di accesso.

3. La chiamata a `az login` restituisce un URL e un codice. Passare all'URL specificato, immettere il codice e accedere con l'identità di Azure. Questa operazione potrebbe venire eseguita automaticamente se l'accesso è già stato eseguito. Sarà quindi possibile accedere all'account tramite l'interfaccia della riga di comando.

4. Usando il comando `az account list`, ottenere l'ID della sottoscrizione e del tenant che saranno necessari per lavorare con i pacchetti di Azure. Il testo seguente è un esempio di output di questo comando:

    ```shell
    {
    "cloudName": "AzureCloud",
    "id": "<subscriptionId>",
    "isDefault": true,
    "name": "<subscriptionName>",
    "registeredProviders": [],
    "state": "Enabled",
    "tenantId": "<tenantId>",
        "user": {
            "name": "hello@example.com",
            "type": "user"
        }
    }
    ```

5. Seguire la procedura illustrata nell'argomento [Creare un'entità servizio di Azure con l'interfaccia della riga di comando di Azure](/cli/azure/create-an-azure-service-principal-azure-cli) per generare l'entità servizio. L'oggetto JSON nell'output conterrà le informazioni necessarie per eseguire l'autenticazione con Azure.


## <a name="using-the-service-principal"></a>Uso dell'entità servizio

Dopo aver creato un'entità servizio, vedere l'argomento [Eseguire l'autenticazione con i moduli di gestione di Azure per JavaScript](./node-sdk-azure-authenticate.md) per informazioni su come creare un oggetto credenziali utilizzabile per autenticare il client con Azure Active Directory.
