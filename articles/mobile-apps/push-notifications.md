---
title: L'importanza delle notifiche push nelle app per dispositivi mobili con Visual Studio App Center e Hub di notifica di Azure
description: Informazioni sui servizi come Visual Studio App Center che è possibile usare per interagire con gli utenti delle applicazioni per dispositivi mobili.
author: codemillmatt
ms.assetid: 12bbb070-9b3c-4faf-8588-ccff02097224
ms.service: mobile-services
ms.topic: article
ms.date: 06/08/2020
ms.author: masoucou
ms.openlocfilehash: 6dab56319c04fbf7864094fbaf254621cac82b36
ms.sourcegitcommit: f02ff55cef47581303a217cf1d310879cd722237
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/09/2020
ms.locfileid: "84632443"
---
# <a name="engage-with-your-application-users-by-sending-push-notifications"></a>Inviare notifiche push per interagire con gli utenti dell'applicazione

Che si tratti di applicazioni consumer o aziendali, è auspicabile che gli utenti le usino attivamente. Spesso si vogliono condividere le ultime notizie, aggiornamenti dei videogiochi, offerte interessanti, aggiornamenti dei prodotti o altre informazioni pertinenti per gli utenti. Per aumentare il coinvolgimento degli utenti e stimolarli a continuare a usare l'applicazione, è necessario un modo per comunicare con loro in tempo reale.

Le notifiche push consentono di comunicare con gli utenti dell'applicazione in modo rapido ed efficiente. È possibile contattare gli utenti al momento giusto e notificare le informazioni desiderate, in genere in un elemento popup o in una finestra di dialogo in un dispositivo mobile, indipendentemente dalle attività in corso.

## <a name="value-of-push-notifications"></a>Vantaggi delle notifiche push

Le notifiche push forniscono valore a utenti e aziende.

Le aziende possono:

- Comunicare direttamente con gli utenti inviando messaggi al momento giusto sulle piattaforme supportate.
- Incrementare il coinvolgimento degli utenti inviando aggiornamenti e promemoria in tempo reale, incoraggiandoli a interagire con l'applicazione a intervalli regolari.
- Mantenere gli utenti e riprendere a interagire con quelli inattivi che hanno scaricato l'applicazione ma non la usano per invitarli a diventare di nuovo attivi.
- Aumentare i tassi di conversione analizzando il comportamento degli utenti, segmentandoli e sfruttando campagne basate su notifiche push per dispositivi mobili.
- Promuovere prodotti e servizi inviando messaggi push e aggiornamenti tempestivi agli utenti.
- Inviare messaggi push personalizzati a utenti mirati.

Per gli utenti dell'applicazione, le notifiche push:

- Forniscono valore e informazioni in tempo reale.
- Ricordano di usare l'applicazione.

Usare i servizi seguenti per abilitare le notifiche push nelle app per dispositivi mobili.

## <a name="azure-notification-hubs"></a>Hub di notifica di Azure

[Hub di notifica](/azure/notification-hubs/notification-hubs-push-notification-overview) offre un motore di push di facile uso, multipiattaforma e con scalabilità orizzontale. È possibile usarlo per inviare notifiche a qualsiasi piattaforma e da qualsiasi back-end nel cloud o in locale.

### <a name="azure-notification-hub-features"></a>Funzionalità di Hub di notifica di Azure

- Inviare notifiche push a qualsiasi piattaforma da qualunque back-end, nel cloud o in locale.
- Trasmettere rapidamente le notifiche push a milioni di dispositivi mobili con una singola chiamata API.
- Personalizzare le notifiche push in base a cliente, lingua e località.
- Definire e notificare dinamicamente i segmenti dei clienti.
- Raggiungere istantaneamente milioni di dispositivi mobili.
- Acquisire il supporto per piattaforme iOS, Android, Windows, Kindle e Baidu.

### <a name="azure-notification-hub-references"></a>Informazioni di riferimento per Hub di notifica di Azure

- [Azure portal](https://portal.azure.com) 
- [Introduzione ad Hub di notifica di Azure](/azure/notification-hubs/)
- [Guide introduttive](/azure/notification-hubs/create-notification-hub-portal)
- [Esempi](/azure/notification-hubs/samples)
