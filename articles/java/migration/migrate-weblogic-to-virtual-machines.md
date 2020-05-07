---
title: Eseguire la migrazione di applicazioni WebLogic alle macchine virtuali di Azure
description: Questa guida descrive gli aspetti da considerare per la migrazione di un'applicazione WebLogic esistente da eseguire nelle macchine virtuali di Azure.
author: edburns
ms.author: edburns
ms.topic: conceptual
ms.date: 1/27/2020
ms.openlocfilehash: 10edb96e4e0781945da85d5a872b14178db3122f
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/05/2020
ms.locfileid: "81673517"
---
# <a name="migrate-weblogic-applications-to-azure-virtual-machines"></a>Eseguire la migrazione di applicazioni WebLogic alle macchine virtuali di Azure

Questa guida descrive gli aspetti da considerare per la migrazione di un'applicazione WebLogic esistente da eseguire nelle macchine virtuali di Azure.

## <a name="pre-migration"></a>Pre-migrazione

### <a name="define-what-you-mean-by-migration-complete"></a>Definire cosa si intende per "migrazione completata"

Questa guida e le offerte di Azure Marketplace corrispondenti costituiscono un punto di partenza per accelerare la migrazione dei carichi di lavoro di WebLogic Server ad Azure. È importante definire l'ambito dell'attività di migrazione. Ad esempio, si sta eseguendo un trasferimento "lift-and-shift" rigoroso dall'infrastruttura esistente alle macchine virtuali di Azure? In caso affermativo, si potrebbe essere tentati di adottare un modello di migrazione "lift-and-improve".

È preferibile attenersi il più strettamente possibile all'approccio "lift-and-shift", tenendo conto delle necessarie modifiche descritte in questa guida. Definire cosa si intende per "migrazione completata", in modo da sapere quando viene raggiunto questo traguardo. Una volta raggiunta la fase di "migrazione completata", è possibile acquisire uno snapshot delle macchine virtuali, come descritto in [Creare uno snapshot](/azure/virtual-machines/windows/snapshot-copy-managed-disk). Dopo aver verificato che sia possibile eseguire correttamente il ripristino dallo snapshot, è più sicuro apportare i miglioramenti senza pregiudicare le fasi avanzate della migrazione finora completate.

### <a name="determine-whether-the-pre-built-marketplace-offers-are-a-good-starting-point"></a>Determinare se le offerte predefinite del Marketplace rappresentano un punto di partenza valido

Oracle e Microsoft hanno collaborato per introdurre un set di modelli di soluzioni di Azure in Azure Marketplace, in modo da offrire un punto di partenza solido per la migrazione ad Azure. Consultare la documentazione sul [middleware Oracle Fusion](https://docs.oracle.com/en/middleware/fusion-middleware/weblogic-server/12.2.1.4/wlazu/) per l'elenco di offerte disponibili e scegliere quella più indicata per la distribuzione esistente. È possibile vedere l'elenco di offerte nella [documentazione di Oracle](https://docs.oracle.com/en/middleware/fusion-middleware/weblogic-server/12.2.1.4/wlazu/select-required-oracle-weblogic-server-offer-azure-marketplace.html#GUID-187739C5-EE7A-47C6-B3BA-C0A0333DC398)

Se nessuna delle offerte esistenti è un punto di partenza valido, sarà necessario riprodurre la distribuzione manualmente usando le risorse delle macchine virtuali di Azure. Per altre informazioni, vedere [Informazioni su IaaS](https://azure.microsoft.com/overview/what-is-iaas/).

### <a name="determine-whether-the-weblogic-version-is-compatible"></a>Determinare se la versione di WebLogic è compatibile

La versione di WebLogic esistente deve essere compatibile con la versione presente nelle offerte IaaS. Questa query mostrerà le offerte per [WebLogic versione 12.2.1.3](https://azuremarketplace.microsoft.com/marketplace/apps?search=oracle%20weblogic%2012.2.1.3&page=1). Se la versione di WebLogic esistente non è compatibile con quella versione, sarà necessario riprodurre la distribuzione manualmente usando le risorse di Azure IaaS. Per altre informazioni, vedere la [documentazione di Azure](https://azure.microsoft.com/overview/what-is-iaas/).

[!INCLUDE [inventory-server-capacity-virtual-machines](includes/inventory-server-capacity-virtual-machines.md)]

[!INCLUDE [inventory-all-secrets](includes/inventory-all-secrets.md)]

[!INCLUDE [inventory-all-certificates](includes/inventory-all-certificates.md)]

[!INCLUDE [validate-that-the-supported-java-version-works-correctly](includes/validate-that-the-supported-java-version-works-correctly.md)]

[!INCLUDE [inventory-jndi-resources](includes/inventory-jndi-resources.md)]

[!INCLUDE [domain-configuration](includes/domain-configuration.md)]

[!INCLUDE [determine-whether-session-replication-is-used](includes/determine-whether-session-replication-is-used.md)]

[!INCLUDE [document-datasources](includes/document-datasources.md)]

[!INCLUDE [determine-whether-weblogic-has-been-customized](includes/determine-whether-weblogic-has-been-customized.md)]

[!INCLUDE [determine-whether-management-over-rest-is-used](includes/determine-whether-management-over-rest-is-used.md)]

[!INCLUDE [determine-whether-a-connection-to-on-premises-is-needed](includes/determine-whether-a-connection-to-on-premises-is-needed.md)]

[!INCLUDE [determine-whether-jms-queues-or-topics-are-in-use-virtual-machines](includes/determine-whether-jms-queues-or-topics-are-in-use-virtual-machines.md)]

[!INCLUDE [determine-whether-you-are-using-your-own-custom-created-shared-java-ee-libraries](includes/determine-whether-you-are-using-your-own-custom-created-shared-java-ee-libraries.md)]

[!INCLUDE [determine-whether-osgi-bundles-are-used](includes/determine-whether-osgi-bundles-are-used.md)]

[!INCLUDE [determine-whether-your-application-contains-os-specific-code](includes/determine-whether-your-application-contains-os-specific-code.md)]

[!INCLUDE [determine-whether-oracle-service-bus-is-in-use](includes/determine-whether-oracle-service-bus-is-in-use.md)]

[!INCLUDE [determine-whether-your-application-is-composed-of-multiple-wars](includes/determine-whether-your-application-is-composed-of-multiple-wars.md)]

[!INCLUDE [determine-whether-your-application-is-packaged-as-an-ear](includes/determine-whether-your-application-is-packaged-as-an-ear.md)]

[!INCLUDE [identify-all-outside-processes-and-daemons-running-on-the-production-servers](includes/identify-all-outside-processes-and-daemons-running-on-the-production-servers.md)]

[!INCLUDE [determine-whether-wlst-is-used](includes/determine-whether-wlst-is-used.md)]

[!INCLUDE [validate-whether-and-how-the-file-system-is-used](includes/validate-whether-and-how-the-file-system-is-used.md)]

[!INCLUDE [determine-the-network-topology](includes/determine-the-network-topology.md)]

[!INCLUDE [account-for-the-use-of-jca-adapters-and-resource-adapters](includes/account-for-the-use-of-jca-adapters-and-resource-adapters.md)]

[!INCLUDE [account-for-the-use-of-custom-security-providers-and-jaas](includes/account-for-the-use-of-custom-security-providers-and-jaas.md)]

[!INCLUDE [determine-whether-weblogic-clustering-is-used](includes/determine-whether-weblogic-clustering-is-used.md)]

[!INCLUDE [determine-whether-the-java-ee-application-client-feature-is-used](includes/determine-whether-the-java-ee-application-client-feature-is-used.md)]

## <a name="migration"></a>Migrazione

### <a name="select-a-weblogic-on-azure-virtual-machines-offer"></a>Selezionare un'offerta di WebLogic in macchine virtuali di Azure

Le offerte seguenti sono disponibili per WebLogic in macchine virtuali di Azure.

Durante la distribuzione di un'offerta, verrà chiesto di scegliere le dimensioni delle macchine virtuali per i nodi di WebLogic Server. Nella scelta delle dimensioni delle VM, è importante considerare tutti gli aspetti, ossia memoria, processore e disco. Per altre informazioni, vedere la [documentazione per le offerte](https://docs.oracle.com/en/middleware/fusion-middleware/weblogic-server/12.2.1.4/wlazu/deploy-oracle-weblogic-server-administration-server-single-node.html) e anche la [documentazione di Azure per le dimensioni delle macchine virtuali](/azure/cloud-services/cloud-services-sizes-specs)

#### <a name="weblogic-server-single-node-with-no-admin-server"></a>Singolo nodo di WebLogic Server senza server di amministrazione

Questa offerta crea una singola macchina virtuale in cui viene installato WebLogic, ma senza configurare alcun dominio, il che è utile per gli scenari con una configurazione di domini altamente personalizzata.

#### <a name="weblogic-server-single-node-with-admin-server"></a>Singolo nodo di WebLogic con server di amministrazione

Questa offerta prevede il provisioning di una singola VM in cui viene installato WebLogic Server 12.1.2.3. Viene creato un dominio e viene avviato il server di amministrazione.

#### <a name="weblogic-server-n-node-cluster"></a>Cluster a n nodi di WebLogic Server

Questa offerta crea un cluster a disponibilità elevata di macchine virtuali WebLogic Server.

#### <a name="weblogic-server-n-node-dynamic-cluster"></a>Cluster dinamico a n nodi di WebLogic Server

Questa offerta crea un cluster dinamico, scalabile e a disponibilità elevata di macchine virtuali WebLogic Server

### <a name="provision-the-offer"></a>Effettuare il provisioning dell'offerta

Dopo aver selezionato l'offerta da cui iniziare, seguire le istruzioni riportate nella [documentazione per le offerte](https://wls-eng.github.io/arm-oraclelinux-wls/) per effettuarne il provisioning. Assicurarsi di scegliere il nome di dominio che corrisponde al nome di dominio esistente. È anche possibile usare la stessa password di dominio esistente.

### <a name="migrate-the-domains"></a>Eseguire la migrazione dei domini

Dopo aver effettuato il provisioning dell'offerta, è possibile esaminare la configurazione dei domini e seguire [questa guida](https://support.oracle.com/knowledge/Middleware/2336356_1.html) per informazioni su come eseguirne la migrazione.

### <a name="connect-the-databases"></a>Connettere i database

Dopo aver eseguito la migrazione dei domini, è possibile connettere i database seguendo le istruzioni riportate nella [documentazione dell'offerta](https://wls-eng.github.io/arm-oraclelinux-wls/#connecting-a-database-to-a-cluster). Queste istruzioni consentono di tenere conto dei segreti e delle stringe di accesso dei database coinvolti.

### <a name="account-for-keystores"></a>Tenere conto degli archivi chiavi

È necessario tenere conto della migrazione di eventuali archivi chiavi SSL usati dall'applicazione. Per altre informazioni, vedere [Configurazione degli archivi chiavi](https://docs.oracle.com/en/middleware/fusion-middleware/weblogic-server/12.2.1.4/secmg/identity_trust.html#GUID-7F03EB9C-9755-430B-8B86-17199E0C01DC).

### <a name="connect-the-jms-sources"></a>Connettere le origini JMS

Dopo aver connesso i database, è possibile configurare JMS seguendo le istruzioni riportate in [Middleware di Fusion per l'amministrazione di risorse JMS per Oracle WebLogic Server](https://docs.oracle.com/middleware/12213/wls/JMSAD/toc.htm) nella documentazione di WebLogic.

### <a name="account-for-logging"></a>Tenere conto della registrazione

La configurazione della registrazione esistente deve essere trasferita al momento della migrazione del dominio. Per altre informazioni, vedere [Configurare i livelli del logger java.util.logging](https://docs.oracle.com/en/middleware/fusion-middleware/weblogic-server/12.2.1.4/wlach/taskhelp/logging/ConfigureJavaLoggingLevels.html) e [Configurazione dei file di log e applicazione di filtri ai messaggi dei log per Oracle WebLogic Server](https://docs.oracle.com/en/middleware/fusion-middleware/weblogic-server/12.2.1.4/wllog/index.html)

### <a name="migrating-your-applications"></a>Migrazione delle applicazioni

Le tecniche usate per distribuire le applicazioni dai computer di sviluppo ai server di test, staging e produzione variano enormemente da un caso all'altro. In alcuni casi, è disponibile una piattaforma CI/CD estremamente avanzata che genera la distribuzione delle applicazioni in WebLogic Server. In altri casi, il processo può essere maggiormente manuale. Uno dei vantaggi dell'uso di macchine virtuali di Azure per la migrazione di applicazioni WebLogic al cloud è che i processi esistenti continueranno a funzionare.

Sarà necessario configurare il gruppo di sicurezza di rete fornito dall'offerta per consentire l'accesso dalla pipeline CI/CD o dal sistema di distribuzione manuale. Per altre informazioni, vedere [Gruppi di sicurezza](/azure/virtual-network/security-overview) nella documentazione di Azure.

### <a name="testing"></a>Test

Tutti i test effettuati in contenitori sulle applicazioni devono essere configurati per l'accesso ai nuovi server eseguiti in Azure. Come nel caso di CI/CD, è necessario assicurarsi che le regole di sicurezza di rete necessarie consentano ai test di accedere alle applicazioni distribuite in Azure. Per altre informazioni, vedere [Gruppi di sicurezza](/azure/virtual-network/security-overview) nella documentazione di Azure.

## <a name="post-migration"></a>Post-migrazione

Una volta raggiunti gli obiettivi di migrazione definiti nel passaggio di [pre-migrazione](#pre-migration), eseguire alcuni test di accettazione end-to-end per verificare che tutto funzioni come previsto. Alcuni argomenti per i miglioramenti post-migrazione includono, ma non sono certamente limitati a quanto segue:

* Uso di Archiviazione di Azure per gestire il contenuto statico montato nelle macchine virtuali. Per altre informazioni, vedere [Collegare o scollegare un disco dati da una macchina virtuale](/azure/lab-services/devtest-lab-attach-detach-data-disk).

* Distribuire le applicazioni con Azure DevOps nel cluster WebLogic dopo la migrazione. Per altre informazioni, vedere la [documentazione introduttiva di Azure DevOps](/azure/devops/get-started/?view=azure-devops).

* Ottimizzare la topologia di rete con servizi avanzati di bilanciamento del carico. Per altre informazioni, vedere [Uso dei servizi di bilanciamento del carico in Azure](/azure/traffic-manager/traffic-manager-load-balancing-azure).

* Sfruttare le identità gestite di Azure per i segreti gestiti e assegnare l'accesso basato sui ruoli alle risorse di Azure. Per altre informazioni, vedere [Informazioni sulle identità gestite per le risorse di Azure](/azure/active-directory/managed-identities-azure-resources/overview).

* Integrare l'autenticazione e l'autorizzazione di WebLogic Java EE con Azure Active Directory. Per altre informazioni, vedere [Introduzione all'integrazione di Azure Active Directory Premium](/azure/active-directory/manage-apps/plan-an-application-integration).

* Usare Azure Key Vault per archiviare qualsiasi informazione che funga da "segreto". Per altre informazioni, vedere [Concetti di base di Azure Key Vault](/azure/key-vault/basic-concepts).
