---
title: Eseguire la migrazione delle applicazioni Spring Cloud ad Azure Spring Cloud
description: Questa guida descrive gli aspetti da considerare per la migrazione di un'applicazione Spring Cloud esistente da eseguire in Azure Spring Cloud.
author: yevster
ms.author: yebronsh
ms.topic: conceptual
ms.date: 2/12/2020
ms.custom: devx-track-java
ms.openlocfilehash: e4be32594940d2c207610e7d709ccb328c38ecf6
ms.sourcegitcommit: 39f3f69e3be39e30df28421a30747f6711c37a7b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/21/2020
ms.locfileid: "90831697"
---
# <a name="migrate-spring-cloud-applications-to-azure-spring-cloud"></a>Eseguire la migrazione delle applicazioni Spring Cloud ad Azure Spring Cloud

Questa guida descrive gli aspetti da considerare per la migrazione di un'applicazione Spring Cloud esistente da eseguire in Azure Spring Cloud.

## <a name="pre-migration"></a>Pre-migrazione

Per garantire una corretta migrazione, prima di iniziare completare i passaggi di valutazione e inventario descritti nelle sezioni seguenti.

Se non è possibile soddisfare i requisiti di pre-migrazione, vedere le guide alla migrazione complementari seguenti:

* Eseguire la migrazione di applicazioni JAR eseguibili ai contenitori nel servizio Azure Kubernetes (indicazioni in pianificazione)
* Eseguire la migrazione di applicazioni JAR eseguibili alle macchine virtuali di Azure (indicazioni in pianificazione)

### <a name="inspect-application-components"></a>Esaminare i componenti dell'applicazione

[!INCLUDE [determine-whether-and-how-the-file-system-is-used-azure-spring-cloud](includes/determine-whether-and-how-the-file-system-is-used-azure-spring-cloud.md)]

#### <a name="determine-whether-any-of-the-services-contain-os-specific-code"></a>Determinare se uno dei servizi contiene codice specifico del sistema operativo

[!INCLUDE [determine-whether-your-application-contains-os-specific-code](includes/determine-whether-your-application-contains-os-specific-code-no-title.md)]

[!INCLUDE [switch-to-a-supported-platform-azure-spring-cloud](includes/switch-to-a-supported-platform-azure-spring-cloud.md)]

[!INCLUDE [identify-spring-boot-versions](includes/identify-spring-boot-versions.md)]

Per tutte le applicazioni che usano Spring Boot 1.x, seguire la [Guida alla migrazione di Spring Boot 2.0](https://github.com/spring-projects/spring-boot/wiki/Spring-Boot-2.0-Migration-Guide) per aggiornarle a una versione di Spring Boot supportata. Per le versioni supportate, vedere [Preparare un'app Java Spring per la distribuzione](/azure/spring-cloud/spring-cloud-tutorial-prepare-app-deployment#spring-boot-and-spring-cloud-versions).

#### <a name="identify-spring-cloud-versions"></a>Identificare le versioni di Spring Cloud

Esaminare le dipendenze di ogni applicazione di cui si intende eseguire la migrazione per determinare la versione dei componenti di Spring Cloud che usa.

##### <a name="maven"></a>Maven

Nei progetti Maven la versione di Spring Cloud è in genere impostata nella proprietà `spring-cloud.version`:

```xml
  <properties>
    <java.version>1.8</java.version>
    <spring-cloud.version>Hoxton.SR3</spring-cloud.version>
  </properties>
```

##### <a name="gradle"></a>Gradle

Nei progetti Gradle la versione di Spring Cloud è in genere impostata nel blocco delle proprietà aggiuntive "extra properties":

```gradle
ext {
  set('springCloudVersion', "Hoxton.SR3")
}
```

Sarà necessario aggiornare tutte le applicazioni per usare le versioni supportate di Spring Cloud. Per un elenco delle versioni supportate, vedere [Preparare un'app Java Spring per la distribuzione](/azure/spring-cloud/spring-cloud-tutorial-prepare-app-deployment#spring-boot-and-spring-cloud-versions).

[!INCLUDE [identify-logs-metrics-apm-azure-spring-cloud.md](includes/identify-logs-metrics-apm-azure-spring-cloud.md)]

#### <a name="identify-zipkin-dependencies"></a>Identificare le dipendenze di Zipkin

Determinare se l'applicazione ha dipendenze esplicite da Zipkin. Cercare le dipendenze nel gruppo `io.zipkin.java` nelle dipendenze di Maven o Gradle.

### <a name="inventory-external-resources"></a>Inventario delle risorse esterne

Identificare le risorse esterne, ad esempio le origini dati, i broker dei messaggi JMS e gli URL di altri servizi. Nelle applicazioni Spring Cloud è in genere possibile trovare la configurazione per tali risorse in uno dei percorsi seguenti:

* Nella cartella *src/main/directory*, in un file normalmente denominato *application.properties* o *application.yml*.
* Nel repository Spring Cloud Config identificato nel passaggio precedente.

[!INCLUDE [inventory-databases-spring-boot](includes/inventory-databases-spring-boot.md)]

[!INCLUDE [identify-jms-brokers-in-spring](includes/identify-jms-brokers-in-spring.md)]

Dopo aver identificato i broker in uso, trovare le impostazioni corrispondenti. Nelle applicazioni Spring Cloud sono in genere disponibili nei file *application.properties* e *application.yml* nella directory dell'applicazione o nel repository di Spring Cloud Config Server.

[!INCLUDE [jms-broker-settings-examples-in-spring](includes/jms-broker-settings-examples-in-spring.md)]

[!INCLUDE [identify-external-caches-azure-spring-cloud](includes/identify-external-caches-azure-spring-cloud.md)]

#### <a name="identity-providers"></a>Provider di identità

Identificare tutti i provider di identità e tutte le applicazioni Spring Cloud che richiedono l'autenticazione e/o l'autorizzazione. Per informazioni sul modo in cui è possibile configurare i provider di identità, consultare gli argomenti seguenti:

* Per la configurazione di OAuth2, vedere la guida di [avvio rapido su Spring Cloud Security](https://cloud.spring.io/spring-cloud-security/2.1.x/multi/multi__quickstart.html#_quickstart).
* Per la configurazione di Auth0 Spring Security, vedere la [documentazione di Auth0 Spring Security](https://auth0.com/docs/quickstart/backend/java-spring-security5/01-authorization).
* Per la configurazione della sicurezza di PingFederate Spring Security, vedere le [istruzioni di Auth0 PingFederate](https://auth0.com/authenticate/java-spring-security/ping-federate/).

#### <a name="resources-configured-through-vmware-tanzu-application-service-tas-formerly-pivotal-cloud-foundry"></a>Risorse configurate tramite il servizio VMware TAS (Tanzu Application Service) (in precedenza Pivotal Cloud Foundry)

Per le applicazioni gestite con TAS, le risorse esterne, incluse quelle descritte in precedenza, vengono spesso configurate tramite le associazioni del servizio TAS. Per esaminare la configurazione di tali risorse, usare l'[interfaccia della riga di comando di TAS (Cloud Foundry)](https://docs.cloudfoundry.org/cf-cli/) per visualizzare la variabile `VCAP_SERVICES` per l'applicazione.

```bash
# Log into TAS, if needed (enter credentials when prompted)
cf login -a <API endpoint>

# Set the organization and space containing the application, if not already selected during login.
cf target org <organization name>
cf target space <space name>

# Display variables for the application
cf env <Application Name>
```

Esaminare la variabile `VCAP_SERVICES` per le impostazioni di configurazione dei servizi esterni associati all'applicazione. Per altre informazioni, vedere la [documentazione di TAS (Cloud Foundry)](https://docs.cloudfoundry.org/devguide/deploy-apps/environment-variable.html#VCAP-SERVICES).

#### <a name="all-other-external-resources"></a>Tutte le altre risorse esterne

Non è possibile documentare tutte le possibili dipendenze esterne in questa guida. Dopo la migrazione, è responsabilità dell'utente verificare che sia possibile soddisfare tutte le dipendenze esterne dell'applicazione.

[!INCLUDE [inventory-configuration-sources-and-secrets-spring-cloud](includes/inventory-configuration-sources-and-secrets-spring-cloud.md)]

[!INCLUDE [inspect-the-deployment-architecture-spring-cloud](includes/inspect-the-deployment-architecture-spring-cloud.md)]

## <a name="migration"></a>Migrazione

### <a name="remove-explicit-configuration-server-settings"></a>Rimuovere le impostazioni del server di configurazione esplicite

Nei servizi di cui verrà eseguita la migrazione individuare le assegnazioni esplicite delle impostazioni Eureka e rimuoverle. Tali impostazioni sono in genere disponibili nei file *application.properties* o *application.yml*:

**application.yml**

```yaml
eureka:
  client:
    serviceUrl:
      defaultZone: http://myusername:mysecretpassword@localhost:8761/eureka/
```

Se un'impostazione come questa viene visualizzata nella configurazione dell'applicazione, rimuoverla. Azure Spring Cloud inserirà automaticamente le informazioni di connessione del relativo server di configurazione.

### <a name="create-an-azure-spring-cloud-instance-and-apps"></a>Creare un'istanza di Azure Spring Cloud e le app

Effettuare il provisioning di un'istanza di Azure Spring Cloud nella sottoscrizione di Azure. Effettuare quindi il provisioning di un'app per ogni servizio di cui verrà eseguita la migrazione. Non includere i server di configurazione e registro di Spring Cloud. Includere il servizio Spring Cloud Gateway. Per le istruzioni, vedere [Avvio rapido: Avviare un'applicazione Azure Spring Cloud esistente con il portale di Azure](/azure/spring-cloud/spring-cloud-quickstart-launch-app-portal).

### <a name="prepare-the-spring-cloud-config-server"></a>Preparare il server di configurazione di Spring Cloud

Configurare il server di configurazione nell'istanza di Azure Spring Cloud. Per altre informazioni, vedere [Esercitazione: Configurare un'istanza di Spring Cloud Config Server per il servizio](/azure/spring-cloud/spring-cloud-tutorial-config-server).

> [!NOTE]
> Se il repository di configurazione di Spring Cloud corrente si trova nel file system locale o nell'ambiente locale, è prima necessario eseguire la migrazione o la replica dei file di configurazione in un repository privato basato sul cloud, ad esempio GitHub, Azure Repos o BitBucket.

[!INCLUDE [ensure-console-logging-and-configure-diagnostic-settings-azure-spring-cloud](includes/ensure-console-logging-and-configure-diagnostic-settings-azure-spring-cloud.md)]

[!INCLUDE [configure-persistent-storage-azure-spring-cloud](includes/configure-persistent-storage-azure-spring-cloud.md)]

### <a name="migrate-spring-cloud-vault-secrets-to-azure-keyvault"></a>Eseguire la migrazione dei segreti dell'insieme di credenziali di Spring Cloud ad Azure Key Vault

È possibile inserire i segreti direttamente nelle applicazioni tramite Spring usando Spring Boot Starter per Azure Key Vault. Per altre informazioni, vedere [Come usare Spring Boot Starter per Azure Key Vault](../spring-framework/configure-spring-boot-starter-java-app-with-azure-key-vault.md).

> [!NOTE]
> Per la migrazione potrebbe essere necessario rinominare alcuni segreti. Aggiornare il codice dell'applicazione di conseguenza.

### <a name="migrate-all-certificates-to-keyvault"></a>Eseguire la migrazione di tutti i certificati a Key Vault

Azure Spring Cloud non fornisce l'accesso all'archivio chiavi JRE, quindi è necessario eseguire la migrazione dei certificati ad Azure Key Vault e modificare il codice dell'applicazione per accedere ai certificati in Key Vault. Per altre informazioni, vedere [Introduzione ai certificati Key Vault](/azure/key-vault/certificates/certificate-scenarios) e [Libreria client dei certificati di Azure Key Vault per Java](/java/api/overview/azure/security-keyvault-certificates-readme).

### <a name="remove-application-performance-management-apm-integrations"></a>Rimuovere le integrazioni di gestione delle prestazioni delle applicazioni

Eliminare tutte le integrazioni con gli strumenti o gli agenti di gestione delle prestazioni delle applicazioni. Per informazioni sulla configurazione della gestione delle prestazioni con Monitoraggio di Azure, vedere la sezione [Post-migrazione](#post-migration).

### <a name="replace-explicit-zipkin-dependencies-with-spring-cloud-starters"></a>Sostituire le dipendenze di Zipkin esplicite con le utilità di avvio Spring Cloud

Se una delle applicazioni migrate presenta dipendenze di Zipkin esplicite, rimuoverle e sostituirle con le utilità di avvio Spring Cloud, come descritto nella sezione [Dipendenza Traccia distribuita](/azure/spring-cloud/spring-cloud-tutorial-prepare-app-deployment#distributed-tracing-dependency) di [Preparare un'applicazione Java Spring per la distribuzione in Azure Spring Cloud](/azure/spring-cloud/spring-cloud-tutorial-prepare-app-deployment). Per informazioni sulla traccia distribuita con Azure Application Insights, vedere la sezione [Post-migrazione](#post-migration).

### <a name="disable-metrics-clients-and-endpoints-in-your-applications"></a>Disabilitare i client e gli endpoint delle metriche nelle applicazioni

Rimuovere tutti i client o gli endpoint delle metriche esposti nelle applicazioni.

### <a name="deploy-the-services"></a>Distribuire i servizi

Distribuire tutti i microservizi sottoposti a migrazione, esclusi i server di configurazione e registro di Spring Cloud, come descritto nell'[Avvio rapido: Avviare un'applicazione Azure Spring Cloud esistente con il portale di Azure](/azure/spring-cloud/spring-cloud-quickstart-launch-app-portal).

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
* Se il provider di identità è una foresta Active Directory locale, provare a implementare una soluzione di gestione delle identità ibrida con Azure Active Directory. Per indicazioni, vedere la [documentazione relativa alla soluzione ibrida di gestione delle identità](/azure/active-directory/hybrid/).
* Se il provider di identità è un'altra soluzione locale, ad esempio PingFederate, vedere l'argomento [Installazione personalizzata di Azure AD Connect](/azure/active-directory/hybrid/how-to-connect-install-custom) per configurare la federazione con Azure Active Directory. In alternativa, valutare l'uso di Spring Security per usare il provider di identità personalizzato tramite [OAuth2/OpenID Connect](https://docs.spring.io/spring-security/site/docs/current/reference/html5/#oauth2) o [SAML](https://docs.spring.io/spring-security/site/docs/current/reference/html5/#servlet-saml2).

### <a name="update-client-applications"></a>Aggiornare le applicazioni client

Aggiornare la configurazione di tutte le applicazioni client in modo da usare gli endpoint Azure Spring Cloud pubblicati per le applicazioni migrate.

## <a name="post-migration"></a>Post-migrazione

* Valutare la possibilità di aggiungere una pipeline di distribuzione per distribuzioni automatiche e coerenti. Sono disponibili istruzioni [per Azure Pipelines](/azure/spring-cloud/spring-cloud-howto-cicd), [per GitHub Actions](/azure/spring-cloud/spring-cloud-howto-github-actions) e [per Jenkins](/azure/jenkins/tutorial-jenkins-deploy-cli-spring-cloud-service).

* Provare a usare le distribuzioni di staging per testare le modifiche del codice in produzione prima che siano disponibili per alcuni o tutti gli utenti finali. Per altre informazioni, vedere [Configurare un ambiente di staging in Azure Spring Cloud](/azure/spring-cloud/spring-cloud-howto-staging-environment).

* Valutare la possibilità di aggiungere associazioni a servizi per connettere l'applicazione ai database di Azure supportati. Queste associazioni a servizi elimineranno la necessità di fornire le informazioni di connessione, incluse le credenziali, alle applicazioni Spring Cloud.

* Prendere in considerazione l'[uso della traccia distribuita e di Azure Application Insights](/azure/spring-cloud/spring-cloud-tutorial-distributed-tracing) per monitorare le prestazioni e le interazioni delle applicazioni.

* Valutare la possibilità di aggiungere regole di avviso e gruppi di azioni di Monitoraggio di Azure per rilevare rapidamente e risolvere le condizioni anomale. Per altre informazioni, vedere [Esercitazione: Monitorare le risorse di Spring Cloud tramite avvisi e gruppi di azioni](/azure/spring-cloud/spring-cloud-tutorial-alerts-action-groups).

* Provare a replicare la distribuzione di Azure Spring Cloud in un'altra area per una latenza più bassa e affidabilità e tolleranza di errore più elevate. Usare [Gestione traffico di Azure](/azure/traffic-manager) per bilanciare il carico tra le distribuzioni o usare [Frontdoor di Azure](/azure/frontdoor) per aggiungere l'offload SSL e Web Application Firewall con protezione DDoS.

* Se la replica geografica non è necessaria, è consigliabile aggiungere un [gateway applicazione di Azure](/azure/application-gateway) per aggiungere l'offload SSL e Web Application Firewall con protezione DDoS.

* Se le applicazioni usano i componenti legacy di Spring Cloud Netflix, provare a sostituirli con le alternative correnti:

   | Legacy                        | Corrente                                                |
   |-------------------------------|--------------------------------------------------------|
   | Spring Cloud Eureka           | Spring Cloud Service Registry                          |
   | Spring Cloud Netflix Zuul     | Spring Cloud Gateway                                   |
   | Spring Cloud Netflix Archaius | Spring Cloud Config Server                             |
   | Spring Cloud Netflix Ribbon   | Spring Cloud Load Balancer (servizio di bilanciamento del carico lato client) |
   | Spring Cloud Hystrix          | Spring Cloud Circuit Breaker + Resilience4J            |
   | Spring Cloud Netflix Turbine  | Micrometer + Prometheus                                |