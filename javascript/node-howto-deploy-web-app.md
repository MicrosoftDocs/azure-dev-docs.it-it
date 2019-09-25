---
title: Distribuire app Web Node.js in Azure
description: Introduzione a Servizio app di Azure e ad altre opzioni di hosting per app Web, incluse le app Web progressive
author: kraigb
manager: barbkess
ms.devlang: nodejs
ms.topic: article
ms.service: azure-nodejs
ms.date: 08/20/2019
ms.author: kraigb
ms.openlocfilehash: c58cd1ed3480fae88d9839b44f0f4062c825a068
ms.sourcegitcommit: f519a1ee8017850b2fa37049af3bac1ea5ca5516
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/21/2019
ms.locfileid: "69892345"
---
# <a name="how-to-deploy-web-apps-to-azure"></a>Come distribuire app Web in Azure

Sono disponibili diverse opzioni per l'hosting di app Web in Azure:

- L'opzione di hosting migliore per le app Web è Servizio app di Azure, un'offerta di piattaforma distribuita come servizio (PaaS). Per iniziare, provare una delle risorse seguenti:

  - [Creare un'app Web Node.js in Azure](/azure/app-service/app-service-web-get-started-nodejs)
  - [Provare Servizio app di Azure - Creare un'app Express da un modello](https://code.visualstudio.com/tryappservice/?utm_source=msftdocs&utm_medium=microsoft&utm_campaign=tryappservice)
  - [Ospitare un'app Web con Servizio app di Azure - Modulo Learn](/learn/modules/host-a-web-app-with-azure-app-service/index)
  - [Creare un'app Node.js e MongoDB in Azure](/azure/app-service/app-service-web-tutorial-nodejs-mongodb-app)
  - [Esempi di servizio app](/samples/browse/?languages=javascript%2Cnodejs&products=azure-app-service)

- È possibile creare contenitori personalizzati e distribuirli in Azure usando Registro Azure Container e il servizio Azure Kubernetes. Per informazioni dettagliate, vedere [Come distribuire contenitori Node.js in Azure](node-howto-deploy-containers.md).

- Se si vuole usare principalmente codice serverless, vedere [Come scrivere codice Node.js serverless in Azure](node-howto-write-serverless-code.md).

- Per informazioni dettagliate sulla creazione di un sito JAMstack (statico), vedere [Come creare app Web JAMstack (sito statico) con Azure](node-howto-create-static-site-jamstack.md).

- Se si vuole controllare l'infrastruttura, è possibile usare semplicemente una macchina virtuale. Per iniziare, seguire il percorso [Distribuire un sito Web con macchine virtuali di Azure](/learn/paths/deploy-a-website-with-azure-virtual-machines/) in Microsoft Learn.

Per una panoramica completa delle diverse opzioni di hosting, vedere [Albero delle decisioni per i servizi di calcolo di Azure](/azure/architecture/guide/technology-choices/compute-decision-tree) e il modulo [Servizi cloud di base - Opzioni di calcolo di Azure](/learn/modules/intro-to-azure-compute/) in Microsoft Learn.