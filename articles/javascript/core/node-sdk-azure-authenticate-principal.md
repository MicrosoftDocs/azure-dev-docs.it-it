---
title: Creare un'entità servizio di Azure con Node.js
description: Informazioni su come usare l'autenticazione basata su entità servizio in Azure con Node.js e JavaScript
ms.topic: how-to
ms.date: 11/05/2020
ms.custom: devx-track-js
ms.openlocfilehash: e7f885b0af5e7a25d0e706c235a1521bb44c4b36
ms.sourcegitcommit: 12f80b1e0fe08db707c198271d0c399c3aba343a
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/11/2020
ms.locfileid: "94515172"
---
# <a name="create-an-azure-service-principal-for-nodejs"></a>Creare un'entità servizio di Azure per Node.js

Quando un'app deve accedere alle risorse, è possibile configurare un'identità per l'app ed eseguirne l'autenticazione con credenziali specifiche. Questa identità è nota come *entità servizio*. Essenzialmente, si creano le chiavi per l'account Azure Active Directory da fornire all'SDK per l'autenticazione invece di richiedere l'intervento dell'utente o il nome utente e la password.

L'approccio basato sull'entità servizio consente di:
- Assegnare all'identità dell'app autorizzazioni diverse rispetto a quelle dell'utente. Tali autorizzazioni sono in genere limitate alle specifiche operazioni che devono essere eseguite dall'app.
- Usare un certificato per l'autenticazione in caso di esecuzione di uno script automatico.

Questo argomento illustra tre tecniche per creare un'entità servizio.

- Portale di Azure
- Interfaccia della riga di comando di Azure 2.0

[!INCLUDE [chrome-note](../includes/chrome-note.md)]

## <a name="create-a-service-principal-using-the-azure-cli-20"></a>Creare un'entità servizio usando l'interfaccia della riga di comando di Azure 2.0

Per eseguire i passaggi seguenti, [installare l'interfaccia della riga di comando di Azure](/cli/azure/install-azure-cli) e [accedere ad Azure](/cli/azure/authenticate-azure-cli). 

1. Usando il comando `az account list`, ottenere l'ID della sottoscrizione e del tenant che saranno necessari per lavorare con i pacchetti di Azure. Il testo seguente è un esempio di output di questo comando:

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

1. Seguire la procedura illustrata nell'argomento [Creare un'entità servizio di Azure con l'interfaccia della riga di comando di Azure](/cli/azure/create-an-azure-service-principal-azure-cli) per generare l'entità servizio. L'oggetto JSON nell'output conterrà le informazioni necessarie per eseguire l'autenticazione con Azure.

## <a name="using-the-service-principal"></a>Uso dell'entità servizio

Con un'entità servizio è possibile:

1. Eseguire l'autenticazione ad Azure a livello di codice con l'entità servizio usando un certificato, variabili di ambiente o un file `.json`. 
1. Creare risorse di Azure con l'entità servizio e usare il servizio.

Vedere l'argomento [Eseguire l'autenticazione con i moduli di gestione di Azure per JavaScript](./node-sdk-azure-authenticate.md) per informazioni su come creare un oggetto credenziali da usare per autenticare il client con Azure Active Directory.
