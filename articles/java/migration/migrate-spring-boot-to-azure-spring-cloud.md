---
title: Eseguire la migrazione di applicazioni Spring Boot ad Azure Spring Cloud
description: Questa guida descrive gli aspetti da considerare per la migrazione di un'applicazione Spring Boot esistente da eseguire in Azure Spring Cloud.
author: yevster
ms.author: yebronsh
ms.topic: conceptual
ms.date: 5/26/2020
ms.custom: devx-track-java
ms.openlocfilehash: 7bc4a5188181f3b4b6d98b5308a5027a42bbac5e
ms.sourcegitcommit: b224b276a950b1d173812f16c0577f90ca2fbff4
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/05/2020
ms.locfileid: "87810584"
---
# <a name="migrate-spring-boot-applications-to-azure-spring-cloud"></a>Eseguire la migrazione di applicazioni Spring Boot ad Azure Spring Cloud

Questa guida descrive gli aspetti da considerare per la migrazione di un'applicazione Spring Boot esistente da eseguire in Azure Spring Cloud.

## <a name="pre-migration"></a>Pre-migrazione

Per garantire una corretta migrazione, prima di iniziare completare i passaggi di valutazione e inventario descritti nelle sezioni seguenti.

Se non è possibile soddisfare questi requisiti di pre-migrazione, vedere le guide alla migrazione complementari seguenti:

* Eseguire la migrazione di applicazioni JAR eseguibili ai contenitori nel servizio Azure Kubernetes (indicazioni in pianificazione)
* Eseguire la migrazione di applicazioni JAR eseguibili alle macchine virtuali di Azure (indicazioni in pianificazione)

### <a name="inspect-application-components"></a>Esaminare i componenti dell'applicazione

[!INCLUDE [identify-local-state](includes/identify-local-state-azure-spring-cloud.md)]

[!INCLUDE [static-content-azure-spring-cloud](includes/determine-whether-and-how-the-file-system-is-used-azure-spring-cloud.md)]

#### <a name="determine-whether-any-of-the-services-contain-os-specific-code"></a>Determinare se uno dei servizi contiene codice specifico del sistema operativo

[!INCLUDE [determine-whether-your-application-contains-os-specific-code](includes/determine-whether-your-application-contains-os-specific-code-no-title.md)]

[!INCLUDE [switch-to-a-supported-platform-azure-spring-cloud](includes/switch-to-a-supported-platform-azure-spring-cloud.md)]

[!INCLUDE [identify-spring-boot-versions](includes/identify-spring-boot-versions.md)]

Per tutte le applicazioni che usano Spring Boot 1.x, seguire la [Guida alla migrazione di Spring Boot 2.0](https://github.com/spring-projects/spring-boot/wiki/Spring-Boot-2.0-Migration-Guide) per aggiornarle a una versione di Spring Boot supportata. Per le versioni supportate, vedere [Preparare un'app Java Spring per la distribuzione](/azure/spring-cloud/spring-cloud-tutorial-prepare-app-deployment#spring-boot-and-spring-cloud-versions).

[!INCLUDE [identify-logs-metrics-apm-azure-spring-cloud.md](includes/identify-logs-metrics-apm-azure-spring-cloud.md)]

### <a name="inventory-external-resources"></a>Inventario delle risorse esterne

Identificare le risorse esterne, ad esempio le origini dati, i broker dei messaggi JMS e gli URL di altri servizi. Nelle applicazioni Spring Boot la configurazione per tali risorse si trova di solito nella cartella *src/main/directory*, in un file generalmente denominato *application.properties* o *application.yml*.

[!INCLUDE [inventory-databases-spring-boot](includes/inventory-databases-spring-boot.md)]

[!INCLUDE [identify-jms-brokers-in-spring](includes/identify-jms-brokers-in-spring.md)]

Dopo aver identificato i broker in uso, trovare le impostazioni corrispondenti. Nelle applicazioni Spring Boot si trovano in genere nei file *application.properties* e *application.yml* nella directory dell'applicazione.

[!INCLUDE [jms-broker-settings-examples-in-spring](includes/jms-broker-settings-examples-in-spring.md)]

[!INCLUDE [identify-external-caches-azure-spring-cloud](includes/identify-external-caches-azure-spring-cloud.md)]

[!INCLUDE [inventory-identity-providers-spring-boot.md](includes/inventory-identity-providers-spring-boot.md)]

#### <a name="identify-any-clients-relying-on-a-non-standard-port"></a>Identificare i client che si basano su una porta non standard

Azure Spring Cloud sovrascrive l'impostazione di `server.port` nell'applicazione distribuita. Se eventuali client dipendono dalla disponibilità dell'applicazione su una porta diversa da 443, sarà necessario modificarli.

#### <a name="all-other-external-resources"></a>Tutte le altre risorse esterne

Non è possibile documentare tutte le possibili dipendenze esterne in questa guida. Dopo la migrazione, è responsabilità dell'utente verificare che sia possibile soddisfare tutte le dipendenze esterne dell'applicazione.

[!INCLUDE [inventory-configuration-sources-and-secrets-spring-boot](includes/inventory-configuration-sources-and-secrets-spring-boot.md)]

[!INCLUDE [inspect-the-deployment-architecture-spring-boot](includes/inspect-the-deployment-architecture-spring-boot.md)]

## <a name="migration"></a>Migrazione

### <a name="create-an-azure-spring-cloud-instance-and-apps"></a>Creare un'istanza di Azure Spring Cloud e le app

Effettuare il provisioning di un'istanza di Azure Spring Cloud nella sottoscrizione di Azure, se non ne esiste già una. Quindi, crearvi un'applicazione. Per altre informazioni, vedere [Avvio rapido: Avviare un'applicazione Azure Spring Cloud esistente con il portale di Azure](/azure/spring-cloud/spring-cloud-quickstart-launch-app-portal).

[!INCLUDE [ensure-console-logging-and-configure-diagnostic-settings-azure-spring-cloud](includes/ensure-console-logging-and-configure-diagnostic-settings-azure-spring-cloud.md)]

[!INCLUDE [configure-persistent-storage-azure-spring-cloud](includes/configure-persistent-storage-azure-spring-cloud.md)]

### <a name="migrate-all-certificates-to-keyvault"></a>Eseguire la migrazione di tutti i certificati a Key Vault

Azure Spring Cloud non fornisce l'accesso all'archivio chiavi JRE, quindi è necessario eseguire la migrazione dei certificati ad Azure Key Vault e modificare il codice dell'applicazione per accedere ai certificati in Key Vault. Per altre informazioni, vedere [Introduzione ai certificati Key Vault](/azure/key-vault/certificates/certificate-scenarios) e [Libreria client dei certificati di Azure Key Vault per Java](/java/api/overview/azure/security-keyvault-certificates-readme).

### <a name="remove-application-performance-management-apm-integrations"></a>Rimuovere le integrazioni di gestione delle prestazioni delle applicazioni

Eliminare tutte le integrazioni con gli strumenti o gli agenti di gestione delle prestazioni delle applicazioni. Per informazioni sulla configurazione della gestione delle prestazioni con Monitoraggio di Azure, vedere la sezione [Post-migrazione](#post-migration).

### <a name="disable-metrics-clients-and-endpoints-in-your-applications"></a>Disabilitare i client e gli endpoint delle metriche nelle applicazioni

Rimuovere tutti i client o gli endpoint delle metriche esposti nelle applicazioni.

### <a name="deploy-the-application"></a>Distribuire l'applicazione

Distribuire tutti i microservizi sottoposti a migrazione, esclusi i server Spring Cloud Config e Registry, come descritto in [Avvio rapido: Avviare un'applicazione Azure Spring Cloud esistente con il portale di Azure](/azure/spring-cloud/spring-cloud-quickstart-launch-app-portal).

### <a name="configure-per-service-secrets-and-externalized-settings"></a>Configurare i segreti per servizio e le impostazioni esternalizzate

È possibile inserire tutte le impostazioni di configurazione per servizio in ogni servizio come variabili di ambiente. Nel portale di Azure seguire questa procedura:

1. Passare all'istanza di Azure Spring Cloud e selezionare **App**.
1. Selezionare il servizio da configurare.
1. Selezionare **Configurazione**.
1. Immettere le variabili da configurare.
1. Selezionare **Salva**.

![Impostazioni di configurazione app Spring Cloud](media/migrate-spring-cloud-to-azure-spring-cloud/spring-cloud-app-configuration-settings.png)

### <a name="migrate-and-enable-the-identity-provider"></a>Eseguire la migrazione e abilitare il provider di identità

Se le applicazioni Spring Cloud richiedono l'autenticazione o l'autorizzazione, assicurarsi che siano configurate per l'accesso al provider di identità:

* Se il provider di identità è Azure Active Directory, non è necessario apportare alcuna modifica.
* Se il provider di identità è una foresta Active Directory locale, provare a implementare una soluzione di gestione delle identità ibrida con Azure Active Directory. Per altre informazioni, vedere la [documentazione delle identità ibride](/azure/active-directory/hybrid/).
* Se il provider di identità è un'altra soluzione locale, ad esempio PingFederate, vedere l'argomento [Installazione personalizzata di Azure AD Connect](/azure/active-directory/hybrid/how-to-connect-install-custom) per configurare la federazione con Azure Active Directory. In alternativa, valutare l'uso di Spring Security per usare il provider di identità personalizzato tramite [OAuth2/OpenID Connect](https://docs.spring.io/spring-security/site/docs/current/reference/html5/#oauth2) o [SAML](https://docs.spring.io/spring-security/site/docs/current/reference/html5/#servlet-saml2).

### <a name="expose-the-application"></a>Esporre l'applicazione

Per impostazione predefinita, le applicazioni distribuite in Azure Spring Cloud non sono visibili esternamente. È possibile esporre l'applicazione rendendola pubblica con il comando seguente:

```azurecli
az spring-cloud app update -n <application name> --is-public true
```

Ignorare questo passaggio se si usa o si intende usare Spring Cloud Gateway. Per altre informazioni, vedere la sezione seguente.

## <a name="post-migration"></a>Post-migrazione

Ora che è stata completata la migrazione, verificare che l'applicazione funzioni come previsto. È quindi possibile rendere l'applicazione maggiormente nativa del cloud seguendo queste raccomandazioni.

* Valutare se consentire l'uso dell'applicazione con Spring Cloud Registry. In questo modo l'applicazione potrà essere individuata dinamicamente da altri microservizi e client distribuiti. Per altre informazioni, vedere [Esercitazione: Preparare un'app Java Spring per la distribuzione](/azure/spring-cloud/spring-cloud-tutorial-prepare-app-deployment). Modificare quindi tutti i client dell'applicazione per l'uso del servizio di bilanciamento del carico del client Spring. In questo modo il client può ottenere gli indirizzi di tutte le istanze dell'applicazione in esecuzione e trovarne una che funziona, se un'altra risulta danneggiata o non risponde. Per altre informazioni, vedere [Suggerimenti su Spring: Spring Cloud LoadBalancer](https://spring.io/blog/2020/03/25/spring-tips-spring-cloud-loadbalancer) nel blog di Spring.

* Invece di rendere pubblica l'applicazione, è consigliabile aggiungere un'istanza di [Spring Cloud Gateway](https://cloud.spring.io/spring-cloud-gateway/reference/html/). Spring Cloud Gateway fornisce un singolo endpoint per tutte le applicazioni o i microservizi distribuiti nell'istanza di Azure Spring Cloud. Se Spring Cloud Gateway è già stato distribuito, assicurarsi che sia configurato per instradare il traffico all'applicazione appena distribuita.

* È consigliabile aggiungere un server Spring Cloud Config per gestire centralmente la configurazione e controllarne la versione per tutti i microservizi di Spring Cloud. Prima di tutto, creare un repository Git in cui ospitare la configurazione e configurare l'istanza di Azure Spring Cloud per usarlo. Per altre informazioni, vedere [Esercitazione: Configurare un'istanza di Spring Cloud Config Server per il servizio](/azure/spring-cloud/spring-cloud-tutorial-config-server). Eseguire quindi la migrazione della configurazione seguendo questa procedura:

  1. Creare una directory nel repository Git di configurazione con lo stesso nome dell'applicazione definita nell'istanza di Azure Spring Cloud.

  1. All'interno di questa directory creare un file *bootstrap.yml* con il contenuto seguente:

     ```yml
     spring:
       application:
         name: <Your the application name used in the previous step>
     ```

  1. Creare un file *application.yml* all'intero della directory indicata sopra e quindi spostarvi le impostazioni dell'applicazione. Se le impostazioni si trovavano in precedenza in un file con estensione *properties*, dovranno essere convertite in YAML.

  1. Eseguire il commit e il push delle modifiche nel repository Git.

* Valutare la possibilità di aggiungere una pipeline di distribuzione per distribuzioni automatiche e coerenti. Sono disponibili istruzioni [per Azure Pipelines](/azure/spring-cloud/spring-cloud-howto-cicd), [per GitHub Actions](/azure/spring-cloud/spring-cloud-howto-github-actions) e [per Jenkins](/azure/jenkins/tutorial-jenkins-deploy-cli-spring-cloud-service).

* Provare a usare le distribuzioni di staging per testare le modifiche del codice in produzione prima che siano disponibili per alcuni o tutti gli utenti finali. Per altre informazioni, vedere [Configurare un ambiente di staging in Azure Spring Cloud](/azure/spring-cloud/spring-cloud-howto-staging-environment).

* Valutare la possibilità di aggiungere associazioni a servizi per connettere l'applicazione ai database di Azure supportati. Queste associazioni a servizi elimineranno la necessità di fornire le informazioni di connessione, incluse le credenziali, alle applicazioni Spring Cloud.

* È consigliabile usare la traccia distribuita e Azure Application Insights per monitorare le prestazioni e le interazioni delle applicazioni. Per altre informazioni, vedere [Usare la traccia distribuita con Azure Spring Cloud](/azure/spring-cloud/spring-cloud-tutorial-distributed-tracing).

* Valutare la possibilità di aggiungere regole di avviso e gruppi di azioni di Monitoraggio di Azure per rilevare rapidamente e risolvere le condizioni anomale. Per altre informazioni, vedere [Esercitazione: Monitorare le risorse di Spring Cloud tramite avvisi e gruppi di azioni](/azure/spring-cloud/spring-cloud-tutorial-alerts-action-groups).

* Provare a replicare la distribuzione di Azure Spring Cloud in un'altra area per una latenza più bassa e affidabilità e tolleranza di errore più elevate. Usare [Gestione traffico di Azure](/azure/traffic-manager) per bilanciare il carico tra le distribuzioni o usare [Frontdoor di Azure](/azure/frontdoor) per aggiungere l'offload SSL e Web Application Firewall con protezione DDoS.

* Se la replica geografica non è necessaria, è consigliabile aggiungere un [gateway applicazione di Azure](/azure/application-gateway) per aggiungere l'offload SSL e Web Application Firewall con protezione DDoS.
