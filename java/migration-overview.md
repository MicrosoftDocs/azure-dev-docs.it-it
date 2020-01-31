---
title: Eseguire la migrazione di applicazioni Java ad Azure
description: Questo argomento fornisce una panoramica delle strategie consigliate per la migrazione di applicazioni Java ad Azure.
author: yevster
ms.author: yebronsh
ms.topic: conceptual
ms.date: 1/20/2020
ms.openlocfilehash: d32c38d763901152135b965484362031dfac7f0a
ms.sourcegitcommit: 367780fe48d977c82cb84208c128b0bf694b1029
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2020
ms.locfileid: "76825795"
---
# <a name="migrate-java-applications-to-azure"></a>Eseguire la migrazione di applicazioni Java ad Azure

Questo argomento fornisce una panoramica delle strategie consigliate per la migrazione di applicazioni Java ad Azure.

## <a name="identifying-application-type"></a>Identificazione del tipo di applicazione

Prima di selezionare una destinazione cloud per l'applicazione Java, è necessario identificare il tipo di applicazione. La maggior parte delle applicazioni Java è di uno dei seguenti tipi:

* [Applicazioni Spring Boot/JAR](#spring-boot--jar-applications)
* [Spring Cloud/microservizi](#spring-cloud--microservices)
* [Applicazioni Web](#web-applications)
* [Applicazioni Java EE](#java-ee-applications)
* [Processi batch/pianificati](#batch--scheduled-jobs)

Questi tipi vengono descritti nelle sezioni seguenti.

### <a name="spring-boot--jar-applications"></a>Applicazioni Spring Boot/JAR

Molte applicazioni più recenti vengono richiamate direttamente dalla riga di comando. Queste applicazioni gestiscono comunque le richieste Web, ma invece di basarsi su un server applicazioni per fornire la gestione delle richieste HTTP, incorporano la comunicazione HTTP e tutte le altre dipendenze direttamente nel pacchetto dell'applicazione. Queste applicazioni vengono spesso compilate con framework come Spring Boot, Dropwizard, Micronaut, MicroProfile, Vert.x e altri.

Queste applicazioni vengono compresse in archivi con l'estensione *jar* (file JAR).

### <a name="spring-cloud--microservices"></a>Spring Cloud/microservizi

Lo stile di architettura dei microservizi è un approccio per sviluppare una singola applicazione come gruppo di piccoli servizi, ognuno in esecuzione nel proprio processo e che comunica con meccanismi leggeri, spesso un'API di risorse HTTP. Questi servizi sono compilati in base alle funzionalità aziendali e possono essere distribuiti in modo indipendente da meccanismi di distribuzione completamente automatizzati. Esiste una gestione centralizzata minima dei servizi, che può essere scritta in linguaggi di programmazione diversi e usare tecnologie di archiviazione dati diverse. Tali servizi sono spesso compilati con framework come Spring Cloud.

Questi servizi vengono compressi in più applicazioni con l'estensione *jar* (file JAR).

### <a name="web-applications"></a>Applicazioni Web

Le applicazioni Web vengono eseguite all'interno di un contenitore [Servlet](https://en.wikipedia.org/wiki/Java_servlet). Alcune usano direttamente le API servlet, mentre molte usano framework aggiuntivi che incapsulano le API servlet come Apache Struts, Spring MVC, JavaServer Faces (JSF) e altre.

Le applicazioni Web vengono compresse in archivi con l'estensione *war* (file WAR).

### <a name="java-ee-applications"></a>Applicazioni Java EE

Le applicazioni Java EE, denominate anche applicazioni J2EE o, più di recente, applicazioni JakartaEE, possono contenere alcuni, tutti o nessuno degli elementi delle applicazioni Web. Possono inoltre contenere e utilizzare molti componenti aggiuntivi, come definito dalla [specifica Java EE](https://en.wikipedia.org/wiki/Java_Platform,_Enterprise_Edition).

Le applicazioni Java EE possono essere compresse come archivi con l'estensione *ear* (file EAR) o come archivi con l'estensione *war* (file WAR).

Le applicazioni Java EE devono essere distribuite in server applicazioni compatibili con Java EE (ad esempio WebLogic, WebSphere, WildFly, GlassFish, Payara e altri).

È possibile eseguire la migrazione delle applicazioni che si basano solo sulle funzionalità fornite dalla specifica Java EE, ovvero le applicazioni indipendenti dal server app, da un server applicazioni conforme a un altro. Se l'applicazione dipende da un server applicazioni specifico (dipendente dal server app), potrebbe essere necessario selezionare una destinazione del servizio di Azure che consenta di ospitare tale server applicazioni.

### <a name="batch--scheduled-jobs"></a>Processi batch/pianificati

Alcune applicazioni sono progettate per essere eseguite brevemente, eseguire un carico di lavoro specifico e quindi terminare anziché attendere richieste o input dell'utente. A volte tali processi devono essere eseguiti una sola volta o a intervalli regolari e pianificati. In locale, i processi di questo tipo vengono spesso richiamati dal crontab di un server.

Queste applicazioni vengono compresse in archivi con l'estensione *jar* (file JAR).

> [!NOTE]
> Se l'applicazione usa un'utilità di pianificazione, ad esempio Spring Batch o Quartz, per eseguire le attività pianificate, è consigliabile eseguire tali attività all'esterno dell'applicazione. Se l'applicazione viene ridimensionata in più istanze nel cloud, lo stesso processo viene eseguito più di una volta. Inoltre, se il meccanismo di pianificazione usa il fuso orario locale dell'host, è possibile che si verifichi un comportamento indesiderato durante il ridimensionamento dell'applicazione tra le aree.

## <a name="selecting-the-target-azure-service-destination"></a>Selezione della destinazione del servizio di Azure di destinazione

Le sezioni seguenti illustrano quali destinazioni del servizio soddisfano i requisiti dell'applicazione e quali responsabilità comportano.

### <a name="feature-grid"></a>Griglia delle funzionalità

Usare la griglia seguente per identificare le destinazioni che supportano i tipi di applicazione e le funzionalità necessarie.

|   |App<br>Service<br>Java SE|App<br>Service<br>Tomcat|App<br>Service<br>WildFly|Azure<br>Spring<br>Cloud|Servizio Azure Kubernetes|Macchine virtuali|
|---|---|---|---|---|---|---|
| Applicazioni Spring Boot/JAR                                    |&#x2714;|        |        |        |&#x2714;|&#x2714;|
| Spring Cloud/microservizi                                      |        |        |        |&#x2714;|&#x2714;|&#x2714;|
| Applicazioni Web                                                  |        |&#x2714;|&#x2714;|        |&#x2714;|&#x2714;|
| Applicazioni Java EE                                              |        |        |&#x2714;|        |&#x2714;|&#x2714;|
| Server applicazioni commerciali<br>(ad esempio WebLogic o WebSphere) |        |        |        |        |&#x2714;|&#x2714;|
| Persistenza a lungo termine nel file system locale                         |&#x2714;|&#x2714;|&#x2714;|        |&#x2714;|&#x2714;|
| Clustering delle applicazioni a livello di server                               |        |        |        |        |&#x2714;|&#x2714;|
| Processi batch/pianificati                                            |        |        |        |&#x2714;|&#x2714;|&#x2714;|

### <a name="ongoing-responsibility-grid"></a>Griglia di responsabilità continuative

Usare la griglia seguente per comprendere la responsabilità assegnata al team per ogni destinazione dopo la migrazione.

Il team è responsabile in maniera continuativa delle attività indicate con "&#x1F449;". Si consiglia di implementare un processo solido e altamente automatizzato per soddisfare tutte queste responsabilità. 

> [!NOTE]
> Questo non è un elenco completo di responsabilità.

|   | Servizio app | Azure Spring Cloud | Servizio Azure Kubernetes | Macchine virtuali |
|---|---|---|---|---|
| Aggiornamento delle librerie<br>(compresa la correzione delle vulnerabilità)                 | &#x1F449;   | &#x1F449;   | &#x1F449;   | &#x1F449; |
| Aggiornamento del server applicazioni<br>(compresa la correzione delle vulnerabilità)    | ![Azure][1] | ![Azure][1] | &#x1F449;   | &#x1F449; |
| Aggiornamento di Java Runtime<br>(compresa la correzione delle vulnerabilità)          | ![Azure][1] | ![Azure][1] | &#x1F449;   | &#x1F449; |
| Attivazione degli aggiornamenti di Kubernetes<br>(eseguito da Azure con un trigger manuale) | N/D         | N/D         | &#x1F449;   | N/D       |
| Risoluzione delle differenze per le modifiche all'API Kubernetes non compatibili con le versioni precedenti                  | N/D         | N/D         | &#x1F449;   | N/D       |
| Aggiornamento dell'immagine di base del contenitore<br>(compresa la correzione delle vulnerabilità)      | N/D         | N/D         | &#x1F449;   | N/D       |
| Aggiornamento del sistema operativo<br>(compresa la correzione delle vulnerabilità)      | ![Azure][1] | ![Azure][1] | ![Azure][1] | &#x1F449; |
| Rilevamento e riavvio delle istanze non riuscite                                   | ![Azure][1] | ![Azure][1] | ![Azure][1] | &#x1F449; |
| Implementazione dello svuotamento e del riavvio in sequenza degli aggiornamenti                       | ![Azure][1] | ![Azure][1] | ![Azure][1] | &#x1F449; |
| Gestione dell'infrastruttura                                                   | ![Azure][1] | ![Azure][1] | &#x1F449;   | &#x1F449; |
| Monitoraggio e gestione degli avvisi                                             | &#x1F449;   | &#x1F449;   | &#x1F449;   | &#x1F449; |

Se si distribuisce il contenitore servlet (ad esempio Spring Boot) come parte dell'applicazione, è necessario considerarlo come una libreria e, di conseguenza, è sempre responsabilità dell'utente.

## <a name="ensuring-on-premises-connectivity"></a>Verifica della connettività locale

Se l'applicazione deve accedere ai servizi locali, è necessario effettuare il provisioning di uno dei servizi di connettività di Azure. Per altre informazioni, vedere [Scegliere una soluzione per la connessione di una rete locale ad Azure](/azure/architecture/reference-architectures/hybrid-networking/). In alternativa, è necessario effettuare il refactoring dell'applicazione per usare le API disponibili pubblicamente esposte dalle risorse locali.

Prima di avviare qualsiasi migrazione, è necessario completare questa operazione.

## <a name="inventory-current-capacity-and-resource-usage"></a>Inventario della capacità corrente e dell'utilizzo delle risorse

Documentare l'hardware dei server di produzione correnti oltre al numero medio e massimo di richieste e all'utilizzo delle risorse. Queste informazioni saranno necessarie per effettuare il provisioning delle risorse nella destinazione del servizio.

## <a name="migration-guidance"></a>Indicazioni sulla migrazione

Usare le griglie seguenti per trovare indicazioni sulla migrazione in base al tipo di applicazione e alla destinazione del servizio di Azure di destinazione.

**Applicazioni Java**

Usare le righe seguenti per trovare il tipo di applicazione Java e le colonne per trovare la destinazione del servizio di Azure che ospiterà l'applicazione.

|Destinazione&nbsp;→<br><br>Tipo di&nbsp;applicazione&nbsp;↓|App<br>Service<br>Java SE|App<br>Service<br>Tomcat|App<br>Service<br>WildFly|Azure<br>Spring<br>Cloud|Servizio Azure Kubernetes|Macchine virtuali|
|---|---|---|---|---|---|---|
| Spring Boot/<br>Applicazioni JAR | [disponibile][5] | pianificato        | pianificato | pianificato | pianificato        | pianificato |
| Spring Cloud/<br>microservizi   | N/D            | N/D            | N/D     | pianificato | pianificato        | pianificato |
| Applicazioni Web<br>su Tomcat     | N/D            | [disponibile][2] | N/D     | N/D     | [disponibile][3] | pianificato |

**Applicazioni Java EE**

Usare le righe seguenti per trovare il tipo di applicazione Java EE in esecuzione in un server app specifico. Usare le colonne per trovare la destinazione del servizio di Azure che ospiterà l'applicazione.

|Destinazione&nbsp;→<br><br>Server app&nbsp;↓|App<br>Service<br>Java SE|App<br>Service<br>Tomcat|App<br>Service<br>WildFly|Azure<br>Spring<br>Cloud|Servizio Azure Kubernetes|Macchine virtuali|
|---|---|---|---|---|---|---|
| WildFly/<br>JBoss AS | N/D | N/D | pianificato | N/D | pianificato | pianificato        |
| WebLogic              | N/D | N/D | pianificato | N/D | pianificato | [disponibile][4] |
| WebSphere             | N/D | N/D | pianificato | N/D | pianificato | pianificato        |
| JBoss EAP             | N/D | N/D | pianificato | N/D | N/D     | pianificato        |

<!-- reference links, for use with tables -->
[1]: media/migration-overview/logo_azure.svg
[2]: migrate-tomcat-to-tomcat-app-service.md
[3]: migrate-tomcat-to-containers-on-azure-kubernetes-service.md
[4]: migrate-weblogic-to-virtual-machines.md
[5]: migrate-java-se-to-java-se-app-service.md