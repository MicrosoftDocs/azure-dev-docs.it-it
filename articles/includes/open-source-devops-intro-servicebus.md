---
author: tomarchermsft
ms.service: ansible
ms.topic: include
ms.date: 04/22/2019
ms.author: tarcher
ms.openlocfilehash: eb96027351cf244e9cd4404f702544411130db5e
ms.sourcegitcommit: eabc9e3fb8ad0f067be5ed878c2eacebd461b6ce
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/22/2020
ms.locfileid: "81743291"
---
Il [bus di servizio di Azure](/azure/service-bus-messaging/service-bus-messaging-overview) è un broker messaggi di [integrazione](https://azure.microsoft.com/product-categories/integration/) aziendale. Il bus di servizio supporta due tipi di comunicazione, code e argomenti. 

Le code supportano le comunicazioni asincrone tra applicazioni. Un'applicazione invia messaggi a una coda, che archivia i messaggi. L'applicazione ricevente si connette a e legge i messaggi dalla coda.

Gli argomenti supportano il modello di pubblicazione-sottoscrizione, che permette una relazione uno-a-molti tra il mittente del messaggio e uno o più destinatari del messaggio.