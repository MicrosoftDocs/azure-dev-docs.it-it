---
author: mikeparker104
ms.author: miparker
ms.date: 07/27/2020
ms.service: mobile-services
ms.topic: include
ms.openlocfilehash: 98e39efca4ddaacb0e46bf5e42bd2df496529beb
ms.sourcegitcommit: e97cb81a245ce7dcabeac3260abc3db7c30edd79
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/29/2020
ms.locfileid: "91493154"
---
Un back-end dell'[API Web ASP.NET Core](https://dotnet.microsoft.com/apps/aspnet/apis)viene usato per gestire la [registrazione del dispositivo](/azure/notification-hubs/notification-hubs-push-notification-registration-management#what-is-device-registration) per il client usando il metodo di [installazione](/azure/notification-hubs/notification-hubs-push-notification-registration-management#installations) più recente e migliore. Il servizio invierà inoltre notifiche push in modalità multipiattaforma. 

Queste operazioni vengono gestite tramite l'[SDK di Hub di notifica per le operazioni back-end](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/). Per altre informazioni sull'approccio generale, vedere [Registrazione dal back-end dell'app](/azure/notification-hubs/notification-hubs-push-notification-registration-management#registration-management-from-a-backend).