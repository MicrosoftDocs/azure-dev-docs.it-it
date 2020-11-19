---
title: Eseguire la migrazione di applicazioni Tomcat ai contenitori nel servizio Azure Kubernetes
description: Questa guida descrive gli aspetti da considerare per la migrazione di un'applicazione Tomcat esistente da eseguire in un contenitore del servizio Azure Kubernetes.
author: yevster
ms.author: yebronsh
ms.topic: conceptual
ms.date: 1/20/2020
ms.custom: devx-track-java, devx-track-azurecli
ms.openlocfilehash: 7311aba602ec2fd482d11e2a8a37751f0d742eae
ms.sourcegitcommit: dc74b60217abce66fe6cc93923e869e63ac86a8f
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/18/2020
ms.locfileid: "94872872"
---
# <a name="migrate-tomcat-applications-to-containers-on-azure-kubernetes-service"></a>Eseguire la migrazione di applicazioni Tomcat ai contenitori nel servizio Azure Kubernetes

Questa guida descrive gli aspetti da considerare per la migrazione di un'applicazione Tomcat esistente da eseguire nel servizio Azure Kubernetes.

## <a name="pre-migration"></a>Pre-migrazione

Per garantire una corretta migrazione, prima di iniziare completare i passaggi di valutazione e inventario descritti nelle sezioni seguenti.

[!INCLUDE [inventory-external-resources](includes/inventory-external-resources.md)]

[!INCLUDE [inventory-secrets](includes/inventory-secrets.md)]

[!INCLUDE [determine-whether-and-how-the-file-system-is-used](includes/determine-whether-and-how-the-file-system-is-used.md)]

<!-- AKS-specific addendum to inventory-persistence-usage -->
[!INCLUDE [dynamic-or-internal-content-aks](includes/dynamic-or-internal-content-aks.md)]

### <a name="identify-session-persistence-mechanism"></a>Identificare il meccanismo di persistenza delle sessioni

Per identificare il gestore di persistenza delle sessioni in uso, esaminare i file *context.xml* nell'applicazione e nella configurazione di Tomcat. Cercare l'elemento `<Manager>`, quindi notare il valore dell'attributo `className`.

Le implementazioni predefinite di [PersistentManager](https://tomcat.apache.org/tomcat-9.0-doc/config/manager.html) di Tomcat, ad esempio [StandardManager](https://tomcat.apache.org/tomcat-9.0-doc/config/manager.html#Standard_Implementation) o [FileStore](https://tomcat.apache.org/tomcat-9.0-doc/config/manager.html#Nested_Components) non sono progettate per l'uso con una piattaforma distribuita e scalabile come Kubernetes. Il servizio Azure Kubernetes può bilanciare il carico tra diversi pod e riavviare in modo trasparente qualsiasi pod in qualsiasi momento, quindi non è consigliabile rendere persistente lo stato modificabile di un file system.

Se è richiesta la persistenza delle sessioni, è necessario usare un'implementazione di `PersistentManager` alternativa che scriverà in un archivio dati esterno, ad esempio VMware Tanzu Session Manager con Cache Redis. Per altre informazioni, vedere [Usare Redis come cache di sessione con Tomcat](/azure/app-service/containers/configure-language-java#use-redis-as-a-session-cache-with-tomcat).

### <a name="special-cases"></a>Casi speciali

Alcuni scenari di produzione possono richiedere ulteriori modifiche o imporre limitazioni aggiuntive. Sebbene tali scenari siano poco frequenti, è importante assicurarsi che siano inapplicabili all'applicazione o risolti correttamente.

#### <a name="determine-whether-application-relies-on-scheduled-jobs"></a>Determinare se l'applicazione si basa su processi pianificati

I processi pianificati, ad esempio le attività di Quartz Scheduler o i processi Cron, non possono essere usati con le distribuzioni di Tomcat in contenitori. Se l'applicazione viene ampliata, un processo pianificato può essere eseguito più di una volta per ogni periodo pianificato. Questa situazione può provocare conseguenze indesiderate.

Creare un inventario di tutti i processi pianificati, all'interno o all'esterno del server applicazioni.

#### <a name="determine-whether-your-application-contains-os-specific-code"></a>Determinare se l'applicazione contiene codice specifico del sistema operativo

Se l'applicazione contiene codice che ospita il sistema operativo in cui è in esecuzione l'applicazione, è necessario eseguire il refactoring dell'applicazione in modo che NON si basi sul sistema operativo sottostante. Ad esempio, potrebbe essere necessario sostituire qualsiasi utilizzo di `/` o `\` nei percorsi del file system con [`File.Separator`](https://docs.oracle.com/javase/8/docs/api/java/io/File.html#separator) o [`Path.get`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Paths.html#get-java.lang.String-java.lang.String...-).

#### <a name="determine-whether-memoryrealm-is-used"></a>Determinare se viene usato MemoryRealm

[MemoryRealm](https://tomcat.apache.org/tomcat-9.0-doc/api/org/apache/catalina/realm/MemoryRealm.html) richiede un file XML persistente. In Kubernetes questo file deve essere aggiunto all'immagine del contenitore o caricato nello [spazio di archiviazione condiviso reso disponibile per i contenitori](#identify-session-persistence-mechanism). Il parametro `pathName` dovrà essere modificato di conseguenza.

Per determinare se `MemoryRealm` è attualmente in uso, esaminare i file *server.xml* e *context.xml* e cercare gli elementi `<Realm>` in cui l'attributo `className` è impostato su `org.apache.catalina.realm.MemoryRealm`.

#### <a name="determine-whether-ssl-session-tracking-is-used"></a>Determinare se viene usato il monitoraggio delle sessioni SSL

Nelle distribuzioni in contenitori, il carico delle sessioni SSL viene in genere trasferito all'esterno del contenitore dell'applicazione, in genere dal controller in ingresso. Se l'applicazione richiede il [monitoraggio delle sessioni SSL](https://tomcat.apache.org/tomcat-9.0-doc/servletapi/javax/servlet/SessionTrackingMode.html#SSL), verificare che il traffico SSL passi direttamente al contenitore dell'applicazione.

#### <a name="determine-whether-accesslogvalve-is-used"></a>Determinare se viene usato AccessLogValve

Se viene usato [AccessLogValve](https://tomcat.apache.org/tomcat-9.0-doc/api/org/apache/catalina/valves/AccessLogValve.html), il parametro `directory` deve essere impostato su una [condivisione montata di File di Azure](/azure/aks/azure-files-dynamic-pv) o su una delle relative sottodirectory.

### <a name="in-place-testing"></a>Test sul posto

Prima di creare immagini del contenitore, eseguire la migrazione dell'applicazione all'istanza di JDK e Tomcat che si intende usare nel servizio Azure Kubernetes. Testare accuratamente l'applicazione per garantirne la compatibilità e le prestazioni.

### <a name="parameterize-the-configuration"></a>Parametrizzare la configurazione

Nella fase di pre-migrazione è probabile che nei file *server.xml* e *context.xml* siano stati identificati segreti e dipendenze esterne, ad esempio origini dati. Per ogni elemento di questo tipo identificato, sostituire l'eventuale nome utente, password, stringa di connessione o URL con una variabile di ambiente.

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

## <a name="migration"></a>Migrazione

Fatta eccezione per il primo passaggio ("Provisioning del registro contenitori e del servizio Azure Kubernetes"), è consigliabile seguire la procedura descritta di seguito singolarmente per ogni applicazione (file WAR) di cui eseguire la migrazione.

> [!NOTE]
> Alcune distribuzioni di Tomcat possono avere più applicazioni in esecuzione in un singolo server Tomcat. Se questo è il caso, è consigliabile eseguire ogni applicazione in un pod separato. In questo modo è possibile ottimizzare l'utilizzo delle risorse per ogni applicazione riducendo al tempo stesso la complessità e l'accoppiamento.

### <a name="provision-container-registry-and-aks"></a>Provisioning del registro contenitori e del servizio Azure Kubernetes

Creare un registro contenitori e un cluster di Azure Kubernetes la cui entità servizio abbia il ruolo di lettore nel registro. Assicurarsi di [scegliere il modello di rete appropriato](/azure/aks/operator-best-practices-network#choose-the-appropriate-network-model) per i requisiti di rete del cluster.

```bash
az group create -g $resourceGroup -l eastus
az acr create -g $resourceGroup -n $acrName --sku Standard
az aks create -g $resourceGroup -n $aksName --attach-acr $acrName --network-plugin azure
```

### <a name="prepare-the-deployment-artifacts"></a>Preparare gli artefatti della distribuzione

Clonare il [repository GitHub indicato nell'argomento di avvio rapido su Tomcat nei contenitori](https://github.com/Azure/tomcat-container-quickstart). Contiene un Dockerfile e i file di configurazione di Tomcat con diverse ottimizzazioni consigliate. Nei passaggi seguenti vengono illustrate le modifiche che è probabile sia necessario apportare a questi file prima di creare l'immagine del contenitore e distribuirla nel servizio Azure Kubernetes.

#### <a name="open-ports-for-clustering-if-needed"></a>Se necessario, aprire le porte per il clustering

Se si intende usare il [clustering Tomcat](https://tomcat.apache.org/tomcat-9.0-doc/cluster-howto.html) nel servizio Azure Kubernetes, verificare che gli intervalli di porte necessari siano esposti nel Dockerfile. Per specificare l'indirizzo IP del server in *server.xml*, assicurarsi di usare un valore di una variabile inizializzata all'avvio del contenitore nell'indirizzo IP del pod.

In alternativa, lo stato della sessione può essere [reso persistente in una posizione alternativa](#identify-session-persistence-mechanism) ai fini della disponibilità tra repliche.

Per determinare se l'applicazione usa il clustering, cercare l'elemento `<Cluster>` all'interno degli elementi `<Host>` o `<Engine>` nel file *server.xml*.

#### <a name="add-jndi-resources"></a>Aggiungere risorse JNDI

Modificare il file *server.xml* per aggiungere le risorse preparate nei passaggi di pre-migrazione, ad esempio le origini dati.

Ad esempio:

```xml
<!-- Global JNDI resources
      Documentation at /docs/jndi-resources-howto.html
-->
<GlobalNamingResources>
    <!-- Editable user database that can also be used by
         UserDatabaseRealm to authenticate users
    -->
    <Resource name="UserDatabase" auth="Container"
              type="org.apache.catalina.UserDatabase"
              description="User database that can be updated and saved"
              factory="org.apache.catalina.users.MemoryUserDatabaseFactory"
              pathname="conf/tomcat-users.xml"
               />

    <!-- Migrated datasources here: -->
    <Resource
        name="jdbc/dbconnection"
        type="javax.sql.DataSource"
        url="${postgresdb.connectionString}"
        driverClassName="org.postgresql.Driver"
        username="${postgresdb.username}"
        password="${postgresdb.password}"
    />
    <!-- End of migrated datasources -->
</GlobalNamingResources>
```

[!INCLUDE[Tomcat datasource additional instructions](includes/tomcat-datasource-additional-instructions.md)]

### <a name="build-and-push-the-image"></a>Creare l'immagine ed eseguirne il push

Il modo più semplice per creare e caricare l'immagine in Registro Azure Container per l'uso da parte del servizio Azure Kubernetes consiste nell'usare il comando `az acr build`. Questo comando non richiede l'installazione di Docker nel computer. Ad esempio, se il Dockerfile precedente e il pacchetto dell'applicazione *petclinic.war* si trovano nella directory corrente, è possibile creare l'immagine del contenitore in Registro Azure Container con un solo passaggio:

```bash
az acr build -t "${acrName}.azurecr.io/petclinic:{{.Run.ID}}" -r $acrName --build-arg APP_FILE=petclinic.war --build-arg=prod.server.xml .
```

È possibile omettere il parametro `--build-arg APP_FILE...` se il file WAR è denominato *ROOT.war*. È possibile omettere il parametro `--build-arg SERVER_XML...` se il file XML del server è denominato *server.xml*. Entrambi i file devono trovarsi nella stessa directory del *Dockerfile*.

In alternativa, è possibile usare l'interfaccia della riga di comando di Docker per creare l'immagine localmente. Questo approccio può semplificare il test e il perfezionamento dell'immagine prima della distribuzione iniziale in Registro Azure Container. Tuttavia, è necessario che l'interfaccia della riga di comando di Docker sia installata e che il daemon Docker sia in esecuzione.

```bash
# Build the image locally
sudo docker build . --build-arg APP_FILE=petclinic.war -t "${acrName}.azurecr.io/petclinic:1"

# Run the image locally
sudo docker run -d -p 8080:8080 "${acrName}.azurecr.io/petclinic:1"

# Your application can now be accessed with a browser at http://localhost:8080.

# Log into ACR
sudo az acr login -n $acrName

# Push the image to ACR
sudo docker push "${acrName}.azurecr.io/petclinic:1"
```

Per informazioni più dettagliate sulla creazione e l'archiviazione di immagini di contenitore in Azure, vedere il rispettivo [corso di Microsoft Learn](/learn/modules/build-and-store-container-images/).

### <a name="provision-a-public-ip-address"></a>Provisioning di un indirizzo IP pubblico

Se l'applicazione deve essere accessibile dall'esterno delle reti virtuali o interne, sarà necessario un indirizzo IP statico pubblico. È necessario eseguire il provisioning di questo indirizzo IP nel gruppo di risorse del nodo del cluster.

```bash
nodeResourceGroup=$(az aks show -g $resourceGroup -n $aksName --query 'nodeResourceGroup' -o tsv)
publicIp=$(az network public-ip create -g $nodeResourceGroup -n applicationIp --sku Standard --allocation-method Static --query 'publicIp.ipAddress' -o tsv)
echo "Your public IP address is ${publicIp}."
```

### <a name="deploy-to-aks"></a>Distribuire in servizio Azure Kubernetes

[Creare e applicare i file YAML di Kubernetes](/azure/aks/kubernetes-walkthrough#run-the-application). Se si crea un servizio di bilanciamento del carico esterno (per l'applicazione o per un controller di ingresso), assicurarsi di fornire l'indirizzo IP di cui è stato effettuato il provisioning nella sezione precedente come `LoadBalancerIP`.

Includere [parametri esternalizzati come variabili di ambiente](https://kubernetes.io/docs/tasks/inject-data-application/define-environment-variable-container/). Non includere segreti, ad esempio password, chiavi API e stringhe di connessione JDBC. I segreti sono descritti nella sezione [Configurare KeyVault FlexVolume](#configure-keyvault-flexvolume).

### <a name="configure-persistent-storage"></a>Configurare la risorsa di archiviazione persistente

Se l'applicazione richiede una risorsa di archiviazione non volatile, configurare uno o più [volumi persistenti](/azure/aks/azure-disks-dynamic-pv).

Si potrebbe scegliere di creare un volume persistente usando File di Azure, montato nella directory dei log di Tomcat ( */tomcat_logs*) per mantenere i log a livello centrale. Per altre informazioni, vedere [Creare dinamicamente e usare un volume persistente con File di Azure nel servizio Azure Kubernetes](/azure/aks/azure-files-dynamic-pv).

### <a name="configure-keyvault-flexvolume"></a>Configurare KeyVault FlexVolume

[Creare un'istanza di Azure KeyVault](/azure/key-vault/quick-create-cli) e popolarla con i segreti necessari. Configurare quindi [KeyVault FlexVolume](https://github.com/Azure/kubernetes-keyvault-flexvol/blob/master/README.md) per rendere tali segreti accessibili ai pod.

Sarà necessario modificare lo script di avvio (*startup.sh*) nel repository di GitHub per [Tomcat nei contenitori](https://github.com/Azure/tomcat-container-quickstart) per importare i certificati nell'archivio chiavi locale del contenitore.

### <a name="migrate-scheduled-jobs"></a>Eseguire la migrazione dei processi pianificati

Per eseguire i processi pianificati nel cluster del servizio Azure Kubernetes, definire [processi Cron](https://kubernetes.io/docs/tasks/job/automated-tasks-with-cron-jobs/) in base alle esigenze.

## <a name="post-migration"></a>Post-migrazione

Ora che è stata eseguita la migrazione dell'applicazione al servizio Azure Kubernetes, è necessario verificare che funzioni come previsto. Una volta completata questa operazione, sono disponibili alcune raccomandazioni per rendere l'applicazione maggiormente nativa del cloud.

* Valutare se aggiungere un nome DNS all'indirizzo IP allocato al controller in ingresso o al servizio di bilanciamento del carico dell'applicazione. Per altre informazioni, vedere [Creare un controller di ingresso con un indirizzo IP pubblico statico nel servizio Azure Kubernetes](/azure/aks/ingress-static-ip).

* Valutare se [aggiungere grafici Helm per l'applicazione](https://helm.sh/docs/topics/charts/). Un grafico Helm consente di parametrizzare la distribuzione dell'applicazione per l'uso e la personalizzazione da parte di un set più diversificato di clienti.

* Progettare e implementare una strategia DevOps. Per mantenere l'affidabilità aumentando al tempo stesso la velocità di sviluppo, è consigliabile [automatizzare le distribuzioni e i test con Azure Pipelines](/azure/devops/pipelines/ecosystems/kubernetes/aks-template).

* Abilitare [Monitoraggio di Azure per il cluster](/azure/azure-monitor/insights/container-insights-enable-existing-clusters) per consentire la raccolta di log del contenitore, tenere traccia dell'utilizzo e così via.

* Valutare se esporre metriche specifiche dell'applicazione tramite Prometheus. Prometheus è un framework di metriche open source ampiamente adottato nella community di Kubernetes. È possibile configurare lo [scraping delle metriche di Prometheus in Monitoraggio di Azure](/azure/azure-monitor/insights/container-insights-prometheus-integration) invece di ospitare il proprio server Prometheus per consentire l'aggregazione delle metriche dalle applicazioni e la risposta automatizzata o l'escalation delle condizioni anomale.

* Progettare e implementare una strategia di continuità aziendale e ripristino di emergenza. Per le applicazioni cruciali, considerare un'[architettura di distribuzione in più aree](/azure/aks/operator-best-practices-multi-region).

* Vedere [Criteri di supporto della versione di Kubernetes](/azure/aks/supported-kubernetes-versions#kubernetes-version-support-policy). È responsabilità dell'utente mantenere [aggiornato il cluster del servizio Azure Kubernetes](/azure/aks/upgrade-cluster) per assicurarsi che sia sempre in esecuzione una versione supportata.

* Chiedere a tutti i membri del team responsabili dell'amministrazione del cluster e dello sviluppo di applicazioni di esaminare le [procedure consigliate per il servizio Azure Kubernetes](/azure/aks/best-practices).

* Valutare gli elementi nel file *logging.properties*. Valutare se eliminare o ridurre parte dell'output della registrazione per migliorare le prestazioni.

* Valutare se [monitorare le dimensioni della cache del codice](https://docs.oracle.com/javase/8/embedded/develop-apps-platforms/codecache.htm) e aggiungere i parametri `-XX:InitialCodeCacheSize` e `-XX:ReservedCodeCacheSize` alla variabile `JAVA_OPTS` nel Dockerfile per ottimizzare ulteriormente le prestazioni.
