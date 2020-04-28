---
author: mriem
ms.author: manriem
ms.date: 2/28/2020
ms.openlocfilehash: 1ffc12fa2a99e2abdbbc43e0024afd8e5e98c4b9
ms.sourcegitcommit: 0af39ee9ff27c37ceeeb28ea9d51e32995989591
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/21/2020
ms.locfileid: "81673257"
---
### <a name="recommendations"></a>Consigli

* Valutare se aggiungere un nome DNS all'indirizzo IP allocato al controller in ingresso o al servizio di bilanciamento del carico dell'applicazione. Per altre informazioni, vedere [Creare un controller di ingresso con un indirizzo IP pubblico statico nel servizio Azure Kubernetes](/azure/aks/ingress-static-ip).

* Valutare se aggiungere [grafici HELM](https://helm.sh/docs/topics/charts/) per l'applicazione. Un grafico Helm consente di parametrizzare la distribuzione dell'applicazione per l'uso e la personalizzazione da parte di un set più diversificato di clienti.

* Progettare e implementare una strategia DevOps. Per mantenere l'affidabilità aumentando al tempo stesso la velocità di sviluppo, è consigliabile automatizzare le distribuzioni e i test con Azure Pipelines. Per altre informazioni, vedere [Compilare e distribuire nel servizio Azure Kubernetes](/azure/devops/pipelines/ecosystems/kubernetes/aks-template).

* Abilitare Monitoraggio di Azure per il cluster. Per altre informazioni, vedere [Abilitare il monitoraggio di un cluster del servizio Azure Kubernetes già distribuito](/azure/azure-monitor/insights/container-insights-enable-existing-clusters). In questo modo Monitoraggio di Azure può raccogliere i log dei contenitori, tenere traccia dell'utilizzo e così via.

* Valutare se esporre metriche specifiche dell'applicazione tramite Prometheus. Prometheus è un framework di metriche open source ampiamente adottato nella community di Kubernetes. È possibile configurare lo scraping delle metriche di Prometheus in Monitoraggio di Azure invece di ospitare il proprio server Prometheus per consentire l'aggregazione delle metriche dalle applicazioni e la risposta automatizzata o l'escalation delle condizioni anomale. Per altre informazioni, vedere [Configurare lo scraping delle metriche di Prometheus con Monitoraggio di Azure per i contenitori](/azure/azure-monitor/insights/container-insights-prometheus-integration).

* Progettare e implementare una strategia di continuità aziendale e ripristino di emergenza. Per le applicazioni cruciali, considerare un'architettura di distribuzione in più aree. Per altre informazioni, vedere le [procedure consigliate per continuità aziendale e ripristino di emergenza nel servizio Azure Kubernetes](/azure/aks/operator-best-practices-multi-region).

* Vedere [Criteri di supporto della versione di Kubernetes](/azure/aks/supported-kubernetes-versions#kubernetes-version-support-policy). È responsabilità dell'utente mantenere aggiornato il cluster del servizio Azure Kubernetes per assicurarsi che sia sempre in esecuzione una versione supportata. Per altre informazioni, vedere [Aggiornare un cluster del servizio Azure Kubernetes](/azure/aks/upgrade-cluster).

* Chiedere a tutti i membri del team responsabili dell'amministrazione del cluster e dello sviluppo di applicazioni di esaminare le procedure consigliate per il servizio Azure Kubernetes. Per altre informazioni, vedere [Procedure consigliate per sviluppatori e operatori del cluster per la creazione e la gestione di applicazioni nel servizio Azure Kubernetes](/azure/aks/best-practices).

* Assicurarsi che il file di distribuzione specifichi il modo in cui vengono eseguiti gli aggiornamenti in sequenza. Per altre informazioni, vedere [Distribuzione di aggiornamenti in sequenza](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#rolling-update-deployment) nella documentazione di Kubernetes.

* Configurare il ridimensionamento automatico per gestire i carichi nei periodi di picco. Per altre informazioni, vedere [Ridimensionare automaticamente un cluster per soddisfare le richieste delle applicazioni nel servizio Azure Kubernetes](/azure/aks/cluster-autoscaler).

* Valutare se monitorare le dimensioni della cache del codice e aggiungere i parametri JVM `-XX:InitialCodeCacheSize` e `-XX:ReservedCodeCacheSize` nel Dockerfile per ottimizzare ulteriormente le prestazioni. Per altre informazioni, vedere come [ottimizzare la cache di codice](https://docs.oracle.com/javase/8/embedded/develop-apps-platforms/codecache.htm) nella documentazione di Oracle.
