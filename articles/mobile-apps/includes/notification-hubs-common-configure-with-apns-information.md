---
author: mikeparker104
ms.author: miparker
ms.date: 07/27/2020
ms.service: mobile-services
ms.topic: include
ms.openlocfilehash: 6d9f12534c8bd5ab4fac8a1e337196fcc7ad559e
ms.sourcegitcommit: cf23d382eee2431a3958b1c87c897b270587bde0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/29/2020
ms.locfileid: "87401669"
---
### <a name="configure-your-notification-hub-with-apns-information"></a>Configurare l'hub di notifica con le informazioni del servizio APN

In **Notification Services** (Servizi di notifica) selezionare **Apple** e quindi seguire i passaggi appropriati in base all'approccio scelto in precedenza nella sezione [Creazione di un certificato per Hub di notifica](#creating-a-certificate-for-notification-hubs).  

> [!NOTE]
> Usare la modalità **Production** (Produzione) per **Application Mode** (Modalità applicazione) solo se si vuole inviare notifiche push agli utenti che hanno acquistato l'app dallo Store.

### <a name="option-1-using-a-p12-push-certificate"></a>OPZIONE 1: Uso di un certificato push con estensione .p12

1. Selezionare **Certificate**.

1. Selezionare l'icona del file.

1. Selezionare il file con estensione p12 esportato in precedenza e quindi selezionare **Open** (Apri).

1. Se necessario, specificare la password corretta.

1. Selezionare la modalità **Sandbox**.

1. Selezionare **Salva**.

### <a name="option-2-using-token-based-authentication"></a>OPZIONE 2: Uso dell'autenticazione basata su token

1. Selezionare **Token**.
1. Immettere i valori seguenti acquisiti in precedenza:

    - **Key ID** (ID chiave)
    - **Bundle ID** (ID bundle)
    - **Team ID** (ID team)
    - **Token**

1. Scegliere **Sandbox**
1. Selezionare **Salva**.
