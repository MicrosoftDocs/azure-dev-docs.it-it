---
author: mikeparker104
ms.author: miparker
ms.date: 07/27/2020
ms.service: mobile-services
ms.topic: include
ms.openlocfilehash: 20a8fa10a27a41621c2b08839682fefbd023ee2e
ms.sourcegitcommit: cf23d382eee2431a3958b1c87c897b270587bde0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/29/2020
ms.locfileid: "87401599"
---
Un back-end dell'[API Web ASP.NET Core](https://dotnet.microsoft.com/apps/aspnet/apis)viene usato per gestire la [registrazione del dispositivo](https://docs.microsoft.com/azure/notification-hubs/notification-hubs-push-notification-registration-management#what-is-device-registration) per il client usando il metodo di [installazione](https://docs.microsoft.com/azure/notification-hubs/notification-hubs-push-notification-registration-management#installations) più recente e migliore. Il servizio invierà inoltre notifiche push in modalità multipiattaforma. 

Queste operazioni vengono gestite tramite l'[SDK di Hub di notifica per le operazioni back-end](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/). Per altre informazioni sull'approccio generale, vedere [Registrazione dal back-end dell'app](https://docs.microsoft.com/azure/notification-hubs/notification-hubs-push-notification-registration-management#registration-management-from-a-backend).
