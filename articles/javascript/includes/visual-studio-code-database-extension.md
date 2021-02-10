---
ms.custom: devx-track-js
ms.topic: include
ms.date: 02/08/2021
ms.openlocfilehash: 693e78bef335871dbee846666f889ad1cf2982b1
ms.sourcegitcommit: 98a7e855206ff463c1d95f93c23dd665b26a0aa1
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/09/2021
ms.locfileid: "100006264"
---
## <a name="create-azure-database-with-visual-studio-code-extension"></a>Creare database di Azure con estensione Visual Studio Code

Usare questa procedura per i tipi di risorse seguenti:

* API Azure Cosmos DB per MongoDB
* SQL
* Tabella di Azure
* Gremlin
* [PostgreSQL](#create-a-postgresql-database) 

### <a name="create-a-postgresql-database"></a>Creare un database PostgreSQL

1. Installare l'estensione [database di Azure](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb) per Visual Studio Code.
1. In Visual Studio Code selezionare **Azure** dalla [barra attività](https://code.visualstudio.com/docs/getstarted/userinterface), quindi selezionare **database** nella barra [laterale](https://code.visualstudio.com/docs/getstarted/userinterface).
1. Fare clic con il pulsante destro del mouse sul nome della sottoscrizione, quindi scegliere **Crea server**.
1. Selezionare **PostgreSQL** dall'elenco. 

    :::image type="content" source="../media/howto-visual-studio-code/create-azure-database-server.png" alt-text="Selezionare ' PostgreSQL ' nell'elenco.":::

1. Immettere un nome per il server PostgreSQL. Questo nome viene utilizzato come parte della stringa di connessione. 
1. Immettere un nome utente amministratore. 
1. Immettere una password di amministratore e quindi immetterla una seconda volta nella schermata successiva per confermare. 
1. Selezionare l'indirizzo IP corrente da aggiungere come regola del firewall. 
1. Selezionare un nome di gruppo di risorse o crearne uno nuovo. 
1. Selezionare un'area di Azure per il server. 
1. Nella finestra di notifica viene visualizzato lo stato. 

    :::image type="content" source="../media/howto-visual-studio-code/create-azure-database-server-status.png" alt-text="Nella finestra di notifica viene visualizzato lo stato.":::