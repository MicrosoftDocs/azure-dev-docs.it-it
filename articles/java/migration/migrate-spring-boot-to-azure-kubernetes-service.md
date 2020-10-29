---
title: Eseguire la migrazione di applicazioni Spring Boot da eseguire nel servizio Azure Kubernetes
description: Questa guida descrive gli aspetti da considerare per la migrazione di un'applicazione Spring Boot esistente da eseguire in un contenitore del servizio Azure Kubernetes.
author: mnriem
ms.author: manriem
ms.topic: conceptual
ms.date: 4/10/2020
ms.custom: devx-track-java, devx-track-azurecli
ms.openlocfilehash: 77ad38a4fb1290e392ee933a04aaf802a910e577
ms.sourcegitcommit: 1ddcb0f24d2ae3d1f813ec0f4369865a1c6ef322
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/27/2020
ms.locfileid: "92689040"
---
# <a name="migrate-spring-boot-applications-to-azure-kubernetes-service"></a>Eseguire la migrazione di applicazioni Spring Boot al servizio Azure Kubernetes

Questa guida descrive gli aspetti da considerare per la migrazione di un'applicazione Spring Boot esistente da eseguire nel servizio Azure Kubernetes.

## <a name="pre-migration"></a>Pre-migrazione

Per garantire una corretta migrazione, prima di iniziare completare i passaggi di valutazione e inventario descritti nelle sezioni seguenti.

### <a name="validate-that-the-supported-java-version-works-correctly"></a>Verificare che la versione di Java supportata funzioni correttamente

Quando si esegue un'applicazione Spring Boot nel servizio Azure Kubernetes, è consigliabile usare una versione supportata di Java. Verificare che l'applicazione venga eseguita correttamente usando la versione supportata.

[!INCLUDE [note-obtain-your-current-java-version](includes/note-obtain-your-current-java-version.md)]

### <a name="determine-whether-and-how-the-file-system-is-used"></a>Determinare se e come viene usato il file system

Qualsiasi utilizzo del file system da parte dell'applicazione Spring Boot richiede modifiche della configurazione o, in casi rari, dell'architettura. È possibile identificare alcuni o tutti gli scenari descritti nelle sezioni seguenti.

[!INCLUDE [static-content](includes/static-content.md)]

[!INCLUDE [dynamic-or-internal-content-aks](includes/dynamic-or-internal-content-aks.md)]

[!INCLUDE [determine-whether-your-application-relies-on-scheduled-jobs](includes/determine-whether-your-application-relies-on-scheduled-jobs.md)]

[!INCLUDE [determine-whether-a-connection-to-on-premises-is-needed](includes/determine-whether-a-connection-to-on-premises-is-needed.md)]

[!INCLUDE [determine-whether-your-application-contains-os-specific-code](includes/determine-whether-your-application-contains-os-specific-code.md)]

[!INCLUDE [identify-all-outside-processes-and-daemons-running-on-the-production-servers](includes/identify-all-outside-processes-and-daemons-running-on-the-production-servers.md)]

[!INCLUDE [identify-spring-boot-versions](includes/identify-spring-boot-versions.md)]

Per tutte le applicazioni che usano Spring Boot 1.x, seguire la [Guida alla migrazione di Spring Boot 2.0](https://github.com/spring-projects/spring-boot/wiki/Spring-Boot-2.0-Migration-Guide) per aggiornarle a una versione di Spring Boot supportata.

### <a name="review-your-database-properties"></a>Esaminare le proprietà del database

Se l'applicazione usa un database, esaminare le proprietà del database nel file *application.properties* per assicurarsi che l'applicazione Spring Boot possa ancora accedere al database dopo la migrazione al servizio Azure Kubernetes. Se il database è locale, è necessario eseguirne la migrazione nel cloud o stabilire la connettività al database locale.

### <a name="identify-log-aggregation-solutions"></a>Identificare le soluzioni di aggregazione dei log

Identificare le soluzioni di aggregazione dei log usate dalle applicazioni di cui si esegue la migrazione.

### <a name="identify-application-performance-management-apm-agents"></a>Identificare gli agenti di gestione delle prestazioni delle applicazioni

Identificare gli agenti Application Performance Monitoring in uso con le applicazioni, ad esempio Dynatrace e Datadog. È necessario riconfigurare questi agenti Application Performance Monitoring in modo che siano inclusi in una configurazione Dockerfile o Jib oppure che usino l'agente Java in-process di Application Insights.

### <a name="identify-zipkin-dependencies"></a>Identificare le dipendenze di Zipkin

Determinare se l'applicazione ha dipendenze esplicite da Zipkin. Cercare le dipendenze nel gruppo `io.zipkin.java` nelle dipendenze di Maven o Gradle.

### <a name="inventory-external-resources"></a>Inventario delle risorse esterne

Identificare le risorse esterne, ad esempio le origini dati, i broker dei messaggi JMS e gli URL di altri servizi. Nelle applicazioni Spring Boot la configurazione per tali risorse si trova di solito nella cartella *src/main/directory* , in un file generalmente denominato *application.properties* o *application.yml* . Verificare inoltre le variabili di ambiente della distribuzione di produzione per rilevare eventuali impostazioni di configurazione pertinenti.

[!INCLUDE [inventory-databases-spring-boot](includes/inventory-databases-spring-boot.md)]

[!INCLUDE [identify-jms-brokers-in-spring](includes/identify-jms-brokers-in-spring.md)]

Dopo aver identificato i broker in uso, trovare le impostazioni corrispondenti. Nelle applicazioni Spring Boot si trovano in genere nei file *application.properties* e *application.yml* nella directory dell'applicazione.

[!INCLUDE [jms-broker-settings-examples-in-spring](includes/jms-broker-settings-examples-in-spring.md)]

[!INCLUDE [identify-external-caches-azure-spring-cloud](includes/identify-external-caches-azure-spring-cloud.md)]

[!INCLUDE [inventory-configuration-sources-and-secrets-spring-boot](includes/inventory-configuration-sources-and-secrets-spring-boot.md)]

[!INCLUDE [inspect-the-deployment-architecture-spring-boot](includes/inspect-the-deployment-architecture-spring-boot.md)]

#### <a name="identity-providers"></a>Provider di identità

Identificare tutti i provider di identità e tutte le applicazioni Spring Boot che richiedono l'autenticazione e/o l'autorizzazione. Per informazioni su come configurare i provider di identità, consultare gli argomenti seguenti:

* Per la configurazione di sicurezza OAuth o OAuth2 Spring, vedere [Spring Security](https://spring.io/projects/spring-security).
* Per la configurazione di Auth0 Spring Security, vedere la [documentazione di Auth0 Spring Security](https://auth0.com/docs/quickstart/backend/java-spring-security5/01-authorization).
* Per la configurazione della sicurezza di PingFederate Spring Security, vedere le [istruzioni di Auth0 PingFederate](https://auth0.com/authenticate/java-spring-security/ping-federate/).

#### <a name="resources-configured-through-vmware-tanzu-application-service-tas-formerly-pivotal-cloud-foundry"></a>Risorse configurate tramite il servizio VMware TAS (Tanzu Application Service) (in precedenza Pivotal Cloud Foundry)

Per le applicazioni gestite con TAS, le risorse esterne, incluse quelle descritte in precedenza, vengono spesso configurate tramite le associazioni del servizio TAS. Per esaminare la configurazione di tali risorse, usare l'[interfaccia della riga di comando di TAS (Cloud Foundry)](https://docs.cloudfoundry.org/cf-cli/) per visualizzare la variabile `VCAP_SERVICES` per l'applicazione.

```bash
# Log into TAS, if needed (enter credentials when prompted)
cf login -a <API endpoint>

# Set the organization and space containing the application, if not already selected during login.
cf target org <Organization Name>
cf target space <Space Name>

# Display variables for the application
cf env <Application Name>
```

Esaminare la variabile `VCAP_SERVICES` per le impostazioni di configurazione dei servizi esterni associati all'applicazione. Per altre informazioni, vedere la [documentazione di TAS (Cloud Foundry)](https://docs.cloudfoundry.org/devguide/deploy-apps/environment-variable.html#VCAP-SERVICES).

### <a name="in-place-testing"></a>Test sul posto

Prima di creare immagini del contenitore, eseguire la migrazione dell'applicazione alla versione di JDK e Spring Boot che si intende usare nel servizio Azure Kubernetes. Testare accuratamente l'applicazione per garantirne la compatibilità e le prestazioni.

## <a name="migration"></a>Migrazione

[!INCLUDE [provision-azure-container-registry-and-azure-kubernetes-service](includes/provision-azure-container-registry-and-azure-kubernetes-service.md)]

### <a name="create-a-docker-image-for-spring-boot"></a>Creare un'immagine Docker per Spring Boot

Per creare un Dockerfile, è necessario soddisfare i prerequisiti seguenti:

* Una versione supportata di JDK.
* Le opzioni di runtime della JVM.
* Un modo per passare le variabili di ambiente (se applicabile).

È quindi possibile eseguire i passaggi descritti nelle sezioni seguenti, ove applicabile. È possibile usare il [repository Avvio rapido di Spring Boot in contenitori](https://github.com/Azure/spring-boot-container-quickstart) come punto di partenza per il Dockerfile e l'applicazione Spring Boot.

#### <a name="configure-azure-key-vault-provider-for-secrets-store-csi-driver"></a>Configurare il provider di Azure Key Vault per il driver CSI dell'archivio di segreti

Creare un'istanza di Azure KeyVault e popolarla con i segreti necessari. Per altre informazioni, vedere [Avvio rapido: Impostare e recuperare un segreto da Azure Key Vault usando l'interfaccia della riga di comando di Azure](/azure/key-vault/quick-create-cli). Configurare quindi il [provider di Azure Key Vault per il driver CSI dell'archivio di segreti](https://github.com/Azure/secrets-store-csi-driver-provider-azure) per rendere i segreti accessibili ai pod.

Sarà anche necessario aggiornare lo script di avvio usato per eseguire il bootstrap dell'applicazione Spring Boot. Questo script deve importare i certificati nell'archivio chiavi usato da Spring Boot prima di avviare l'applicazione.

### <a name="build-and-push-the-docker-image-to-azure-container-registry"></a>Creare l'immagine Docker ed eseguirne il push in Registro Azure Container

Dopo aver creato il Dockerfile, è necessario creare l'immagine Docker e pubblicarla nel registro contenitori di Azure.

Se è stato [usato il repository GitHub Avvio rapido di Spring Boot in contenitori](https://github.com/Azure/spring-boot-container-quickstart), il processo di creazione dell'immagine e push nel registro contenitori di Azure equivale a richiamare i tre comandi seguenti.

In questi esempi la variabile di ambiente `MY_ACR` include il nome del registro contenitori di Azure e la variabile `MY_APP_NAME` il nome dell'applicazione Web che si vuole usare al suo interno.

Creare il file di distribuzione:

```bash
mvn package
```

Accedere al registro contenitori di Azure:

```azurecli
az acr login -n ${MY_ACR}
```

Creare l'immagine ed eseguirne il push:

```azurecli
az acr build -t ${MY_ACR}.azurecr.io/${MY_APP_NAME} .
```

In alternativa, è possibile usare l'interfaccia della riga di comando di Docker per creare e testare l'immagine in locale, come illustrato nei comandi seguenti. Questo approccio può semplificare il test e il perfezionamento dell'immagine prima della distribuzione iniziale in Registro Azure Container. Tuttavia, è necessario installare l'interfaccia della riga di comando di Docker e assicurarsi che il daemon Docker sia in esecuzione.

Creare l'immagine:

```bash
docker build -t ${MY_ACR}.azurecr.io/${MY_APP_NAME}
```

Eseguire l'immagine in locale:

```bash
docker run -it -p 8080:8080 ${MY_ACR}.azurecr.io/${MY_APP_NAME}
```

È ora possibile accedere all'applicazione all'indirizzo `http://localhost:8080`.

Accedere al registro contenitori di Azure:

```azurecli
az acr login -n ${MY_ACR}
```

Eseguire il push delle immagini nel registro contenitori di Azure:

```bash
docker push ${MY_ACR}.azurecr.io/${MY_APP_NAME}
```

Per informazioni più dettagliate sulla creazione e l'archiviazione di immagini di contenitore in Azure, vedere il modulo Learn [Compilare e archiviare immagini del contenitore con Registro Azure Container](/learn/modules/build-and-store-container-images/).

Se è stato usato il [repository GitHub Avvio rapido di Spring Boot in contenitori](https://github.com/Azure/spring-boot-container-quickstart), è anche possibile includere un archivio chiavi personalizzato che verrà aggiunto alla JVM all'avvio. L'archivio chiavi verrà aggiunto se si inserisce il file relativo in */opt/spring-boot/mycert.crt* . Per eseguire questa operazione, è possibile aggiungere il file direttamente al Dockerfile oppure usare il provider di Azure Key Vault per il driver CSI dell'archivio di segreti, come indicato in precedenza.

Se è stato usato il [repository GitHub Avvio rapido di Spring Boot in contenitori](https://github.com/Azure/spring-boot-container-quickstart), è anche possibile abilitare Application Insights impostando la variabile di ambiente `APPLICATIONINSIGHTS_CONNECTION_STRING` nel file di distribuzione Kubernetes (il valore della variabile di ambiente dovrebbe essere `InstrumentationKey=00000000-0000-0000-0000-000000000000`). Per altre informazioni, vedere [Monitoraggio di applicazioni codeless Java con Application Insights di Monitoraggio di Azure](/azure/azure-monitor/app/java-in-process-agent).

Se non è necessario personalizzare l'immagine Docker, è possibile scegliere se esplorare l'uso del [plug-in Jib Maven](https://github.com/GoogleContainerTools/jib/tree/master/jib-maven-plugin) o eseguire la distribuzione nel servizio Azure Kubernetes. Per altre informazioni, vedere [Distribuire un'applicazione Spring Boot nel servizio Azure Kubernetes](../spring-framework/deploy-spring-boot-java-app-on-kubernetes.md).

[!INCLUDE [provision-a-public-ip-address](includes/provision-a-public-ip-address.md)]

### <a name="deploy-to-aks"></a>Distribuire in servizio Azure Kubernetes

Creare e applicare i file YAML di Kubernetes. Per altre informazioni, vedere [Avvio rapido: Distribuire un cluster del servizio Azure Kubernetes tramite l'interfaccia della riga di comando di Azure](/azure/aks/kubernetes-walkthrough#run-the-application). Se si crea un servizio di bilanciamento del carico esterno (per l'applicazione o per un controller in ingresso), assicurarsi di fornire l'indirizzo IP di cui è stato effettuato il provisioning nella sezione precedente come `LoadBalancerIP`.

Includere i parametri esternalizzati come variabili di ambiente. Per altre informazioni, vedere [Definire le variabili di ambiente per un contenitore](https://kubernetes.io/docs/tasks/inject-data-application/define-environment-variable-container/).

Assicurarsi di includere le impostazioni della memoria e della CPU durante la creazione della distribuzione YAML, in modo che i contenitori siano dimensionati correttamente.

### <a name="ensure-console-logging-and-configure-diagnostic-settings"></a>Verificare la registrazione della console e configurare le impostazioni di diagnostica

Configurare la registrazione in modo che tutte le applicazioni registrino gli eventi nella console e non su file.

Dopo la distribuzione di un'applicazione nel servizio Azure Kubernetes, è possibile visualizzare i log usando `kubectl`.

#### <a name="logstashelk-stack"></a>Stack LogStash/ELK

Se si usa lo stack LogStash/ELK per l'aggregazione dei log, configurare l'impostazione di diagnostica per trasmettere l'output della console a un'istanza di [Hub eventi di Azure](/azure/event-hubs/). Usare quindi il plug-in [LogStash EventHub](https://github.com/logstash-plugins/logstash-input-azure_event_hubs) per inserire gli eventi registrati in LogStash.

#### <a name="splunk"></a>Splunk

Se si usa Splunk per l'aggregazione dei log, configurare l'impostazione di diagnostica per trasmettere l'output della console ad [Archiviazione BLOB di Azure](/azure/storage/blobs/). Usare quindi il [componente aggiuntivo Splunk per Servizi cloud Microsoft](https://splunkbase.splunk.com/app/3757/) per inserire gli eventi registrati in Splunk.

### <a name="migrate-and-enable-the-identity-provider"></a>Eseguire la migrazione e abilitare il provider di identità

Se le applicazioni Spring Boot richiedono l'autenticazione o l'autorizzazione, assicurarsi che siano configurate per l'accesso al provider di identità:

* Se il provider di identità è Azure Active Directory, non è necessario apportare alcuna modifica.
* Se il provider di identità è una foresta Active Directory locale, provare a implementare una soluzione di gestione delle identità ibrida con Azure Active Directory. Per indicazioni, vedere la [documentazione relativa alla soluzione ibrida di gestione delle identità](/azure/active-directory/hybrid/).
* Se il provider di identità è un'altra soluzione locale, ad esempio PingFederate, vedere l'argomento [Installazione personalizzata di Azure AD Connect](/azure/active-directory/hybrid/how-to-connect-install-custom) per configurare la federazione con Azure Active Directory. In alternativa, valutare l'uso di Spring Security per usare il provider di identità personalizzato tramite [OAuth2/OpenID Connect](https://docs.spring.io/spring-security/site/docs/current/reference/html5/#oauth2) o [SAML](https://docs.spring.io/spring-security/site/docs/current/reference/html5/#servlet-saml2).

### <a name="configure-persistent-storage"></a>Configurare la risorsa di archiviazione persistente

Se l'applicazione richiede una risorsa di archiviazione non volatile, configurare uno o più [volumi persistenti](/azure/aks/azure-disks-dynamic-pv).

[!INCLUDE [migrate-scheduled-jobs-aks](includes/migrate-scheduled-jobs-aks.md)]

## <a name="post-migration"></a>Post-migrazione

Ora che è stata eseguita la migrazione dell'applicazione al servizio Azure Kubernetes, è necessario verificare che funzioni come previsto. Al termine, sono disponibili alcune raccomandazioni per rendere l'applicazione maggiormente nativa del cloud.

### <a name="recommendations"></a>Consigli

* Valutare se aggiungere un nome DNS all'indirizzo IP allocato al controller in ingresso o al servizio di bilanciamento del carico dell'applicazione. Per altre informazioni, vedere [Creare un controller di ingresso con un indirizzo IP pubblico statico nel servizio Azure Kubernetes](/azure/aks/ingress-static-ip).

* Valutare se aggiungere [grafici HELM](https://helm.sh/docs/topics/charts/) per l'applicazione. Un grafico Helm consente di parametrizzare la distribuzione dell'applicazione per l'uso e la personalizzazione da parte di un set più diversificato di clienti.

* Progettare e implementare una strategia DevOps. Per mantenere l'affidabilità aumentando al tempo stesso la velocità di sviluppo, è consigliabile automatizzare le distribuzioni e i test con Azure Pipelines. Per altre informazioni, vedere [Compilare e distribuire nel servizio Azure Kubernetes](/azure/devops/pipelines/ecosystems/kubernetes/aks-template).

* Abilitare [Monitoraggio di Azure per il cluster](/azure/azure-monitor/insights/container-insights-enable-existing-clusters) per consentire la raccolta di log del contenitore, tenere traccia dell'utilizzo e così via.

* Valutare se esporre metriche specifiche dell'applicazione tramite Prometheus. Prometheus è un framework di metriche open source ampiamente adottato nella community di Kubernetes. È possibile configurare lo [scraping delle metriche di Prometheus in Monitoraggio di Azure](/azure/azure-monitor/insights/container-insights-prometheus-integration) invece di ospitare il proprio server Prometheus per consentire l'aggregazione delle metriche dalle applicazioni e la risposta automatizzata o l'escalation delle condizioni anomale.

* Progettare e implementare una strategia di continuità aziendale e ripristino di emergenza. Per le applicazioni cruciali, considerare un'[architettura di distribuzione in più aree](/azure/aks/operator-best-practices-multi-region).

* Vedere [Criteri di supporto della versione di Kubernetes](/azure/aks/supported-kubernetes-versions#kubernetes-version-support-policy). È responsabilità dell'utente mantenere [aggiornato il cluster del servizio Azure Kubernetes](/azure/aks/upgrade-cluster) per assicurarsi che sia sempre in esecuzione una versione supportata.

* Chiedere a tutti i membri del team responsabili dell'amministrazione del cluster e dello sviluppo di applicazioni di esaminare le [procedure consigliate per il servizio Azure Kubernetes](/azure/aks/best-practices).

* Assicurarsi che il file di distribuzione specifichi il modo in cui vengono eseguiti gli aggiornamenti in sequenza. Per altre informazioni, vedere [Distribuzione di aggiornamenti in sequenza](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#rolling-update-deployment) nella documentazione di Kubernetes.

* Configurare il ridimensionamento automatico per gestire i carichi nei periodi di picco. Per altre informazioni, vedere [Ridimensionare automaticamente un cluster per soddisfare le richieste delle applicazioni nel servizio Azure Kubernetes](/azure/aks/cluster-autoscaler).

* Valutare se [monitorare le dimensioni della cache del codice](https://docs.oracle.com/javase/8/embedded/develop-apps-platforms/codecache.htm) e aggiungere i parametri `-XX:InitialCodeCacheSize` e `-XX:ReservedCodeCacheSize` alla variabile `JAVA_OPTS` nel Dockerfile per ottimizzare ulteriormente le prestazioni.
