---
title: Eseguire la migrazione di applicazioni Tomcat a Tomcat nel servizio app di Azure
description: Questa guida descrive gli aspetti da considerare per la migrazione di un'applicazione Tomcat esistente da eseguire nel servizio app di Azure con Tomcat.
author: yevster
ms.author: yebronsh
ms.topic: conceptual
ms.date: 1/20/2020
ms.custom: devx-track-java
ms.openlocfilehash: 1b22dfa4269cc0c51450ca307bb96b787278625b
ms.sourcegitcommit: 44016b81a15b1625c464e6a7b2bfb55938df20b6
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/14/2020
ms.locfileid: "86379705"
---
# <a name="migrate-tomcat-applications-to-tomcat-on-azure-app-service"></a>Eseguire la migrazione di applicazioni Tomcat a Tomcat nel servizio app di Azure

Questa guida descrive gli aspetti da considerare per la migrazione di un'applicazione Tomcat esistente da eseguire nel servizio app di Azure con Tomcat 9.0.

## <a name="pre-migration"></a>Pre-migrazione

Per garantire una corretta migrazione, prima di iniziare completare i passaggi di valutazione e inventario descritti nelle sezioni seguenti.

Se non è possibile soddisfare i requisiti di pre-migrazione, vedere le guide alla migrazione complementari seguenti:

* [Eseguire la migrazione di applicazioni Tomcat ai contenitori nel servizio Azure Kubernetes](migrate-tomcat-to-containers-on-azure-kubernetes-service.md)
* Eseguire la migrazione di applicazioni Tomcat alle macchine virtuali di Azure (indicazioni in pianificazione)

### <a name="switch-to-a-supported-platform"></a>Passare a una piattaforma supportata

Il servizio app offre versioni specifiche di Tomcat per versioni specifiche di Java. Per garantire la compatibilità, eseguire la migrazione dell'applicazione a una delle versioni supportate di Tomcat e Java nell'ambiente corrente prima di procedere con i passaggi rimanenti. Assicurarsi di testare completamente la configurazione risultante. Usare la versione stabile più recente della distribuzione Linux in questi test.

[!INCLUDE [note-obtain-your-current-java-version-app-service](includes/note-obtain-your-current-java-version-app-service.md)]

Per ottenere la versione corrente di Java, accedere al server di produzione ed eseguire il comando seguente:

```bash
${CATALINA_HOME}/bin/version.sh
```

Per ottenere la versione corrente usata dal servizio app di Azure, scaricare [Tomcat 9](https://tomcat.apache.org/download-90.cgi), a seconda della versione che si intende usare nel servizio app di Azure.

[!INCLUDE [inventory-external-resources](includes/inventory-external-resources.md)]

[!INCLUDE [inventory-secrets](includes/inventory-secrets.md)]

### <a name="inventory-certificates"></a>Inventario dei certificati

[!INCLUDE [inventory-certificates](includes/inventory-certificates.md)]

[!INCLUDE [determine-whether-and-how-the-file-system-is-used](includes/determine-whether-and-how-the-file-system-is-used.md)]

<!-- App-Service-specific addendum to inventory-persistence-usage -->
#### <a name="dynamic-or-internal-content"></a>Contenuto dinamico o interno

Per i file scritti e letti di frequente dall'applicazione, ad esempio i file di dati temporanei, o i file statici visibili solo all'applicazione, è possibile montare Archiviazione di Azure nel file system del servizio app. Per altre informazioni, vedere [Rendere disponibile contenuto di Archiviazione di Azure nel servizio app in Linux](/azure/app-service/containers/how-to-serve-content-from-azure-storage).

### <a name="identify-session-persistence-mechanism"></a>Identificare il meccanismo di persistenza delle sessioni

Per identificare il gestore di persistenza delle sessioni in uso, esaminare i file *context.xml* nell'applicazione e nella configurazione di Tomcat. Cercare l'elemento `<Manager>`, quindi notare il valore dell'attributo `className`.

Le implementazioni predefinite di [PersistentManager](https://tomcat.apache.org/tomcat-9.0-doc/config/manager.html) di Tomcat, ad esempio [StandardManager](https://tomcat.apache.org/tomcat-9.0-doc/config/manager.html#Standard_Implementation) o [FileStore](https://tomcat.apache.org/tomcat-9.0-doc/config/manager.html#Nested_Components), non sono progettate per l'uso con una piattaforma distribuita e scalabile come il servizio app. Poiché il servizio app può bilanciare il carico tra diverse istanze e riavviare in modo trasparente qualsiasi istanza in qualsiasi momento, non è consigliabile rendere persistente lo stato modificabile di un file system.

Se è necessaria la persistenza delle sessioni, è necessario usare un'implementazione di `PersistentManager` alternativa che scriverà in un archivio dati esterno, ad esempio Pivotal Session Manager con Cache Redis. Per altre informazioni, vedere [Usare Redis come cache di sessione con Tomcat](/azure/app-service/containers/configure-language-java#use-redis-as-a-session-cache-with-tomcat).

### <a name="special-cases"></a>Casi speciali

Alcuni scenari di produzione possono richiedere ulteriori modifiche o imporre limitazioni aggiuntive. Sebbene tali scenari siano poco frequenti, è importante assicurarsi che siano inapplicabili all'applicazione o risolti correttamente.

#### <a name="determine-whether-application-relies-on-scheduled-jobs"></a>Determinare se l'applicazione si basa su processi pianificati

I processi pianificati, ad esempio le attività dell'utilità di Quartz Scheduler o i processi Cron, non possono essere usati con il servizio app. Il servizio app non impedisce la distribuzione interna di un'applicazione contenente attività pianificate. Tuttavia, se l'applicazione viene ampliata, lo stesso processo pianificato può essere eseguito più di una volta per ogni periodo pianificato. Questa situazione può provocare conseguenze indesiderate.

Creare un inventario di tutti i processi pianificati, all'interno o all'esterno del server applicazioni.

#### <a name="determine-whether-your-application-contains-os-specific-code"></a>Determinare se l'applicazione contiene codice specifico del sistema operativo

[!INCLUDE [determine-whether-your-application-contains-os-specific-code-no-title](includes/determine-whether-your-application-contains-os-specific-code-no-title.md)]

#### <a name="determine-whether-tomcat-clustering-is-used"></a>Determinare se viene usato il clustering Tomcat

Il [clustering Tomcat](https://tomcat.apache.org/tomcat-9.0-doc/cluster-howto.html) non è supportato nel servizio app di Azure. È invece possibile configurare e gestire il ridimensionamento e il bilanciamento del carico tramite il servizio app di Azure senza funzionalità specifiche di Tomcat. È possibile salvare in modo permanente lo stato della sessione in un percorso alternativo per renderlo disponibile tra le repliche. Per altre informazioni, vedere [Identificare il meccanismo di persistenza delle sessioni](#identify-session-persistence-mechanism).

Per determinare se l'applicazione usa il clustering, cercare l'elemento `<Cluster>` all'interno degli elementi `<Host>` o `<Engine>` nel file *server.xml*.

#### <a name="identify-all-outside-processesdaemons-running-on-the-production-servers"></a>Identificare tutti i processi/daemon esterni in esecuzione nei server di produzione

Sarà necessario eseguire la migrazione altrove o eliminare i processi in esecuzione all'esterno del server applicazioni, ad esempio i daemon di monitoraggio.

#### <a name="determine-whether-non-http-connectors-are-used"></a>Determinare se vengono usati i connettori non HTTP

Il servizio app supporta solo un singolo connettore HTTP. Se l'applicazione richiede connettori aggiuntivi, ad esempio il connettore AJP, non usare il servizio app.

Per identificare i connettori HTTP usati dall'applicazione, cercare gli elementi `<Connector>` all'interno del file *server.xml* nella configurazione di Tomcat.

#### <a name="determine-whether-memoryrealm-is-used"></a>Determinare se viene usato MemoryRealm

[MemoryRealm](https://tomcat.apache.org/tomcat-9.0-doc/api/org/apache/catalina/realm/MemoryRealm.html) richiede un file XML persistente. Nel servizio app di Azure sarà necessario caricare questo file nella directory */home* o in una delle relative sottodirectory oppure in una risorsa di archiviazione montata. Sarà quindi necessario modificare il parametro `pathName` di conseguenza.

Per determinare se `MemoryRealm` è attualmente in uso, esaminare i file *server.xml* e *context.xml* e cercare gli elementi `<Realm>` in cui l'attributo `className` è impostato su `org.apache.catalina.realm.MemoryRealm`.

#### <a name="determine-whether-ssl-session-tracking-is-used"></a>Determinare se viene usato il monitoraggio delle sessioni SSL

Il servizio app esegue l'offload della sessione all'esterno del runtime di Tomcat, di conseguenza non è possibile usare il [monitoraggio della sessione SSL](https://tomcat.apache.org/tomcat-9.0-doc/servletapi/javax/servlet/SessionTrackingMode.html#SSL). Usare invece una modalità di monitoraggio delle sessioni diversa (`COOKIE` o `URL`). Se il monitoraggio delle sessioni SSL è necessario, non usare il servizio app.

#### <a name="determine-whether-accesslogvalve-is-used"></a>Determinare se viene usato AccessLogValve

Se si usa [AccessLogValve](https://tomcat.apache.org/tomcat-9.0-doc/api/org/apache/catalina/valves/AccessLogValve.html), è necessario impostare il parametro `directory` su `/home/LogFiles` o su una delle relative sottodirectory.

## <a name="migration"></a>Migrazione

### <a name="parameterize-the-configuration"></a>Parametrizzare la configurazione

Nei passaggi di pre-migrazione è probabile che nei file *server.xml* e *context.xml* siano stati identificati alcuni segreti e dipendenze esterne, ad esempio origini dati. Per ogni elemento di questo tipo identificato, sostituire l'eventuale nome utente, password, stringa di connessione o URL con una variabile di ambiente.

Si supponga, ad esempio, che il file *context.xml* contenga l'elemento seguente:

```xml
<Resource
    name="jdbc/dbconnection"
    type="javax.sql.DataSource"
    url="jdbc:postgresql://postgresdb.contoso.com/wickedsecret?ssl=true"
    driverClassName="org.postgresql.Driver"
    username="postgres"
    password="t00secure2gue$$"
/>
```

In questo caso, è possibile cambiarlo come illustrato nell'esempio seguente:

```xml
<Resource
    name="jdbc/dbconnection"
    type="javax.sql.DataSource"
    url="${postgresdb.connectionString}"
    driverClassName="org.postgresql.Driver"
    username="${postgresdb.username}"
    password="${postgresdb.password}"
/>
```

### <a name="provision-an-app-service-plan"></a>Effettuare il provisioning di un piano di servizio app

Dall'elenco dei piani di servizio disponibili nei [prezzi del servizio app](https://azure.microsoft.com/pricing/details/app-service/linux/), selezionare il piano le cui specifiche soddisfano o superano quelle dell'hardware di produzione corrente.

> [!NOTE]
> Se si prevede di eseguire distribuzioni di staging/canary o usare gli slot di distribuzione, il piano di servizio app deve includere tale capacità aggiuntiva. È consigliabile usare piani Premium o superiori per le applicazioni Java. Per altre informazioni, vedere [Configurare gli ambienti di gestione temporanea nel Servizio app di Azure](/azure/app-service/deploy-staging-slots).

Creare quindi il piano di servizio app. Per altre informazioni, vedere [Gestire un piano di servizio app in Azure](/azure/app-service/app-service-plan-manage).

### <a name="create-and-deploy-web-apps"></a>Creare e distribuire app Web

È necessario creare un'app Web nel piano di servizio app (scegliendo una versione di Tomcat come stack di runtime) per ogni file WAR distribuito nel server Tomcat.

> [!NOTE]
> Sebbene sia possibile distribuire più file WAR in un'unica app Web, questo approccio non è consigliabile. La distribuzione di più file WAR in un'unica app Web impedisce il ridimensionamento di ogni applicazione in base alle proprie esigenze di utilizzo, aumentando anche la complessità delle pipeline di distribuzione successive. Se più applicazioni devono essere disponibili in un singolo URL, provare a usare una soluzione di routing come il [gateway applicazione di Azure](/azure/application-gateway/).

#### <a name="maven-applications"></a>Applicazioni Maven

Se l'applicazione viene compilata da un file POM Maven, [usare il plug-in Webapp per Maven](/azure/app-service/containers/quickstart-java#configure-the-maven-plugin) per creare l'app Web e distribuire l'applicazione.

#### <a name="non-maven-applications"></a>Applicazioni non Maven

Se non è possibile usare il plug-in Maven, sarà necessario effettuare il provisioning dell'app Web tramite altri meccanismi, ad esempio:

* [Azure portal](https://portal.azure.com/#create/Microsoft.WebSite)
* [Interfaccia della riga di comando di Azure](/cli/azure/webapp?view=azure-cli-latest#az-webapp-create)
* [Azure PowerShell](/powershell/module/az.websites/new-azwebapp)

Una volta creata l'app Web, usare uno dei [meccanismi di distribuzione disponibili](/azure/app-service/deploy-zip) per distribuire l'applicazione.

### <a name="migrate-jvm-runtime-options"></a>Eseguire la migrazione delle opzioni di runtime JVM

Se l'applicazione richiede opzioni di runtime specifiche, [usare il meccanismo più appropriato per specificarle](/azure/app-service/containers/configure-language-java#set-java-runtime-options).

### <a name="populate-secrets"></a>Popolare i segreti

Usare le impostazioni dell'applicazione per archiviare i segreti specifici dell'applicazione. Se si intende usare gli stessi segreti tra più applicazioni o se sono necessari criteri di accesso e funzionalità di controllo con granularità fine, [usare Azure Key Vault](/azure/app-service/containers/configure-language-java#use-keyvault-references).

[!INCLUDE [configure-custom-domain-and-ssl](includes/configure-custom-domain-and-ssl.md)]

[!INCLUDE [import-backend-certificates](includes/import-backend-certificates.md)]

### <a name="migrate-data-sources-libraries-and-jndi-resources"></a>Eseguire la migrazione di origini dati, librerie e risorse JNDI

Per la procedura di configurazione dell'origine dati, vedere la sezione [Origini dati](/azure/app-service/containers/configure-language-java#data-sources) dell'articolo [Configurare un'app Java Linux per il servizio app Azure](/azure/app-service/containers/configure-language-java).

[!INCLUDE[Tomcat datasource additional instructions](includes/tomcat-datasource-additional-instructions.md)]

Eseguire la migrazione di eventuali altre dipendenze del classpath a livello di server, seguendo [la stessa procedura indicata per i file JAR delle origini dati](/azure/app-service/containers/configure-language-java#finalize-configuration).

Eseguire la migrazione di eventuali altre [risorse JDNI condivise a livello di server](/azure/app-service/containers/configure-language-java#shared-server-level-resources).

> [!NOTE]
> Se si sta usando l'architettura consigliata di un solo file WAR per ogni app Web, provare a eseguire la migrazione delle librerie di classpath a livello di server e delle risorse JNDI nell'applicazione. Questo approccio semplifica notevolmente la governance dei componenti e la gestione delle modifiche.

### <a name="migrate-remaining-configuration"></a>Eseguire la migrazione della configurazione rimanente

Al termine della sezione precedente, si avrà la configurazione del server personalizzabile in */home/tomcat/conf*.

Completare la migrazione copiando eventuali altre configurazioni, ad esempio [aree di autenticazione](https://tomcat.apache.org/tomcat-9.0-doc/config/realm.html) e [JASPIC](https://tomcat.apache.org/tomcat-9.0-doc/config/jaspic.html)

[!INCLUDE [migrate-scheduled-jobs](includes/migrate-scheduled-jobs.md)]

### <a name="restart-and-smoke-test"></a>Riavvio e smoke test

Infine, sarà necessario riavviare l'app Web per applicare tutte le modifiche alla configurazione. Al termine del riavvio, verificare che l'applicazione venga eseguita correttamente.

## <a name="post-migration"></a>Post-migrazione

Ora che è stata eseguita la migrazione dell'applicazione al servizio app di Azure, è necessario verificare che funzioni come previsto. Una volta completata questa operazione, sono disponibili alcune raccomandazioni per rendere l'applicazione maggiormente nativa del cloud.

### <a name="recommendations"></a>Consigli

* Se si è scelto di usare la directory */home* per l'archiviazione dei file, provare a [sostituirla con Archiviazione di Azure](/azure/app-service/containers/how-to-serve-content-from-azure-storage).

* Se nella directory */home* è presente una configurazione contenente stringhe di connessione, chiavi SSL e altre informazioni segrete, provare a usare una combinazione di [Azure Key Vault](/azure/app-service/app-service-key-vault-references) e/o [inserimento di parametri con le impostazioni dell'applicazione](/azure/app-service/configure-common#configure-app-settings) laddove possibile.

* Provare a [usare gli slot di distribuzione](/azure/app-service/deploy-staging-slots) per distribuzioni affidabili senza tempi di inattività.

* Progettare e implementare una strategia DevOps. Per mantenere l'affidabilità aumentando al tempo stesso la velocità di sviluppo, è consigliabile [automatizzare le distribuzioni e i test con Azure Pipelines](/azure/devops/pipelines/ecosystems/java-webapp). Se si usano gli slot di distribuzione, è possibile [automatizzare la distribuzione in uno slot](/azure/devops/pipelines/targets/webapp?view=azure-devops&tabs=yaml#deploy-to-a-slot), seguita dallo scambio di slot.

* Progettare e implementare una strategia di continuità aziendale e ripristino di emergenza. Per le applicazioni cruciali, considerare un'[architettura di distribuzione in più aree](/azure/architecture/reference-architectures/app-service-web-app/multi-region).
