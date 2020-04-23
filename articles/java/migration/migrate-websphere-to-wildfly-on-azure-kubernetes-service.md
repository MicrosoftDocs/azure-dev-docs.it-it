---
title: Eseguire la migrazione di applicazioni WebSphere a WildFly nel servizio Azure Kubernetes
description: Questa guida descrive gli aspetti da considerare per la migrazione di un'applicazione WebSphere esistente da eseguire in WildFly in un contenitore del servizio Azure Kubernetes.
author: mriem
ms.author: manriem
ms.topic: conceptual
ms.date: 2/28/2020
ms.openlocfilehash: 279051f626c09e63637fdf99b323857f2751b813
ms.sourcegitcommit: 0af39ee9ff27c37ceeeb28ea9d51e32995989591
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/21/2020
ms.locfileid: "81673317"
---
# <a name="migrate-websphere-applications-to-wildfly-on-azure-kubernetes-service"></a>Eseguire la migrazione di applicazioni WebSphere a WildFly nel servizio Azure Kubernetes

Questa guida descrive gli aspetti da considerare per la migrazione di un'applicazione WebSphere esistente da eseguire in WildFly in un contenitore del servizio Azure Kubernetes.

## <a name="pre-migration"></a>Pre-migrazione

[!INCLUDE [inventory-server-capacity-aks](includes/inventory-server-capacity-aks.md)]

### <a name="inventory-all-secrets"></a>Inventario di tutti i segreti

Controllare tutte le proprietà e i file di configurazione nei server di produzione per verificare la presenza di segreti e password. Assicurarsi di controllare *ibm-web-bnd.xml* nei WAR. I file di configurazione contenenti password o credenziali possono trovarsi anche all'interno dell'applicazione.

[!INCLUDE [inventory-all-certificates](includes/inventory-all-certificates.md)]

### <a name="validate-that-the-supported-java-version-works-correctly"></a>Verificare che la versione di Java supportata funzioni correttamente

L'uso di WildFly nel servizio Azure Kubernetes richiede una versione specifica di Java. Pertanto, sarà necessario verificare che l'applicazione sia in grado di funzionare correttamente usando tale versione supportata. Questa convalida è particolarmente importante se il server corrente è usa una versione di JDK non supportata, ad esempio Oracle JDK o IBM OpenJ9.

Per ottenere la versione corrente, accedere al server di produzione ed eseguire

```bash
java -version
```

### <a name="inventory-jndi-resources"></a>Inventario delle risorse JNDI

Creare un inventario di tutte le risorse JNDI. Alcune, ad esempio i broker di messaggi JMS, possono richiedere la migrazione o la riconfigurazione.

#### <a name="inside-your-application"></a>All'interno dell'applicazione

Esaminare i file *WEB-INF/ibm-web-bnd.xml* e/o *WEB-INF/web.xml*.

### <a name="document-datasources"></a>Documentare le origini dati

Se l'applicazione usa qualsiasi database, è necessario acquisire le informazioni seguenti:

* Qual è il nome dell'origine dati?
* Qual è la configurazione del pool di connessioni?
* Dove è possibile trovare il file JAR del driver JDBC?

Per altre informazioni, vedere la sezione relativa alla [configurazione della connettività del database](https://www.ibm.com/support/knowledgecenter/SSQP76_8.10.x/com.ibm.odm.distrib.config.was/config_dc_websphere/tpc_was_create_datasrc_cpl.html) nella documentazione di WebSphere.

### <a name="determine-whether-and-how-the-file-system-is-used"></a>Determinare se e come viene usato il file system

Qualsiasi utilizzo del file system nel server applicazioni richiede modifiche della configurazione o, in casi rari, dell'architettura. Il file system può essere usato da moduli WebSphere o dal codice dell'applicazione. È possibile identificare alcuni o tutti gli scenari descritti nelle sezioni seguenti.

#### <a name="read-only-static-content"></a>Contenuto statico di sola lettura

Se l'applicazione attualmente distribuisce contenuto statico, è necessario modificarne la posizione. Si può scegliere di spostare il contenuto statico in Archiviazione BLOB di Azure e di aggiungere la rete di distribuzione dei contenuti di Azure per accelerare i download a livello globale. Per altre informazioni, vedere [Hosting di siti Web statici in Archiviazione di Azure](/azure/storage/blobs/storage-blob-static-website) e [Avvio rapido: Integrare un account di archiviazione di Azure con la rete CDN di Azure](/azure/cdn/cdn-create-a-storage-account-with-cdn).

#### <a name="dynamically-published-static-content"></a>Contenuto statico pubblicato dinamicamente

Se l'applicazione consente contenuto statico caricato/prodotto dall'applicazione ma non modificabile dopo la creazione, è possibile usare Archiviazione BLOB di Azure e la rete di distribuzione dei contenuti di Azure, come descritto sopra, con una funzione di Azure per gestire i caricamenti e l'aggiornamento della rete CDN. Nell'articolo [Caricamento e precaricamento nella rete CDN di contenuto statico con Funzioni di Azure](https://github.com/Azure-Samples/functions-java-push-static-contents-to-cdn) è riportata un'implementazione di esempio che è possibile usare.

#### <a name="dynamic-or-internal-content"></a>Contenuto dinamico o interno

Per i file scritti e letti di frequente dall'applicazione, ad esempio i file di dati temporanei, o i file statici visibili solo all'applicazione, è possibile montare le condivisioni di archiviazione di Azure come volumi persistenti. Per altre informazioni, vedere [Creare dinamicamente e usare un volume persistente con File di Azure nel servizio Azure Kubernetes](/azure/aks/azure-files-dynamic-pv).

[!INCLUDE [determine-whether-your-application-relies-on-scheduled-jobs](includes/determine-whether-your-application-relies-on-scheduled-jobs.md)]

[!INCLUDE [determine-whether-a-connection-to-on-premises-is-needed](includes/determine-whether-a-connection-to-on-premises-is-needed.md)]

[!INCLUDE [determine-whether-jms-queues-or-topics-are-in-use](includes/determine-whether-jms-queues-or-topics-are-in-use.md)]

### <a name="determine-whether-your-application-uses-websphere-specific-apis"></a>Determinare se l'applicazione usa API specifiche di WebSphere

Se l'applicazione usa API specifiche di WebSphere, sarà necessario effettuarne il refactoring per rimuovere tali dipendenze. Se, ad esempio, è stata usata una classe indicata nella [specifica dell'API IBM WebSphere Application Server, versione 9.0](https://www.ibm.com/support/knowledgecenter/en/SSEQTJ_9.0.5/com.ibm.websphere.javadoc.doc/web/apidocs/overview-summary.html?view=embed), è stata usata un'API specifica di WebSphere nell'applicazione.

[!INCLUDE [determine-whether-your-application-uses-entity-beans](includes/determine-whether-your-application-uses-entity-beans.md)]

[!INCLUDE [determine-whether-the-java-ee-application-client-feature-is-in-use-aks](includes/determine-whether-the-java-ee-application-client-feature-is-in-use-aks.md)]

[!INCLUDE [determine-whether-your-application-contains-os-specific-code](includes/determine-whether-your-application-contains-os-specific-code.md)]

[!INCLUDE [determine-whether-ejb-timers-are-in-use](includes/determine-whether-ejb-timers-are-in-use.md)]

### <a name="determine-whether-jca-connectors-are-in-use"></a>Determinare se vengono usati i connettori JCA

Se l'applicazione usa connettori JCA, è necessario verificare che possano essere usati in WildFly. Se l'implementazione di JCA è vincolata a WebSphere, sarà necessario effettuare il refactoring dell'applicazione per rimuovere tale dipendenza. Se il connettore può essere usato, sarà necessario aggiungere i file JAR al classpath del server e inserire i file di configurazione necessari nella posizione corretta nelle directory del server WildFly per renderlo disponibile.

[!INCLUDE [determine-whether-jaas-is-in-use](includes/determine-whether-jaas-is-in-use.md)]

[!INCLUDE [determine-whether-your-application-uses-a-resource-adapter](includes/determine-whether-your-application-uses-a-resource-adapter.md)]

[!INCLUDE [determine-whether-your-application-is-composed-of-multiple-wars](includes/determine-whether-your-application-is-composed-of-multiple-wars.md)]

### <a name="determine-whether-your-application-is-packaged-as-an-ear"></a>Determinare se l'applicazione è assemblata come EAR

Se l'applicazione è assemblata come file EAR, assicurarsi di esaminare i file *application.xml* e *application-bnd.xml* e acquisire le relative configurazioni.

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
