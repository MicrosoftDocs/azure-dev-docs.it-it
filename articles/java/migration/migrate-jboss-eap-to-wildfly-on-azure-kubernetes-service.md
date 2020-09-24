---
title: Eseguire la migrazione di applicazioni JBoss EAP a WildFly nel servizio Azure Kubernetes
description: Questa guida descrive gli aspetti da considerare per la migrazione di un'applicazione JBoss EAP esistente da eseguire in WildFly in un contenitore del servizio Azure Kubernetes.
author: mnriem
ms.author: manriem
ms.topic: conceptual
ms.date: 3/16/2020
ms.custom: devx-track-java
ms.openlocfilehash: 5de4032f4413f4c72bf51990723c47874bc39ab2
ms.sourcegitcommit: 850856d3fa2ddd8f96616ee6a1f092d8e0aedab3
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/17/2020
ms.locfileid: "90738569"
---
# <a name="migrate-jboss-eap-applications-to-wildfly-on-azure-kubernetes-service"></a>Eseguire la migrazione di applicazioni JBoss EAP a WildFly nel servizio Azure Kubernetes

Questa guida descrive gli aspetti da considerare per la migrazione di un'applicazione JBoss EAP esistente da eseguire in WildFly in un contenitore del servizio Azure Kubernetes.

## <a name="pre-migration"></a>Pre-migrazione

Per garantire una corretta migrazione, prima di iniziare completare i passaggi di valutazione e inventario descritti nelle sezioni seguenti.

[!INCLUDE [inventory-server-capacity-aks](includes/inventory-server-capacity-aks.md)]

### <a name="inventory-all-secrets"></a>Inventario di tutti i segreti

Controllare tutte le proprietà e i file di configurazione nei server di produzione per verificare la presenza di segreti e password. Assicurarsi di controllare *jboss-web.xml* nei WAR. I file di configurazione contenenti password o credenziali possono trovarsi anche all'interno dell'applicazione.

È consigliabile archiviare tali segreti in Azure KeyVault. Per altre informazioni, vedere [Concetti di base di Azure Key Vault](/azure/key-vault/basic-concepts).

[!INCLUDE [inventory-all-certificates](includes/inventory-all-certificates.md)]

[!INCLUDE [validate-that-the-supported-java-version-works-correctly-wildfly](includes/validate-that-the-supported-java-version-works-correctly-wildfly.md)]

### <a name="inventory-jndi-resources"></a>Inventario delle risorse JNDI

Creare un inventario di tutte le risorse JNDI. Alcune, ad esempio i broker di messaggi JMS, possono richiedere la migrazione o la riconfigurazione.

### <a name="determine-whether-session-replication-is-used"></a>Determinare se viene usata la replica delle sessioni

Se l'applicazione si basa sulla replica delle sessioni, sarà necessario cambiarla per rimuovere questa dipendenza.

#### <a name="inside-your-application"></a>All'interno dell'applicazione

Esaminare i file *WEB-INF/jboss-web.xml* e/o *WEB-INF/web.xml*.

### <a name="document-datasources"></a>Documentare le origini dati

Se l'applicazione usa qualsiasi database, è necessario acquisire le informazioni seguenti:

* Qual è il nome dell'origine dati?
* Qual è la configurazione del pool di connessioni?
* Dove è possibile trovare il file JAR del driver JDBC?

Per altre informazioni, vedere la sezione relativa alle [origini dati di JBoss EAP](https://access.redhat.com/documentation/en-us/red_hat_jboss_enterprise_application_platform/7.3/html/configuration_guide/datasource_management) nella documentazione di JBoss EAP.

### <a name="determine-whether-and-how-the-file-system-is-used"></a>Determinare se e come viene usato il file system

Qualsiasi utilizzo del file system nel server applicazioni richiede modifiche della configurazione o, in casi rari, dell'architettura. Il file system può essere usato da moduli JBoss EAP o dal codice dell'applicazione. È possibile identificare alcuni o tutti gli scenari descritti nelle sezioni seguenti.

[!INCLUDE [static-content](includes/static-content.md)]

[!INCLUDE [dynamic-or-internal-content-aks](includes/dynamic-or-internal-content-aks.md)]

[!INCLUDE [determine-whether-your-application-relies-on-scheduled-jobs](includes/determine-whether-your-application-relies-on-scheduled-jobs.md)]

[!INCLUDE [determine-whether-a-connection-to-on-premises-is-needed](includes/determine-whether-a-connection-to-on-premises-is-needed.md)]

[!INCLUDE [determine-whether-jms-queues-or-topics-are-in-use](includes/determine-whether-jms-queues-or-topics-are-in-use.md)]

### <a name="determine-whether-your-application-uses-jboss-eap-specific-apis"></a>Determinare se l'applicazione usa API specifiche di JBoss EAP

Se l'applicazione usa API specifiche di JBoss EAP, sarà necessario effettuarne il refactoring per rimuovere tali dipendenze.

[!INCLUDE [determine-whether-your-application-uses-entity-beans](includes/determine-whether-your-application-uses-entity-beans.md)]

[!INCLUDE [determine-whether-the-java-ee-application-client-feature-is-in-use-aks](includes/determine-whether-the-java-ee-application-client-feature-is-in-use-aks.md)]

[!INCLUDE [determine-whether-your-application-contains-os-specific-code](includes/determine-whether-your-application-contains-os-specific-code.md)]

[!INCLUDE [determine-whether-ejb-timers-are-in-use](includes/determine-whether-ejb-timers-are-in-use.md)]

### <a name="determine-whether-jca-connectors-are-in-use"></a>Determinare se vengono usati i connettori JCA

Se l'applicazione usa connettori JCA, è necessario verificare che sia possibile usare il connettore JCA in WildFly. Se l'implementazione di JCA è vincolata a JBoss EAP, sarà necessario effettuare il refactoring dell'applicazione per rimuovere tale dipendenza. Se è possibile usare il connettore JCA, affinché sia disponibile sarà necessario aggiungere i file JAR al parametro classpath del server e includere i file di configurazione necessari nel percorso corretto nelle directory del server WildFly.

[!INCLUDE [determine-whether-jaas-is-in-use](includes/determine-whether-jaas-is-in-use.md)]

[!INCLUDE [determine-whether-your-application-uses-a-resource-adapter](includes/determine-whether-your-application-uses-a-resource-adapter.md)]

[!INCLUDE [determine-whether-your-application-is-composed-of-multiple-wars](includes/determine-whether-your-application-is-composed-of-multiple-wars.md)]

### <a name="determine-whether-your-application-is-packaged-as-an-ear"></a>Determinare se l'applicazione è assemblata come EAR

Se l'applicazione è assemblata come file EAR, assicurarsi di esaminare il file *application.xml* e acquisire la relativa configurazione.

> [!NOTE]
> Per ridimensionare ogni applicazione Web in modo indipendente per un uso più efficace delle risorse del servizio Azure Kubernetes, è necessario dividere EAR in applicazioni Web distinte.

[!INCLUDE [identify-all-outside-processes-and-daemons-running-on-the-production-servers](includes/identify-all-outside-processes-and-daemons-running-on-the-production-servers.md)]

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

Ora che è stata eseguita la migrazione dell'applicazione al servizio Azure Kubernetes, è necessario verificare che funzioni come previsto. Al termine, sono disponibili alcune raccomandazioni per rendere l'applicazione maggiormente nativa del cloud.

[!INCLUDE [recommendations-wildfly-on-aks](includes/recommendations-wildfly-on-aks.md)]
