---
author: mriem
ms.author: manriem
ms.date: 2/28/2020
ms.openlocfilehash: 29ea7925e91b1f42e0f83b88fbc7f7d9a8fd3629
ms.sourcegitcommit: 56e5f51daf6f671f7b6e84d4c6512473b35d31d2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/07/2020
ms.locfileid: "78897641"
---
### <a name="deploy-to-aks"></a>Distribuire in servizio Azure Kubernetes

Creare e applicare i file YAML di Kubernetes. Per altre informazioni, vedere [Avvio rapido: Distribuire un cluster del servizio Azure Kubernetes tramite l'interfaccia della riga di comando di Azure](/azure/aks/kubernetes-walkthrough#run-the-application). Se si crea un servizio di bilanciamento del carico esterno (per l'applicazione o per un controller in ingresso), assicurarsi di fornire l'indirizzo IP di cui è stato effettuato il provisioning nella sezione precedente come `LoadBalancerIP`.

Includere i parametri esternalizzati come variabili di ambiente. Per altre informazioni, vedere [Definire le variabili di ambiente per un contenitore](https://kubernetes.io/docs/tasks/inject-data-application/define-environment-variable-container/). Non includere segreti, ad esempio password, chiavi API e stringhe di connessione JDBC. Questi verranno illustrati nelle sezioni seguenti.

Assicurarsi di includere le impostazioni della memoria e della CPU durante la creazione della distribuzione YAML, in modo che i contenitori siano dimensionati correttamente.
