---
author: mnriem
ms.author: manriem
ms.date: 2/28/2020
ms.openlocfilehash: abf3824dd83d0da27a411694e144a8e6bf7285ac
ms.sourcegitcommit: 850856d3fa2ddd8f96616ee6a1f092d8e0aedab3
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/17/2020
ms.locfileid: "90738492"
---
### <a name="deploy-to-aks"></a>Distribuire in servizio Azure Kubernetes

Creare e applicare i file YAML di Kubernetes. Per altre informazioni, vedere [Avvio rapido: Distribuire un cluster del servizio Azure Kubernetes tramite l'interfaccia della riga di comando di Azure](/azure/aks/kubernetes-walkthrough#run-the-application). Se si crea un servizio di bilanciamento del carico esterno (per l'applicazione o per un controller in ingresso), assicurarsi di fornire l'indirizzo IP di cui è stato effettuato il provisioning nella sezione precedente come `LoadBalancerIP`.

Includere i parametri esternalizzati come variabili di ambiente. Per altre informazioni, vedere [Definire le variabili di ambiente per un contenitore](https://kubernetes.io/docs/tasks/inject-data-application/define-environment-variable-container/). Non includere segreti, ad esempio password, chiavi API e stringhe di connessione JDBC. Questi verranno illustrati nelle sezioni seguenti.

Assicurarsi di includere le impostazioni della memoria e della CPU durante la creazione della distribuzione YAML, in modo che i contenitori siano dimensionati correttamente.
