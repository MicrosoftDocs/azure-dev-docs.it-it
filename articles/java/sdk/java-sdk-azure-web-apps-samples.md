---
title: Esempi di librerie di gestione di Azure per app Web Java
description: Ottenere codice di esempio per la creazione e l'aggiornamento di app Web di Azure ospitate nel servizio app tramite le librerie di gestione di Azure per Java
keywords: Azure, Java, SDK, API, Maven, Gradle, app Web, servizio app
author: rloutlaw
ms.date: 04/16/2017
ms.topic: article
ms.service: multiple
ms.assetid: 43633e5c-9fb1-4807-ba63-e24c126754e2
ms.custom: seo-java-august2019, seo-java-september2019, devx-track-java
ms.openlocfilehash: 585cb33b6eabc5e353ddda37f687bcf0bacc6901
ms.sourcegitcommit: c6642cae6fdb5e3025ed66fcd4ef89792c3b436a
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/15/2020
ms.locfileid: "86405702"
---
# <a name="azure-management-libraries-for-java---web-app-samples"></a>Librerie di gestione di Azure per Java - Esempi per app Web 

La tabella seguente offre collegamenti a codice sorgente Java che è possibile usare per creare e configurare app Web.

| Esempio | Descrizione |
|---|---|
| **Creare un'app** ||
| [Creare un'app Web e distribuirla da FTP o GitHub][1] | Distribuire app Web da Git locale, FTP e tramite l'integrazione continua da GitHub. |
| [Creare un'app Web e gestire gli slot di distribuzione][2] | Creare un'app Web e distribuirla negli slot di staging, quindi scambiare le distribuzioni tra gli slot. |
| **Configurare un'app** ||
| [Creare un'app Web e configurare un dominio personalizzato][3] | Creare un'app Web con un dominio personalizzato e un certificato SSL autofirmato. |
| **Dimensionare un'app** ||
| [Ridimensionare un'app Web a disponibilità elevata in più aree][4] | Ridimensionare un'app Web in tre aree geografiche diverse e renderle disponibili tramite un singolo endpoint con Gestione traffico di Azure. | 
| **Connettere un'app alle risorse** ||
| [Connettere un'App Web a un account di archiviazione][5] | Creare un account di archiviazione di Azure e aggiungere la stringa di connessione dell'account di archiviazione alle impostazioni dell'app. |
| [Connettere un'app Web a un database SQL][6] | Creare un'app Web e un database SQL e quindi aggiungere la stringa di connessione del database SQL alle impostazioni dell'app. |

[1]: java-sdk-configure-webapp-sources.md
[2]: https://github.com/Azure-Samples/app-service-java-manage-staging-and-production-slots-for-web-apps/
[3]: https://github.com/Azure-Samples/app-service-java-manage-web-apps-with-custom-domains/
[4]: https://azure.microsoft.com/resources/samples/app-service-java-scale-web-apps-on-linux/
[5]: https://github.com/Azure-Samples/app-service-java-manage-storage-connections-for-web-apps/
[6]: https://github.com/Azure-Samples/app-service-java-manage-data-connections-for-web-apps/