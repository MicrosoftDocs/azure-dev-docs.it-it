---
author: yevster
ms.author: yebronsh
ms.date: 8/25/2020
ms.openlocfilehash: 787b31e71f630c91f952afab4cc5c682d2ae9dd6
ms.sourcegitcommit: 9e282fc2ec967bee181c3034e7e70b28ae308905
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/04/2020
ms.locfileid: "89494274"
---
Ora che è stata completata la migrazione, verificare che l'applicazione funzioni come previsto. È quindi possibile rendere l'applicazione maggiormente nativa del cloud seguendo queste raccomandazioni.

* Valutare se consentire l'uso dell'applicazione con Spring Cloud Registry. In questo modo l'applicazione potrà essere individuata dinamicamente da altri microservizi e client distribuiti. Per altre informazioni, vedere [Esercitazione: Preparare un'app Java Spring per la distribuzione](/azure/spring-cloud/spring-cloud-tutorial-prepare-app-deployment). Modificare quindi tutti i client dell'applicazione per l'uso del servizio di bilanciamento del carico del client Spring. In questo modo il client può ottenere gli indirizzi di tutte le istanze dell'applicazione in esecuzione e trovarne una che funziona, se un'altra risulta danneggiata o non risponde. Per altre informazioni, vedere [Suggerimenti su Spring: Spring Cloud Load Balancer](https://spring.io/blog/2020/03/25/spring-tips-spring-cloud-loadbalancer) nel blog di Spring.

* Invece di rendere pubblica l'applicazione, è consigliabile aggiungere un'istanza di [Spring Cloud Gateway](https://cloud.spring.io/spring-cloud-gateway/reference/html/). Spring Cloud Gateway fornisce un singolo endpoint per tutte le applicazioni o i microservizi distribuiti nell'istanza di Azure Spring Cloud. Se Spring Cloud Gateway è già stato distribuito, assicurarsi che sia configurato per instradare il traffico all'applicazione appena distribuita.

* È consigliabile aggiungere un server Spring Cloud Config per gestire centralmente la configurazione e controllarne la versione per tutti i microservizi di Spring Cloud. Prima di tutto, creare un repository Git in cui ospitare la configurazione e configurare l'istanza di Azure Spring Cloud per usarlo. Per altre informazioni, vedere [Esercitazione: Configurare un'istanza di Spring Cloud Config Server per il servizio](/azure/spring-cloud/spring-cloud-tutorial-config-server). Eseguire quindi la migrazione della configurazione seguendo questa procedura:

  1. All'interno della directory *src/main/resources* dell'applicazione creare un file *bootstrap.yml* con il contenuto seguente:

        ```yml
          spring:
            application:
              name: <your-application-name>
        ```

  1. Nel repository Git di configurazione creare un file *<your-application-name>.yml*, dove `your-application-name` è uguale a quello del passaggio precedente. Spostare le impostazioni dal file *application.yml* in *src/main/resources* nel nuovo file appena creato. Se le impostazioni si trovavano in precedenza in un file con estensione *properties*, dovranno prima essere convertite in YAML. Per eseguire questa conversione, è possibile trovare gli strumenti online o i plug-in IntelliJ.

  1. Creare un file *application.yml* nella directory precedente. È possibile usare questo file per definire le impostazioni e le risorse che verranno condivise tra tutte le applicazioni nell'istanza di Azure Spring Cloud. Tali impostazioni includono in genere origini dati, impostazioni di registrazione, configurazione di Spring Boot Actuator e altro ancora.

  1. Eseguire il commit e il push delle modifiche nel repository Git.

  1. Rimuovere il file *application.properties* o *application.yml* dall'applicazione.

* Valutare la possibilità di aggiungere una pipeline di distribuzione per distribuzioni automatiche e coerenti. Sono disponibili istruzioni [per Azure Pipelines](/azure/spring-cloud/spring-cloud-howto-cicd), [per GitHub Actions](/azure/spring-cloud/spring-cloud-howto-github-actions) e [per Jenkins](/azure/jenkins/tutorial-jenkins-deploy-cli-spring-cloud-service).

* Provare a usare le distribuzioni di staging per testare le modifiche del codice in produzione prima che siano disponibili per alcuni o tutti gli utenti finali. Per altre informazioni, vedere [Configurare un ambiente di staging in Azure Spring Cloud](/azure/spring-cloud/spring-cloud-howto-staging-environment).

* Valutare la possibilità di aggiungere associazioni a servizi per connettere l'applicazione ai database di Azure supportati. Queste associazioni a servizi elimineranno la necessità di fornire le informazioni di connessione, incluse le credenziali, alle applicazioni Spring Cloud.

* È consigliabile usare la traccia distribuita e Azure Application Insights per monitorare le prestazioni e le interazioni delle applicazioni. Per altre informazioni, vedere [Usare la traccia distribuita con Azure Spring Cloud](/azure/spring-cloud/spring-cloud-tutorial-distributed-tracing).

* Valutare la possibilità di aggiungere regole di avviso e gruppi di azioni di Monitoraggio di Azure per rilevare rapidamente e risolvere le condizioni anomale. Per altre informazioni, vedere [Esercitazione: Monitorare le risorse di Spring Cloud tramite avvisi e gruppi di azioni](/azure/spring-cloud/spring-cloud-tutorial-alerts-action-groups).

* Provare a replicare la distribuzione di Azure Spring Cloud in un'altra area per una latenza più bassa e affidabilità e tolleranza di errore più elevate. Usare [Gestione traffico di Azure](/azure/traffic-manager) per bilanciare il carico tra le distribuzioni o usare [Frontdoor di Azure](/azure/frontdoor) per aggiungere l'offload SSL e Web Application Firewall con protezione DDoS.

* Se la replica geografica non è necessaria, è consigliabile aggiungere un [gateway applicazione di Azure](/azure/application-gateway) per aggiungere l'offload SSL e Web Application Firewall con protezione DDoS.
