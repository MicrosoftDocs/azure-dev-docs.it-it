---
title: Eseguire la migrazione di applicazioni WebLogic a WildFly nel servizio Azure Kubernetes
description: Questa guida descrive gli aspetti da considerare per la migrazione di un'applicazione WebLogic esistente da eseguire in WildFly in un contenitore del servizio Azure Kubernetes.
author: mnriem
ms.author: manriem
ms.topic: conceptual
ms.date: 2/28/2020
ms.custom: devx-track-java
ms.openlocfilehash: 8a55c9e7aaffba5a65f2d195763900aac3f3c336
ms.sourcegitcommit: 850856d3fa2ddd8f96616ee6a1f092d8e0aedab3
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/17/2020
ms.locfileid: "90738432"
---
# <a name="migrate-weblogic-applications-to-wildfly-on-azure-kubernetes-service"></a>Eseguire la migrazione di applicazioni WebLogic a WildFly nel servizio Azure Kubernetes

Questa guida descrive gli aspetti da considerare per la migrazione di un'applicazione WebLogic esistente da eseguire in WildFly in un contenitore del servizio Azure Kubernetes.

## <a name="pre-migration"></a>Pre-migrazione

Per garantire una corretta migrazione, prima di iniziare completare i passaggi di valutazione e inventario descritti nelle sezioni seguenti.

Se non è possibile soddisfare i requisiti di pre-migrazione, vedere le guide alla migrazione complementari:

* [Eseguire la migrazione di applicazioni WebLogic alle macchine virtuali di Azure](migrate-weblogic-to-virtual-machines.md)

[!INCLUDE [inventory-server-capacity-aks](includes/inventory-server-capacity-aks.md)]

[!INCLUDE [inventory-all-secrets](includes/inventory-all-secrets.md)]

[!INCLUDE [inventory-all-certificates](includes/inventory-all-certificates.md)]

[!INCLUDE [inventory-jndi-resources](includes/inventory-jndi-resources.md)]

### <a name="determine-whether-session-replication-is-used"></a>Determinare se viene usata la replica delle sessioni

Se l'applicazione si basa sulla replica delle sessioni, con o senza Oracle Coherence*Web, sono disponibili due opzioni:

* Effettuare il refactoring dell'applicazione per l'uso di un database per la gestione delle sessioni.
* Effettuare il refactoring dell'applicazione per esternalizzare la sessione nel servizio Azure Redis. Per altre informazioni, vedere [Azure Cache for Redis](/azure/azure-cache-for-redis/cache-overview).

[!INCLUDE [document-datasources](includes/document-datasources.md)]

[!INCLUDE [determine-whether-weblogic-has-been-customized](includes/determine-whether-weblogic-has-been-customized.md)]

[!INCLUDE [determine-whether-a-connection-to-on-premises-is-needed](includes/determine-whether-a-connection-to-on-premises-is-needed.md)]

[!INCLUDE [determine-whether-jms-queues-or-topics-are-in-use](includes/determine-whether-jms-queues-or-topics-are-in-use.md)]

[!INCLUDE [determine-whether-you-are-using-your-own-custom-created-shared-java-ee-libraries](includes/determine-whether-you-are-using-your-own-custom-created-shared-java-ee-libraries.md)]

[!INCLUDE [determine-whether-osgi-bundles-are-used](includes/determine-whether-osgi-bundles-are-used.md)]

[!INCLUDE [determine-whether-your-application-contains-os-specific-code](includes/determine-whether-your-application-contains-os-specific-code.md)]

[!INCLUDE [determine-whether-oracle-service-bus-is-in-use](includes/determine-whether-oracle-service-bus-is-in-use.md)]

[!INCLUDE [determine-whether-your-application-is-composed-of-multiple-wars](includes/determine-whether-your-application-is-composed-of-multiple-wars.md)]

[!INCLUDE [determine-whether-your-application-is-packaged-as-an-ear](includes/determine-whether-your-application-is-packaged-as-an-ear.md)]

<!-- AKS-specific extension of the last INCLUDE. -->
> [!NOTE]
> Per ridimensionare ogni applicazione Web in modo indipendente per un uso più efficace delle risorse del servizio Azure Kubernetes, è necessario dividere EAR in applicazioni Web distinte.
<!-- end extension -->

[!INCLUDE [identify-all-outside-processes-and-daemons-running-on-the-production-servers](includes/identify-all-outside-processes-and-daemons-running-on-the-production-servers.md)]

[!INCLUDE [validate-that-the-supported-java-version-works-correctly-wildfly](includes/validate-that-the-supported-java-version-works-correctly-wildfly.md)]

[!INCLUDE [determine-whether-your-application-relies-on-scheduled-jobs](includes/determine-whether-your-application-relies-on-scheduled-jobs.md)]

### <a name="determine-whether-weblogic-scripting-tool-wlst-is-used"></a>Determinare se viene usato lo strumento WLST (WebLogic Scripting Tool)

Se attualmente si usa lo strumento WLST per eseguire la distribuzione, sarà necessario valutare quali operazioni esegue. Se WLST cambia qualsiasi parametro (runtime) dell'applicazione come parte della distribuzione, è necessario assicurarsi che tali parametri siano conformi a una delle opzioni seguenti:

* Sono esternalizzati come impostazioni dell'app.
* Sono incorporati nell'applicazione.
* Usano l'interfaccia della riga di comando di JBoss durante la distribuzione.

Se WLST esegue altro oltre a quanto indicato sopra, è necessario completare alcune operazioni aggiuntive durante la migrazione.

### <a name="determine-whether-your-application-uses-weblogic-specific-apis"></a>Determinare se l'applicazione usa API specifiche di WebLogic

Se l'applicazione usa API specifiche di WebLogic, sarà necessario effettuarne il refactoring per rimuovere tali dipendenze. Se, ad esempio, è stata usata una classe menzionata nelle [informazioni di riferimento sulle API Java per Oracle WebLogic Server](https://docs.oracle.com/en/middleware/fusion-middleware/weblogic-server/12.2.1.4/wlapi/index.html?overview-summary.html), è stata usata un'API specifica di WebLogic nell'applicazione.

[!INCLUDE [determine-whether-your-application-uses-entity-beans](includes/determine-whether-your-application-uses-entity-beans.md)]

[!INCLUDE [determine-whether-the-java-ee-application-client-feature-is-in-use-aks](includes/determine-whether-the-java-ee-application-client-feature-is-in-use-aks.md)]

### <a name="determine-whether-a-deployment-plan-was-used"></a>Determinare se è stato usato un piano di distribuzione

Se l'app è stata distribuita usando un piano di distribuzione, sarà necessario valutare le operazioni previste. Se il piano prevede una distribuzione diretta, sarà possibile distribuire l'applicazione Web senza modifiche. Se invece il piano è più elaborato, sarà necessario determinare se è possibile usare l'interfaccia della riga di comando di JBoss per configurare correttamente l'applicazione come parte della distribuzione. Se non è possibile usare l'interfaccia della riga di comando di JBoss, sarà necessario effettuare il refactoring dell'applicazione in modo che non sia più necessario un piano di distribuzione.

[!INCLUDE [determine-whether-ejb-timers-are-in-use](includes/determine-whether-ejb-timers-are-in-use.md)]

### <a name="determine-whether-and-how-the-file-system-is-used"></a>Determinare se e come viene usato il file system

Qualsiasi utilizzo del file system nel server applicazioni richiede modifiche della configurazione o, in casi rari, dell'architettura. Il file system può essere usato da moduli condivisi di WebLogic o dal codice dell'applicazione. È possibile identificare alcuni o tutti gli scenari descritti nelle sezioni seguenti.

[!INCLUDE [static-content](includes/static-content.md)]

[!INCLUDE [dynamic-or-internal-content-aks](includes/dynamic-or-internal-content-aks.md)]

### <a name="determine-whether-jca-connectors-are-used"></a>Determinare se vengono usati i connettori JCA

Se l'applicazione usa connettori JCA, è necessario verificare che possano essere usati in WildFly. Se l'implementazione di JCA è vincolata a WebLogic, sarà necessario effettuare il refactoring dell'applicazione per rimuovere tale dipendenza. Se il connettore può essere usato, sarà necessario aggiungere i file JAR al classpath del server e inserire i file di configurazione necessari nella posizione corretta nelle directory del server WildFly per renderlo disponibile.

[!INCLUDE [determine-whether-your-application-uses-a-resource-adapter](includes/determine-whether-your-application-uses-a-resource-adapter.md)]

[!INCLUDE [determine-whether-jaas-is-in-use](includes/determine-whether-jaas-is-in-use.md)]

### <a name="determine-whether-weblogic-clustering-is-used"></a>Determinare se viene usato il clustering WebLogic

Molto probabilmente, l'applicazione viene distribuita in più server WebLogic per ottenere disponibilità elevata. Il servizio Azure Kubernetes è scalabile, ma se è stata usata l'API WebLogic Cluster, sarà necessario effettuare il refactoring del codice per eliminare l'uso di tale API.

[!INCLUDE [perform-in-place-testing](includes/perform-in-place-testing.md)]

## <a name="migration"></a>Migrazione

[!INCLUDE [provision-azure-container-registry-and-azure-kubernetes-service](includes/provision-azure-container-registry-and-azure-kubernetes-service.md)]

[!INCLUDE [create-a-docker-image-for-wildfly](includes/create-a-docker-image-for-wildfly.md)]

[!INCLUDE [build-and-push-the-docker-image-to-azure-container-registry](includes/build-and-push-the-docker-image-to-azure-container-registry.md)]

[!INCLUDE [provision-a-public-ip-address](includes/provision-a-public-ip-address.md)]

[!INCLUDE [deploy-to-aks](includes/deploy-to-aks.md)]

### <a name="configure-persistent-storage"></a>Configurare la risorsa di archiviazione persistente

Se l'applicazione richiede una risorsa di archiviazione non volatile, configurare uno o più [volumi persistenti](/azure/aks/azure-disks-dynamic-pv).

[!INCLUDE [migrate-scheduled-jobs-aks](includes/migrate-scheduled-jobs-aks.md)]

## <a name="post-migration"></a>Post-migrazione

Ora che è stata eseguita la migrazione dell'applicazione al servizio Azure Kubernetes, è necessario verificare che funzioni come previsto. Al termine, vedere le raccomandazioni seguenti per rendere l'applicazione maggiormente nativa del cloud.

[!INCLUDE [recommendations-wildfly-on-aks](includes/recommendations-wildfly-on-aks.md)]
