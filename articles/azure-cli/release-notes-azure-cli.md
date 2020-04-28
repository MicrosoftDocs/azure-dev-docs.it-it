---
title: Note sulla versione dell'interfaccia della riga di comando di Azure
description: Informazioni sugli aggiornamenti più recenti dell'interfaccia della riga di comando di Azure
author: dbradish-microsoft
ms.author: dbradish
manager: barbkess
ms.date: 04/21/2020
ms.topic: article
ms.service: azure-cli
ms.devlang: azurecli
ms.openlocfilehash: 10dfdc316ba00f8a7019f0724aab231e344c1c6d
ms.sourcegitcommit: 36e02e96b955ed0531f98b9c0f623f4acb508661
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/22/2020
ms.locfileid: "82031313"
---
# <a name="azure-cli-release-notes"></a>Note sulla versione dell'interfaccia della riga di comando di Azure

## <a name="april-21-2020"></a>21 aprile 2020

Versione 2.4.0

### <a name="acr"></a>ACR

* `az acr run --cmd`: disabilita l'override della directory di lavoro
* Supporto dell'endpoint dati dedicato

### <a name="aks"></a>Servizio Azure Kubernetes

* `az aks list -o table` dovrebbe mostrare privateFqdn come FQDN per cluster privati
* Aggiunta di --uptime-sla
* Aggiornamento del pacchetto containerservice
* Aggiunta del supporto di IP pubblici del nodo
* Correzione dell'errore di digitazione nel comando help

### <a name="appconfig"></a>AppConfig

* Risoluzione del riferimento all'insieme di credenziali delle chiavi per i comandi kv list ed export
* Correzione del bug per i valori della chiave list

### <a name="appservice"></a>AppService

* `az functionapp create`: modifica del modo in cui viene impostato linuxFxVersion per le app per le funzioni .NET Linux. Questa modifica dovrebbe correggere un bug che impedisce la creazione di app a consumo .NET Linux
* [MODIFICA DI RILIEVO] `az webapp create`: correzione per il mantenimento di AppSettings esistenti con az webapp create
* [MODIFICA DI RILIEVO] `az webapp up`: correzione per la creazione di gruppi di risorse per il comando az webapp up quando si usa il flag -g
* [MODIFICA DI RILIEVO] `az webapp config`: correzione per la visualizzazione di valori per output non JSON con az webapp config connection-string list

### <a name="arm"></a>ARM

* `az deployment create/validate`: aggiunta del parametro `--no-prompt` per supportare la possibilità di ignorare la richiesta di parametri mancanti per il modello di Resource Manager
* `az deployment group/mg/sub/tenant validate`: supporto di commenti nel file di parametri della distribuzione
* `az deployment`: rimozione di `is_preview` per il parametro `--handle-extended-json-format`
* `az deployment group/mg/sub/tenant cancel`: supporto dell'annullamento della distribuzione per il modello di Resource Manager
* `az deployment group/mg/sub/tenant validate`: miglioramento del messaggio di errore visualizzato quando la verifica della distribuzione non riesce
* `az deployment-scripts`: aggiunta di nuovi comandi per DeploymentScripts
* `az resource tag`: aggiunta del parametro `--is-incremental` per supportare l'aggiunta incrementale di tag alla risorsa

### <a name="aro"></a>ARO

* `az aro`:  aggiunta del modulo di comandi ARO di Azure RedHat OpenShift V4

### <a name="batch"></a>Batch

* Aggiornamento dell'API Batch

### <a name="compute"></a>Calcolo

* `az sig image-version create`: aggiunta del tipo di account di archiviazione Premium_LRS
* `az vmss update`: correzione del problema di aggiornamento della notifica di termine
* `az vm/vmss create`: aggiunta del supporto per la versione di immagini speciali
* SIG API versione 2019-12-01
* `az sig image-version create`: aggiunta di --target-region-encryption
* Correzione del problema per cui i test non riescono quando vengono eseguiti in seriale a causa del nome keyvault duplicato nella cache in memoria globale

### <a name="cosmosdb"></a>Cosmos DB

* Supporto di `az cosmosdb private-link-resource/private-endpoint-connection`

### <a name="iot-central"></a>IoT Central

* Deprecazione di `az iotcentral`
* Aggiunta del modulo di comandi `az iot central`

### <a name="monitor"></a>Monitorare

* Supporto dello scenario di collegamento privato per il monitoraggio
* Correzione di una modalità fittizia errata in test_monitor_general_operations.py

### <a name="network"></a>Rete

* Deprecazione dello SKU per il comando di aggiornamento di IP pubblici
* `az network private-endpoint`: Supporto del gruppo di zone DNS privato
* Abilitazione della funzionalità di contesto locale per il parametro vnet/subnet
* Correzione dell'esempio di utilizzo errato in test_nw_flow_log_delete

### <a name="packaging"></a>Packaging

* Rimozione del supporto per il pacchetto Ubuntu/Disco

### <a name="rbac"></a>Controllo degli accessi in base al ruolo

* `az ad app create/update`: supporto di --optional-claims come parametro

### <a name="rdbms"></a>RDBMS

* Aggiunta di comandi dell'amministratore di Active Directory per PostgreSQL e MySQL

### <a name="service-fabric"></a>Service Fabric

* Correzione #12891: rimozione da `az sf application update --application-parameters` dei parametri meno recenti non presenti nella richiesta
* Correzione #12470 di az sf create cluster, correzione dei bug nella durabilità e nell'affidabilità degli aggiornamenti e ricerca corretta di set di scalabilità di macchine virtuali tramite il codice assegnato a un nome di tipo nodo

### <a name="sql"></a>SQL

* Aggiunta di `az sql mi op list`, `az sql mi op get`, `az sql mi op cancel`
* `az sql midb`: aggiornamento/visualizzazazione dei criteri di conservazione a lungo termine, visualizzazione/eliminazione di backup con conservazione a lungo termine, ripristino di backup con conservazione a lungo termine

### <a name="storage"></a>Archiviazione

* Aggiornamento di azure-mgmt-storage alla versione 9.0.0
* `az storage logging off`: supporto della disattivazione della registrazione per un account di archiviazione
* `az storage account update`: abilitazione della rotazione automatica delle chiavi gestite dal cliente
* `az storage account encryption-scope create/update/list/show`: aggiunta del supporto per personalizzare l'ambito di crittografia
* `az storage container create`: Aggiunta di --default-encryption-scope e --deny-encryption-scope-override per impostare l'ambito di crittografia a livello di contenitore

### <a name="survey"></a>Sondaggio

* Aggiunta dell'opzione per disattivare il collegamento al sondaggio

## <a name="april-01-2020"></a>1° aprile 2020

Versione 2.3.1

### <a name="acr"></a>ACR

* Correzione di una versione errata di azure-mgmt-containerregistr per Linux

### <a name="profile"></a>Profilo

* az login: Correzione di un errore di accesso con profili cloud diversi da `latest`

## <a name="march-31-2020"></a>31 marzo 2020

Versione 2.3.0

### <a name="acr"></a>ACR

* 'az acr task update': eccezione dovuta a puntatore Null
* `az acr import`: modifica della guida e del messaggio di errore per chiarire l'utilizzo di --source e --registry
* Aggiunta di un validator per l'argomento 'registry_name'
* `az acr login`: rimozione del flag preview in '--expose-token'
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Rimozione del parametro 'az acr task create/update' di branch
* 'az acr task update': il cliente ora può aggiornare singolarmente context, git-token e/o triggers
* 'az acr agentpool': nuova funzionalità

### <a name="aks"></a>Servizio Azure Kubernetes

* Correzione di apiServerAccessProfile durante l'aggiornamento di --api-server-authorized-ip-ranges
* aks update: sostituzione degli IP in uscita con i valori di input durante l'aggiornamento
* SPN non creato per i cluster MSI e supporto di attach acr per i cluster MSI

### <a name="ams"></a>AMS

* Correzione #12469: l'aggiunta di content-key-policy di Fairplay non riesce a causa di problemi con il parametro 'ask'

### <a name="appconfig"></a>AppConfig

* Aggiunta di --skip-keyvault per kv export

### <a name="appservice"></a>AppService

* Correzione #12509: rimozione del tag per az webapp up per impostazione predefinita
* az functionapp create: aggiornamento del menu della Guida di --runtime-version e aggiunta dell'avviso quando l'utente specifica --runtime-version per dotnet
* az functionapp create: aggiornamento della modalità di impostazione di javaVersion per le app per le funzioni di Windows

### <a name="arm"></a>ARM

* az deployment create/validate: uso di --handle-extended-json-format per impostazione predefinita
* az lock create: aggiunta di esempi di creazione di sottorisorse nella documentazione della Guida
* az deployment {group/mg/sub/tenant} list: supporto del filtro provisioningState
* az deployment: correzione del bug di analisi per comment nell'ultimo argomento

### <a name="backup"></a>Backup

* Aggiunta di funzionalità per il ripristino di più file
* Aggiunta del supporto per il backup solo dei dischi del sistema operativo
* Aggiunta del parametro restore-as-unmanaged-disk per specificare il ripristino non gestito

### <a name="compute"></a>Calcolo

* az vm create: aggiunta dell'opzione NONE di --nsg-rule
* az vmss create/update: rimozione del tag preview per le riparazioni automatica vmss
* az vm update: supporto di --workspace
* Correzione di un bug nel codice di inizializzazione di VirtualMachineScaleSetExtension
* Aggiornamento di VMAccessAgent alla versione 2.4
* az vmss set-orchestration-service-state: supporto dello stato di impostazione del servizio orchestrazione vmss
* Aggiornamento dell'API del disco alla versione 2019-11-01
* az disk create: add --disk-iops-read-only, --disk-mbps-read-only, --max-shares, --image-reference, --image-reference-lun, --gallery-image-reference, --gallery-image-reference-lun

### <a name="cosmos-db"></a>Cosmos DB

* Correzione dell'opzione --type mancante per i reindirizzamenti di deprecazione

### <a name="docker"></a>Docker

* Aggiornamento ad Alpine 3.11 e Python 3.6.10

### <a name="extension"></a>Estensione

* Consentito il caricamento di estensioni nel percorso di sistema tramite pacchetti

### <a name="hdinsight"></a>HDInsight

* az hdinsight create: aggiunta del supporto per consentire ai clienti di specificare la versione minima supportata di tls con il parametro `--minimal-tls-version`. Il valore consentito è 1.0,1.1,1.2

### <a name="iot"></a>IoT

* Aggiunta del proprietario del codice
* az iot hub create: modifica dello SKU predefinito da F1 a S1
* iot hub: supporto di IotHub nel profilo di 2019-03-01-hybrid

### <a name="iotcentral"></a>IoTCentral

* Aggiornamento dei dettagli degli errori, nonché del modello di applicazione predefinito e del messaggio di richiesta

### <a name="keyvault"></a>Insieme di credenziali delle chiavi

* Supporto del backup/ripristino dei certificati
* keyvault create/update: supporto di --retention-days
* Chiavi/segreti gestiti non sono più inclusi nell'elenco
* az keyvault create: supporto di `--network-acls`, `--network-acls-ips` e `--network-acls-vnets` per la specifica di regole di rete durante la creazione del vault

### <a name="lock"></a>Lock

* az lock delete fix bug: az lock delete non funziona su Microsoft.DocumentDB

### <a name="monitor"></a>Monitorare

* az monitor clone: supporto delle regole per le metriche di clonazione da una risorsa a un'altra
* Correzione IcM179210086: non è possibile creare un avviso di metrica personalizzato per la relativa metrica Application Insights

### <a name="netappfiles"></a>NetAppFiles

* az volume create: Consentita l'aggiunta di operazioni di replica per i volumi di protezione dati: approve, suspend, resume, status, remove

### <a name="network"></a>Rete

* az network application-gateway waf-policy managed-rule rule-set add: supporto di Microsoft_BotManagerRuleSet
* network watcher flow-log show: correzione delle informazioni di deprecazione errate
* Supporto dei nomi host nel listener del gateway applicazione
* az network nat gateway: supporto per la creazione di una risorsa vuota senza IP pubblico o prefisso di IP pubblico
* Supporto per la generazione del gateway VPN
* Supporto di `--if-none-match` in `az network dns record-set {} add-record`

### <a name="packaging"></a>Packaging

* Rimozione del supporto per Python 3.5

### <a name="profile"></a>Profilo

* az login: visualizzazione dell'avviso per l'errore MFA

### <a name="rdbms"></a>RDBMS

* Aggiunta dei comandi di gestione delle chiavi di crittografia dei dati server per PostgreSQL e MySQL

## <a name="march-10-2020"></a>10 marzo 2020

Versione 2.2.0

### <a name="acr"></a>ACR

* Correzione della generazione di un errore in `az acr login`
* Aggiunta del nuovo comando `az acr helm install-cli`
* Aggiunta del supporto per collegamento privato e CMK
* Aggiunta del comando 'private-link-resource list'

### <a name="aks"></a>Servizio Azure Kubernetes

* Correzione dell'esplorazione del servizio Azure Kubernetes in Cloud Shell
* az aks: correzione degli errori NoneType per il monitoraggio dei componenti aggiuntivi e dei pool di agenti
* Aggiunta di --nodepool-tags al pool di nodi durante la creazione del cluster Kubernetes
* Aggiunta di --tags durante l'aggiunta o l'aggiornamento di un pool di nodi al cluster
* aks create: aggiunta di `--enable-private-cluster`
* Aggiunta di --nodepool-labels durante la creazione del cluster Kubernetes
* Aggiunta di --labels quando si aggiunge un nuovo pool di nodi al cluster Azure kubernetes
* Aggiunta di una barra / mancante nell'URL del dashboard
* Supporto della creazione di cluster del servizio Azure Kubernetes con identità gestita
* az aks: convalida del plug-in di rete affinché sia "Azure" o "kubenet"
* az aks: aggiunta del supporto della chiave della sessione AAD
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] az aks: supporto delle modifiche dell'identità del servizio gestita per GF e BF per omsagent (monitoraggio contenitore)(#1)
* az aks use-dev-spaces: aggiunta dell'opzione per il tipo di endpoint al comando use-dev-spaces per personalizzare l'endpoint creato in un controller di Azure Dev Spaces

### <a name="appconfig"></a>AppConfig

* Sblocco con "kv set" per aggiungere la funzionalità e il riferimento all'insieme di credenziali delle chiavi...

### <a name="appservice"></a>AppService

* az webapp create: correzione del problema durante l'esecuzione del comando con --runtime
* az functionapp deployment source config-zip: aggiunta di un messaggio di errore se il nome del gruppo di risorse o della funzione non è valido o non esiste
* functionapp create: correzione del messaggio di avviso visualizzato con `functionapp create` oggi che indica un flag `--functions_version` ma usa erroneamente un `_` anziché un `-` nel nome del flag
* az functionapp create: aggiornata la modalità di impostazione di linuxFxVersion e del nome dell'immagine del contenitore per le app per le funzioni Linux
* az functionapp deployment source config-zip: correzione di un problema causato dalla modifica delle impostazioni di race condition dell'app durante la distribuzione con zipdeploy, che restituisce errori con codice 5xx durante la distribuzione
* Correzione #5720946: az webapp backup non riesce a impostare il nome

### <a name="arm"></a>ARM

* az resource: miglioramento degli esempi del modulo resource
* az policy assignment list: supporto dell'elenco delle assegnazioni di criteri nell'ambito del gruppo di gestione
* Aggiunta di `az deployment group` e `az deployment operation group` per la distribuzione di modelli a livello dei gruppi di risorse. Si tratta di un duplicato di `az group deployment` e `az group deployment operation`
* Aggiunta di `az deployment sub` e `az deployment operation sub` per la distribuzione di modelli a livello della sottoscrizione. Si tratta di un duplicato di `az deployment` e `az deployment operation`
* Aggiunta di `az deployment mg` e `az deployment operation mg` per la distribuzione di modelli a livello dei gruppi di gestione
* Aggiunta di `az deployment tenant` e `az deployment operation tenant` per la distribuzione di modelli a livello del tenant
* az policy assignment create: aggiunta di una descrizione al parametro `--location`
* az group deployment create: aggiunta del parametro `--aux-tenants` per il supporto tra tenant

### <a name="cdn"></a>RETE CDN

* Aggiunta dei comandi WAF della rete CDN

### <a name="compute"></a>Calcolo

* az sig image-version: aggiunta di --data-snapshot-luns
* az ppg show: aggiunta di --colocation-status per abilitare il recupero dello stato di condivisione risorse per tutte le risorse nel gruppo di selezione host di prossimità
* az vmss create/update: supporto per le riparazioni automatiche
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] az image template: sostituito template con builder
* az image builder create: aggiunta di --image-template

### <a name="cosmos-db"></a>Cosmos DB

* Aggiunti cmdlet per trigger, funzioni definite dall'utente e stored procedure SQL
* az cosmosdb create: aggiunta di --key-uri per il supporto dell'aggiunta delle informazioni di crittografia dell'insieme di credenziali delle chiavi

### <a name="keyvault"></a>Insieme di credenziali delle chiavi

* keyvault create: abilitazione dell'eliminazione temporanea per impostazione predefinita

### <a name="monitor"></a>Monitorare

* az monitor metrics alert create: supporto di `~` in `--condition`

### <a name="network"></a>Rete

* az network application-gateway rewrite-rule create: supporto della configurazione URL
* az network dns zone import: --zone-name non farà distinzione tra maiuscole/minuscole in futuro
* az network private-endpoint/private-link-service: rimozione dell'etichetta anteprima
* az network bastion: supporto di bastion
* az network vnet list-available-ips: supporto dell'elenco degli indirizzi IP disponibili in una rete virtuale
* az network watcher flow-log create/list/delete/update: aggiunta di nuovi comandi per gestire il log del flusso del watcher ed esposizione di --location per identificare in modo esplicito il watcher
* az network watcher flow-log configure: deprecato
* az network watcher flow-log show: supporto di --location e --name per il recupero di un risultato formattato per ARM, l'output con il formato precedente è deprecato

### <a name="policy"></a>Policy

* az policy assignment create: correzione del bug relativo al superamento del limite del nome automaticamente generato dell'assegnazione dei criteri

### <a name="rbac"></a>Controllo degli accessi in base al ruolo

* az ad group show: correzione del valore --group considerato come problema di espressione regolare

### <a name="rdbms"></a>RDBMS

* Bump della versione di azure-mgmt-rdbms SDK alla versione 2.0.0
* az postgres private-endpoint-connection: gestione delle connessioni endpoint privato di Postgres
* az postgres private-link-resource: gestione delle risorse Collegamento privato di Postgres
* az mysql private-endpoint-connection: gestione delle risorse endpoint privato di MySQL
* az mysql private-link-resource: gestione delle risorse Collegamento privato di MySQL
* az mariadb private-endpoint-connection: gestione delle risorse endpoint privato di MariaDB
* az mariadb private-link-resource: gestione delle risorse Collegamento privato di MariaDB
* Aggiornamento dei test di endpoint privato di RDBMS

### <a name="sql"></a>SQL

* Sql midb Add: list-deleted, show-deleted, update-retention, show-retention
* (sql server create:) aggiunta del flag facoltativo public-network-access 'Enable'/'Disable' a sql server create
* (sql server update:) apportate alcune modifiche per i clienti
* Aggiunta della proprietà minimal_tls_version per Istanza gestita e database SQL

### <a name="storage"></a>Archiviazione

* az storage blob delete-batch: flag `--dryrun` con comportamento non corretto
* az storage account network-rule add (correzione di bug): l'operazione add deve essere idempotente
* az storage account create/update: aggiunta del supporto delle preferenze di routing
* Aggiornamento di azure-mgmt-storage alla versione 8.0.0
* az storage container immutability create: aggiunta del parametro --allow-protected-append-write
* az storage account private-link-resource list: aggiunta del supporto per elencare le risorse Collegamento privato per l'account di archiviazione
* az storage account private-endpoint-connection approve/reject/show/delete: supporto per la gestione delle connessioni endpoint privato
* az storage account blob-service-properties update: aggiunta di --enable-restore-policy e --restore-days
* az storage blob restore: aggiunta del supporto per il ripristino di intervalli BLOB

## <a name="february-18-2020"></a>18 febbraio 2020

Versione 2.1.0

### <a name="acr"></a>ACR

* Aggiunta di un nuovo argomento `--expose-token` per `az acr login`
* Correzione dell'output non corretto di `az acr task identity show -n Name -r Registry -o table`
* az acr login: generazione di un errore dell'interfaccia della riga di comando se vengono restituiti errori dal comando docker

### <a name="acs"></a>ACS

* aks create/update: aggiunta la convalida di `--vnet-subnet-id`

### <a name="aladdin"></a>Aladdin

* Analisi degli esempi generati nei comandi _help.py

### <a name="ams"></a>AMS

* az ams è ora disponibile a livello generale

### <a name="appconfig"></a>AppConfig

* Modifica del messaggio della Guida per escludere il filtro chiave/etichetta non supportato
* Rimozione del tag di anteprima per la maggior parte dei comandi esclusi i flag di identità gestite e funzionalità
* Aggiunta di una chiave gestita dal cliente durante l'aggiornamento degli archivi

### <a name="appservice"></a>AppService

* az webapp list-runtimes: correzione del bug per list-runtimes
* Aggiunta di az webapp|functionapp config ssl create
* Aggiunta del supporto per le app per le funzioni v3 e nodo 12

### <a name="arm"></a>ARM

* az policy assignment create: correzione del messaggio di errore quando il parametro `--policy` non è valido
* az group deployment create: correzione dell'errore "stat: percorso troppo lungo per Windows" quando si usa il file parameters.json di grandi dimensioni

### <a name="backup"></a>Backup

* Correzione per il flusso di ripristino a livello di elemento in OLR
* Aggiunta del supporto del ripristino come file per database SQL e SAP

### <a name="compute"></a>Calcolo

* vm/vmss/availability-set update: aggiunta di --ppg per consentire l'aggiornamento di ProximityPlacementGroup
* vmss create: aggiunta di --data-disk-iops e --data-disk-mbps
* az vm host: rimozione del tag di anteprima per `vm host` e `vm host group`
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Correzione #10728: `az vm create`: creazione automatica della subnet se viene specificata una rete virtuale e la subnet non esiste
* Aumento dell'affidabilità dell'elenco di immagini della macchina virtuale

### <a name="eventhub"></a>Eventhub

* Supporto di Azure Stack per il profilo 2019-03-01-hybrid

### <a name="keyvault"></a>Insieme di credenziali delle chiavi

* az keyvault key create: aggiunta di un nuovo valore `import` per il parametro `--ops`
* az keyvault key list-versions: supporto del parametro `--id` per la specifica delle chiavi
* Supporto delle connessioni all'endpoint privato

### <a name="network"></a>Rete

* Aggiornamento ad azure-mgmt-network 9.0.0
* az network private-link-service update/create: supporto di --enable-proxy-protocol
* Aggiunta la funzionalità di connessione Monitor V2

### <a name="packaging"></a>Packaging

* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Eliminazione del supporto per Python 2.7

### <a name="profile"></a>Profilo

* Anteprima: Aggiunti i nuovi attributi `homeTenantId` e `managedByTenants` agli account della sottoscrizione. Eseguire di nuovo `az login` per rendere effettive le modifiche
* az login: visualizzazione di un avviso quando una sottoscrizione viene elencata da più tenant e per impostazione predefinita viene selezionato il primo. Per selezionare un tenant specifico quando si accede alla sottoscrizione, includere `--tenant` in `az login`

### <a name="role"></a>Ruolo

* az role assignment create: correzione dell'errore per cui l'assegnazione di un ruolo a un'entità servizio con il nome visualizzato restituisce l'errore HTTP 400

### <a name="sql"></a>SQL

* Aggiornamento del cmdlet `az sql mi update` dell'Istanza gestita SQL con due nuovi parametri: tier e family

### <a name="storage"></a>Archiviazione

* [MODIFICA DI RILIEVO] `az storage account create`: modifica del tipo di account di archiviazione predefinito con StorageV2

## <a name="february-04-2020"></a>4 febbraio 2020

Versione 2.0.81

### <a name="acs"></a>ACS

* Aggiunta del supporto per impostare le porte allocate in uscita e i timeout di inattività per il bilanciamento del carico standard
* Aggiornamento alla versione API 2019-11-01

### <a name="acr"></a>ACR

* [MODIFICA CHE CAUSA UN'INTERRUZIONE]  Richiesta di conferma per `az acr delete`
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Richiesta di conferma per 'az acr task delete'
* Aggiunta di un nuovo gruppo di comandi 'az acr taskrun show/list/delete' per la gestione dell'esecuzione di attività

### <a name="aks"></a>Servizio Azure Kubernetes

* Ogni cluster ottiene un'entità servizio separata per migliorare l'isolamento

### <a name="appconfig"></a>AppConfig

* Supporto dell'importazione/esportazione dei riferimenti a Key Vault da/in Servizio app
* Supporto dell'importazione/esportazione di tutte le etichette da appconfig ad appconfig
* Convalida dei nomi di chiavi e funzionalità prima dell'impostazione e dell'impostazione
* Esposizione della modifica dello SKU per l'archivio di configurazione.
* Aggiunta di un gruppo di comandi per l'identità gestita.

### <a name="appservice"></a>AppService

* Azure Stack: comandi Surface nel profilo di 2019-03-01-hybrid
* functionapp: aggiunta la possibilità di creare app per le funzioni Java in Linux

### <a name="arm"></a>ARM

* Correzione del problema #10246: `az resource tag` si arresta in modo anomalo quando il parametro `--ids` passato è un ID gruppo di risorse
* Correzione del problema #11658: il comando `az group export` non supporta i parametri `--query` e `--output`
* Correzione del problema #10279: il codice di uscita di `az group deployment validate` è 0 quando la verifica ha esito negativo
* Correzione del problema #9916: migliorare il messaggio di errore del conflitto tra un tag e altre condizioni di filtro per il comando `az resource list`
* Aggiunta del nuovo parametro `--managed-by` per supportare l'aggiunta di informazioni di managedBy per il comando `az group create`

### <a name="azure-red-hat-openshift"></a>Azure Red Hat OpenShift

* Aggiunta del sottogruppo `monitor` per gestire il monitoraggio di Log Analytics nel cluster Azure Red Hat OpensShift

### <a name="botservice"></a>BotService

* Correzione del problema #11697: `az bot create` non è idempotente
* Modifica dei test di correzione del nome solo per l'esecuzione in modalità live

### <a name="cdn"></a>RETE CDN

* Aggiunta del supporto per la funzionalità rulesEngine
* Aggiunta del nuovo gruppo di comandi 'cdn endpoint rule' per gestire le regole
* Aggiornamento di azure-mgmt-cdn alla versione 4.0.0 per usare l'API versione 2019-04-15

### <a name="deployment-manager"></a>Deployment Manager

* Aggiunta dell'operazione di elenco per tutte le risorse.
* Miglioramento della risorsa di passaggio per il nuovo tipo di passaggio.
* Aggiornamento del pacchetto azure-mgmt-deploymentmanager per usare la versione 0.2.0.

### <a name="iot"></a>IoT

* Deprecamento dei comandi 'IoT hub Job'.

### <a name="iot-central"></a>IoT Central

* Supporto della creazione/aggiornamento di app con il nuovo nome dello SKU ST0, ST1, ST2.

### <a name="key-vault"></a>Key Vault

* Aggiunta del nuovo comando `az keyvault key download` per il download delle chiavi.

### <a name="misc"></a>Varie

* Correzione #6371: Supporto del nome file e della variabile di ambiente in Bash

### <a name="network"></a>Rete

* Correzione #2092: az network dns record-set add/remove: aggiunta di un avviso quando record-set non viene trovato. In futuro, sarà supportato un argomento aggiuntivo per confermare la creazione automatica.

### <a name="policy"></a>Policy

* Aggiunta del nuovo comando `az policy metadata` per recuperare le risorse metadati dei criteri avanzate
* `az policy remediation create`: consente di specificare se la conformità deve essere rivalutata prima della correzione con il parametro `--resource-discovery-mode`

### <a name="profile"></a>Profilo

* `az account get-access-token`: aggiunta del parametro `--tenant` per acquisire direttamente un token per il tenant, senza che sia necessario specificare una sottoscrizione

### <a name="rbac"></a>Controllo degli accessi in base al ruolo

* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Correzione #11883: `az role assignment create`: l'ambito vuoto visualizzerà un errore

### <a name="security"></a>Sicurezza

* Aggiunta dei nuovi comandi `az atp show` e `az atp update` per visualizzare e gestire le impostazioni di Advanced Threat Protection per gli account di archiviazione.

### <a name="sql"></a>SQL

* `sql dw create`: deprecazione dei parametri `--zone-redundant` e `--read-replica-count`. Questi parametri non si applicano a Data Warehouse.
* [MODIFICA DI RILIEVO] `az sql db create`: rimozione di "WideWorldImportersStd" e "WideWorldImportersFull" come valori consentiti documentati per "az sql db create --sample-name". Questi database di esempio comporteranno sempre un errore di creazione.
* Aggiunta dei nuovi comandi `sql db classification show/list/update/delete` e `sql db classification recommendation list/enable/disable` per gestire le classificazioni di riservatezza per i database SQL.
* `az sql db audit-policy`: correzione per azioni e gruppi di controllo vuoti

### <a name="storage"></a>Archiviazione

* Aggiunta di un nuovo gruppo di comandi `az storage share-rm` per usare il provider di risorse Microsoft.Storage per le operazioni di gestione delle condivisioni file di Azure.
* Correzione del problema #11415: errore di autorizzazione per `az storage blob update`
* Integrazione di Azcopy 10.3.3 e supporto di Win32.
* `az storage copy`: aggiunta dei parametri `--include-path`, `--include-pattern`, `--exclude-path` e `--exclude-pattern`
* `az storage remove`: sostituzione dei parametri `--inlcude` e `--exclude` con i parametri `--include-path`, `--include-pattern`, `--exclude-path` e `--exclude-pattern`
* `az storage sync`: aggiunta dei parametri `--include-pattern`, `--exclude-path` e `--exclude-pattern`

### <a name="servicefabric"></a>ServiceFabric

* Aggiunta di nuovi comandi per la gestione di applicazioni e servizi.

## <a name="january-13-2020"></a>13 gennaio 2020

Versione 2.0.80

### <a name="compute"></a>Calcolo

* disk update: aggiunta di --disk-encryption-set e --encryption-type
* snapshot create/update: aggiunta di --disk-encryption-set e --encryption-type

### <a name="storage"></a>Archiviazione

* Aggiornamento di azure-mgmt-storage alla versione 7.1.0
* `az storage account create`: Aggiunta di `--encryption-key-type-for-table` e `--encryption-key-type-for-queue` per supportare il servizio di crittografia di tabelle e code

## <a name="january-07-2020"></a>7 gennaio 2020

Versione 2.0.79

### <a name="acr"></a>ACR

* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Rimozione del parametro '--os' per 'acr build', 'acr task create/update', 'acr run' e 'acr pack'. Usare invece '--platform'.

### <a name="appconfig"></a>AppConfig

* Aggiunta del supporto per l'importazione/esportazione di flag di funzionalità
* Aggiunta del nuovo comando 'az appconfig kv set-keyvault' per la creazione di un riferimento all'insieme di credenziali delle chiavi
* Supporto di varie convenzioni di denominazione per l'esportazione dei flag di funzionalità in un file

### <a name="appservice"></a>AppService

* Correzione del problema #7154: aggiornamento della documentazione per il comando <> per usare gli apici inversi invece delle virgolette singole
* Correzione del problema #11287: webapp up: per impostazione predefinita, l'app creata con 'webapp up' deve essere 'abilitata per SSL'
* Correzione del problema #11592: aggiunta del flag az webapp up per i siti HTML statici

### <a name="arm"></a>ARM

* Correzione di `az resource tag`: non è possibile aggiornare i tag dell'insieme di credenziali dei servizi di ripristino

### <a name="backup"></a>Backup

* Aggiunta del nuovo comando 'backup protection undelete' per abilitare la funzionalità di eliminazione temporanea per il carico di lavoro IaasVM
* Aggiunta del nuovo parametro '--soft-delete-feature-state' per impostare il comando backup-properties
* Aggiunta del supporto per l'esclusione disco per il carico di lavoro IaasVM

### <a name="compute"></a>Calcolo

* Correzione dell'errore `vm create` nel profilo di Azure Stack.
* vm monitor metrics tail/list-definitions: supporto delle definizioni di elenchi e metriche delle query per una VM.
* Aggiunta della nuova azione di comando reapply per az vm

### <a name="hdinsight"></a>HDInsight

* Supporto per la creazione di un cluster Kafka con proxy Kafka Rest
* Aggiornamento di azure-mgmt-hdinsight alla versione 1.3.0

### <a name="misc"></a>Varie

* Aggiunta del comando di anteprima `az version show` per mostrare le versioni dei moduli e delle estensioni dell'interfaccia della riga di comando di Azure in formato JSON per impostazione predefinita o nel formato configurato da --output

### <a name="event-hubs"></a>Hub eventi

* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Rimozione dell'opzione di stato 'ReceiveDisabled' dai comandi 'az eventhubs eventhub update' e 'az eventhubs eventhub create'. Questa opzione non è valida per le entità dell'hub eventi.

### <a name="service-bus"></a>Bus di servizio

* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Rimozione dell'opzione di stato 'ReceiveDisabled' dai comandi 'az servicebus topic create', 'az servicebus topic update', 'az servicebus queue create' e 'az servicebus queue update'. Questa opzione non è valida per argomenti e code del bus di servizio.

### <a name="rbac"></a>Controllo degli accessi in base al ruolo

* Correzione #11712: `az ad app/sp show` non restituisce il codice di uscita 3 se l'applicazione o l'entità servizio non esiste

### <a name="storage"></a>Archiviazione

* `az storage account create`: Rimozione del flag di anteprima per il parametro --enable-hierarchical-namespace
* Aggiornamento di azure-mgmt-storage alla versione 7.0.0 per usare l'API versione 2019-06-01
* Aggiunta dei nuovi parametri `--enable-delete-retention` e `--delete-retention-days` per supportare la gestione dell'eliminazione di criteri di conservazione per le proprietà del servizio BLOB dell'account di archiviazione.

## <a name="december-17-2019"></a>17 dicembre 2019

2.0.78

### <a name="acr"></a>ACR

* Aggiunta del supporto del contesto locale nell'esecuzione di Attività Registro Azure Container

### <a name="acs"></a>ACS

* [MODIFICA CHE CAUSA UN'INTERRUZIONE] az openshift create: ridenominazione di `--workspace-resource-id` in `--workspace-id`.

### <a name="ams"></a>AMS

* Aggiornamento del comando show per restituire 3 quando la risorsa non è stata trovata

### <a name="appconfig"></a>AppConfig

* Correzione del bug durante l'aggiunta di api-version all'URL della richiesta. La soluzione esistente non funziona con la paginazione.
* Aggiunta del supporto per la visualizzazione di lingue oltre all'inglese perché il servizio back-end supporta Unicode per la globalizzazione.

### <a name="appservice"></a>AppService

* Correzione del problema #11217: webapp: az webapp config ssl upload deve supportare il parametro slot
* Correzione del problema #10965: Errore: Il nome non può essere vuoto. Rimozione consentita in base a ip_address e subnet
* Aggiunta del supporto per l'importazione di certificati da Key Vault `az webapp config ssl import`

### <a name="arm"></a>ARM

* Aggiornamento del pacchetto azure-mgmt-resource per l'uso della versione 6.0.0
* Supporto tra tenant per il comando `az group deployment create` tramite l'aggiunta del nuovo parametro `--aux-subs`
* Aggiunta del nuovo parametro `--metadata` per supportare l'aggiunta di informazioni sui metadati per le definizioni di set di criteri.

### <a name="backup"></a>Backup

* Aggiunta del supporto di backup per il carico di lavoro SQL e SAP Hana.

### <a name="botservice"></a>BotService

* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Rimozione del flag '--version' dal comando di anteprima 'az bot create'. Sono supportati solo i bot dell'SDK v4.
* Aggiunta verifica della disponibilità dei nomi per 'az bot create'.
* Aggiunta del supporto per l'aggiornamento dell'URL dell'icona per un bot tramite 'az bot update'.
* Aggiunta del supporto per l'aggiornamento di un canale Direct Line tramite 'az bot directline update'.
* Aggiunta del supporto del flag '--enable-enhanced-auth' in 'az bot directline create'.
* I gruppi di comandi seguenti sono disponibili a livello generale e non in anteprima: 'az bot authsetting'.
* I comandi seguenti di 'az bot' sono disponibili a livello generale e non in anteprima: 'create', 'prepare-deploy', 'show', 'delete', 'update'.
* Correzione di 'az bot prepare-deploy' cambiando il valore di '--proj-file-path' in caratteri minuscoli (ad esempio da "Test.csproj" a "test.csproj").

### <a name="compute"></a>Calcolo

* vmss create/update: aggiunta di --scale-in-policy, che stabilisce quali macchine virtuali vengono scelte per la rimozione quando un set di scalabilità di macchine virtuali viene ridotto.
* vm/vmss update: aggiunta di --priority.
* vm/vmss update: aggiunta di --max-price.
* Aggiunta del gruppo di comandi disk-encryption-set (create, show, update, delete, list).
* disk create: aggiunta di --encryption-type e --disk-encryption-set.
* vm/vmss create: aggiunta di --os-disk-encryption-set e --data-disk-encryption-sets.

### <a name="core"></a>Core

* Rimozione del supporto per Python 3.4
* Invio del sondaggio HaTS in più comandi

### <a name="dls"></a>DLS

* Aggiornamento della versione di ADLS SDK (0.0.48).

### <a name="install"></a>Installazione

* Supporto dello script di installazione in Python 3.8

### <a name="iot"></a>IoT

* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Rimozione del parametro --failover-region da manual-failover. Ora il failover verrà eseguito in un'area secondaria geograficamente abbinata assegnata.

### <a name="key-vault"></a>Key Vault

* Correzione #8095: `az keyvault storage remove`: miglioramento del messaggio della Guida
* Correzione #8921: `az keyvault key/secret/certificate list/list-deleted/list-versions`: correzione del bug di convalida nel parametro `--maxresults`
* Correzione #10512: `az keyvault set-policy`: miglioramento del messaggio di errore quando non viene specificato alcun elemento `--object-id`, `--spn` o `--upn`
* Correzione #10846: `az keyvault secret show-deleted`: quando viene specificato `--id`, `--name/-n` non è necessario
* Correzione #11084: `az keyvault secret download`: miglioramento del messaggio della Guida del parametro `--encoding`

### <a name="network"></a>Rete

* az network application-gateway probe: aggiunta del supporto dell'opzione --port per specificare una porta per il probe dei server back-end durante la creazione e l'aggiornamento
* az network application-gateway url-path-map create/update: correzione del bug per `--waf-policy`
* az network application-gateway: aggiunta del supporto per `--rewrite-rule-set`
* az network list-service-aliases: aggiunta del supporto per gli alias del servizio elenco che è possibile usare per i criteri degli endpoint di servizio
* az network dns zone import: Aggiunta del supporto per .@ nel nome del record

### <a name="packaging"></a>Packaging

* Aggiunta di compilazioni back edge per installazioni pip
* Aggiunta del pacchetto Ubuntu eoan

### <a name="policy"></a>Policy

* Aggiunta del supporto per l'API di criteri versione 2019-09-01.
* az policy set-definition: aggiunta del supporto per il raggruppamento all'interno di definizioni di set di criteri con il parametro `--definition-groups`

### <a name="redis"></a>Redis

* Aggiunta del parametro di anteprima `--replicas-per-master` al comando `az redis create`
* Aggiornamento di azure-mgmt-redis da 6.0.0 a 7.0.0rc1

### <a name="servicefabric"></a>ServiceFabric

* Correzione della logica di aggiunta del tipo di nodo che include #10963: l'aggiunta di un nuovo tipo di nodo con livello di durabilità Gold genera sempre un errore dell'interfaccia della riga di comando
* Aggiornamento di ServiceFabricNodeVmExt alla versione 1.1 nel modello di creazione

### <a name="sql"></a>SQL

* Aggiunta dei parametri "--read-scale" e "--read-replicas" ai comandi sql db create e update per supportare la gestione della scalabilità in lettura.

### <a name="storage"></a>Archiviazione

* Rilascio in disponibilità generale della proprietà di condivisioni file di grandi dimensioni per i comandi create e update dell'account di archiviazione
* Rilascio in disponibilità generale del supporto del token di firma di accesso condiviso con delega utente
* Aggiunta dei nuovi comandi `az storage account blob-service-properties show` e `az storage account blob-service-properties update --enable-change-feed` per gestire le proprietà del servizio BLOB per l'account di archiviazione.
* [MODIFICA DI RILIEVO IMMINENTE] `az storage copy`: il carattere `*` non è più supportato come carattere jolly nell'URL, ma i nuovi parametri --include-pattern e --exclude-pattern verranno aggiunti con il supporto del carattere jolly `*`.
* Correzione del problema #11043: aggiunta del supporto per la rimozione di un intero contenitore/condivisione nel comando `az storage remove`

## <a name="november-26-2019"></a>26 novembre 2019

Versione 2.0.77

### <a name="acr"></a>ACR

* Deprecazione del parametro `--branch` per la creazione/aggiornamento di Attività Registro Azure Container

### <a name="azure-red-hat-openshift"></a>Azure Red Hat OpenShift

* Aggiunta del flag `--workspace-resource-id` per consentire la creazione del cluster Azure Red Hat Openshift con monitoraggio
* Aggiunta di `monitor_profile` per la creazione del cluster Azure Red Hat Openshift con monitoraggio

### <a name="aks"></a>Servizio Azure Kubernetes

* Aggiunta del supporto per l'operazione di rotazione del certificato cluster tramite "az aks rotate-certs".

### <a name="appconfig"></a>AppConfig

* Aggiunta del supporto per l'utilizzo di ":" per il separatore di `as az appconfig kv import`
* Risoluzione del problema relativo all'elenco dei valori delle chiavi con più etichette, inclusa l'etichetta Null. 
* Aggiornamento dell'SDK del piano di gestione, azure-mgmt-appconfiguration, alla versione 0.3.0. 

### <a name="appservice"></a>AppService

* Risoluzione del problema #11100: AttributeError per az webapp up durante la creazione del piano del servizio
* az webapp up: Forzando la creazione o la distribuzione in un sito per le lingue supportate, non viene usato alcun valore predefinito.
* Aggiunta del supporto per le operazioni dell'ambiente del servizio app seguenti: az appservice ase show | list | list-addresses | list-plans | create | update | delete

### <a name="backup"></a>Backup

* Correzione di un problema in az backup policy list-associated-items. Aggiunta del parametro BackupManagementType facoltativo.

### <a name="compute"></a>Calcolo

* Aggiornamento della versione API per calcolo, dischi e snapshot alla versione 2019-07-01
* vmss create: miglioramento per --orchestration-mode
* sig image-definition create: aggiunta di --os-state per consentire di specificare se le macchine virtuali create in questa immagine sono 'generalizzate' o 'specializzate'
* sig image-definition create: aggiunta di --hyper-v-generation per consentire di specificare la generazione dell'hypervisor
* sig image-version create: aggiunta del supporto per --os-snapshot e --data-snapshots
* image create: aggiunta di --data-disk-caching per consentire di specificare l'impostazione della memorizzazione nella cache dei dischi dati
* Aggiornamento di Python Compute SDK alla versione 10.0.0
* vm/vmss create: aggiunta di 'Spot' alla proprietà di enumerazione 'Priority'
* [Modifica che causa un'interruzione] Ridenominazione del parametro '--max-billing' in '--max-price' per macchine virtuali e set di scalabilità di macchine virtuali per garantire la coerenza con Swagger e i cmdlet di PowerShell
* vm monitor log show: aggiunta del supporto per l'esecuzione di query sui log nell'area di lavoro Log Analytics collegata.

### <a name="iot"></a>IoT

* Correzione #2531: aggiunta di argomenti utili per l'aggiornamento dell'hub.
* Correzione #8323: aggiunta di parametri mancanti per la creazione dell'endpoint personalizzato di archiviazione.
* Correzione bug di regressione: annullamento delle modifiche che eseguono l'override dell'endpoint di archiviazione predefinito.

### <a name="key-vault"></a>Key Vault

* Correzione #11121: quando si usa `az keyvault certificate list`, il passaggio di `--include-pending` ora non richiede un valore `true` o `false`

### <a name="netappfiles"></a>NetAppFiles

* Aggiornamento di azure-mgmt-netapp alla versione 0.7.0 che include alcune proprietà aggiuntive del volume associate alle operazioni di replica future

### <a name="network"></a>Rete

* application-gateway waf-config: deprecata
* application-gateway waf-policy: aggiunta di regole gestite del sottogruppo per gestire set di regole gestite e regole di esclusione
* application-gateway waf-policy: aggiunta dell'impostazione dei criteri del sottogruppo per gestire la configurazione globale di un criterio WAF
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] application-gateway waf-policy: ridenominazione della regola del sottogruppo in regola personalizzata
* application-gateway http-listener: aggiunta di --firewall-policy durante la creazione
* application-gateway url-path-map rule: aggiunta di --firewall-policy durante la creazione

### <a name="packaging"></a>Packaging

* Riscrittura di az wrapper in Python
* Aggiunta del supporto per Python 3.8
* Modifica in Python 3 per il pacchetto RPM

### <a name="profile"></a>Profilo

* Correzione dell'errore durante l'esecuzione di `az login -u {} -p {}` con l'account Microsoft
* Correzione dell'errore `SSLError` durante l'esecuzione di `az login` usando un proxy con un certificato radice autofirmato
* Correzione #10578: `az login` si blocca quando vengono avviate contemporaneamente più istanze in Windows o WSL
* Correzione #11059: `az login --allow-no-subscriptions` ha esito negativo se sono presenti sottoscrizioni nel tenant
* Correzione #11238: dopo la ridenominazione di una sottoscrizione, l'accesso con un'identità del servizio gestita comportava la visualizzazione della stessa sottoscrizione due volte

### <a name="rbac"></a>Controllo degli accessi in base al ruolo

* Correzione #10996: correzione dell'errore `--force-change-password-next-login` in `az ad user update` quando non è specificato il parametro `--password`

### <a name="redis"></a>Redis

* Correzione #2902: evitare di impostare configurazioni di memoria durante l'aggiornamento della cache con SKU Basic

### <a name="reservations"></a>Prenotazioni

* Aggiornamento della versione dell'SDK alla versione 0.6.0
* Aggiunta delle informazioni dettagliate su billingplan dopo la chiamata a Get-Gatalogs
* Aggiunta del nuovo comando `az reservations reservation-order calculate` per calcolare il prezzo di una prenotazione
* Aggiunta del nuovo comando `az reservations reservation-order purchase` per acquistare una nuova prenotazione

### <a name="rest"></a>Rest
* Modifica di `az rest` nella versione disponibile a livello generale

### <a name="sql"></a>SQL

* Aggiornamento di azure-mgmt-sql alla versione 0.15.0.

### <a name="storage"></a>Archiviazione

* storage account create: aggiunta di --Enable-Hierarchical-Namespace per supportare la semantica del file system nel servizio BLOB.
* Rimozione dell'eccezione non correlata dal messaggio di errore
* Correzione di problemi con il messaggio di errore non corretto "Non si dispone delle autorizzazioni necessarie per eseguire questa operazione". in caso di blocco da parte delle regole di rete o di autenticazione non riuscita.

## <a name="november-4-2019"></a>4 novembre 2019

Versione 2.0.76

### <a name="acr"></a>ACR

* Aggiunta del parametro di anteprima `--pack-image-tag` al comando `az acr pack build`.
* Aggiunta del supporto per l'abilitazione del controllo durante la creazione di un registro
* Aggiunta del supporto per il controllo degli accessi in base al ruolo a livello di repository

### <a name="aks"></a>Servizio Azure Kubernetes

* Aggiunta di `--enable-cluster-autoscaler`, `--min-count` e `--max-count` al comando `az aks create` per abilitare la scalabilità automatica del cluster per il pool di nodi.
* Aggiunta dei flag precedenti, nonché di `--update-cluster-autoscaler` e `--disable-cluster-autoscaler` al comando `az aks update` per consentire gli aggiornamenti della scalabilità automatica del cluster.

### <a name="appconfig"></a>AppConfig

* Aggiunta del gruppo di comandi della funzionalità AppConfig per gestire i flag di funzionalità archiviati in una configurazione dell'app.
* Correzione del bug secondario per il comando di esportazione nel file del Key Vault di AppConfig. Interruzione della lettura del contenuto del file di destinazione durante l'esportazione.

### <a name="appservice"></a>AppService

* `az appservice plan create`: aggiunta del supporto per l'impostazione di 'persitescaling' su appservice plan create.
* Correzione di un problema per cui l'operazione di binding SSL di webapp config rimuoveva i tag esistenti dalla risorsa
* Aggiunta del flag `--build-remote` a `az functionapp deployment source config-zip` per supportare l'azione di compilazione remota durante la distribuzione di app per le funzioni.
* Impostazione della versione del nodo predefinita nelle app per le funzioni su ~10 per Windows
* Aggiunta della proprietà `--runtime-version` a `az functionapp create`

### <a name="arm"></a>ARM

* `az deployment/group deployment validate`: aggiunta del parametro `--handle-extended-json-format` per supportare più righe e commenti nel modello JSON durante la distribuzione.
* Aggiornamento di azure-mgmt-resource alla versione 2019-07-01

### <a name="backup"></a>Backup

* Aggiunta del supporto per il backup di file di azure

### <a name="compute"></a>Calcolo

* `az vm create`: aggiunta di un avviso quando si specificano contemporaneamente una rete accelerata e una scheda di interfaccia di rete esistente.
* `az vm create`: aggiunta di `--vmss` per specificare un set di scalabilità di macchine virtuali esistente a cui deve essere assegnata la macchina virtuale.
* `az vm/vmss create`: aggiunta di una copia locale del file di alias di immagine per consentire l'accesso in un ambiente di rete con restrizioni.
* `az vmss create`: aggiunta di `--orchestration-mode` per specificare la modalità di gestione delle macchine virtuali da parte del set di scalabilità.
* `az vm/vmss update`: aggiunta di `--ultra-ssd-enabled` per consentire l'aggiornamento dell'impostazione Ultra SSD.
* [MODIFICA DI RILIEVO] `az vm extension set`: correzione del bug per cui gli utenti non potevano impostare un'estensione in una macchina virtuale con `--ids`.
* Aggiunta di nuovi comandi `az vm image terms accept/cancel/show` per gestire le condizioni per le immagini di Azure Marketplace.
* Aggiornamento di VMAccessForLinux alla versione 1.5

### <a name="cosmosdb"></a>Cosmos DB

* [MODIFICA DI RILIEVO] `az sql container create`: modifica di `--partition-key-path` al parametro obbligatorio
* [MODIFICA DI RILIEVO] `az gremlin graph create`: modifica di `--partition-key-path` al parametro obbligatorio
* `az sql container create`: Aggiunta di `--unique-key-policy` e `--conflict-resolution-policy`
* `az sql container create/update`: aggiornamento dello schema predefinito di `--idx`
* `gremlin graph create`: Aggiunta di `--conflict-resolution-policy`
* `gremlin graph create/update`: aggiornamento dello schema predefinito di `--idx`
* Correzione di un errore di digitazione in un messaggio della Guida
* database: aggiunta delle informazioni sulla deprecazione
* collection: aggiunta delle informazioni sulla deprecazione

### <a name="iot"></a>IoT

* Aggiunta di un nuovo tipo di origine di routing: DigitalTwinChangeEvents
* Correzione per le funzionalità mancanti in `az iot hub create`

### <a name="key-vault"></a>Key Vault

* Correzione di un errore imprevisto che si verifica quando il file di certificato non esiste
* Correzione per l'errore di funzionamento di `az keyvault recover/purge`

### <a name="netappfiles"></a>NetAppFiles

* Aggiornamento di azure-mgmt-netapp alla versione 0.6.0 per l'uso dell'API versione 2019-07-01. Questa nuova versione dell'API include le novità seguenti:

    - Il parametro `--protocol-types` per la creazione del volume accetta ora "NFSv 4.1" e non "NFSv4"
    - La proprietà dei criteri di esportazione del volume è ora denominata 'nfsv41' e non 'nfsv4'
    - Il parametro `--creation-token` del volume è stato rinominato in `--file-path`
    - La data di creazione snapshot è ora indicata semplicemente dalla dicitura 'created'

### <a name="network"></a>Rete

* `az network private-dns link vnet create/update`: Supporto per il collegamento di rete virtuale tra tenant.
* [MODIFICA DI RILIEVO] `az network vnet subnet list`: impostazione di `--resource-group` e `--vnet-name` come obbligatori.
* `az network public-ip prefix create`: Aggiunta del supporto per specificare la versione dell'indirizzo IP (IPv4, IPv6) durante la creazione
* Aggiornamento di azure-mgmt-network alla versione 7.0.0 e di api-version alla versione 2019-09-01
* `az network vrouter`: Aggiunta del supporto per il nuovo servizio router virtuale e peering router virtuale
* `az network express-route gateway connection`: Aggiunta del supporto per `--internet-security`

### <a name="profile"></a>Profilo

* Correzione per l'errore di funzionamento di `az account get-access-token --resource-type ms-graph`
* Rimozione dell'avviso da `az login`

### <a name="rbac"></a>Controllo degli accessi in base al ruolo

* Correzione per l'errore di funzionamento di `az ad app update --id {} --display-name {}`

### <a name="servicefabric"></a>ServiceFabric

* `az sf cluster create`: correzione di un problema mediante la modifica del set di scalabilità di macchine virtuali di calcolo di template.json Linux e Windows per Service Fabric da dischi standard a dischi gestiti

### <a name="sql"></a>SQL

* Aggiunta dei parametri `--compute-model`, `--auto-pause-delay`e `--min-capacity` per supportare le operazioni CRUD per la nuova offerta del database SQL: modello di calcolo serverless.

### <a name="storage"></a>Archiviazione

* `az storage account create/update`: aggiunta del parametro --enable-files-adds e del gruppo di proprietà di Azure Active Directory per supportare l'autenticazione Active Directory Domain Services per File di Azure
* Espansione di `az storage account keys list/renew` per supportare la visualizzazione dell'elenco o la rigenerazione delle chiavi Kerberos dell'account di archiviazione.

## <a name="october-15-2019"></a>15 ottobre 2019

Versione 2.0.75

### <a name="aks"></a>Servizio Azure Kubernetes

* Valore predefinito `--load-balancer-sku` cambiato in `standard` se supportato dalla versione di Kubernetes
* Valore predefinito `--vm-set-type` cambiato in `virtualmachinescalesets` se supportato dalla versione di Kubernetes

### <a name="ams"></a>AMS

* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Nome di `job start` cambiato in `job create`
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Parametro `--ask` di `content-key-policy create` cambiato in modo da usare una stringa esadecimale di 32 caratteri anziché UTF8

### <a name="appservice"></a>AppService

* Aggiunta dei comandi `webapp config access-restriction show|set|add|remove`
* Gestione degli errori migliorata per `webapp up`
* Aggiunta del supporto per lo SKU `Isolated` in `appservice plan update`

### <a name="arm"></a>ARM

* Aggiunta del parametro `--handle-extended-json-format` a `deployment create` per supportare più righe e commenti nel modello JSON

### <a name="compute"></a>Calcolo

* Aggiunta del parametro `--enable-agent` a `vm create`
* `vm create` modificato in modo da usare automaticamente lo SKU per l'indirizzo IP pubblico quando si usano le zone
* `vm create` modificato per creare automaticamente un nome computer valido per una macchina virtuale se non ne viene fornito alcuno
* Aggiunta del parametro `--computer-name-prefix` a `vmss create` per supportare il prefisso del nome computer personalizzato delle macchine virtuali nel set di scalabilità di macchine virtuali
* Aggiunta del parametro `--workspace` a `vm create` per abilitare automaticamente l'area di lavoro Log Analytics
* API delle raccolte aggiornata alla versione 2019-07-01

### <a name="core"></a>Core

* Aggiunta del controllo della sintassi per il parametro `--set` nel comando di aggiornamento generico

### <a name="iot"></a>IoT

* Risolto un problema a causa del quale `iot hub show` restituiva un errore di "risorsa non trovata"

### <a name="monitor"></a>Monitorare

* Aggiunta del supporto per CRUD a `monitor log-analytics workspace`

### <a name="network"></a>Rete

* Aggiunta del supporto per il collegamento virtuale tra tenant a `network private-dns link vnet [create|update]`
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Modifica di `network vnet subnet list` in modo da richiedere i parametri `--resource-group` e `--vnet-name`

### <a name="sql"></a>SQL

* Aggiunta a `sql mi ad-admin` di comandi che supportano l'impostazione di un amministratore di AAD nelle istanze gestite

### <a name="storage"></a>Archiviazione

* Aggiunta del parametro `--preserve-s2s-access-tier` a `storage copy` per mantenere il livello di accesso durante la copia da servizio a servizio
* Aggiunta del parametro `--enable-large-file-share` a `storage account [create|update]` per supportare le condivisioni file di grandi dimensioni per l'account di archiviazione

## <a name="september-24-2019"></a>24 settembre 2019

Versione 2.0.74

### <a name="acr"></a>ACR

* Aggiunta di un parametro `--type` obbligatorio a `acr config retention update`
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Ridenominazione del parametro `--name -n` in `--registry -r ` per il gruppo di comandi `acr config`

### <a name="aks"></a>Servizio Azure Kubernetes

* Aggiunta del parametro `--load-balancer-sku` al comando `aks create`, per consentire la creazione del cluster del servizio Azure Kubernetes con il servizio di bilanciamento del carico software
* Aggiunta dei parametri `--load-balancer-managed-outbound-ip-count`, `--load-balancer-outbound-ips` e `--load-balancer-outbound-ip-prefixes` ai comandi `aks [create|update]`, per consentire l'aggiornamento del profilo di bilanciamento del carico di un cluster del servizio Azure Kubernetes con il servizio di bilanciamento del carico software
* Aggiunta del parametro `--vm-set-type` al comando `aks create` per consentire la specifica dei tipi di macchina virtuale di un cluster del servizio Azure Kubernetes (vmas o vmss)

### <a name="arm"></a>ARM

* Aggiunta del parametro `--handle-extended-json-format` al comando `group deployment create` per supportare più righe e commenti nel modello JSON

### <a name="compute"></a>Calcolo

* Aggiunta del parametro `--terminate-notification-time` ai comandi `vmss [create|update]` per supportare la configurabilità degli eventi pianificati di terminazione
* Aggiunta del parametro `--enable-terminate-notification` al comando `vmss update` per supportare la configurabilità degli eventi pianificati di terminazione
* Aggiunta dei parametri `--priority,` `--eviction-policy,` `--max-billing` ai comandi `[vm|vmss] create`
* Modifica di `disk create` per consentire la specifica delle dimensioni esatte del caricamento del disco
* Aggiunta del supporto per gli snapshot incrementali per dischi gestiti a `snapshot create`

### <a name="cosmos-db"></a>Cosmos DB

* Aggiunta del parametro `--type <key-type>` al comando `cosmosdb keys list` per visualizzare la chiave, le chiavi di sola lettura o le stringhe di connessione
* Aggiunta del comando `cosmosdb keys regenerate`
* [DEPRECATO] Deprecamento dei comandi `cosmosdb list-connection-strings`, `cosmosdb regenerate-key` e `cosmosdb list-read-only-keys`

### <a name="eventgrid"></a>EventGrid

* Correzione del testo della Guida dell'endpoint per fare riferimento al parametro appropriato

### <a name="key-vault"></a>Key Vault

* Correzione di un problema per cui l'accesso con un tenant (`login -t`) poteva impedire l'esecuzione di `keyvault create`

### <a name="monitor"></a>Monitorare

* Correzione del problema per cui il carattere `:` non era consentito nell'argomento `--condition` di `monitor metrics alert create`

### <a name="policy"></a>Policy

* Aggiunta del supporto per l'API di Criteri versione 2019-06-01
* Aggiunta del parametro `--enforcement-mode` al comando `policy assignment create`

### <a name="storage"></a>Archiviazione

* Aggiunta del parametro `--blob-type` al comando `az storage copy`

## <a name="september-10-2019"></a>10 settembre 2019

### <a name="acr"></a>ACR

* Aggiunta del gruppo di comandi `acr config retention` per configurare i criteri di conservazione

### <a name="aks"></a>Servizio Azure Kubernetes

* Aggiunta del supporto per l'integrazione del Registro Azure Container con i comandi seguenti:
  * Aggiunta del parametro `--attach-acr` a `aks [create|update]` per il collegamento di un Registro Azure Container a un cluster del servizio Azure Kubernetes
  * Aggiunta del parametro `--detach-acr` a `aks update` per lo scollegamento di un Registro Azure Container da un cluster del servizio Azure Kubernetes

### <a name="arm"></a>ARM

* Aggiornamento per l'uso della versione 2019-05-10 dell'API

### <a name="batch"></a>Batch

* Aggiunta di nuove impostazioni di configurazione JSON a `--json-file` per `batch pool create`:
  * Aggiunta di `MountConfigurations` per i montaggi di file system (vedere https://docs.microsoft.com/rest/api/batchservice/pool/add#request-body per i dettagli)
  * Aggiunta della proprietà facoltativa `publicIPs` in `NetworkConfiguration` per gli indirizzi IP pubblici nei pool (vedere https://docs.microsoft.com/rest/api/batchservice/pool/add#request-body per i dettagli)
* Aggiunta del supporto per le raccolte di immagini condivise a `--image`
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Modifica del valore predefinito di `--start-task-wait-for-success` in `batch pool create` che è ora `true`
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Modifica del valore predefinito di `Scope` in `AutoUserSpecification` che è ora sempre Pool (era `Task` nei nodi Windows e `Pool` nei nodi Linux)
  * Questo argomento può essere impostato solo da una configurazione JSON con `--json-file`

### <a name="hdinsight"></a>HDInsight

* Versione di disponibilità generale (GA)
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Modifica del parametro `--workernode-count/-c` di `az hdinsight resize` che è ora obbligatorio.

### <a name="key-vault"></a>Key Vault

* Correzione di un problema per cui non è possibile eliminare le subnet dalle regole di rete
* Correzione di un problema per cui non è possibile aggiungere subnet e indirizzi IP duplicati alle regole di rete

### <a name="network"></a>Rete

* Aggiunta del parametro `--interval` a `network watcher flow-log` per impostare il valore dell'intervallo di analisi del traffico
* Aggiunta di `network application-gateway identity` per gestire l'identità del gateway
* Aggiunta del supporto per impostare l'ID di Key Vault su `network application-gateway ssl-cert`
* Aggiunta di `network express-route peering peer-connection [show|list]`

### <a name="policy"></a>Policy

* Aggiornamento per l'uso della versione 2019-01-01 dell'API

## <a name="august-27-2019"></a>27 agosto 2019

Versione 2.0.72

### <a name="acr"></a>ACR

* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Rimozione del supporto per lo SKU `classic`

### <a name="api-management"></a>Gestione API

* [ANTEPRIMA] Aggiunta del gruppo di comandi `apim`

### <a name="appservice"></a>AppService

* Correzione del problema del comando `webapp webjob continuous start` quando si specifica uno slot
* Modifica di `webapp up` per rilevare la cartella `env` e rimuoverla dal file usato per la distribuzione

### <a name="keyvault"></a>Key Vault

* Correzione di un bug in `keyvault secret set` per cui l'argomento `--expires` viene ignorato

### <a name="network"></a>Rete

* Aggiunta del supporto per gli indirizzi IPv6 agli argomenti `--private-ip-address-version`
* Aggiunta di nuovi comandi `network private-endpoint [create|update|list-types]` per la gestione degli endpoint privati
* Aggiunta del gruppo di comandi `network private-link-service`
* Aggiunta degli argomenti `--private-endpoint-network-policies` e `--private-link-service-network-policies` a `network vnet subnet update`

### <a name="rbac"></a>Controllo degli accessi in base al ruolo

* Correzione del problema di `ad app update --homepage` per cui non è possibile aggiornare la home page

### <a name="servicefabric"></a>ServiceFabric

* Aggiunta del supporto per i nomi di Key Vault con combinazioni di maiuscole e minuscole
* Correzione del problema relativo all'uso dei certificati in Key Vault
* Correzione del problema relativo all'uso dei file di certificato PFX
* Correzione del problema di `sf cluster certificate add` quando non viene specificato il gruppo di risorse di Key Vault
* Correzione del problema relativo al mancato funzionamento di `sf cluster set`

### <a name="signalr"></a>SignalR

* Aggiunta di nuovi comandi:
  * `signalr cors`: Gestire CORS SignalR
  * `signalr restart`: Riavviare un servizio SignalR
  * `signalr update`: Aggiornare un servizio SignalR
* Aggiunta dell'argomento `--service-mode` a `signalr create`

### <a name="storage"></a>Archiviazione

* Aggiunta del comando `storage account revoke-delegation-keys`

## <a name="august-13-2019"></a>13 agosto 2019

Versione 2.0.71

### <a name="appservice"></a>AppService

* Correzione del problema per cui i comandi `webapp webjob continuous` non riescono per gli slot

### <a name="botservice"></a>BotService

* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Rimozione del supporto per la creazione di bot SDK v3

### <a name="cognitiveservices"></a>CognitiveServices

* Aggiunta dei comandi `cognitiveservices account network-rule`

### <a name="cosmos-db"></a>Cosmos DB

* Rimozione dell'avviso durante l'aggiornamento di più percorsi di scrittura
* Aggiunta di comandi CRUD per le risorse di CosmosDB SQL, MongoDB, Cassandra, Gremlin e Tabella e relative unità elaborate

### <a name="hdinsight"></a>HDInsight

Questa versione contiene un numero elevato di modifiche di rilievo.

* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Ridenominazione dei parametri per `hdinsight create`:
  * Rinominato `--storage-default-container` in `--storage-container`
  * Rinominato `--storage-default-filesystem` in `--storage-filesystem`
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Modifica dell'argomento `--name` di `application create` per rappresentare il nome dell'applicazione invece del nome del cluster
* Aggiunta dell'argomento `--cluster-name` a `application create` per sostituire la funzionalità `--name` precedente
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Ridenominazione dei parametri per `application create`:
  * Rinominato `--application-type` in `--type`
  * Rinominato `--marketplace-identifier` in `--marketplace-id`
  * Rinominato `--https-endpoint-access-mode` in `--access-mode`
  * Ridenominazione di `--https-endpoint-destination-port` in `--destination-port`
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Rimozione dei parametri per `application create`:
  * `--https-endpoint-location`
  * `--https-endpoint-public-port`
  * `--ssh-endpoint-destination-port`
  * `--ssh-endpoint-location`
  * `--ssh-endpoint-public-port`
* [MODIFICA DI RILIEVO] Ridenominazione di `--target-instance-count` in `--workernode-count` per `hdinsight resize`
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Modifica di tutti i comandi del gruppo `hdinsight script-action` per usare il parametro `--name` come nome dell'azione di script.
* Aggiunta dell'argomento `--cluster-name` a tutti i comandi `hdinsight script-action` per sostituire la funzionalità `--name` precedente
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Ridenominazione di `--script-execution-id` in `--execution-id` per tutti i comandi `hdinsight script-action`
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Ridenominazione di `hdinsight script-action show` in `hdinsight script-action show-execution-details`
* [MODIFICA DI RILIEVO] Modifica dei parametri in `hdinsight script-action execute --roles` in modo che siano separati da spazi anziché da virgole
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Rimozione del parametro `--persisted` di `hdinsight script-action list`
* Modifica del `hdinsight create --cluster-configurations` per accettare un percorso di un file JSON locale o di una stringa JSON
* Aggiunta del comando `hdinsight script-action list-execution-history`
* Modifica di `hdinsight monitor enable --workspace` per accettare un ID o un nome area di lavoro di Log Analytics
* Aggiunta dell'argomento `hdinsight monitor enable --primary-key`, necessario se come parametro viene specificato un ID area di lavoro
* Aggiunta di altri esempi e aggiornamento delle descrizioni per i messaggi della Guida

### <a name="interactive"></a>Interattività

* Correzione di un errore di caricamento

### <a name="kubernetes"></a>Kubernetes

* Modifica per usare `https` se la porta del contenitore del dashboard usa `https`

### <a name="network"></a>Rete

* Aggiunta dell'argomento `--yes` a `network dns record-set cname delete`

### <a name="profile"></a>Profilo

* Aggiunta dell'argomento `--resource-type` a `account get-access-token` per ottenere i token di accesso alle risorse

### <a name="servicefabric"></a>ServiceFabric

* Aggiunta di tutte le versioni del sistema operativo supportate per la creazione del cluster di ServiceFabric
* Correzione del bug di convalida del certificato primario

### <a name="storage"></a>Archiviazione

* Aggiunta del comando `storage copy`

## <a name="july-30-2019"></a>30 luglio 2019

Versione 2.0.70

### <a name="acr"></a>ACR

* Correzione del problema 9952 (regressione nel comando `acr pack build`)
* Rimozione del nome dell'immagine del generatore predefinito in `acr pack build`

### <a name="appservice"></a>Servizio app

* Modifica di `webapp config ssl` per consentire la visualizzazione di un messaggio in caso di risorsa non trovata
* Correzione del problema per cui `functionapp create` non accetta il tipo di account di archiviazione `Standard_RAGRS`
* Correzione di un problema che impediva la corretta esecuzione di `webapp up` con versioni meno recenti di Python

### <a name="network"></a>Rete

* Rimozione del parametro `--ids` da `network nic ip-config add` (correzione per il problema 9861)
* Correzione per il problema 9604. Aggiunta del parametro `--root-certs` a `network application-gateway http-settings [create|update]` per supportare i certificati radice attendibili associati all'utente.
* Correzione dell'argomento `--subscription` per `network dns record-set ns create` (problema 9965)

### <a name="rbac"></a>Controllo degli accessi in base al ruolo

* Aggiunta del comando `user update`
* [DEPRECATO] Argomento `--upn-or-object-id` deprecato da comandi correlati all'utente
    * In sostituzione usare l'argomento `--id`
* Aggiunta dell'argomento `--id` ai comandi correlati all'utente

### <a name="sql"></a>SQL

* Aggiunta dei comandi di gestione per chiavi di istanza gestita e TDE Protector

### <a name="storage"></a>Archiviazione

* Aggiunta del comando `storage remove`
* Correzione di un problema relativo a `storage blob update`

### <a name="vm"></a>VM

* Modifica di `list-skus` per consentire l'uso della versione API più recente per l'output dei dettagli della zona
* Modifica dell'impostazione predefinita di `--single-placement-group` in `false` per `vmss create`
* Aggiunta della possibilità di selezionare SKU di archiviazione con ridondanza della zona per `[snapshot|disk] create`
* Aggiunta del nuovo gruppo di comandi `vm host` per il supporto di host dedicati
* Aggiunta dei parametri `--host` e `--host-group` in `vm create` per l'impostazione dell'host dedicato delle macchine virtuali

## <a name="july-16-2019"></a>16 luglio 2019

Versione 2.0.69

### <a name="appservice"></a>Servizio app

* Modifica dei comandi di `webapp identity` in modo che venga restituito un messaggio di errore corretto se il valore di ResourceGroupName o il nome dell'app non è valido
* Correzione di `webapp list` in modo che venga restituito il valore corretto di numberOfSites se non è stato specificato alcun elemento ResourceGroup
* Correzione degli effetti collaterali di `appservice plan create` e `webapp create`

### <a name="core"></a>Core

* Correzione dell'errore che causava la visualizzazione di `--subscription` anche se non era applicabile

### <a name="batch"></a>Batch

* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Sostituzione di `batch pool node-agent-skus list` con `batch pool supported-images list`
* Aggiunta del supporto per le regole di sicurezza che bloccano l'accesso di rete a un pool in base alla porta di origine del traffico quando si usa l'opzione `--json-file` di `batch pool create network`
* Aggiunta del supporto per l'esecuzione dell'attività nella directory di lavoro del contenitore o in quella dell'attività Batch quando si usa l'opzione `--json-file` di `batch task create`
* Correzione dell'errore nell'opzione `--application-package-references` di `batch pool create` che funzionava solo con le impostazioni predefinite

### <a name="eventhubs"></a>Hub eventi

* Aggiunta della convalida per il parametro `--rights` dei comandi di `authorizationrule`

### <a name="rdbms"></a>RDBMS

* Aggiunta del parametro facoltativo per specificare lo SKU di replica per il comando di creazione replica
* Correzione dell'errore che impediva il superamento del test di integrazione continua durante la creazione della replica MySQL

### <a name="relay"></a>Relay

* Correzione dell'errore che impediva la creazione della connessione ibrida quando l'autorizzazione dei client era disabilitata ([errore 8775](https://github.com/azure/azure-cli/issues/8775))
* Aggiunta del parametro `--requires-transport-security` a `relay wcfrelay create`

### <a name="servicebus"></a>Bus di servizio

* Aggiunta della convalida per il parametro `--rights` dei comandi di `authorizationrule`

### <a name="storage"></a>Archiviazione

* Abilitazione di AADDS di File per l'aggiornamento dell'account di archiviazione
* Risoluzione del problema `storage blob service-properties update --set`

## <a name="july-2-2019"></a>2 luglio 2019

Versione 2.0.68

### <a name="core"></a>Core

* I moduli dei comandi ora sono consolidati in un unico file ridistribuibile Python. In questo modo viene deprecato l'uso diretto di numerosi pacchetti `azure-cli-` su PyPI.
  Questa novità implica una riduzione delle dimensioni dell'installazione e interessa solo gli utenti che hanno eseguito l'installazione direttamente tramite `pip`.

### <a name="acr"></a>ACR

* Aggiunta del supporto per i trigger del timer per l'attività

### <a name="appservice"></a>Servizio app

* Modifica di `functionapp create` per abilitare le informazioni dettagliate dell'applicazione per impostazione predefinita
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Rimozione del comando deprecato `functionapp devops-build`.
  *  In alternativa, usare il nuovo comando `az functionapp devops-pipeline`
* Aggiunta del supporto per il piano dell'app per le funzioni a consumo per Linux per `functionapp deployment config-zip`

### <a name="cosmos-db"></a>Cosmos DB

* Aggiunta del supporto per la disabilitazione di TTL

### <a name="dls"></a>DLS

* Aggiornamento di ADLS versione (0.0.45)

### <a name="feedback"></a>Commenti e suggerimenti

* Quando viene segnalato un comando dell'estensione non riuscito, `az feedback` ora prova ad aprire nel browser l'URL del progetto/repository dell'estensione dall'indice

### <a name="hdinsight"></a>HDInsight

* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Modifica del nome del gruppo di comandi `oms` in `monitor`
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Impostazione di `--http-password/-p` come parametro obbligatorio 
* Aggiunta degli strumenti di completamento per i parametri `--cluster-admin-account` e `cluster-users-group-dns` 
* Modifica del parametro `cluster-users-group-dns` in modo che sia obbligatorio in presenza di `—esp`
* Aggiunta di un timeout per tutti gli strumenti di completamento automatico argomenti esistenti
* Aggiunta di un timeout per la trasformazione del nome di risorsa nell'ID risorsa
* Modifica degli strumenti di completamento automatico per la selezione di risorse da qualsiasi gruppo di risorse. Può essere un gruppo di risorse diverso rispetto a quello specificato con `-g`
* Aggiunta del supporto per i parametri `--sub-domain-suffix` e `--disable_gateway_auth` nel comando `hdinsight application create`

### <a name="managed-services"></a>Servizi gestiti

* Introduzione del modulo per i comandi dei servizi gestito in anteprima

### <a name="profile"></a>Profilo
* Eliminazione dell'argomento `--subscription` per il comando logout

### <a name="rbac"></a>Controllo degli accessi in base al ruolo

* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Rimozione dell'argomento `--password` per `create-for-rbac`
* Aggiunta del parametro `--assignee-principal-type` al comando `create` per evitare errori intermittenti causati dalla latenza di replica dei server Graph di Azure AD
* Correzione del problema che causava un arresto anomalo del sistema in `ad signed-in-user` quando venivano elencati gli oggetti di proprietà
* Correzione del problema che impediva a `ad sp` di trovare l'applicazione corretta da un'entità servizio

### <a name="rdbms"></a>RDBMS

* Aggiunta del supporto per la replica per MariaDB

### <a name="sql"></a>SQL

* Documentazione dei valori consentiti per `sql db create --sample-name`

### <a name="storage"></a>Archiviazione

* Aggiunta del supporto del token di firma di accesso condiviso di delega utente con `--as-user` per `storage blob generate-sas` 
* Aggiunta del supporto del token di firma di accesso condiviso di delega utente con `--as-user` per `storage container generate-sas` 

### <a name="vm"></a>VM

* Correzione del bug per cui `vmss create` restituisce un messaggio di errore quando viene eseguito con `--no-wait`
* Rimozione della convalida lato client per `vmss create --single-placement-group`. L'errore non si verifica se `--single-placement-group` è impostato su `true` e il valore di `--instance-count` è maggiore di 100 oppure si specificano zone di disponibilità, ma questa convalida viene lasciata al servizio di calcolo
* Correzione del bug che causa un errore di `[vm|vmss] extension image list` quando viene usato con `--latest`


## <a name="june-18-2019"></a>18 giugno 2019

Versione 2.0.67

### <a name="core"></a>Core

Con questa versione viene introdotto un nuovo tag [Preview] per indicare più chiaramente ai clienti quando un gruppo di comandi, un comando o un argomento è in stato di anteprima. In precedenza, questo stato veniva richiamato nel testo della Guida oppure indicato in modo implicito dal numero di versione del modulo del comando.
In futuro i numeri di versione per i singoli pacchetti verranno rimossi dall'interfaccia della riga di comando. Se un comando è disponibile in anteprima, lo sono anche tutti i relativi argomenti. Se un gruppo di comandi viene etichettato come in fase di anteprima, saranno considerati in anteprima anche tutti i comandi e argomenti.

In seguito a questa modifica, diversi gruppi di comandi potrebbero "improvvisamente" risultare in stato di anteprima con questa versione. In realtà la maggior parte dei pacchetti si trovava in uno stato di anteprima, ma viene ora considerata disponibile a livello generale con questa versione

### <a name="acr"></a>ACR
* Aggiunta del comando 'acr check-health'
* Miglioramento della gestione degli errori per i token AAD e per il recupero di comandi esterni

### <a name="acs"></a>ACS
* I comandi ACS deprecati nono sono più inclusi nella visualizzazione della Guida

### <a name="ams"></a>AMS
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] È stata apportata una modifica per consentire la restituzione delle stringhe dell'ora in formato ISO 8601 per archive-window-length e key-frame-interval-duration

### <a name="appservice"></a>AppService
* Aggiunta del routing basato sul percorso per `webapp deleted list` e `webapp deleted restore`
* Correzione del problema per cui l'URL di destinazione registrato per l'app Web ("È possibile avviare l'app all'indirizzo...") non era selezionabile in Azure Cloud Shell
* Correzione di un problema per cui risultava impossibile creare app con alcuni SKU e veniva restituito un errore di AlwaysOn
* Aggiunta della convalida preliminare per `[appservice|webapp] create`
* Correzione di `[webapp|functionapp] traffic-routing` per consentire l'uso dell'elemento actionHostName corretto
* Aggiunta del supporto slot per i comandi `functionapp`

### <a name="batch"></a>Batch
* Correzione della regressione dell'autenticazione AAD causata dalla segnalazione eccessiva di errori per Autenticazione con chiave condivisa

### <a name="batchai"></a>Batch per intelligenza artificiale
* I comandi di Batch per intelligenza artificiale sono ora deprecati e risultano nascosti

### <a name="botservice"></a>BotService
* Aggiunta dei messaggi di avviso "supporto sospeso"/"modalità manutenzione" per i comandi che supportano l'SDK v3

### <a name="cosmosdb"></a>Cosmos DB
* [DEPRECATO] Il comando `cosmosdb list-keys` è stato deprecato
* Aggiunta del comando `cosmosdb keys list` in sostituzione di `cosmosdb list-keys`
* `cosmsodb create/update`: Aggiunta del nuovo formato per --location per consentire l'impostazione della proprietà "isZoneRedundant". Formato precedente deprecato

### <a name="eventgrid"></a>EventGrid
* Aggiunta dei comandi `eventgrid domain` per le operazioni CRUD su domini
* Aggiunta dei comandi `eventgrid domain topic` per le operazioni CRUD su argomenti di dominio
* Aggiunta dell'argomento `--odata-query` a `eventgrid [topic|event-subscription] list` per filtrare i risultati con la sintassi OData
* `event-subscription create/update`: Aggiunta di servicebusqueue come nuovo valore del parametro `--endpoint-type`
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Rimozione del supporto per `--included-event-types All` con `eventgrid event-subscription [create|update]`

### <a name="hdinsight"></a>HDInsight
* Aggiunta del supporto per il parametro `--ssh-public-key` nel comando `hdinsight create`

### <a name="iot"></a>IoT
* Aggiunta del supporto per la rigenerazione delle chiavi dei criteri di autorizzazione
* Aggiunta dell'SDK e del supporto per il servizio DigitalTwin Repository Provisioning Service

### <a name="network"></a>Rete
* Aggiunta del supporto di zona per il gateway NAT
* Aggiunta del comando `network list-service-tags`
* Correzione del problema relativo a `dns zone import` che impediva agli utenti di importare record A con caratteri jolly
* Correzione del problema relativo a `watcher flow-log configure` che impediva l'abilitazione della registrazione del flusso in alcune aree

### <a name="resource"></a>Risorsa
* Aggiunta del comando `az rest` per effettuare chiamate REST
* Correzione dell'errore rilevato durante l'uso di `policy assignment list` con un gruppo di risorse o il livello di sottoscrizione `--scope`

### <a name="servicebus"></a>ServiceBus
* Correzione del problema di `servicebus topic create --max-size` [#9319](https://github.com/azure/azure-cli/issues/9319)

### <a name="sql"></a>SQL
* Modifica di `--location` in modo che sia facoltativo per `sql [server|mi] create`; se non specificato, viene usato il percorso del gruppo di risorse
* Correzione dell'errore "L'oggetto 'NoneType' non è iterabile" per `sql db list-editions --available`

### <a name="sqlvm"></a>SQLVm
* [MODIFICA DI RILIEVO] Modifica di `sql vm create` in modo da richiedere il parametro `--license-type`
* Modifica per consentire l'impostazione dello SKU dell'immagine SQL durante la creazione o l'aggiornamento di una macchina virtuale SQL

### <a name="storage"></a>Archiviazione
* Correzione del problema relativo alla chiave dell'account mancante per `storage container generate-sas`
* Correzione del problema relativo a `storage blob sync` in Linux

### <a name="vm"></a>VM
* [ANTEPRIMA] Aggiunta dei comandi `vm image template` per la creazione di immagini di macchina virtuale

## <a name="june-4-2019"></a>4 giugno 2019

Versione 2.0.66

### <a name="core"></a>Core
* Corretto un bug per cui i comandi non riescono se si usa `--output yaml` con `--query`

### <a name="acr"></a>ACR
* Aggiunta del gruppo di comandi 'acr pack' per la creazione rapida di attività di compilazione tramite Buildpacks.

### <a name="acs"></a>ACS
* Consentita l'abilitazione/disabilitazione del componente aggiuntivo kube-dashboard del servizio Azure Kubernetes
* Stampa di un messaggio descrittivo quando la sottoscrizione non è inclusa in un elenco elementi consentiti per l'uso di Azure Red Hat OpenShift

### <a name="batch"></a>Batch
* Miglioramento della gestione degli errori quando non è stato eseguito l'accesso a un account \[[#9165](https://github.com/Azure/azure-cli/issues/9165)\]\[[#8978](https://github.com/Azure/azure-cli/issues/8978)\]

### <a name="iot"></a>IoT
* Aggiunta del supporto per il failover manuale

### <a name="network"></a>Rete
* Aggiunta dei comandi `network application-gateway waf-policy` per supportare regole personalizzate di WAF.
* Aggiunta degli argomenti `--waf-policy` e `--max-capacity` a `network application-gateway [create|update]` 

### <a name="resource"></a>Risorsa
* Miglioramento del messaggio di errore di `deployment create` quando non sono disponibili TTY

### <a name="role"></a>Ruolo
* Aggiornamento del testo della Guida.

### <a name="compute"></a>Calcolo
* Aggiunta del supporto di `vm create` per creare VM da un'immagine gestita con LUN di dischi dati che non iniziano da 0 o che ignorano numeri

## <a name="may-21-2019"></a>21 maggio 2019

Versione 2.0.65

### <a name="core"></a>Core
* Aggiunta di un feedback più efficace per gli errori di autenticazione
* Correzione del problema per cui l'interfaccia della riga di comando carica estensioni non compatibili con la relativa versione di base
* Correzione di un problema di avvio quando `clouds.config` è danneggiato

### <a name="acr"></a>ACR
* Aggiunta del supporto per le identità gestite nelle attività

### <a name="acs"></a>ACS
* Correzione del comando `openshift create` quando viene usato con il client AAD

### <a name="appservice"></a>AppService
* [DEPRECATO] Comando `functionapp devops-build` deprecato - verrà rimosso nella nuova versione
* Modifica di `functionapp devops-pipeline` per recuperare il log di compilazione da Azure DevOps in modalità dettagliata
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Rimozione del flag `--use_local_settings` dal comando `functionapp devops-pipeline` (è un'istruzione no-op)
* Modifica di `webapp up` per restituire output JSON se `--logs` non viene usato
* Aggiunta del supporto per la scrittura delle risorse predefinite nel file di configurazione locale per `webapp up`
* Aggiunta in `webapp up` del supporto per la ridistribuzione di un'app senza usare l'argomento `--location`
* Correzione di un problema per cui l'uso di Free come valore dello SKU per la creazione di ASP con lo SKU gratuito di Linux non funziona

### <a name="botservice"></a>BotService
* Modifica per consentire l'uso di qualsiasi combinazione di maiuscole/minuscole per i parametri `--lang` per i comandi
* Aggiornamento della descrizione per il modulo dei comandi

### <a name="consumption"></a>Consumo
* Aggiunta del parametro obbligatorio mancante durante l'esecuzione di `consumption usage list --billing-period-name`

### <a name="iot"></a>IoT
* Aggiunta del supporto per l'elenco di tutte le chiavi

### <a name="network"></a>Rete
* [MODIFICA CHE CAUSA UN'INTERRUZIONE]: Removed `network interface-endpoints` command group - use `network private-endpoints` 
* Aggiunta dell'argomento `--nat-gateway` a `network vnet subnet [create|update]` per il collegamento a un gateway NAT
* Correzione di un problema di `dns zone import` per cui i nomi dei record non corrispondono a un tipo di record

### <a name="rdbms"></a>RDBMS
* Aggiunta del supporto di postgres e mysql per la replica geografica

### <a name="rbac"></a>Controllo degli accessi in base al ruolo
* Aggiunta del supporto per l'ambito dei gruppi di gestione in `role assignment`

### <a name="storage"></a>Archiviazione
* `storage blob sync`: aggiunta del comando di sincronizzazione per il BLOB di archiviazione

### <a name="compute"></a>Calcolo
* Aggiunta di `--computer-name` a `vm create` per l'impostazione del nome computer di una VM
* Ridenominazione di `--ssh-key-value` in `--ssh-key-values` per `[vm|vmss] create`, che ora accetta più percorsi o valori di chiave pubblica SSH
  * __Nota__: questa modifica **non** causa un'interruzione, `--ssh-key-value` verrà analizzato correttamente perché corrisponde solo a `--ssh-key-values`
* L'argomento `--type` di `ppg create` è ora facoltativo

## <a name="may-6-2019"></a>6 maggio 2019

Versione 2.0.64

### <a name="acs"></a>ACS
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Rimozione del flag `--fqdn` dai comandi `openshift`
* Modifica per l'uso della versione API in disponibilità generale di Azure Red Hat Openshift
* Aggiunta del flag `customer-admin-group-id` a `openshift create`
* [Disponibilità generale] Rimozione di `(PREVIEW)` dall'opzione `--network-policy` di `aks create`

### <a name="appservice"></a>Servizio app
* [DEPRECATO] Comando `functionapp devops-build` deprecato
  * Ridenominazione in `functionapp devops-pipeline`
* Risoluzione del problema nell'ottenere il nome utente corretto per cloudshell che causa l'esito negativo di `webapp up`
* Aggiornamento della documentazione di `appservice plan --sku` aggiornata per riflettere gli oggetti appserviceplans supportati
* Aggiunta di argomenti facoltativi per il piano e il gruppo di risorse a `webapp up`
* Aggiunta del supporto a `webapp ssh` per rispettare la variabile di ambiente `AZURE_CLI_DISABLE_CONNECTION_VERIFICATION`
* Aggiunta del supporto di `appserviceplan create` per lo SKU gratuito Linux
* Modifica di `webapp up` per avere una sospensione di 30 secondi dopo l'impostazione app di `SCM_DO_BUILD_DURING_DEPLOYMENT=true` per gestire l'avvio a freddo kudu
* Aggiunta del supporto per il runtime `powershell` a `functionapp create` in Windows
* Aggiunta del comando `create-remote-connection`

### <a name="batch"></a>Batch
* Correzione di un bug nel validator per le opzioni `--application-package-references`

### <a name="botservice"></a>Botservice
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Modifica di `bot create -v v4 -k webapp` per creare un bot vuoto in App Web per impostazione predefinita, ossia con nessun bot distribuito al servizio app
* Aggiunta del flag `--echo` a `bot create` per usare il comportamento precedente con `-v v4`
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Modifica del valore predefinito di `--version` in `v4`
  * __NOTA:__ `bot prepare-publish` continua a usare il valore predefinito precedente
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Modifica di `--lang` per cui `Csharp` non è più l'impostazione predefinita. Se `--lang` è richiesto e non viene specificato, il comando restituisce un errore
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Gli argomenti `--appid` e `--password` per `bot create` sono ora obbligatori e possono essere creati tramite `ad app create`
* Aggiunta della convalida `--appid` e `--password`
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Modifica di `bot create -v v4` per non creare o usare un account di archiviazione o Application Insights
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Modifica di `bot create -v v3` per richiedere un'area in cui Application Insights è disponibile
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Modifica di `bot update` per influire solo su specifiche proprietà di un bot
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Modifica dei flag `--lang` per accettare `Javascript` invece di `Node`
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Rimozione di `Node` come valore `--lang` consentito
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Modifica di `bot create -v v4 -k webapp` per cui `SCM_DO_BUILD_DURING_DEPLOYMENT` non è più impostato su True. Tutte le distribuzioni tramite Kudu seguiranno il rispettivo comportamento predefinito
* Modifica di `bot download` per i bot senza file `.bot` per creare il file di configurazione specifico della lingua con i valori delle impostazioni dell'applicazione per il bot
* Aggiunta del supporto di `Typescript` a `bot prepare-deploy`
* Aggiunta del messaggio di avviso in `bot prepare-deploy` per i bot `Javascript` e `Typescript` nei casi in cui `--code-dir` non contenga `package.json`
* Modifica di `bot prepare-deploy` per restituire `true` in caso di esito positivo
* Aggiunta della registrazione dettagliata in `bot prepare-deploy`
* Aggiunta di altre aree di Application Insights disponibili in `az bot create -v v3`

### <a name="configure"></a>Configurare
* Aggiunta del supporto per configurazioni dei valori predefiniti degli argomenti basati su cartella

### <a name="eventhubs"></a>Hub eventi
* Aggiunta dei comandi `namespace network-rule`
* Aggiunta dell'argomento `--default-action` per le regole di rete in `namespace [create|update]`

### <a name="network"></a>Rete
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Sostituzione dell'argomento `--cache` con `--defer` per `vnet [create|update]` 

### <a name="policy-insights"></a>Policy Insights
* Aggiunta del supporto per `--expand PolicyEvaluationDetails` per eseguire una query sulla risorsa e richiedere i dettagli di valutazione dei criteri

### <a name="role"></a>Ruolo
* [DEPRECATO] Modifica di `create-for-rbac` per nascondere l'argomento '--password'. Il supporto verrà rimosso a maggio 2019.

### <a name="service-bus"></a>Bus di servizio
* Aggiunta dei comandi `namespace network-rule`
* Aggiunta dell'argomento `--default-action` per le regole di rete in `namespace [create|update]`
* Correzione di `topic [create|update]` per consentire il supporto di `--max-size` per i valori 10, 20, 40 e 80 GB con SKU premium

### <a name="sql"></a>SQL
* Aggiunta dei comandi `sql virtual-cluster [list|show|delete]`

### <a name="vm"></a>VM
* Aggiunta di `--protect-from-scale-in` e `--protect-from-scale-set-actions` a `vmss update` per abilitare gli aggiornamenti ai criteri di protezione per le istanze di VM dei set di scalabilità di macchine virtuali
* Aggiunta di `--instance-id` a `vmss update` per abilitare l'aggiornamento generico di istanze di VM dei set di scalabilità di macchine virtuali
* Aggiunta di `--instance-id` a `vmss wait`
* Aggiunta del nuovo gruppo di comandi `ppg` per la gestione dei gruppi di selezione host di prossimità
* Aggiunta di `--ppg` a `[vm|vmss] create` e `vm availability-set create` per la gestione dei gruppi di selezione host di prossimità
* Aggiunta del parametro `--hyper-v-generation` a `image create`

## <a name="april-23-2019"></a>23 aprile 2019

Versione 2.0.63

### <a name="acs"></a>ACS
* Modifica di `aks get-credentials` per richiedere la sovrascrittura di valori duplicati
* Rimozione di `(PREVIEW)` dai comandi di Dev Spaces "aks use-dev-spaces" e "aks remove-dev-spaces"

### <a name="ams"></a>AMS
* Correzione del bug con l'aggiornamento dei filtri asset e account

### <a name="appservice"></a>AppService
* Aggiunta del supporto per l'ambiente del servizio app e il timeout a `webapp ssh`
* Aggiunta del supporto per la creazione di CI CD in una pipeline di Azure DevOps da un repository Github alle app per le funzioni
* Aggiunta dell'argomento `--github-pat` a `functionapp devops-build create` per accettare un token di accesso personale Github
* Aggiunta dell'argomento `--github-repository` a `functionapp devops-build create` per accettare il repository Github che contiene un codice sorgente di functionapp
* Risoluzione del problema in cui `az webapp up --logs` non riesce restituendo un errore e aggiorna la versione predefinita di .NETCORE alla versione 2.1
* Rimozione delle impostazioni di functionapp non necessarie durante la creazione di un'app per le funzioni con piano a consumo
* Modifica di `webapp up` in modo che la stringa asp predefinita aggiunge ora un numero alla fine per creare un nuovo piano di servizio app in base alle opzioni dello SKU
* Aggiunta di `-b` come opzione a `webapp up` per avviare l'app nel browser
* Modifica di `webapp deployment source config zip` per gestire la variabile di ambiente `AZURE_CLI_DISABLE_CONNECTION_VERIFICATION`

### <a name="deployment-manager"></a>Deployment Manager
* [ANTEPRIMA] Creazione e gestione degli artefatti che supportano le implementazioni

### <a name="lab"></a>Lab
* Correzione del bug che causa un'uscita anticipata

### <a name="network"></a>Rete
* Aggiunta della delega automatica del server dei nomi a `dns zone create` nell'elemento padre durante la creazione della zona figlio

### <a name="resource"></a>Risorsa
* [DEPRECATO] Argomenti `--link-id`, `--target-id` e `--filter-string` di `resource link` deprecati
  * Usare gli argomenti `--link`, `--target` e `--filter`
* Risoluzione del problema relativo al mancato funzionamento dei comandi `resource link [create|update]`
* Risoluzione di un problema per cui l'eliminazione con un ID risorsa può determinare l'arresto anomalo in caso di errore

### <a name="sql"></a>SQL
* Aggiunta del supporto per il fuso orario personalizzato nelle istanze gestite
* Modifica per consentire l'uso del nome del pool elastico con `sql db update`
* Aggiunta del supporto di `--no-wait` a `sql server [create|update]`
* Aggiunta del comando `sql server wait`

### <a name="storage"></a>Archiviazione
* Risoluzione del problema relativo ai token di firma di accesso condiviso con doppia codifica in `storage blob generate-sas`

### <a name="vm"></a>VM
* Aggiunta del flag `--skip-shutdown` a `vm|vmss stop` per spegnere le macchine virtuali senza arresto
* Aggiunta dell'argomento `--storage-account-type` a `sig image-version create` per impostare il tipo di account del profilo di pubblicazione
* Aggiunta dell'argomento `--target-regions` a `sig image-version create` per consentire l'impostazione di tipi di account di archiviazione specifici dell'area

## <a name="april-9-2019"></a>9 aprile 2019

### <a name="core"></a>Core
* Risoluzione di un problema per cui alcune estensioni che visualizzano `Unknown` per la versione e non possono essere aggiornate

### <a name="acr"></a>ACR
* Aggiunta del supporto per l'esecuzione di un'immagine senza contesto

### <a name="ams"></a>AMS
* [DEPRECATO]: Deprecated the `--bitrate` parameter of `account-filter` and `asset-filter`
* [MODIFICA CHE CAUSA UN'INTERRUZIONE]: Renamed the `--bitrate` parameter to `--first-quality`
* Aggiunta del supporto di nuovi parametri di crittografia in `ams streaming-policy create`
* Aggiunta del nuovo parametro `--filters` a `ams streaming-locator create`

### <a name="appservice"></a>AppService
* Aggiunta del supporto di `--logs` a `webapp up`
* Correzione dei problemi di generazione di `azure-pipelines.yml` del comando `functionapp devops-build create`
* Miglioramento della gestione di errori e indicatori di `unctionapp devops-build create`
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Rimozione del flag `--local-git` per il comando `devops-build`; il rilevamento e la gestione dell'archivio Git locale sono obbligatori per la creazione di pipeline di Azure DevOps
* Aggiunta del supporto per la creazione di piani per le funzioni Linux
* Aggiunta della possibilità di cambiare un piano sottostante un'app per le funzioni tramite `functionapp update --plan`
* Aggiunta del supporto per le impostazioni di scalabilità orizzontale del piano Premium di Funzioni di Azure

### <a name="cdn"></a>RETE CDN
* Aggiunta del supporto per `Microsoft_Standard` e `Standard_ChinaCdn`

### <a name="feedback"></a>Commenti e suggerimenti
* Modifica di `feedback` per visualizzare i metadati dei comandi eseguiti di recente
* Modifica di `feedback` per chiedere all'utente di assistere nel processo di creazione di problemi aprendo un browser e usando un modello di invio
* Modifica di `feedback` per stampare il corpo di un problema quando viene eseguito con '--verbose'

### <a name="monitor"></a>Monitorare
* Correzione del problema per cui il valore "count" non è consentito con `metrics alert [create|update]` 

### <a name="network"></a>Rete
* Correzione del formato tabella che non viene visualizzato con `vnet-gateway list-bgp-peer-status`
* Aggiunta dei comandi `list-request-headers` e `list-response-headers` a `application-gateway rewrite-rule`
* Aggiunta del comando `list-server-variables` a `application-gateway rewrite-rule condition`
* Correzione di un problema per cui l'aggiornamento dello stato di un collegamento su una porta ExpressRoute genera un'eccezione di attributo sconosciuto `express-route port update`

### <a name="privatedns"></a>PrivateDNS
* Aggiunta di `network private-dns` per le zone del DNS privato

### <a name="resource"></a>Risorsa
* Correzione del problema di `deployment create` e `group deployment create` per cui un file di parametri con un set di parametri vuoto non funziona

### <a name="role"></a>Ruolo
* Correzione di `create-for-rbac` per gestire correttamente `--years`
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Modifica di `role assignment delete` per la richiesta di conferma quando vengono eliminate tutte le assegnazioni della sottoscrizione senza condizioni

### <a name="sql"></a>SQL
* Aggiornamento di `sql mi [create|update]` con le proprietà proxyOverride e publicDataEndpointEnabled

### <a name="storage"></a>Archiviazione
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Rimozione del risultato di `storage blob delete`
* Aggiunta di `--full-uri` a `storage blob generate-sas` per creare l'URI completo per il BLOB con la firma di accesso condiviso
* Aggiunta di `--file-snapshot` a `storage file copy start` per copiare file da snapshot
* Modifica di `storage blob copy cancel` per visualizzare solo l'errore invece dell'eccezione per NoPendingCopyOperation

## <a name="march-26-2019"></a>26 marzo 2019


### <a name="core"></a>Core
* Risoluzione dei problemi con l'incompatibilità dell'estensione di sviluppo
* La gestione degli errori ora indirizza i clienti alla pagina dei problemi

### <a name="cloud"></a>Cloud
* Risolto l'errore 'Sottoscrizione non trovata' in `cloud set`

### <a name="acr"></a>ACR
* Correzione delle origini ridondanti nell'importazione di immagini
* Aggiunta di `--auth-mode` ai comandi `acr build`, `acr run`, `acr task create` e `acr task update`
* Aggiunta del gruppo di comandi 'acr task credential' per la gestione delle credenziali per un'attività
* Aggiunta di '--no-wait' al comando `acr build`

### <a name="appservice"></a>AppService
* Correzione del bug per il quale `webapp up` non gestiva correttamente l'esecuzione da uno scenario di directory vuota o codice sconosciuto
* Correzione del bug per il quale gli slot non funzionavano per `[webapp|functionapp] config ssl bind`

### <a name="bot-service"></a>Servizio BOT
* Aggiunta di `bot prepare-deploy` per la preparazione della distribuzione dei bot tramite `webapp`
* Modifica di `bot create --kind registration` per mostrare la password se non viene fornita
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Modifica di `--endpoint` in `bot create --kind registration` perché sia una stringa vuota per impostazione predefinita anziché essere obbligatorio
* Aggiunta di `SCM_DO_BUILD_DURING_DEPLOYMENT` alle impostazioni dell'applicazione del modello di Azure Resource Manager per i bot per app Web v4

### <a name="cdn"></a>RETE CDN
* Aggiunta del supporto per `--no-wait` a `cdn endpoint [create|update|start|stop|delete|load|purge]`  
* [MODIFICA CHE CAUSA UN'INTERRUZIONE]: modifica del comportamento predefinito di caching delle stringhe di query di `cdn endpoint create`. Non è più "IgnoreQueryString" per impostazione predefinita. L'impostazione viene ora eseguita dal servizio

### <a name="cosmosdb"></a>Cosmosdb
* Aggiunta del supporto per `--enable-multiple-write-locations`nell'aggiornamento dell'account
* Aggiunta del sottogruppo `network-rule` con i comandi `add`, `remove` e `list` per la gestione delle regole della rete virtuale di un account Cosmos DB

### <a name="interactive"></a>Interattività
* Risoluzione dell'incompatibilità con l'estensione interattiva installata tramite azdev

### <a name="monitor"></a>Monitorare
* Modifica per consentire il valore di dimensione `*` per `monitor metrics alert [create|update]`

### <a name="network"></a>Rete
* Aggiunta del gruppo di comandi `rewrite-rule` a `application-gateway`

### <a name="profile"></a>Profilo
* Aggiunta del supporto dell'account a livello di tenant per l'identità del servizio gestita per `login`

### <a name="postgres"></a>Postgres 
* Aggiunta dei comandi `replica` e del comando`restart server` per postgresql
* Modifica per ottenere la posizione predefinita dal gruppo di risorse quando non specificata per la creazione di server e per aggiungere la convalida dei giorni di conservazione

### <a name="resource"></a>Risorsa
* Ottimizzazione dell'output di tabella per `deployment [create|list|show]`
* Risoluzione del problema con `deployment [create|validate]` in cui il tipo secureObject non veniva riconosciuto

### <a name="graph"></a>Grafico
* Aggiunta del supporto per `--end-date` a `ad [app|sp] credential reset`
* Aggiunta del supporto per l'aggiunta di autorizzazioni con `ad app permission add`
* Correzione di un bug con `ad app permission list` quando non erano presenti autorizzazioni
* Modifica di `ad sp delete` per ignorare l'eliminazione dell'assegnazione del ruolo se l'account corrente non ha una sottoscrizione
* Modifica di `ad app create` affinché `--identifier-uris` sia un elenco vuoto per impostazione predefinita se non specificato

### <a name="storage"></a>storage
* Aggiunta di `--snapshot` a `storage file download-batch` per il download da uno snapshot di condivisione
* Modifica dell'indicatore di stato `storage blob [download-batch|upload-batch]` affinché sia meno dettagliato e indichi il BLOB corrente
* Risoluzione del problema con `storage account update` durante l'aggiornamento dei parametri di crittografia
* Risoluzione del problema relativo all'esito negativo di `storage blob show` con l'uso di oauth (`--auth-mode=login`)

### <a name="vm"></a>VM
* Aggiunta del comando `image update`

## <a name="march-12-2019"></a>12 marzo 2019

Versione 2.0.60

### <a name="core"></a>Core

* Risoluzione di un errore non corretto di sottoscrizione non trovata in `cloud set`

### <a name="acr"></a>ACR

* Correzione delle origini ridondanti nell'importazione di immagini

### <a name="acs"></a>ACS

* Modifica per ignorare il parametro `--listen-address` per `aks browse` se non è supportato da kubectl 

### <a name="appservice"></a>AppService

* Aggiunta di `[webapp|functionapp] deployment list-publishing-credentials` per ottenere l'URL di pubblicazione di Kudu e le relative credenziali
* Rimozione dell'istruzione errata di stampa per `webapp auth update`
* Correzione di `functionapp` per impostare l'immagine corretta per il runtime nei piani di servizio app per Linux
* Rimozione del tag di anteprima per `webapp up` e aggiunta di miglioramenti al comando

### <a name="botservice"></a>Botservice

* Aggiunta di `SCM_DO_BUILD_DURING_DEPLOYMENT` alle impostazioni dell'applicazione del modello di Azure Resource Manager per i bot per app Web v4
* Aggiunta di `Microsoft-BotFramework-AppId` e `Microsoft-BotFramework-AppPassword` alle impostazioni dell'applicazione del modello di Azure Resource Manager per i bot per app Web v4
* Rimozione delle virgolette singole dall'output del comando `bot publish` alla fine di `bot create`
* Modifica di `bot publish` in modo che sia asincrono

### <a name="container"></a>Contenitore

* Aggiunta dell'argomento `--no-wait` a `container [start|restart]`

### <a name="eventhub"></a>Hub eventi

* Aggiunta del flag `--skip-empty-archives` a `eventhub create|update` per supportare archivi vuoti nell'acquisizione

### <a name="find"></a>Find

* Aggiornamento di funzionalità di notevole entità

### <a name="hdinsight"></a>HDInsight

* Aggiunta del parametro `--storage-account-managed-identity` a `hdinsight create` per supportare l'identità del servizio gestita di ADLS Gen2

### <a name="network"></a>Rete

* Risoluzione del problema di `vpn-connection update` per cui l'aggiornamento di una connessione VPN tra gateway in sottoscrizioni diverse aveva esito negativo

### <a name="rdbms"></a>Rdbms

* Correzioni minori per ottenere la posizione predefinita dal gruppo di risorse quando non specificata per la creazione di server e per aggiungere la convalida dei giorni di conservazione

### <a name="role"></a>Ruolo

* Correzione di `role definition update` per usare l'ID per la risoluzione corretta della definizione
* Modifica di `ad app credential reset` per rimuovere il presupposto che l'entità servizio dell'app sia sempre presente

### <a name="service-fabric"></a>Service Fabric

* Correzione del problema per cui `sf cluster list` non era iterabile

## <a name="february-26-2019"></a>26 febbraio 2019

Versione 2.0.59

### <a name="core"></a>Core

* Correzione del problema per cui in alcuni casi l'uso di `--subscription NAME` generava un'eccezione

### <a name="acr"></a>ACR

* Aggiunta del parametro `--target` per i comandi `acr build`, `acr task create` e`acr task update`
* Miglioramento della gestione degli errori per i comandi di runtime quando non si è connessi ad Azure

### <a name="acs"></a>ACS

* Aggiunta dell'opzione `--listen-address` a `aks port-forward`

### <a name="appservice"></a>AppService

* Aggiunta del comando `functionapp devops-build`

### <a name="batch"></a>Batch
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Rimozione del comando `batch pool upgrade os`
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Rimozione della proprietà `Pacakges` dalle risposte `Application`
* Aggiunta del comando `batch application package list` per elencare i pacchetti di un'applicazione
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Modifica di `--application-id` in `--application-name` in tutti i comandi `batch application` 
* Aggiunta dell'argomento `--json-file` ai comandi per richiedere la risposta dell'API non elaborata
* Aggiornamento della convalida per includere automaticamente `https://` in tutti gli endpoint se manca

### <a name="cosmosdb"></a>Cosmos DB

* Aggiunta del sottogruppo `network-rule` con i comandi `add`, `remove` e `list` per la gestione delle regole della rete virtuale di un account Cosmos DB

### <a name="kusto"></a>Kusto

* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Modifica dei tipi `hot_cache_period` e `soft_delete_period` del database per il formato di durata ISO8601

### <a name="network"></a>Rete

* Aggiunta dell'argomento `--express-route-gateway-bypass` a `vpn-connection [create|update]`
* Aggiunta di gruppi di comandi dalle estensioni `express-route`
* Aggiunta dei gruppi di comandi `express-route gateway` e `express-route port`
* Aggiunta dell'argomento `--legacy-mode` a `express-route peering [create|update]` 
* Aggiunta degli argomenti `--allow-classic-operations` e `--express-route-port` a `express-route [create|update]`
* Aggiunta dell'argomento `--gateway-default-site` a `vnet-gateway [create|update]`
* Aggiunta dei comandi `ipsec-policy` a `vnet-gateway`

### <a name="resource"></a>Risorsa

* Correzione del problema con `deployment create` in cui per il campo del tipo veniva fatta distinzione tra maiuscole e minuscole
* Aggiunta del supporto per il file di parametri basati su URI in `policy assignment create`
* Aggiunta del supporto per le definizioni e i parametri basati su URI in `policy set-definition update`
* Correzione della gestione di regole e parametri per `policy definition update`
* Correzione del problema con `resource show/update/delete/tag/invoke-action` in cui gli ID tra sottoscrizioni non rispettavano correttamente l'ID sottoscrizione

### <a name="role"></a>Ruolo

* Aggiunta del supporto per i ruoli dell'app in `ad app [create|update]`

### <a name="vm"></a>VM

* Correzione del problema con `vm create where ` in cui "--accelerated-networking" non veniva abilitato per impostazione predefinita per Ubuntu 18.0

## <a name="february-12-2019"></a>12 febbraio 2019

Versione 2.0.58

### <a name="core"></a>Core

* `az --version` visualizza ora una notifica se sono presenti pacchetti che è possibile aggiornare
* Correzione della regressione per cui non era più possibile usare `--ids` con l'output JSON

### <a name="acr"></a>ACR
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Rimozione del gruppo di comandi `acr build-task`
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Rimozione delle opzioni `--tag` e `--manifest` da `acr repository delete`

### <a name="acs"></a>ACS
* Aggiunta del supporto per nomi che non applicano la distinzione tra maiuscole e minuscole in `aks [enable-addons|disable-addons]`
* Aggiunta del supporto per l'operazione di aggiornamento di Azure Active Directory tramite `aks update-credentials --reset-aad`
* Aggiunta di un chiarimento per specificare che `--output` viene ignorato per `aks get-credentials`

### <a name="ams"></a>AMS
* Aggiunta dei comandi `ams streaming-endpoint [start | stop | create | update] wait`
* Aggiunta dei comandi `ams live-event [create | start | stop | reset] wait`

### <a name="appservice"></a>Servizio app
* Aggiunta della possibilità di creare e configurare funzioni tramite contenitori di Registro Azure Container
* Aggiunta del supporto per l'aggiornamento delle configurazioni delle app Web tramite JSON
* Miglioramento della Guida di `appservice-plan-update`
* Aggiunta del supporto per Application Insights in functionapp create
* Correzione dei problemi riscontrati con l'app Web SSH

### <a name="botservice"></a>Botservice
* Miglioramento dell'esperienza utente per `bot publish`
* Aggiunta di un avviso per i timeout quando si esegue `npm install` durante `az bot publish`
* Rimozione del carattere non valido `.` da `--name` in `az bot create`
* Modifica apportata per evitare l'assegnazione casuale di nomi delle risorse durante la creazione di risorse di archiviazione di Azure, piano di servizio app, funzioni/app Web e Application Insights
* [DEPRECATO] Argomento `--proj-name` deprecato e sostituito da `--proj-file-path`
* Modifica di `az bot publish` per rimuovere i file di distribuzione di Node.js IIS recuperati se non esistono già
* Aggiunta dell'argomento `--keep-node-modules` a `az bot publish` per non eliminare la cartella `node_modules` in Servizio app
* Aggiunta della coppia chiave-valore `"publishCommand"` all'output di `az bot create` durante la creazione di una funzione di Azure o un bot di app Web
  * Il valore di `"publishCommand"` è un comando `az bot publish` prepopolato con i parametri necessari per pubblicare il bot appena creato
* Aggiornamento di `"WEBSITE_NODE_DEFAULT_VERSION"` nel modello di Azure Resource Manager per usare la versione 10.14.1 invece di 8.9.4 con i bot dell'SDK v4

### <a name="key-vault"></a>Key Vault
* Correzione del problema di `keyvault secret backup` per cui alcuni utenti ricevevano un errore `unexpected_keyword` durante l'uso di `--id`

### <a name="monitor"></a>Monitorare
* Modifica di `monitor metrics alert [create|update]` per consentire il valore di dimensione `*`

### <a name="network"></a>Rete
* Modifica di `dns zone export` per assicurare che i CNAME esportati siano nomi di dominio completi
* Aggiunta del parametro `--gateway-name` a `nic ip-config address-pool [add|remove]` per supportare i pool di indirizzi back-end del gateway applicazione
* Aggiunta degli argomenti `--traffic-analytics` e `--workspace` a `network watcher flow-log configure` per supportare l'analisi del traffico tramite un'area di lavoro Log Analytics
* Aggiunta di `--idle-timeout` e `--floating-ip` a `lb inbound-nat-pool [create|update]`

### <a name="policy-insights"></a>Policy Insights
* Aggiunta dei comandi `policy remediation` per supportare le funzionalità di correzione dei criteri delle risorse

### <a name="rdbms"></a>RDBMS
* Miglioramento del messaggio e dei parametri dei comandi della Guida

### <a name="redis"></a>Redis
* Aggiunta di comandi per la gestione di firewall-rules (create, update, delete, show, list)
* Aggiunta di comandi per la gestione di server-link (create, delete, show, list)
* Aggiunta di comandi per la gestione di patch-schedule (create, update, delete, show)
* Aggiunta del supporto per le zone di disponibilità e per la versione minima TLS in 'redis create
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Rimozione dei comandi `redis update-settings` e `redis list-all`
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Il parametro per `redis create`: 'tenant settings' non è accettato nel formato chiave[=valore]
* [DEPRECATO] Aggiunta del messaggio di avviso per la deprecazione del comando `redis import-method`

### <a name="role"></a>Ruolo
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Spostamento del comando `az identity` dai comandi `vm`

### <a name="sql-vm"></a>VM SQL
* [DEPRECATO] Argomento `--boostrap-acc-pwd` deprecato a causa di un errore di digitazione

### <a name="vm"></a>VM
* Modifica di `vm list-skus` per consentire l'uso di `--all` al posto di `--all true`
* Aggiunta di `vmss run-command [invoke | list | show]`
* Correzione del bug per cui il comando `vmss encryption enable` non riesce se è già stato eseguito
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Spostamento del comando `az identity` nei comandi `role`

## <a name="january-31-2019"></a>31 gennaio 2019

Versione 2.0.57

### <a name="core"></a>Core

* Correzione rapida del [problema 8399](https://github.com/Azure/azure-cli/issues/8399).

## <a name="january-28-2019"></a>28 gennaio 2019

Versione 2.0.56

### <a name="acr"></a>ACR
* Aggiunta del supporto di regole per reti virtuali/IP

### <a name="acs"></a>ACS
* Aggiunta dell'anteprima di nodi virtuali
* Aggiunta dei comandi di OpenShift gestito
* Aggiunta del supporto per le operazioni di aggiornamento dell'entità servizio con `aks update-credentials -reset-service-principal`

### <a name="ams"></a>AMS
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Ridenominazione di `ams asset get-streaming-locators` in `ams asset list-streaming-locators`
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Ridenominazione di `ams streaming-locator get-content-keys` in `ams streaming-locator list-content-keys`

### <a name="appservice"></a>Servizio app
* Aggiunta del supporto per Application Insights in `functionapp create`
* Aggiunta del support per la creazione di piani del servizio app (incluso Elastico Premium) nelle app per le funzioni
* Correzione dei problemi di impostazione delle app con i piani Elastico Premium

### <a name="container"></a>Contenitore
* Aggiunta del comando `container start`
* Modifica apportata per consentire l'uso di valori decimali per la CPU durante la creazione di contenitori

### <a name="eventgrid"></a>EventGrid
* Aggiunta del parametro `--deadletter-endpoint` a `event-subscription [create|update]`
* Aggiunta di storagequeue e hybridconnection come nuovi valori per 'event-subscription [create|update] --endpoint-type'
* Aggiunta dei parametri `--max-delivery-attempts` e `--event-ttl` a `event-subscription create` per specificare i criteri di ripetizione per gli eventi
* Aggiunta di un messaggio di avviso in `event-subscription [create|update]` quando si usa webhook come destinazione per una sottoscrizione di eventi
* Aggiunta del parametro source-resource-id per tutti i comandi relativi alla sottoscrizione di eventi e aggiunta di un contrassegno di deprecazione per tutti gli altri parametri relativi alle risorse di origine

### <a name="hdinsight"></a>HDInsight
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Rimozione dei parametri `--virtual-network` e `--subnet-name` da `hdinsight [application] create`
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Modifica apportata a `hdinsight create --storage-account` per accettare il nome o l'ID di un account di archiviazione invece degli endpoint BLOB
* Aggiunta dei parametri `--vnet-name` e `--subnet-name` a`hdinsight create`
* Aggiunta del supporto per Enterprise Security Package e per la crittografia del disco in `hdinsight create` 
* Aggiunta del comando `hdinsight rotate-disk-encryption-key`
* Aggiunta del comando `hdinsight update`

### <a name="iot"></a>IoT
* Aggiunta del formato di codifica al comando routing-endpoint

### <a name="kusto"></a>Kusto
* Versione di anteprima

### <a name="monitor"></a>Monitorare
* Modifica del confronto di ID perché non faccia distinzione tra maiuscole e minuscole

### <a name="profile"></a>Profilo
* Abilitazione dell'account a livello di tenant per l'identità del servizio gestita per `login`

### <a name="network"></a>Rete
* Correzione del problema di `express-route update`: per cui l'argomento `--bandwidth` veniva ignorato
* Correzione del problema di `ddos-protection update` per cui set comprehension causava l'analisi dello stack

### <a name="resource"></a>Risorsa
* Aggiunta del supporto per il file di parametri URI in `group deployment create`
* Aggiunta del supporto per l'identità gestita in `policy assignment [create|list|show]`

### <a name="sql-virtual-machine"></a>Macchina virtuale SQL
* Versione di anteprima

### <a name="storage"></a>Archiviazione
* Modifica della correzione per aggiornare solo le proprietà cambiate nello stesso oggetto
* Correzione #8021, i dati binari restituiti vengono codificati in base 64

### <a name="vm"></a>VM
* Modifica di `vm encryption enable` per confermare che l'insieme di credenziali delle chiavi di crittografia disco e quello di crittografia chiave esistono
* Aggiunta del flag `--force` a `vm encryption enable`

## <a name="january-15-2019"></a>15 gennaio 2019

Versione 2.0.55

### <a name="acr"></a>ACR
* Modifica apportata per consentire il push forzato di un grafico Helm che non esiste
* Modifica apportata per consentire operazioni del runtime senza richieste di Azure Resource Manager
* [DEPRECATO] Parametro `--resource-group` deprecato nei comandi:
  * `acr login`
  * `acr repository`
  * `acr helm`

### <a name="acs"></a>ACS
* Aggiunta del supporto per nuove aree ACI

### <a name="appservice"></a>Servizio app
* Correzione del problema di caricamento dei certificati per le app ospitate in un ambiente del servizio app di Azure, in cui il gruppo di risorse dell'ambiente del servizio app di Azure e il gruppo di risorse per app sono differenti
* Modifica di `webapp up` per usare lo SKU P1V1 come predefinito per Linux
* Correzione di `[webapp|functionapp] deployment source config-zip` per visualizzare il messaggio di errore appropriato quando una distribuzione non riesce 
* Aggiunta del comando `webapp ssh`

### <a name="botservice"></a>Botservice
* Aggiunta degli aggiornamenti sullo stato della distribuzione a `bot create`

### <a name="configure"></a>Configurare
* Aggiunta di `none` come formato di output configurabile

### <a name="cosmosdb"></a>Cosmos DB
* Aggiunta del supporto per la creazione di database con velocità effettiva condivisa

### <a name="hdinsight"></a>HDInsight
* Aggiunta di comandi per la gestione di applicazioni
* Aggiunta di comandi per la gestione di azioni script
* Aggiunta di comandi per la gestione di OMS (Operations Management Suite)
* Aggiunta del supporto per indicare l'utilizzo locale in `hdinsight list-usage`
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Rimozione del tipo di cluster predefinito da `hdinsight create`

### <a name="network"></a>Rete
* Aggiunta degli argomenti `--custom-headers` e `--status-code-ranges` a `traffic-manager profile [create|update]`
* Aggiunta di nuovi tipi di routing: Subnet e Multivalore
* Aggiunta degli argomenti `--custom-headers` e `--subnets` a `traffic-manager endpoint [create|update]`  
* Correzione del problema per cui la specifica di `--vnets ""` in `ddos-protection update` generava un errore

### <a name="role"></a>Ruolo
* [DEPRECATO] Argomento `--password` deprecato per `create-for-rbac`. Usare in alternativa le password generate dall'interfaccia della riga di comando

### <a name="security"></a>Sicurezza
* Versione iniziale

### <a name="storage"></a>Archiviazione
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Modifica del requisito per cui il numero predefinito di risultati di `storage [blob|file|container|share] list` deve essere 5.000. Usare `--num-results *` per il comportamento predefinito che prevede la restituzione di tutti i risultati
* Aggiunta del parametro `--marker` a `storage [blob|file|container|share] list`
* Aggiunta del marcatore del log per la pagina successiva a STDERR per `storage [blob|file|container|share] list` 
* Aggiunta del comando `storage blob service-properties update` con il supporto per siti Web statici

### <a name="vm"></a>VM
* Modifica di `vm [disk|unmanaged-disk]` e `vmss disk` in modo che i parametri siano più coerenti
* Aggiunta del supporto per riferimenti alle risorse tra tenant in `[vm|vmss] create`
* Correzione del bug della configurazione predefinita di `vm diagnostics get-default-config --windows-os`
* Aggiunta dell'argomento `--provision-after-extensions` a `vmss extension set` per definire di quali risorse eseguire il provisioning prima dell'impostazione dell'estensione
* Aggiunta dell'argomento `--replica-count` a `sig image-version update` per l'impostazione del numero predefinito di repliche
* Correzione del bug di `image create --source` per cui il disco del sistema operativo di origine viene erroneamente riconosciuto come una VM con lo stesso nome, anche se si specifica l'ID risorsa completo

## <a name="december-20-2018"></a>20 dicembre 2018

Versione 2.0.54
### <a name="appservice"></a>Servizio app
* Risoluzione del problema relativo all'esito negativo della ridistribuzione di `webapp up`
* Aggiunta del supporto per ottenere l'elenco degli snapshot delle app Web e ripristinarli
* Aggiunta del supporto per il flag `--runtime` alle app per le funzioni di Windows

### <a name="iotcentral"></a>IoTCentral
* Correzione della chiamata API del comando di aggiornamento

### <a name="role"></a>Ruolo
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Modifica di `ad [app|sp] list` per elencare solo i primi 100 oggetti per impostazione predefinita

### <a name="sql"></a>SQL
* Aggiunta del supporto per le regole di confronto personalizzate nelle istanze gestite

### <a name="vm"></a>VM
* Aggiunta del parametro `---os-type` a `disk create`

## <a name="december-18-2018"></a>18 dicembre 2018

Versione 2.0.53
### <a name="acr"></a>ACR
* Aggiunta del supporto per l'importazione di immagini da registri contenitori esterni
* Riduzione del layout tabella per l'elenco attività
* Aggiunta del supporto per gli URL di Azure DevOps

### <a name="acs"></a>ACS
* Aggiunta dell'anteprima di nodi virtuali
* Rimozione di "(PREVIEW)" dagli argomenti AAD per `aks create`
* [DEPRECATO] Comandi `az acs` deprecati. Il servizio ACS verrà ritirato il 31 gennaio 2020
* Aggiunta del supporto dei criteri di rete durante la creazione di nuovi cluster del servizio Azure Kubernetes
* Rimozione del requisito dell'argomento `--nodepool-name` per `aks scale` se è presente un solo pool di nodi

### <a name="appservice"></a>Servizio app
* Risoluzione del problema in cui `webapp config container` non rispettava il parametro `--slot`

### <a name="botservice"></a>Botservice
* Aggiunta del supporto per l'analisi dei file `.bot` durante la chiamata di `bot show`
* Correzione del bug del provisioning di AppInsights
* Correzione del bug degli spazi bianchi quando si usano i percorsi dei file
* Riduzione delle chiamate di rete Kudu
* Miglioramenti dell'esperienza utente di comando generale

### <a name="consumption"></a>Consumo
* Correzione di bug per l'API budget per la visualizzazione di notifiche

### <a name="cosmosdb"></a>Cosmos DB
* Aggiunta del supporto per l'aggiornamento dell'account da multimaster a master singolo

### <a name="maps"></a>Mappe
* Aggiunta del supporto per lo SKU S1 a `maps account [create|update]`

### <a name="network"></a>Rete
* Aggiunta del supporto per `--format` e `--log-version` a `watcher flow-log configure`
* Risoluzione del problema con `dns zone update` in cui l'utilizzo di "" per cancellare la risoluzione e la registrazione di reti virtuali non funzionava

### <a name="resource"></a>Risorsa
* Correzione della gestione del parametro di ambito per i gruppi di gestione in `policy assignment [create|list|delete|show|update]` 
* Aggiunta del nuovo comando `resource wait`

### <a name="storage"></a>Archiviazione
*  Aggiunta della possibilità di aggiornare la versione dello schema del log per i servizi di archiviazione in `storage logging update`

### <a name="vm"></a>VM
* Correzione di un problema di arresto anomalo in `vm identity remove` quando la macchina virtuale specificata non dispone di identità del servizio gestito assegnate

## <a name="december-4-2018"></a>4 dicembre 2018

Versione 2.0.52
### <a name="core"></a>Core
* Aggiunta del supporto per il provisioning di risorse tra tenant per un'entità servizio multi-tenant
* Correzione del bug relativo all'analisi non corretta degli ID inviati tramite pipe da un comando con output tsv

### <a name="appservice"></a>Servizio app
* [ANTEPRIMA] Aggiunta del comando `webapp up` per la creazione e la distribuzione di contenuto in un'app
* Correzione di un bug nelle app Windows basate su contenitori causato da una modifica del back-end

### <a name="network"></a>Rete
* Aggiunta dell'argomento `--exclusion` a `application-gateway waf-config set` per supportare le esclusioni del Web application firewall

### <a name="role"></a>Ruolo
* Aggiunta del supporto di identificatori personalizzati per le credenziali della password 

### <a name="vm"></a>VM
* [DEPRECATO]Parametro `vm extension [show|wait] --expand` deprecato
* Aggiunta del parametro `--force` a `vm restart` per la ridistribuzione delle VM che non rispondono
* Modifica di `[vm|vmss] create --authentication-type` in modo da accettare "all" e creare una VM con autenticazione sia tramite password sia SSH
* Aggiunta del parametro `image create --os-disk-caching` per l'impostazione della memorizzazione nella cache del disco del sistema operativo per un'immagine

## <a name="november-20-2018"></a>20 novembre 2018

Versione 2.0.51
### <a name="core"></a>Core
* Modifica dell'account di accesso MSI in modo che il nome della sottoscrizione non venga riutilizzato in un'identità

### <a name="acr"></a>ACR
* Aggiunta del token di contesto al passaggio dell'attività
* Aggiunta di supporto per l'impostazione di segreti nell'esecuzione di ACR per il mirroring dell'attività di ACR
* Miglioramento del supporto per `--top` e `--orderby` per i comandi `show-tags` e `show-manifests`

### <a name="appservice"></a>Servizio app
* Modifica del timeout predefinito per la distribuzione del file ZIP per il polling dello stato con incremento a 5 minuti, oltre all'aggiunta di una proprietà del timeout per la personalizzazione di questo valore
* Aggiornamento del valore `node_version` predefinito. La reimpostazione dell'azione di scambio di slot durante uno scambio in due fasi consente di conservare tutte le impostazioni dell'app e le stringhe di connessione
* Rimozione del controllo dello SKU lato client per la creazione del piano di servizio app per Linux
* Correzione dell'errore relativo al tentativo di recupero dello stato di zipdeploy

### <a name="iotcentral"></a>IotCentral
* Aggiunta della verifica della disponibilità di un sottodominio durante la creazione di un'applicazione IoT Central

### <a name="keyvault"></a>Insieme di credenziali delle chiavi
* Correzione di un bug a causa del quale è possibile che siano stati ignorati errori

### <a name="network"></a>Rete
* Aggiunta dei sottocomandi `root-cert` a `application-gateway` per la gestione dei certificati radice attendibili
* Aggiunta delle opzioni `--min-capacity` e `--custom-error-pages` a `application-gateway [create|update]`:
* Aggiunta di `--zones` per il supporto delle zone di disponibilità in `application-gateway create` 
* Aggiunta degli argomenti `--file-upload-limit`, `--max-request-body-size` e `--request-body-check` a `application-gateway waf-config set`

### <a name="rdbms"></a>Rdbms
* Aggiunta dei comandi della rete virtuale per mariadb

### <a name="rbac"></a>Controllo degli accessi in base al ruolo
* Risoluzione di un problema relativo al tentativo di aggiornare credenziali non modificabili in `ad app update`
* Aggiunta di avvisi relativi all'output per comunicare le modifiche imminenti che causano un'interruzione per `ad [app|sp] list` 

### <a name="storage"></a>Archiviazione
* Miglioramento della gestione dei casi limite per i comandi di copia per le risorse di archiviazione
* Risoluzione del problema relativo al fatto che `storage blob copy start-batch` non usa le credenziali di accesso quando gli account di destinazione e di origine sono uguali
* Correzione del bug di `storage [blob|file] url` in cui `sas_token` non veniva incorporato nell'URL
* Aggiunta dell'avviso relativo alla modifica che causa un'interruzione a `[blob|container] list`: verranno presto restituiti come output per impostazione predefinita solo i primi 5000 risultati

### <a name="vm"></a>VM
* Aggiunta del supporto a `[vm|vmss] create --storage-sku` per specificare lo SKU dell'account di archiviazione separatamente per i dischi gestiti del sistema operativo e di dati
* Modifica dei parametri del nome della versione per `sig image-version` in modo da ottenere `--image-version -e`
* Deprecamento dell'argomento `sig image-version` è per `--image-version-name`, sostituito da `--image-version`
* Aggiunta di supporto per l'uso del disco locale del sistema operativo a `[vm|vmss] create --ephemeral-os-disk`
* Aggiunta del supporto per `--no-wait` a `snapshot create/update`
* Aggiunta del comando `snapshot wait`
* Aggiunta del supporto per l'uso del nome dell'istanza con `[vm|vmss] extension set --extension-instance-name`

## <a name="november-6-2018"></a>6 novembre 2018

Versione 2.0.50

### <a name="core"></a>Core
* Aggiunta del supporto per l'autenticazione dell'entità servizio con sn+autorità di certificazione

### <a name="acr"></a>ACR
* Aggiunta del supporto per gli eventi git di richieste pull e commit per il trigger di origine attività
* Modificato per usare il valore Dockerfile predefinito se non è specificato nel comando di compilazione

### <a name="acs"></a>ACS
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Rimozione di `enable_cloud_console_aks_browse` per abilitare 'az servizio Azure Kubernetes browse' per impostazione predefinita

### <a name="advisor"></a>Advisor
* Versione di disponibilità generale (GA)

### <a name="ams"></a>AMS
* Aggiunta di nuovi gruppi di comandi:
  *  `ams account-filter`
  *  `ams asset-filter`
  *  `ams content-key-policy`
  *  `ams live-event`
  *  `ams live-output`
  *  `ams streaming-endpoint`
  *  `ams mru`
* Aggiunta di nuovi comandi:
  * `ams account check-name`
  * `ams job update`
  * `ams asset get-encryption-key`
  * `ams asset get-streaming-locators`
  * `ams streaming-locator get-content-keys`
* Aggiunta del supporto dei parametri di crittografia a `ams streaming-policy create`
* Aggiunta del supporto a `ams transform output remove` in modo da poter essere eseguito passando l'indice di output da rimuovere
* Aggiunta degli argomenti `--correlation-data` e `--label` al gruppo di comandi `ams job`
* Aggiunta degli argomenti `--storage-account` e `--container` al gruppo di comandi `ams asset`
* Aggiunta dei valori predefiniti per l'ora di scadenza (Now+23h) e delle autorizzazioni (lettura) nel comando `ams asset get-sas-url` 
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Sostituzione del comando `ams streaming locator` con `ams streaming-locator`
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Aggiornamento dell'argomento `--content-keys` di `ams streaming locator`
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Ridenominazione di `--content-policy-name` in `--content-key-policy-name` nel comando `ams streaming locator`
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Sostituzione del comando `ams streaming policy` con `ams streaming-policy`
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Sostituzione dell'argomento `--preset-names` con `--preset` nel gruppo di comandi `ams transform`. Ora è possibile impostare solo 1 output/set di impostazioni alla volta (per aggiungerne altri, è necessario eseguire `ams transform output add`). Inoltre, è possibile impostare il set di impostazioni StandardEncoderPreset personalizzato passando il percorso del file JSON personalizzato.
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Ridenominazione di `--output-asset-names ` in `--output-assets` nel comando `ams job start`. Ora accetta un elenco di asset separati da spazi nel formato 'assetName=label'. Per inviare un asset senza etichetta usare il formato seguente: 'assetName='.

### <a name="appservice"></a>AppService
* Correzione di un bug in `az webapp config backup update` che impediva l'impostazione di una pianificazione di backup se non ne era già impostata una

### <a name="configure"></a>Configurare
* Aggiunta di YAML alle opzioni di formato di output

### <a name="container"></a>Contenitore
* Modificato per mostrare l'identità durante l'esportazione di un gruppo di contenitori in yaml

### <a name="eventhub"></a>Hub eventi
* Aggiunta del flag `--enable-kafka` per il supporto di Kafka in `eventhub namespace [create|update]`

### <a name="interactive"></a>Interattività
* Interactive ora installa l'estensione `interactive` che consentirà aggiornamenti e supporto più rapidi

### <a name="monitor"></a>Monitorare
* Aggiunta del supporto per i nomi delle metriche che includono i caratteri barra (/) e punto (.) a `--condition` in `monitor metrics alert [create|update]`

### <a name="network"></a>Rete
* I nomi di comando `network interface-endpoint` sono stati deprecati a favore di `network private-endpoint`
* Correzione del problema per cui l'argomento `--peer-circuit` in `express-route peering connection create` non accettava un ID
* Correzione del problema per cui `--ip-tags` non funzionava correttamente con `public-ip create` 

### <a name="profile"></a>Profilo
* Aggiunta di `--use-cert-sn-issuer` a `az login` per l'accesso dell'entità servizio con il rollover automatico dei certificati

### <a name="rdbms"></a>RDBMS
* Aggiunta dei comandi di replica mysql

### <a name="resource"></a>Risorsa
* Aggiunta del supporto per gruppi di gestione e sottoscrizioni ai comandi `policy definition|set-definition`

### <a name="role"></a>Ruolo
* Aggiunta del supporto per la gestione di autorizzazioni API, utenti connessi, password delle applicazioni e credenziali dei certificati
* Modifica di `ad sp create-for-rbac` per chiarire la confusione tra displayName e il nome dell'entità servizio
* Aggiunta del supporto per concedere le autorizzazioni alle app AAD

### <a name="storage"></a>Archiviazione
* Aggiunta del supporto per connettersi ai servizi di archiviazione solo con la firma di accesso condiviso e gli endpoint (senza un nome account o una chiave) come descritto in `Configure Azure Storage connection strings <https://docs.microsoft.com/azure/storage/common/storage-configure-connection-string>`

### <a name="vm"></a>VM
* Aggiunta dell'argomento `storage-sku` a `image create` per l'impostazione tipo di account di archiviazione predefinito dell'immagine
* Correzione del bug con `vm resize` in cui l'opzione `--no-wait` causava l'arresto anomalo del comando
* Modifica del formato di output della tabella `vm encryption show` per visualizzare lo stato
* Modifica di `vm secret format` per richiedere l'output json/jsonc. Avvisa l'utente e usa l'output json per impostazione predefinita se viene selezionato un formato di output indesiderato.
* Miglioramento della convalida degli argomenti per `vm create --image`

## <a name="october-23-2018"></a>23 ottobre 2018

Versione 2.0.49

### <a name="core"></a>Core
* Risoluzione del problema con `--ids` per il quale `--subscription` aveva la precedenza sulla sottoscrizione in `--ids`.
* Aggiunta di avvisi espliciti se i parametri vengono ignorati usando `--ids`.

### <a name="acr"></a>ACR
* Risoluzione di un problema di codifica di ACR Build in Python2

### <a name="cdn"></a>RETE CDN
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Modifica del comportamento di memorizzazione nella cache predefinito di `cdn endpoint create` per le stringhe delle query, che non sarà più "IgnoreQueryString". L'impostazione viene ora eseguita dal servizio

### <a name="container"></a>Contenitore
* Aggiunta di `Private` come tipo valido da passare a '--ip-address'
* Modifica per consentire di usare solo l'ID subnet per configurare una rete virtuale per il gruppo di contenitori.
* Modifica per consentire l'uso del nome della rete virtuale o dell'ID risorsa per poter usare reti virtuali di diversi gruppi di risorse.
* Aggiunta di `--assign-identity` per aggiungere un'identità del servizio gestita a un gruppo di contenitori
* Aggiunta di `--scope` per creare un'assegnazione di ruoli per l'identità del servizio gestita assegnata al sistema
* Aggiunta di un avviso quando si crea un gruppo di contenitori con un'immagine senza un processo a esecuzione prolungata
* Correzione dei problemi di output della tabella per i comandi `list` e `show`

### <a name="cosmosdb"></a>Cosmos DB
* Aggiunta del supporto di `--enable-multiple-write-locations` a `cosmosdb create`

### <a name="interactive"></a>Interattività
* Modifica per far sì che il parametro di sottoscrizione globale venga visualizzato nei parametri

### <a name="iot-central"></a>IoT Central
* Aggiunta di un modello e di opzioni per il nome visualizzato per la creazione di applicazioni di IoT Central
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Rimozione del supporto per lo SKU F1. Usare invece lo SKU S1.

### <a name="monitor"></a>Monitorare
* Modifiche a `monitor activity-log list`:
  * Aggiunta del supporto per elencare tutti gli eventi a livello di sottoscrizione
  * Aggiunta del parametro `--offset` per creare più facilmente query temporali
  * Convalida migliorata per `--start-time` e `--end-time` per l'uso di una gamma più ampia di formati ISO8601 e formati di data e ora più semplici da usare.
  * Aggiunta di `--namespace` come alias per l'opzione deprecata `--resource-provider`
  * `--filters` deprecato perché il servizio non supporta valori diversi da quelli con opzioni fortemente tipizzate
* Modifiche a `monitor metrics list`:
  * Aggiunta del parametro `--offset` per creare più facilmente query temporali
  * Convalida migliorata per `--start-time` e `--end-time` per l'uso di una gamma più ampia di formati ISO8601 e formati di data e ora più semplici da usare.
* Convalida migliorata per gli argomenti `--event-hub` e `--event-hub-rule` in `monitor diagnostic-settings create`.

### <a name="network"></a>Rete
* Aggiunta degli argomenti `--app-gateway-address-pools` e `--gateway-name` a `nic create`, per supportare l'aggiunta di pool di indirizzi di back-end del gateway applicazione a una scheda di interfaccia di rete
* Aggiunta degli argomenti `--app-gateway-address-pools` e `--gateway-name` a `nic ip-config create/update`, per supportare l'aggiunta di pool di indirizzi di back-end del gateway applicazione a una scheda di interfaccia di rete

### <a name="servicebus"></a>ServiceBus
* Aggiunta di `migration_state` di sola lettura a MigrationConfigProperties per mostrare lo stato di migrazione corrente del bus di servizio Standard allo spazio dei nomi Premium

### <a name="sql"></a>SQL
* Correzione di `sql failover-group create` e `sql failover-group update` per il funzionamento con il criterio di failover manuale

### <a name="storage"></a>Archiviazione
* Correzione della formattazione dell'output di `az storage cors list`. Tutte le voci mostrano la chiave "Service" corretta.
* Aggiunta del parametro `--bypass-immutability-policy` per l'eliminazione del contenitore bloccato dei criteri di immutabilità

### <a name="vm"></a>VM
* Impostazione della modalità di memorizzazione nella cache del disco su `None` per le macchine della serie Lv/Lv2 in `[vm|vmss] create`.
* Aggiornamento dell'elenco delle dimensioni che supportano l'accelerazione di rete per `vm create`.
* Aggiunta di argomenti fortemente tipizzati per configurazioni ultrassd iops e mbps per `disk create`.

## <a name="october-16-2018"></a>16 ottobre 2018

Versione 2.0.48

### <a name="vm"></a>VM
* Risoluzione del problema dell'SDK che impediva l'installazione di Homebrew

## <a name="october-9-2018"></a>9 ottobre 2018

Versione 2.0.47

### <a name="core"></a>Core
* Miglioramento della gestione degli errori di richiesta non valida

### <a name="acr"></a>ACR
* Aggiunta del supporto per un formato di tabella simile al client Helm

### <a name="acs"></a>ACS
* Aggiunta di `aks [create|scale] --nodepool-name` per la configurazione del nome del pool di nodi, con troncamento a 12 caratteri e - nodepool1 come valore predefinito 
* Correzione per il fallback a "scp" in caso di errori in Parimiko
* Modifica di `aks create` in modo che `--aad-tenant-id` non sia più obbligatorio
* Miglioramento dell'unione delle credenziali Kubernetes in presenza di voci duplicate

### <a name="container"></a>Contenitore
* Modifica di `functionapp create` per supportare la creazione di un tipo di piano a consumo per Linux con un runtime specifico
* [ANTEPRIMA] Aggiunta del supporto per l'hosting di app Web in contenitori Windows

### <a name="event-hub"></a>Hub eventi
* Correzione del comando `eventhub update`
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Modifica dei comandi `list` per gestire gli errori per risorse non trovate (404) nel modo consueto anziché visualizzando un elenco vuoto

### <a name="extensions"></a>Estensioni
* Risoluzione del problema relativo al tentativo di aggiungere un'estensione già installata

### <a name="hdinsight"></a>HDInsight
* Versione iniziale

### <a name="iot"></a>IoT
* Aggiunta del comando per l'installazione di estensioni al banner di prima esecuzione

### <a name="keyvault"></a>Insieme di credenziali delle chiavi
* Modifica per limitare i comandi di archiviazione per gli insiemi di credenziali delle chiavi al profilo API più recente

### <a name="network"></a>Rete
* Correzione del comando `network dns zone create`, che avrà esito positivo anche se l'utente ha configurato una località predefinita (vedere #6052)
* Deprecamento di `--remote-vnet-id` per `network vnet peering create`
* Aggiunta di `--remote-vnet` a `network vnet peering create`, che accetta un nome o un ID
* Aggiunta del supporto per più prefissi di subnet a `network vnet create` con `--subnet-prefixes`
* Aggiunta del supporto per più prefissi di subnet a `network vnet subnet [create|update]` con `--address-prefixes`
* Risoluzione del problema relativo a `network application-gateway create` che impediva la creazione di gateway con SKU `WAF_v2` o `Standard_v2`
* Aggiunta dell'utile argomento `--service-endpoint-policy` a `network vnet subnet update`

### <a name="role"></a>Ruolo
* Aggiunta del supporto per ottenere l'elenco dei proprietari di app Azure AD ad `ad app owner`
* Aggiunta del supporto per ottenere l'elenco dei proprietari di entità servizio di Azure AD ad `ad sp owner`
* Modifica per garantire che i comandi di creazione e aggiornamento di definizioni di ruolo accettino più configurazioni delle autorizzazioni
* Modifica di `ad sp create-for-rbac` per garantire che l'URI della home page sia sempre "https"

### <a name="service-bus"></a>Bus di servizio
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Modifica dei comandi `list` per gestire gli errori per risorse non trovate (404) nel modo consueto anziché visualizzando un elenco vuoto

### <a name="vm"></a>VM
* Correzione del campo `accessSas` vuoto in `disk grant-access`
* Modifica di `vmss create` per riservare un intervallo di porte front-end sufficiente per gestire il provisioning eccessivo
* Correzione dei comandi di aggiornamento per `sig`
* Aggiunta del supporto di `--no-wait` per la gestione delle versioni delle immagini in `sig`
* Modifica di `vm list-ip-addresses` per la visualizzazione della zona di disponibilità degli indirizzi IP pubblici
* Modifica di `[vm|vmss] disk attach` per impostare il LUN predefinito del disco sul primo valore disponibile

## <a name="september-21-2018"></a>21 settembre 2018

Versione 2.0.46

### <a name="acr"></a>ACR
* Aggiunta di comandi ACR Task
* Aggiunta del comando di esecuzione rapida
* Gruppo di comandi `build-task` deprecato
* Aggiunta del gruppo di comandi `helm` per il supporto della gestione dei grafici Helm con Registro Azure Container
* Aggiunta del supporto per la creazione idempotente per il registro gestito
* Aggiunta di un flag no format per la visualizzazione dei log di compilazione

### <a name="acs"></a>ACS
* Modifica del comando `install-connector` per l'impostazione del nome di dominio completo master di servizio Azure Kubernetes
* Correzione della creazione dell'assegnazione di ruolo per vnet-subnet-id quando non si specificano l'entità servizio e skip-role-assignement

### <a name="appservice"></a>AppService

* Aggiunta del supporto per la gestione di operazioni di processi Web (continui e attivati)
* az webapp config set supporta la proprietà --fts-state. Aggiunta del supporto per az functionapp config set e show
* Aggiunta del supporto per l'uso del proprio archivio per le app Web
* Aggiunta del supporto per ottenere l'elenco delle app Web eliminate e ripristinarle

### <a name="batch"></a>Batch
* Modifica dell'aggiunta di attività tramite `--json-file` per il supporto della sintassi AddTaskCollectionParameter
* Aggiornamento della documentazione dei formati `--json-file` accettati
* Aggiunta di `--max-tasks-per-node-option` a `batch pool create`
* Modifica del comportamento di `batch account` per la visualizzazione dell'account attualmente connesso se non sono specificate opzioni

### <a name="batch-ai"></a>Intelligenza artificiale per Batch 
* Correzione dell'errore di creazione automatica di account di archiviazione nel comando `batchai cluster create`

### <a name="cognitive-services"></a>Servizi cognitivi
* Aggiunta dello strumento di completamento per gli argomenti `--sku`, `--kind` e `--location`
* Aggiunta del comando `cognitiveservices account list-usage`
* Aggiunta del comando `cognitiveservices account list-kinds`
* Aggiunta del comando `cognitiveservices account list`
* `cognitiveservices list` deprecato
* Modifica di `--name` in facoltativo per `cognitiveservices account list-skus`

### <a name="container"></a>Contenitore
* Aggiunta della possibilità di riavviare e arrestare un gruppo di contenitori in esecuzione
* Aggiunta di `--network-profile` per il passaggio di un profilo di rete
* Aggiunta di `--subnet`, `--vnet_name`, per consentire la creazione di gruppi di contenitori in una rete virtuale
* Modifica dell'output della tabella per la visualizzazione dello stato del gruppo di contenitori

### <a name="datalake"></a>DataLake
* Aggiunta di comandi per le regole di rete virtuale

### <a name="interactive-shell"></a>Shell interattiva
* Correzione dell'errore in Windows per il quale i comandi non venivano eseguiti correttamente
* Risoluzione del problema di caricamento dei comandi in modalità interattiva provocato da oggetti deprecati

### <a name="iot"></a>IoT
* Aggiunta del supporto per il routing di hub IoT

### <a name="key-vault"></a>Key Vault
* Correzione dell'importazione di chiavi di Key Vault per le chiavi RSA

### <a name="network"></a>Rete
* Aggiunta dei comandi `network public-ip prefix` per il supporto di funzionalità di prefissi IP pubblici
* Aggiunta dei comandi `network service-endpoint` per il supporto di funzionalità di criteri degli endpoint di servizio
* Aggiunta dei comandi `network lb outbound-rule` per il supporto della creazione di regole in uscita per Load Balancer Standard
* Aggiunta di `--public-ip-prefix` a `network lb frontend-ip create/update` per il supporto delle configurazioni IP front-end con prefissi IP pubblici
* Aggiunta di `--enable-tcp-reset` a `network lb rule/inbound-nat-rule/inbound-nat-pool create/update`
* Aggiunta di `--disable-outbound-snat` a `network lb rule create/update`
* Possibilità di usare `network watcher flow-log show/configure` con gruppi di sicurezza di rete classici
* Aggiunta del comando `network watcher run-configuration-diagnostic`
* Correzione del comando `network watcher test-connectivity` e aggiunta delle proprietà `--method`, `--valid-status-codes` e `--headers`
* `network express-route create/update`: aggiunta del flag `--allow-global-reach`
* `network vnet subnet create/update`: aggiunta del supporto per `--delegation`
* Aggiunta del comando `network vnet subnet list-available-delegations`
* `network traffic-manager profile create/update`: aggiunta del supporto per `--interval`, `--timeout` e `--max-failures` per la configurazione di Monitoraggio. Opzioni `--monitor-path`, `--monitor-port` e `--monitor-protocol` deprecate a favore di `--path`, `--port`, `--protocol`
* `network lb frontend-ip create/update`: correzione della logica per l'impostazione del metodo di allocazione di indirizzi IP privati. Se viene specificato un indirizzo IP privato, l'assegnazione sarà statica. Se non viene specificato alcun indirizzo IP privato o se viene specificata una stringa vuota per l'indirizzo IP privato, l'allocazione sarà dinamica.
* `dns record-set * create/update`: aggiunta del supporto per `--target-resource`
* Aggiunta dei comandi `network interface-endpoint` per eseguire query sugli oggetti di endpoint dell'interfaccia
* Aggiunta di `network profile show/list/delete` per la gestione parziale dei profili di rete
* Aggiunta dei comandi `network express-route peering connection` per la gestione delle connessioni di peering tra route ExpressRoute

### <a name="rdbms"></a>RDBMS
* Aggiunta del supporto per il servizio MariaDB

### <a name="reservation"></a>Reservation
* Aggiunta di CosmosDb nel tipo enumerazione della risorsa riservata
* Aggiunta della proprietà name nel modello Patch

### <a name="manage-app"></a>Gestire l'app
* Correzione del bug in `managedapp create --kind MarketPlace` che provocava l'arresto anomalo della creazione dell'istanza di un'app gestita da Marketplace
* Modifica dei comandi `feature` per renderli limitati ai profili supportati

### <a name="role"></a>Ruolo
* Aggiunta del supporto per ottenere l'elenco delle appartenenze ai gruppi degli utenti

### <a name="signalr"></a>SignalR
* Prima versione

### <a name="storage"></a>Archiviazione
* Aggiunta del parametro `--auth-mode login` per l'uso delle credenziali di accesso dell'utente per l'autorizzazione per BLOB e code
* Aggiunta di `storage container immutability-policy/legal-hold` per la gestione dell'archiviazione non modificabile

### <a name="vm"></a>VM
* Correzione del problema per il quale `vm create --generate-ssh-keys` sovrascriveva il file della chiave privata se mancava il file della chiave pubblica (#4725, #6780)
* Aggiunta del supporto per la raccolta di immagini condivisa tramite `az sig`

## <a name="august-28-2018"></a>28 agosto 2018

Versione 2.0.45

### <a name="core"></a>Core

* Correzione di un problema relativo al caricamento di un file di configurazione vuoto
* Aggiunta del supporto per il profilo `2018-03-01-hybrid` per Azure Stack

### <a name="acr"></a>ACR

* Aggiunta di una soluzione alternativa per le operazioni del runtime senza richieste di Azure Resource Manager
* Modifica apportata per escludere per impostazione predefinita i file del controllo della versione (ad esempio, file con estensione git, gitignore) dal file TAR caricato nel comando `build`

### <a name="acs"></a>ACS

* Modifica di `aks create` per ripristinare le impostazioni predefinite per le VM `Standard_DS2_v2`
* Modifica di `aks get-credentials` per consentire ora la chiamata di nuove API per ottenere le credenziali per il cluster

### <a name="appservice"></a>AppService

* Aggiunta di supporto per CORS in functionapp e webapp
* Aggiunta di supporto per i tag di Azure Resource Manager nei comandi di creazione
* Modifica di `[webapp|functionapp] identity show` per generare il codice di uscita 3 in caso di risorsa mancante

### <a name="backup"></a>Backup

* Modifica di `backup vault backup-properties show` per generare il codice di uscita 3 in caso di risorsa mancante

### <a name="bot-service"></a>Servizio bot

* Versione iniziale dell'interfaccia della riga di comando del servizio Bot

### <a name="cognitive-services"></a>Servizi cognitivi

* Aggiunta del nuovo parametro `--api-properties,` obbligatorio per la creazione di alcuni servizi

### <a name="iot"></a>IoT

* Risoluzione di un problema relativo all'associazione degli hub collegati

### <a name="monitor"></a>Monitorare

* Aggiunta dei comandi `monitor metrics alert` per avvisi in tempo quasi reale relativi alle metriche
* Deprecamento dei comandi `monitor alert`

### <a name="network"></a>Rete

* Modifica di `network application-gateway ssl-policy predefined show` per generare il codice di uscita 3 in caso di risorsa mancante

### <a name="resource"></a>Risorsa

* Modifica di `provider operation show` per generare il codice di uscita 3 in caso di risorsa mancante

### <a name="storage"></a>Archiviazione

* Modifica di `storage share policy show` per generare il codice di uscita 3 in caso di risorsa mancante

### <a name="vm"></a>VM

* Modifica di `vm/vmss identity show` per generare il codice di uscita 3 in caso di risorsa mancante 
* Deprecamento di `--storage-caching` per `vm create`

## <a name="auguest-14-2018"></a>14 agosto 2018

Versione 2.0.44

### <a name="core"></a>Core

* Correzione della visualizzazione numerica nell'output di `table`
* Aggiunta del formato di output YAML

### <a name="telemetry"></a>Telemetria

* Miglioramento della creazione di report per telemetria

### <a name="acr"></a>ACR

* Aggiunta dei comandi `content-trust policy`
* Correzione del problema relativo alla gestione non corretta di `.dockerignore`

### <a name="acs"></a>ACS

* Modifica di `az acs/aks install-cli` per l'installazione in `%USERPROFILE%\.azure-kubectl` in Windows
* Modifica di `az aks install-connector` per rilevare se il cluster ha il controllo degli accessi in base al ruolo e per configurare correttamente il connettore ACI
* Passaggio all'assegnazione di ruolo nella subnet quando disponibile
* Aggiunta della nuova opzione per "ignorare l'assegnazione di ruolo" per la subnet quando disponibile                                 
* Passaggio all'opzione per ignorare l'assegnazione di ruolo per la subnet quando l'assegnazione esiste già                

### <a name="appservice"></a>AppService

* Correzione di un bug che impedisce la creazione di app per le funzioni tramite account di archiviazione in gruppi di risorse esterni
* Correzione di un arresto anomalo in caso di distribuzione di file ZIP

### <a name="batchai"></a>Batch per intelligenza artificiale

* Modifica dell'output del logger output per la creazione automatica di account di archiviazione in modo da specificare il "*gruppo* di risorse".        

### <a name="container"></a>Contenitore

* Aggiunta di `--secure-environment-variables` per il passaggio di variabili di ambiente sicure a un contenitore      

### <a name="iot"></a>IoT

* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Rimozione di comandi deprecati che sono stati spostati all'estensione IoT
* Aggiornamento di elementi in modo che non venga presupposto un dominio `azure-devices.net`

### <a name="iot-central"></a>Iot Central

* Versione iniziale del modulo di IoT Central

### <a name="keyvault"></a>Insieme di credenziali delle chiavi


* Aggiunta di comandi per la gestione degli account di archiviazione e delle definizioni delle firme di accesso condiviso
* Aggiunta di comandi per le regole di rete                                                           
* Aggiunta del parametro `--id` alle operazioni relative a segreti, chiavi e certificati
* Aggiunta di supporto per la versione per più API per la gestione di Key Vault
* Aggiunta di supporto per la versione per più API del piano dati di Key Vault

### <a name="relay"></a>Relay

* Versione iniziale

### <a name="sql"></a>Sql

* Aggiunta dei comandi `sql failover-group`

### <a name="storage"></a>Archiviazione

* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Modifica di `storage account show-usage` in modo da richiedere il parametro `--location` ed elencare gli elementi in base all'area
* Modifica del parametro `--resource-group` in modo che sia facoltativo per i comandi `storage account`
* Rimozione degli avvisi di tipo 'Precondizione non riuscita' per singoli errori in comandi batch per singoli messaggi aggregati
* Modifica dei comandi `[blob|file] delete-batch` in modo da non restituire più matrici di valori Null
* Modifica dei comandi `blob [download|upload|delete-batch]` in modo da leggere i token di firma di accesso condiviso dall'URL del contenitore

### <a name="vm"></a>VM

* Aggiunta di filtri comuni a `vm list-skus` per semplificare l'utilizzo

## <a name="july-31-2018"></a>31 luglio 2018

Versione 2.0.43

### <a name="acr"></a>ACR

* Aggiunta del flag `--with-secure-properties` al comando `acr build-task show`
* Aggiunta del comando `acr build-task update-build`

### <a name="acs"></a>ACS

* Modifica del valore restituito 0 (operazione riuscita) quando si termina `az aks browse` premendo [CTRL+C]

### <a name="batch"></a>Batch

* Correzione del bug relativo alla visualizzazione del token di AAD in cloudshell

### <a name="container"></a>Contenitore

* Rimozione del requisito per `--log-analytics-workspace-key` per il nome o l'ID in caso di sottoscrizione configurata

### <a name="network"></a>Rete

* Aggiunta del supporto DNS a 2017-03-09-profile per Azure Stack 

### <a name="resource"></a>Risorsa

* Aggiunta di `--rollback-on-error` e `group deployment create` per eseguire una distribuzione valida nota in caso di errore
* Correzione del problema relativo alla generazione di un errore in caso di `--parameters {}` con `group deployment create`

### <a name="role"></a>Ruolo

* Aggiunta del supporto per il profilo dello stack 2017-03-09-profile
* Correzione di un problema relativo al funzionamento non corretto dei parametri di aggiornamento generici per `app update`

### <a name="search"></a>Ricerca

* Aggiunta di comandi per il servizio Ricerca di Azure

### <a name="service-bus"></a>Bus di servizio

* Aggiunta del gruppo di comandi di migrazione per l'esecuzione della migrazione di uno spazio dei nomi dal bus di servizio Standard a Premium
* Aggiunta di nuove proprietà facoltative alla coda e alla sottoscrizione del bus di servizio
  *  `--enable-batched-operations` e `--enable-dead-lettering-on-message-expiration` in `queue`
  *  `--dead-letter-on-filter-exceptions` in `subscriptions`

### <a name="storage"></a>Archiviazione

* Aggiunta del supporto per il download di file di grandi dimensioni tramite una singola connessione
* Conversione dei comandi `show` non rilevati a causa di un errore con codice di uscita 3 dovuto a una risorsa mancante

### <a name="vm"></a>VM

* Aggiunta di supporto per elencare i set di disponibilità per sottoscrizione
* Aggiunta del supporto per `StandardSSD_LRS`
* Aggiunta di supporto per il gruppo di sicurezza dell'applicazione in caso di creazione di un set di scalabilità della VM
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Modifica di `[vm|vmss] create`, `[vm|vmss] identity assign` e `[vm|vmss] identity remove` per restituire le identità assegnate dall'utente in formato dizionario

## <a name="july-18-2018"></a>18 Luglio 2018

Versione 2.0.42

### <a name="core"></a>Core

* Aggiunta del supporto per l'accesso basato su browser nella finestra bash WSL
* Aggiunta del flag `--force-string` a tutti i comandi generici di aggiornamento
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Modifica dei comandi 'show' per la registrazione del messaggio di errore e l'uscita con codice di errore 3 in caso di risorsa mancante

### <a name="acr"></a>ACR

* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Modifica di '--no-push' in flag puro nel comando 'acr build'
* Aggiunta dei comandi `show` e `update` nel gruppo `acr repository`
* Aggiunta del flag `--detail` per `show-manifests` e `show-tags` per visualizzare informazioni più dettagliate
* Aggiunta del parametro `--image` per supportare l'ottenimento di dettagli della compilazione o log tramite un'immagine

### <a name="acs"></a>ACS

* Modifica di `az aks create` per generare un errore se `--max-pods` è minore di 5

### <a name="appservice"></a>AppService

* Aggiunta del supporto per gli SKU PremiumV2

### <a name="batch"></a>Batch

* Correzione del bug per l'uso delle credenziali del token in modalità Cloud Shell
* Modifica dell'input JSON per renderlo senza distinzione tra maiuscole e minuscole

### <a name="batch-ai"></a>Intelligenza artificiale per Batch

* Correzione del comando `az batchai job exec`

### <a name="container"></a>Contenitore

* Rimozione del requisito di nome utente e password per i registri non Dockerhub
* Correzione dell'errore durante la creazione di gruppi di contenitori da file YAML

### <a name="network"></a>Rete

* Aggiunta del supporto di `--no-wait` a `network nic [create|update|delete]` 
* Aggiunta di `network nic wait`
* L'argomento `--ids` è stato deprecato per `network vnet [subnet|peering] list`
* Aggiunta del flag `--include-default` per includere le regole di sicurezza predefinite nell'output di `network nsg rule list`  

### <a name="resource"></a>Risorsa

* Aggiunta del supporto di `--no-wait` a `group deployment delete`
* Aggiunta del supporto di `--no-wait` a `deployment delete`
* Aggiunta del comando `deployment wait`
* Correzione del problema a causa del quale i comandi `az deployment` a livello di sottoscrizione venivano visualizzati in modo errato per il profilo 2017-03-09-profile

### <a name="sql"></a>SQL

* Correzione dell'errore 'Il nome del gruppo di risorse... specificato non corrisponde al nome nell'URL' quando si specifica il nome del pool elastico per i comandi `sql db copy` e `sql db replica create`
* È possibile configurare il server SQL predefinito eseguendo `az configure --defaults sql-server=<name>`
* Implementazione di formattatori di tabella per i comandi `sql server`, `sql server firewall-rule`, `sql list-usages` e `sql show-usage`

### <a name="storage"></a>Archiviazione

* Aggiunta della proprietà `pageRanges` all'output `storage blob show` che verrà popolato per i BLOB di pagine

### <a name="vm"></a>VM

* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Modifica di `vmss create` per l'uso di `Standard_DS1_v2` come dimensione istanza predefinita
* Aggiunta del supporto per `--no-wait` a `vm extension [set|delete]` e `vmss extension [set|delete]`
* Aggiunta di `vm extension wait`

## <a name="july-3-2018"></a>3 luglio 2018

Versione 2.0.41

### <a name="aks"></a>Servizio Azure Kubernetes

* Modifica al monitoraggio per l'uso dell'ID sottoscrizione

## <a name="july-3-2018"></a>3 luglio 2018

Versione 2.0.40

### <a name="core"></a>Core

* Aggiunta di un nuovo flusso di codici di autorizzazione per l'accesso interattivo

### <a name="acr"></a>ACR

* Aggiunta del polling dello stato della compilazione
* Aggiunta del supporto per i valori di enumerazione che non applicano la distinzione tra maiuscole e minuscole
* Aggiunta dei parametri `--top` e `--orderby` per `show-manifests`

### <a name="acs"></a>ACS

* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Abilitazione del controllo degli accessi in base al ruolo di Kubernetes per impostazione predefinita
* Aggiunta dell'argomento `--disable-rbac` e deprecazione di `--enable-rbac` perché ora è l'impostazione predefinita
* Aggiornamento delle opzioni per il comando `aks browse`. Aggiunta del supporto per `--listen-port`
* Aggiornamento del pacchetto del grafico helm predefinito per il comando `aks install-connector`. Usare virtual-kubelet-for-aks-latest.tgz.
* Aggiunta dei comandi `aks enable-addons` e `aks disable-addons` per l'aggiornamento di un cluster esistente

### <a name="appservice"></a>AppService

* Aggiunta del supporto per la disabilitazione dell'identità tramite `webapp identity remove`
* Rimozione del tag `preview` per la funzionalità Identità

### <a name="backup"></a>Backup

* Aggiornamento della definizione del modulo

### <a name="batchai"></a>Batch per intelligenza artificiale

* Correzione dell'output della tabella per i comandi `batchai cluster node list` e `batchai job node list`

### <a name="cloud"></a>Cloud

* Aggiunta del suffisso del server `acr login` alla configurazione cloud

### <a name="container"></a>Contenitore

* Modifica di `container create` in modo che usi come impostazione predefinita un'operazione a esecuzione prolungata
* Aggiunta dei parametri `--log-analytics-workspace` e `--log-analytics-workspace-key` di Log Analytics
* Aggiunta del parametro `--protocol` per specificare il protocollo di rete da usare

### <a name="extension"></a>Estensione

* Modifica di `extension list-available` per visualizzare solo le estensioni compatibili con la versione dell'interfaccia della riga di comando

### <a name="network"></a>Rete

* Risoluzione del problema relativo all'applicazione della distinzione tra maiuscole e minuscole per i tipi di record ([#6602](https://github.com/Azure/azure-cli/issues/6602))

### <a name="rdbms"></a>Rdbms

* Aggiunta dei comandi `[postgres|myql] server vnet-rule`

### <a name="resource"></a>Risorsa

* Aggiunta di un nuovo gruppo di operazioni `deployment`

### <a name="vm"></a>VM

* Aggiunta del supporto per la rimozione dell'identità assegnata dal sistema

## <a name="june-25-2018"></a>25 giugno 2018

Versione 2.0.39

### <a name="cli"></a>CLI

* Aggiornamento del trimming del file nel programma di installazione MSI per risolvere un problema di installazione dell'estensione

## <a name="june-19-2018"></a>19 giugno 2018

Versione 2.0.38

### <a name="core"></a>Core

* Aggiunta di supporto globale per `--subscription` alla maggior parte dei comandi

### <a name="acr"></a>ACR

* Aggiunta di `azure-storage-blob` come dipendenza
* Modifica della configurazione della CPU predefinita con `acr build-task create` per l'uso di 2 core

### <a name="acs"></a>ACS

* Aggiornamento delle opzioni del comando `aks use-dev-spaces`. Aggiunta del supporto per `--update`
* Modifica di `aks get-credentials --admin` in modo da non sostituire il contenuto utente in `$HOME/.kube/config`
* Esposizione della proprietà `nodeResourceGroup` di sola lettura nei cluster gestiti
* Correzione dell'errore del comando `acs browse`
* Impostazione di `--connector-name` su facoltativo per `aks install-connector`, `aks upgrade-connector` e `aks remove-connector`
* Aggiunta di nuove aree di Istanza di contenitore di Azure per `aks install-connector`
* Aggiunta della posizione normalizzata nel nome della versione Helm e nel nome del nodo in `aks install-connector`

### <a name="appservice"></a>AppService

* Aggiunta di supporto per versioni più recenti di urllib
* Aggiunta di supporto per `functionapp create` per l'uso del piano appservice dai gruppi di risorse esterni

### <a name="batch"></a>Batch

* Rimozione della dipendenza `azure-batch-extensions`

### <a name="batch-ai"></a>Intelligenza artificiale per Batch

* Aggiunta del supporto per le aree di lavoro. Le aree di lavoro consentono di riunire cluster, file server ed esperimenti in gruppi, rimuovendo la limitazione relativa al numero di risorse che è possibile creare
* Aggiunta di supporto per gli esperimenti. Gli esperimenti consentono di raggruppare i processi in raccolte, rimuovendo la limitazione relativa al numero di processi creati.
* Aggiunta di supporto per la configurazione di `/dev/shm` per processi in esecuzione in un contenitore Docker
* Aggiunta dei comandi `batchai cluster node exec` e `batchai job node exec`. Questi comandi consentono di eseguire qualsiasi comando direttamente nei nodi e offrono funzionalità per l'inoltro alla porta.
* Aggiunta del supporto per `--ids` ai comandi `batchai`
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Tutti i cluster e i file server devono essere creati nelle aree di lavoro
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] I processi devono essere creati negli esperimenti
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Rimozione di `--nfs-resource-group` dai comandi `cluster create` e `job create`. Per montare un'unità NFS appartenente a un'area di lavoro o a un gruppo di risorse diverso, specificare l'ID di Azure Resource Manager del file server tramite l'opzione `--nfs`
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Rimozione di `--cluster-resource-group` dal comando `job create`. Per inviare un processo in un cluster appartenente a un'area di lavoro o a un gruppo di risorse diverso, specificare l'ID di Azure Resource Manager del cluster tramite l'opzione `--cluster`
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Rimozione dell'attributo `location` da processi, cluster e file server. La posizione è ora un attributo di un'area di lavoro.
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Rimozione di `--location` dai comandi `job create`, `cluster create` e `file-server create`
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Modifica dei nomi delle opzioni brevi per rendere più coerente l'interfaccia:
  - Ridenominazione di [`--config`, `-c`] in [`--config-file`, `-f`]
  - Ridenominazione di [`--cluster`, `-r`] in [`--cluster`, `-c`]
  - Ridenominazione di [`--cluster`, `-n`] in [`--cluster`, `-c`]
  - Ridenominazione di [`--job`, `-n`] in [`--job`, `-j`]

### <a name="maps"></a>Mappe

* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Modifica di `maps account create` per richiedere l'accettazione delle Condizioni per l'utilizzo del servizio tramite prompt interattivo o flag `--accept-tos`

### <a name="network"></a>Rete

* Aggiunta del supporto per `https` in `network lb probe create` [#6571](https://github.com/Azure/azure-cli/issues/6571)
* Correzione del problema relativo al rispetto delle maiuscole/minuscole per `--endpoint-status`. [#6502](https://github.com/Azure/azure-cli/issues/6502)

### <a name="reservations"></a>Prenotazioni

* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Aggiunta del parametro obbligatorio `ReservedResourceType` a `reservations catalog show`
* Aggiunta del parametro `Location` a `reservations catalog show`
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Rimozione di `kind` da `ReservationProperties`
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Ridenominazione di `capabilities` in `sku_properties` in `Catalog`
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Rimozione delle proprietà `size` e `tier` da `Catalog`
* Aggiunta del parametro `InstanceFlexibility` a `reservations reservation update`

### <a name="role"></a>Ruolo

* Miglioramento della gestione degli errori

### <a name="sql"></a>SQL

* Correzione del problema relativo all'esecuzione di `az sql db list-editions` per una località non disponibile per la sottoscrizione

### <a name="storage"></a>Archiviazione

* Modifica dell'output della tabella per `storage blob download` per facilitarne la lettura

### <a name="vm"></a>VM

* Miglioramento del controllo per l'ottimizzazione delle dimensioni delle VM per offrire il supporto per la rete accelerata in `vm create`
* Aggiunta di un avviso per `vmss create` relativo al passaggio delle dimensioni predefinite delle VM da `Standard_D1_v2` a `Standard_DS1_v2`
* Aggiunta di `--force-update` a `[vm|vmss] extension set` per aggiornare l'estensione anche se la configurazione non è stata modificata

## <a name="june-13-2018"></a>13 giugno 2018

Versione 2.0.37

### <a name="core"></a>Core

* Miglioramento della telemetria interattiva

## <a name="june-13-2018"></a>13 giugno 2018

Versione 2.0.36

### <a name="aks"></a>Servizio Azure Kubernetes

* Aggiunta di opzioni di rete avanzate ad `aks create`
* Aggiunta di argomenti ad `aks create` per abilitare il monitoraggio e il routing HTTP
* Aggiunta dell'argomento `--no-ssh-key` a `aks create`
* Aggiunta dell'argomento `--enable-rbac` a `aks create`
* [ANTEPRIMA] Aggiunta del supporto per l'autenticazione di Azure Active Directory ad `aks create`

### <a name="appservice"></a>AppService

* Risoluzione di un problema relativo a versioni di urllib incompatibili

## <a name="june-5-2018"></a>5 giugno 2018

Versione 2.0.35

### <a name="interactive"></a>Interattività

* Aggiunta di limiti alle dipendenze della modalità interattiva

## <a name="june-5-2018"></a>5 giugno 2018

Versione 2.0.34

### <a name="core"></a>Core

* Aggiunta del supporto per riferimenti alle risorse tra tenant
* Miglioramento dell'affidabilità nel caricamento dei dati di telemetria

### <a name="acr"></a>ACR

* Aggiunta del supporto per VSTS come posizione di origine remota
* Aggiunta del comando `acr import`

### <a name="aks"></a>Servizio Azure Kubernetes

* Modifica di `aks get-credentials` in modo da creare il file di configurazione kube con autorizzazioni a livello di file system più sicure

### <a name="batch"></a>Batch

* Correzione del bug nella formattazione della tabella dell'elenco di pool ([problema #4378](https://github.com/Azure/azure-cli/issues/4378))

### <a name="iot"></a>IoT

* Aggiunta del supporto per la creazione di hub IoT di livello Basic

### <a name="network"></a>Rete

* Miglioramento di `network vnet peering`

### <a name="policy-insights"></a>Policy Insights

* Versione iniziale

### <a name="arm"></a>ARM

* Aggiunta dei comandi `account management-group`

### <a name="sql"></a>SQL

* Aggiunta di nuovi comandi per istanze gestite:
  * `sql mi create`
  * `sql mi show`
  * `sql mi list`
  * `sql mi update`
  * `sql mi delete`
* Aggiunta di nuovi comandi per database gestiti:
  * `sql midb create`
  * `sql midb show`
  * `sql midb list`
  * `sql midb restore`
  * `sql midb delete`

### <a name="storage"></a>Archiviazione

* Aggiunta di tipi MIME supplementari per la deduzione di JSON e JavaScript dalle estensioni di file

### <a name="vm"></a>VM

* Modifica di `vm list-skus` per l'uso di colonne fisse e l'aggiunta di un avviso della rimozione di `Tier` e `Size`
* Aggiunta dell'opzione `--accelerated-networking` a `vm create`
* Aggiunta di `--tags` a `identity create`

## <a name="may-22-2018"></a>22 maggio 2018

Versione 2.0.33

### <a name="core"></a>Core

* Aggiunta del supporto per l'espansione di `@` nei nomi di file

### <a name="acs"></a>ACS

* Aggiunta dei nuovi comandi di Dev Spaces `aks use-dev-spaces` e `aks remove-dev-spaces`
* Correzione di un errore di digitazione in un messaggio della Guida

### <a name="appservice"></a>AppService

* Miglioramento dei comandi di aggiornamento generico
* Aggiunta del supporto asincrono per `webapp deployment source config-zip`

### <a name="container"></a>Contenitore

* Aggiunta del supporto per l'esportazione di un gruppo di contenitori in formato YAML
* Aggiunta del supporto per l'uso di un file YAML per creare/aggiornare un gruppo di contenitori

### <a name="extension"></a>Estensione

* Miglioramento della rimozione delle estensioni

### <a name="interactive"></a>Interattività

* Modifica della registrazione nel parser muto per i completamenti
* Miglioramento della gestione delle cache della Guida con errori

### <a name="keyvault"></a>Insieme di credenziali delle chiavi

* Correzione dei comandi per gli insiemi di credenziali delle chiavi per l'uso in Cloud Shell o in VM con un'identità

### <a name="network"></a>Rete

* Risoluzione del problema relativo al mancato funzionamento di `network watcher show-topology` con il nome di una rete virtuale e/o di una subnet ([#6326](https://github.com/Azure/azure-cli/issues/6326))
* Risoluzione del problema per cui alcuni comandi `network watcher` segnalano che Network Watcher non è abilitato per aree in cui invece lo è ([#6264](https://github.com/Azure/azure-cli/issues/6264))

### <a name="sql"></a>SQL

* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Modifica degli oggetti di risposta restituiti dai comandi `db` e `dw`:
    * Ridenominazione della proprietà `serviceLevelObjective` in `currentServiceObjectiveName`
    * Rimozione delle proprietà `currentServiceObjectiveId` e `requestedServiceObjectiveId`
    * Modifica della proprietà `maxSizeBytes` in valore intero anziché stringa
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Modifica delle proprietà di `db` e `dw` seguenti in proprietà di sola lettura:
    * `requestedServiceObjectiveName`.  Per eseguire l'aggiornamento, usare il parametro `--service-objective` oppure impostare la proprietà `sku.name`
    * `edition`. Per eseguire l'aggiornamento, usare il parametro `--edition` oppure impostare la proprietà `sku.tier`
    * `elasticPoolName`. Per eseguire l'aggiornamento, usare il parametro `--elastic-pool` oppure impostare la proprietà `elasticPoolId`
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Modifica delle proprietà di `elastic-pool` seguenti in proprietà di sola lettura:
    * `edition`. Per eseguire l'aggiornamento, usare il parametro `--edition`
    * `dtu`. Per eseguire l'aggiornamento, usare il parametro `--capacity`
    *  `databaseDtuMin`. Per eseguire l'aggiornamento, usare il parametro `--db-min-capacity`
    *  `databaseDtuMax`. Per eseguire l'aggiornamento, usare il parametro `--db-max-capacity`
* Aggiunta dei parametri `--family` e `--capacity` ai comandi `db`, `dw` ed `elastic-pool`
* Aggiunta di formattatori di tabella ai comandi `db`, `dw` ed `elastic-pool`

### <a name="storage"></a>Archiviazione

* Aggiunta dello strumento di completamento per l'argomento `--account-name`
* Risoluzione del problema relativo a `storage entity query`

### <a name="vm"></a>VM

* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Rimozione di `--write-accelerator` da `vm create`. Lo stesso supporto è accessibile tramite `vm update` o `vm disk attach`
* Risoluzione della corrispondenza delle immagini delle estensioni in `[vm|vmss] extension`
* Aggiunta di `--boot-diagnostics-storage` a `vm create` per l'acquisizione del log di avvio
* Aggiunta di `--license-type` a `[vm|vmss] update`

## <a name="may-7-2018"></a>7 maggio 2018

Versione 2.0.32

### <a name="core"></a>Core

* Correzione di un'eccezione non gestita durante il recupero di segreti da un account di entità servizio con un certificato
* Aggiunta del supporto limitato per argomenti posizionali
* Correzione del problema relativo all'impossibilità di usare `--query` con `--ids`. [#5591](https://github.com/Azure/azure-cli/issues/5591)
* Miglioramento degli scenari di piping da comandi in caso di uso di `--ids`. Supporta `-o tsv` con una query specificata o `-o json` senza query specificata
* Aggiunta di suggerimenti dei comandi in caso di errore se i comandi degli utenti includono errori di digitazione
* Miglioramento dell'errore in caso di digitazione di `az ''` da parte degli utenti
* Aggiunta del supporto per tipi di risorse personalizzati per moduli comandi ed estensioni

### <a name="acr"></a>ACR

* Aggiunta di comandi di compilazione di ACR
* Miglioramento dei messaggi di errore relativi a risorse non trovate
* Miglioramento delle prestazioni della creazione di risorse e della gestione degli errori
* Miglioramento dell'accesso ACR in console non standard e WSL
* Miglioramento dei messaggi di errore per i comandi del repository
* Aggiornamento delle colonne di tabella e dell'ordinamento

### <a name="acs"></a>ACS

* Aggiunta di un avviso relativo al fatto che `az aks` è un servizio in anteprima
* Risoluzione del problema relativo alle autorizzazioni in `aks install-connector` quando `--aci-resource-group` non è specificato

### <a name="ams"></a>AMS

* Versione iniziale - Gestire le risorse di Servizi multimediali di Azure

### <a name="appservice"></a>Servizio app

* Risoluzione di un bug in `webapp delete` quando viene specificato `--slot`
* Rimozione di `--runtime-version` da `webapp auth update`
* Aggiunta di supporto per la versione minima \_tls\_ e https2.0
* Aggiunta di supporto per multicontenitori

### <a name="batch-ai"></a>Intelligenza artificiale per Batch

* Modifica di `batchai create cluster` per rispettare la priorità di macchine virtuali configurata nel file di configurazione del cluster

### <a name="cognitive-services"></a>Servizi cognitivi

* Correzione di un errore di digitazione nell'esempio di `cognitiveservices account create` [#5603](https://github.com/Azure/azure-cli/issues/5603)

### <a name="consumption"></a>Consumo

* Aggiunta di nuovi comandi per l'API budget

### <a name="container"></a>Contenitore

* Rimozione del requisito per `--registry-server` per `container create` quando un server del registro è incluso nel nome dell'immagine

### <a name="cosmos-db"></a>Cosmos DB

* Introduzione del supporto della rete virtuale per l'interfaccia della riga di comando di Azure - Cosmos DB

### <a name="dms"></a>Servizio Migrazione del database

* Versione iniziale - Aggiunta di supporto per lo scenario di migrazione da SQL ad Azure SQL

### <a name="extension"></a>Estensione

* Risoluzione del bug relativo all'interruzione della visualizzazione dei metadati dell'estensione

### <a name="interactive"></a>Interattività

* Gli strumenti di completamento interattivi possono ora funzionare con gli argomenti posizionali
* Output più semplice da usare quando gli utenti digitano '\'
* Correzione dei completamenti per i parametri senza informazioni della guida
* Correzione delle descrizioni per i gruppi di comandi

### <a name="lab"></a>Lab

* Correzione delle regressioni dalla conversione knack

### <a name="network"></a>Rete

* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Rimozione del parametro `--ids` per:
  * `express-route auth list`
  * `express-route peering list`
  * `nic ip-config list`
  * `nsg rule list`
  * `route-filter rule list`
  * `route-table route list`
  * `traffic-manager endpoint list`

### <a name="profile"></a>Profilo

* Correzione del rilevamento dell'origine `disk create`
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Rimozione di `--msi-port` e `--identity-port` in quanto non più usati
* Correzione dell'errore di digitazione nel riepilogo breve di `account get-access-token`

### <a name="redis"></a>Redis

* `redis patch-schedule patch-schedule show` deprecato a favore di `redis patch-schedule show`
* `redis list-all` deprecato. Questa funzionalità è stata inclusa in `redis list`
* `redis import-method` deprecato a favore di `redis import`
* Aggiunta di supporto per `--ids` a vari comandi

### <a name="role"></a>Ruolo

* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Rimozione di `ad sp reset-credentials` deprecato

### <a name="storage"></a>Archiviazione

* Il valore firma di accesso condiviso-token di destinazione può essere ora applicato all'origine per la copia di BLOB se la firma di accesso condiviso di origine e la chiave dell'account non sono specificate
* Esposizione del valore --socket-timeout per caricamenti e download di BLOB
* I nomi di BLOB che iniziano con separatori del percorso vengono considerati percorsi relativi
* È ora possibile usare `storage blob copy --source-sas` con il carattere iniziale '?' per la query
* Correzione di `storage entity query --marker` in modo che venga accettato l'elenco di chiave=valori

### <a name="vm"></a>VM

* Correzione di una logica di rilevamento non valida nell'URI di BLOB non gestito
* Aggiunta del supporto della crittografia del disco senza entità servizio fornite dall'utente
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Non usare 'ManagedIdentityExtension' della macchina virtuale per il supporto per MSI
* Aggiunta di supporto per il criterio di rimozione a `vmss`
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Rimozione di `--ids` da:
  * `vm extension list`
  * `vm secret list`
  * `vm unmanaged-disk list`
  * `vmss nic list`
* Aggiunta del supporto per l'acceleratore di scrittura
* Aggiunta di `vmss perform-maintenance`
* Correzione di `vm diagnostics set` per rilevare in modo affidabile il tipo di sistema operativo della macchina virtuale
* Modifica di `vm resize` per verificare se le dimensioni richieste sono diverse rispetto a quanto impostato attualmente e per aggiornare solo in caso di modifica


## <a name="april-10-2018"></a>10 aprile 2018

Versione 2.0.31

### <a name="acr"></a>ACR

* Miglioramento della gestione degli errori del fallback wincred

### <a name="acs"></a>ACS

* Modifica dei nomi dell'entità servizio creati da servizio Azure Kubernetes in modo che siano validi per 5 anni

### <a name="appservice"></a>Servizio app

* [MODIFICA CHE CAUSA UN'INTERRUZIONE]: Removed `assign-identity`
* Correzione dell'eccezione non rilevata per piani di app Web inesistenti

### <a name="batchai"></a>Batch per intelligenza artificiale

* Aggiunta del supporto per API 2018-03-01

  - Montaggio a livello di processo
  - Variabili di ambiente con valori segreti
  - Impostazione dei contatori delle prestazioni
  - Segnalazione del segmento di percorso specifico del processo
  - Supporto per le sottocartelle nell'API di elenco file
  - Segnalazione di utilizzo e limiti
  - Possibilità di specificare il tipo di caching per i server NFS
  - Supporto per immagini personalizzate
  - Aggiunta del supporto per il toolkit pyTorch

* Aggiunta del comando `job wait`, che consente di attendere il completamento del processo e ne segnala il codice di uscita
* Aggiunta del comando `usage show` per visualizzare un elenco dell'utilizzo e dei limiti correnti delle risorse di Batch per intelligenza artificiale per le diverse aree
* Supporto di cloud nazionali
* Aggiunta di argomenti della riga di comando per i processi per montare file system a livello di processo oltre ai file di configurazione
* Aggiunta di altre opzioni per personalizzare i cluster, come priorità delle VM, subnet e numero di nodi iniziali per i cluster con scalabilità automatica, specificando un'immagine personalizzata
* Aggiunta di un'opzione della riga di comando per specificare il tipo di caching per il server NFS gestito da Batch per intelligenza artificiale
* Specifica semplificata per il montaggio del file system nei file di configurazione. È ora possibile omettere le credenziali per la condivisione file di Azure e i contenitori BLOB di Azure. L'interfaccia della riga di comando inserirà le credenziali usando la chiave dell'account di archiviazione specificata tramite i parametri della riga di comando o una variabile di ambiente oppure eseguirà una query sulla chiave in Archiviazione di Azure (se l'account di archiviazione appartiene alla sottoscrizione corrente)
* Completamento automatico del comando job file stream al termine del processo (riuscito, non riuscito, terminato o eliminato)
* Miglioramento dell'output `table` per le operazioni `show`
* Aggiunta dell'opzione `--use-auto-storage` per la creazione di cluster. Questa opzione semplifica la gestione degli account di archiviazione e il montaggio di una condivisione file di Azure e di contenitori BLOB di Azure nei cluster
* Aggiunta dell'opzione `--generate-ssh-keys` a `cluster create` e `file-server create`
* Aggiunta della possibilità di specificare l'attività di configurazione dei nodi tramite riga di comando
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Spostamento dei comandi `job stream-file` e `job list-files` nel gruppo `job file`
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Ridenominazione di `--admin-user-name` in `--user-name` nel comando `file-server create` per coerenza con il comando `cluster create`

### <a name="billing"></a>Fatturazione

* Aggiunta di comandi per l'account di registrazione

### <a name="consumption"></a>Consumo

* Aggiunta dei comandi `marketplace`
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Ridenominazione di `reservations summaries` in `reservation summary`
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Ridenominazione di `reservations details` in `reservation detail`
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Rimozione delle opzioni brevi `--reservation-order-id` e `--reservation-id` per i comandi `reservation`
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Rimozione delle opzioni brevi `--grain` per i comandi `reservation summary`
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Rimozione delle opzioni brevi `--include-meter-details` per i comandi `pricesheet`

### <a name="container"></a>Contenitore

* Aggiunta dei parametri di montaggio del volume del repository Git `--gitrepo-url`, `--gitrepo-dir`, `--gitrepo-revision` e `--gitrepo-mount-path`
* Risoluzione del problema relativo all'esito negativo di `az container exec` quando viene specificato --container-name ([#5926](https://github.com/Azure/azure-cli/issues/5926))

### <a name="extension"></a>Estensione

* Modifica del messaggio di controllo della distribuzione, che è ora a livello di debug

### <a name="interactive"></a>Interattività

* Modifica per interrompere i completamenti in presenza di comandi non riconosciuti
* Aggiunta di hook di eventi prima e dopo la creazione del sottoalbero del comando
* Aggiunta del completamento per i parametri `--ids`

### <a name="network"></a>Rete

* Risoluzione del problema relativo all'impossibilità di impostare i tag di `application-gateway create` ([#5936](https://github.com/Azure/azure-cli/issues/5936))
* Aggiunta dell'argomento `--auth-certs` per collegare i certificati di autenticazione per `application-gateway http-settings [create|update]` ([#4910](https://github.com/Azure/azure-cli/issues/4910))
* Aggiunta dei comandi `ddos-protection` per la creazione dei piani per la protezione DDoS
* Aggiunta del supporto per `--ddos-protection-plan` a `vnet [create|update]` per associare una rete virtuale a un piano per la protezione DDoS
* Risoluzione del problema relativo al flag `--disable-bgp-route-propagation` in `network route-table [create|update]`
* Rimozione degli argomenti fittizi `--public-ip-address-type` e `--subnet-type` per `network lb [create|update]`
* Aggiunta del supporto per record TXT con sequenze di escape RFC 1035 a `network dns zone [import|export]` e `network dns record-set txt add-record`

### <a name="profile"></a>Profilo

* Aggiunta del supporto per account di Azure classici in `account list`
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Rimozione degli argomenti `--msi` & `--msi-port`

### <a name="rdbms"></a>RDBMS

* Aggiunta del comando `georestore`
* Rimozione della limitazione relativa alle dimensioni di archiviazione dal comando `create`

### <a name="resource"></a>Risorsa

* Aggiunta del supporto per `--metadata` a `policy definition create`
* Aggiunta del supporto per `--metadata`, `--set`, `--add` e `--remove` a `policy definition update`

### <a name="sql"></a>SQL

* Aggiunta di `sql elastic-pool op list` e `sql elastic-pool op cancel`

### <a name="storage"></a>Archiviazione

* Miglioramento dei messaggi di errore per stringhe di connessione in formato non valido

### <a name="vm"></a>VM

* Aggiunta del supporto per la configurazione del numero di domini di errore della piattaforma a `vmss create`
* Modifica di `vmss create` in modo da usare per impostazione predefinita il servizio di bilanciamento del carico standard per un set di scalabilità di zona, di grandi dimensioni o con singolo gruppo di posizionamento disabilitato
* [MODIFICA CHE CAUSA UN'INTERRUZIONE]: Removed `vm assign-identity`, `vm remove-identity and `vm format-secret`
* Aggiunta del supporto per lo SKU dell'indirizzo IP pubblico a `vm create`
* Aggiunta degli argomenti `--keyvault` e `--resource-group` a `vm secret format` per supportare gli scenari in cui il comando non riesce a risolvere l'ID dell'insieme di credenziali ([#5718](https://github.com/Azure/azure-cli/issues/5718))
* Miglioramento degli errori per `[vm|vmss create]` se la località di un gruppo di risorse non offre il supporto delle zone


## <a name="march-27-2018"></a>27 marzo 2018

Versione 2.0.30

### <a name="core"></a>Core

* Visualizzazione del messaggio per le estensioni contrassegnate come anteprima nella Guida

### <a name="acs"></a>ACS

* Risoluzione dell'errore di verifica dei certificati SSL per `aks install-cli` in Cloud Shell

### <a name="appservice"></a>Servizio app

* Aggiunta del supporto solo per HTTPS a `webapp update`
* Aggiunta del supporto per gli slot a `az webapp identity [assign|show]` e `az functionapp identity [assign|show]`

### <a name="backup"></a>Backup

* Aggiunta del nuovo comando `az backup protection isenabled-for-vm`, che può essere usato per verificare se una VM è protetta tramite backup da un insieme di credenziali nella sottoscrizione
* Abilitazione degli ID oggetto di Azure per i parametri `--resource-group` e `--vault-name` per i comandi seguenti:
  * `backup container show`
  * `backup item set-policy`
  * `backup item show`
  * `backup job show`
  * `backup job stop`
  * `backup job wait`
  * `backup policy delete`
  * `backup policy get-default-for-vm`
  * `backup policy list-associated-items`
  * `backup policy set`
  * `backup policy show`
  * `backup protection backup-now`
  * `backup protection disable`
  * `backup protection enable-for-vm`
  * `backup recoverypoint show`
  * `backup restore files mount-rp`
  * `backup restore files unmount-rp`
  * `backup restore restore-disks`
  * `backup vault delete`
  * `backup vault show`
* Modifica dei parametri `--name` in modo da accettare il formato di output dei comandi `backup ... show`

### <a name="container"></a>Contenitore

* Aggiunta del comando `container exec`, che esegue i comandi in un contenitore per un gruppo di contenitori in esecuzione
* Supporto dell'output di tabella per la creazione e l'aggiornamento di un gruppo di contenitori

### <a name="extension"></a>Estensione

* Aggiunta del messaggio per `extension add` se l'estensione è in anteprima
* Modifica di `extension list-available` in modo da visualizzare i dati completi delle estensioni con `--show-details`
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Modifica di `extension list-available` in modo da visualizzare per impostazione predefinita i dati semplificati delle estensioni

### <a name="interactive"></a>Interattività

* Modifica dei completamenti in modo che vengano attivati non appena viene completato il caricamento della tabella dei comandi
* Correzione del bug relativo all'uso del parametro `--style`
* Creazione di un'istanza del lexer interattivo dopo il dump della tabella dei comandi, se mancante
* Miglioramento del supporto dello strumento di completamento

### <a name="lab"></a>Lab

* Correzione dei bug relativi al comando `create environment`

### <a name="monitor"></a>Monitorare

* Aggiunta del supporto per `--top`, `--orderby` e `--namespace` in `metrics list` [#5785](https://github.com/Azure/azure-cli/issues/5785)
* Correzione [#4529](https://github.com/Azure/azure-cli/issues/5785): `metrics list` accetta un elenco separato da spazi delle metriche da recuperare
* Aggiunta del supporto per `--namespace` in `metrics list-definitions` [#5785](https://github.com/Azure/azure-cli/issues/5785)

### <a name="network"></a>Rete

* Aggiunta del supporto per le zone DNS private

### <a name="profile"></a>Profilo

* Aggiunta dell'avviso per `--identity-port` e `--msi-port` a `login`

### <a name="rdbms"></a>RDBMS

* Aggiunta dell'API disponibile a livello generale per modelli aziendali, versione 2017-12-01

### <a name="resource"></a>Risorsa

* [MODIFICA CHE CAUSA UN'INTERRUZIONE]: Changed `provider operation [list|show]` to not require `--api-version`

### <a name="role"></a>Ruolo

* Aggiunta del supporto per le configurazioni di accesso e i client nativi necessari a `az ad app create`
* Modifica dei comandi `rbac` in modo che vengano restituiti meno di 1000 ID alla risoluzione degli oggetti
* Aggiunta dei comandi di gestione delle credenziali `ad sp credential [reset|list|delete]`
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Rimozione di "properties" dall'output di `az role assignment [list|show]`
* Aggiunta del supporto per le autorizzazioni `dataActions` e `notDataActions` a `role definition`

### <a name="storage"></a>Archiviazione

* Risoluzione del problema relativo al caricamento di file di dimensioni comprese tra 195 GB e 200 GB
* Correzione [#4049](https://github.com/Azure/azure-cli/issues/4049): risoluzione dei problemi relativi ai parametri di condizione ignorati nei caricamenti di BLOB di aggiunta

### <a name="vm"></a>VM

* Aggiunta a `vmss create` di un avviso relativo alle prossime modifiche di rilievo per i set con più di 100 istanze
* Aggiunta del supporto con resilienza della zona a `vm [snapshot|image]`
* Modifica della visualizzazione dell'istanza del disco per una migliore segnalazione dello stato di crittografia
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Modifica di `vm extension delete` in modo che non venga più restituito output

## <a name="march-13-2018"></a>13 marzo 2018

Versione 2.0.29

### <a name="acr"></a>ACR

* Aggiunta del supporto per il parametro `--image` a `repository delete`
* Parametri `--manifest` e `--tag` del comando `repository delete` deprecati
* Aggiunta del comando `repository untag` per rimuovere un tag senza eliminare i dati

### <a name="acs"></a>ACS

* Aggiunta del comando `aks upgrade-connector` per aggiornare un connettore esistente
* Modifica dei file di configurazione `kubectl` per usare un formato YAML di tipo blocco più leggibile

### <a name="advisor"></a>Advisor

* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Ridenominazione di `advisor configuration get` in `advisor configuration list`
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Ridenominazione di `advisor configuration set` in `advisor configuration update`
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Rimozione di `advisor recommendation generate`
* Aggiunta del parametro `--refresh` a `advisor recommendation list`
* Aggiunta del comando `advisor recommendation show`

### <a name="appservice"></a>Servizio app

* `[webapp|functionapp] assign-identity` deprecato
* Aggiunta dei comandi di identità gestita `webapp identity [assign|show]` e `functionapp identity [assign|show]`

### <a name="eventhubs"></a>Hub eventi

* Versione iniziale

### <a name="extension"></a>Estensione

* Aggiunta del controllo per avvisare l'utente se la distribuzione usata è diversa da quella archiviata nel file di origine del pacchetto, perché potrebbero verificarsi errori

### <a name="interactive"></a>Interattività

* Correzione [#5625](https://github.com/Azure/azure-cli/issues/5625): salvare in modo permanente la cronologia in sessioni diverse
* Correzione [#3016](https://github.com/Azure/azure-cli/issues/3016): cronologia non registrata durante l'ambito
* Correzione [#5688](https://github.com/Azure/azure-cli/issues/5688): i completamenti non sono stati visualizzati se il caricamento della tabella dei comandi ha rilevato un'eccezione
* Correzione del misuratore stato per le operazioni a esecuzione prolungata

### <a name="monitor"></a>Monitorare

* Comandi `monitor autoscale-settings` deprecati
* Aggiunta dei comandi `monitor autoscale`
* Aggiunta dei comandi `monitor autoscale profile`
* Aggiunta dei comandi `monitor autoscale rule`

### <a name="network"></a>Rete

* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Rimozione del parametro `--tags` da `route-filter rule create`
* Rimozione di alcuni valori predefiniti errati per i comandi seguenti:
  * `network express-route update`
  * `network nsg rule update`
  * `network public-ip update`
  * `traffic-manager profile update`
  * `network vnet-gateway update`
* Aggiunta dei comandi `network watcher connection-monitor`
* Aggiunta dei parametri `--vnet` e `--subnet` a`network watcher show-topology`

### <a name="profile"></a>Profilo

* Parametro `--msi` deprecato per `az login`
* Aggiunta del parametro `--identity` per `az login` in sostituzione di `--msi`

### <a name="rdbms"></a>RDBMS

* [ANTEPRIMA] Modifica per usare l'API 2017-12-01-preview

### <a name="service-bus"></a>Bus di servizio

* Versione iniziale

### <a name="storage"></a>Archiviazione

* Risoluzione del problema [#4971](https://github.com/Azure/azure-cli/issues/4971): `storage blob copy` ora supporta altri cloud di Azure
* Correzione [#5286](https://github.com/Azure/azure-cli/issues/5286): i comandi di Batch `storage blob [delete-batch|download-batch|upload-batch]` non generano più un errore in caso di condizione preliminare non riuscita

### <a name="vm"></a>VM

* Aggiunta del supporto a `[vm|vmss] create` per associare i dischi dati non gestiti e configurare la memorizzazione nella cache
* `[vm|vmss] assign-identity` e`[vm|vmss] remove-identity` deprecati
* Aggiunta dei comandi `vm identity [assign|remove|show]` e `vmss identity [assign|remove|show]` per sostituire i comandi deprecati
* Impostazione della priorità predefinita in `vmss create` su None

## <a name="february-27-2018"></a>27 febbraio 2018

Versione 2.0.28

### <a name="core"></a>Core

* Correzione [#5184](https://github.com/Azure/azure-cli/issues/5184): risoluzione del problema relativo all'installazione con Homebrew
* Aggiunta del supporto per la telemetria delle estensioni con chiavi personalizzate
* Aggiunta della registrazione HTTP a `--debug`

### <a name="acs"></a>ACS

* Modifica per usare il grafico Helm `virtual-kubelet-for-aks` per `aks install-connector` per impostazione predefinita
* Risoluzione del problema relativo alle autorizzazioni insufficienti per le entità servizio per la creazione di un gruppo di contenitori ACI
* Aggiunta dei parametri `--aci-container-group`, `--location` e `--image-tag` a `aks install-connector`
* Rimozione dell'avviso di funzionalità deprecata da `aks get-versions`

### <a name="appservice"></a>Servizio app

* Aggiornamenti per la nuova versione dell'SDK (azure-mgmt-web 0.35.0)
* Risoluzione del problema relativo alla segnalazione di `Free` come SKU non valido ([#5538](https://github.com/Azure/azure-cli/issues/5538))

### <a name="cognitive-services"></a>Servizi cognitivi

* Aggiornamento della "notifica" durante la creazione di un nuovo account Servizi cognitivi

### <a name="consumption"></a>Consumo

* Aggiunta di nuovi comandi per l'API elenco prezzi
* Aggiornamento dei formati esistenti per i dettagli di utilizzo e delle prenotazioni

### <a name="container"></a>Contenitore

* Aggiunta degli argomenti `--secrets` e `--secrets-mount-path` a `container create` per l'uso di segreti in ACI

### <a name="network"></a>Rete

* Correzione [#5559](https://github.com/Azure/azure-cli/issues/5559): client mancante in `network vnet-gateway vpn-client generate`

### <a name="resource"></a>Risorsa

* Modifica di `group deployment export` per visualizzare un modello parziale ed errori in caso di esito negativo

### <a name="role"></a>Ruolo

* Aggiunta di `role assignment list-changelogs` per consentire il controllo dei ruoli delle entità servizio

### <a name="sql"></a>SQL

* Aggiunta del supporto per la ridondanza di zona per database e pool elastici al momento della creazione e dell'aggiornamento

### <a name="storage"></a>Archiviazione

* Abilitazione della specifica di destination-path/prefix per `storage blob [upload-batch|download-batch]`

### <a name="vm"></a>VM

* Aggiunta del supporto per il collegamento/scollegamento di dischi in una singola istanza di un set di scalabilità di macchine virtuali


## <a name="february-13-2018"></a>13 febbraio 2018

Versione 2.0.27

### <a name="core"></a>Core

* Modifica dell'autenticazione in chiave sia per l'ID sottoscrizione che per il nome nell'accesso con MSI

### <a name="acs"></a>ACS

* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Ridenominazione di `aks get-versions` in `aks get-upgrades` per maggiore accuratezza
* Modifica di `aks get-versions` per visualizzare le versioni di Kubernetes disponibili per `aks create`
* Modifica delle impostazioni predefinite di `aks create` per consentire al server di scegliere la versione di Kubernetes
* Aggiornamento dei messaggi della Guida relativi all'entità servizio generata da servizio Azure Kubernetes
* Modifica delle dimensioni predefinite dei nodi per `aks create` da "Standard\_D1\_v2" a "Standard\_DS1\_v2"
* Miglioramento dell'affidabilità nell'individuazione del pod del dashboard per `az aks browse`
* Correzione di `aks get-credentials` per gestire gli errori Unicode durante il caricamento dei file di configurazione di Kubernetes
* Aggiunta di un messaggio a `az aks install-cli` per recuperare `kubectl` in `$PATH`

### <a name="appservice"></a>Servizio app

* Risoluzione del problema che determinava l'esito negativo di `webapp [backup|restore]` a causa di un riferimento Null
* Aggiunta del supporto per piani di servizio app predefiniti tramite `az configure --defaults appserviceplan=my-asp`

### <a name="cdn"></a>RETE CDN

* Aggiunta dei comandi `cdn custom-domain [enable-https|disable-https]`

### <a name="container"></a>Contenitore

* Aggiunta dell'opzione `--follow` a `az container logs` per i log in streaming
* Aggiunta del comando `container attach` che collega i flussi di errore e di output standard locali a un contenitore in un gruppo di contenitori

### <a name="cosmosdb"></a>Cosmos DB

* Aggiunta di supporto per l'impostazione di funzionalità

### <a name="extension"></a>Estensione

* Aggiunta del supporto per il parametro `--pip-proxy` ai comandi `az extension [add|update]`
* Aggiunta del supporto per l'argomento `--pip-extra-index-urls` ai comandi `az extension [add|update]`

### <a name="feedback"></a>Commenti e suggerimenti

* Aggiunta di informazioni sulle estensioni ai dati di telemetria

### <a name="interactive"></a>Interattività

* Risoluzione del problema per cui all'utente viene richiesto di eseguire l'accesso durante l'uso della modalità interattiva in Cloud Shell
* Correzione della regressione con completamento dei parametri mancanti

### <a name="iot"></a>IoT

* Risoluzione del problema relativo alla restituzione di un errore di elemento non trovato da parte di `iot dps access policy [create|update]` in caso di esito positivo
* Risoluzione del problema relativo alla restituzione di un errore di elemento non trovato da parte di `iot dps linked-hub [create|update]` in caso di esito positivo
* Aggiunta del supporto per `--no-wait` a `iot dps access policy [create|update]` e `iot dps linked-hub [create|update]`
* Modifica di `iot hub create` per consentire la specifica del numero di partizioni

### <a name="monitor"></a>Monitorare

* Correzione del comando `az monitor log-profiles create`

### <a name="network"></a>Rete

* Correzione dell'opzione `--tags` per i comandi seguenti:
  * `network public-ip create`
  * `network lb create`
  * `network local-gateway create`
  * `network nic create`
  * `network vnet-gateway create`
  * `network vpn-connection create`

### <a name="profile"></a>Profilo

* Abilitazione di `az login` dalla modalità interattiva

### <a name="resource"></a>Risorsa

* Riaggiunta di `feature show`

### <a name="role"></a>Ruolo

* Aggiunta dell'argomento `--available-to-other-tenants` a `ad app update`

### <a name="sql"></a>SQL

* Aggiunta dei comandi `sql server dns-alias`
* Aggiunta di `sql db rename`
* Aggiunta del supporto per l'argomento `--ids` a tutti comandi SQL

### <a name="storage"></a>Archiviazione

* Aggiunta dei comandi `storage blob service-properties delete-policy` e `storage blob undelete` per consentire l'eliminazione temporanea

### <a name="vm"></a>VM

* Risoluzione del problema relativo all'arresto anomalo quando la crittografia delle VM potrebbe non essere completamente inizializzata
* Aggiunta dell'output dell'ID entità di sicurezza all'abilitazione di MSI
* `vm boot-diagnostics get-boot-log` fisso


## <a name="january-31-2018"></a>31 gennaio 2018

Versione 2.0.26

### <a name="core"></a>Core

* Aggiunta del supporto per il recupero di token non elaborati nel contesto di MSI
* Rimozione della stringa indicatore di polling dopo il completamento di operazioni a esecuzione prolungata in cmd.exe di Windows
* Modifica dell'avviso visualizzato in caso di uso di un'impostazione predefinita configurata in una voce di livello INFO, visualizzabile usando `--verbose`
* Aggiunta di un indicatore di stato per i comandi wait

### <a name="acs"></a>ACS

* Chiarimento dell'argomento `--disable-browser`
* Miglioramento del completamento tramite tasto TAB per gli argomenti `--vm-size`

### <a name="appservice"></a>Servizio app

* `webapp log [tail|download]` fisso
* Rimozione del controllo di `kind` per app Web e funzioni

### <a name="cdn"></a>RETE CDN

* Risoluzione del problema relativo al client mancante con `cdn custom-domain create`

### <a name="cosmosdb"></a>Cosmos DB

* Correzione della descrizione dei parametri per i criteri di failover

### <a name="interactive"></a>Interattività

* Risoluzione del problema per cui non venivano più visualizzati i completamenti delle opzioni dei comandi

### <a name="network"></a>Rete

* Aggiunta della protezione per `--cert-password` a `application-gateway create`
* Risoluzione del problema relativo a `application-gateway update` per cui con `--sku` veniva applicato erroneamente un valore predefinito
* Aggiunta della protezione per `--shared-key` e `--authorization-key` a `vpn-connection create`
* Risoluzione del problema relativo al client mancante con `asg create`
* Aggiunta del parametro `--file-name / -f` per i nomi esportati a `dns zone export`
* Risoluzione dei problemi seguenti relativi a `dns zone export`:
  * Risoluzione del problema relativo all'esportazione errata dei record TXT lunghi
  * Risoluzione del problema per cui i record TXT tra virgolette venivano erroneamente esportati senza virgolette precedute da un carattere di escape
* Risoluzione del problema per cui alcuni record venivano importati due volte con `dns zone import`
* Ripristino dei comandi `vnet-gateway root-cert` e `vnet-gateway revoked-cert`

### <a name="profile"></a>Profilo

* Correzione di `get-access-token` per l'uso in una VM con un'identità

### <a name="resource"></a>Risorsa

* Correzione del bug relativo a `deployment [create|validate]` che determinava l'errata visualizzazione di un avviso quando il campo "type" di un modello conteneva valori con lettere maiuscole

### <a name="storage"></a>Archiviazione

* Risoluzione del problema relativo alla migrazione degli account di archiviazione dalla versione 1 alla versione 2
* Aggiunta di report di stato per tutti i comandi di caricamento/download
* Correzione del bug che impediva l'uso dell'opzione di argomento "-n" con `storage account check-name`
* Aggiunta della colonna "snapshot" all'output di tabella per `blob [list|show]`
* Correzione dei bug relativi a vari parametri che dovevano essere analizzati come valori int

### <a name="vm"></a>VM

* Aggiunta del comando `vm image accept-terms` per consentire la creazione di VM da immagini con addebiti aggiuntivi
* Correzione di `[vm|vmss create]` per garantire la possibilità di eseguire comandi nell'ambito del proxy con certificati non firmati
* [ANTEPRIMA] Aggiunta del supporto per la priorità "bassa" ai set di scalabilità di macchine virtuali
* Aggiunta della protezione per `--admin-password` a `[vm|vmss] create`


## <a name="january-17-2018"></a>17 gennaio 2018

Versione 2.0.25

### <a name="acr"></a>ACR

* Aggiunta del fallback di accesso al Registro Azure Container per gli errori delle credenziali di Windows
* Abilitazione dei log di registro

### <a name="acs"></a>ACS

* Correzione del comando `get-credentials`
* Rimozione del requisito del ruolo SPN

### <a name="appservice"></a>Servizio app

* Risoluzione del bug con `config ssl upload` in cui `hosting_environment_profile` era Null
* Aggiunta del supporto per URL personalizzati a `browse`
* Correzione del supporto slot per `log tail`

### <a name="backup"></a>Backup

* Modifica dell'opzione `--container-name` di `backup item list` in opzione facoltativa
* Aggiunta di opzioni di account di archiviazione a `backup restore restore-disks`
* Correzione della verifica della posizione in `backup protection enable-for-vm` affinché non faccia distinzione tra maiuscole e minuscole
* Risoluzione del problema relativo all'esito negativo dei comandi con un nome contenitore non valido
* Modifica di `backup item list` per includere 'Stato integrità' per impostazione predefinita

### <a name="batch"></a>Batch

* Modifica di `batch login` per la restituzione dei dettagli di autenticazione

### <a name="cloud"></a>Cloud

* Apportata modifica per non richiedere endpoint quando si imposta `--profile` in un cloud

### <a name="consumption"></a>Consumo

* Aggiunta di nuovi comandi per le prenotazioni: `consumption reservations summaries` e `consumption reservations details`

### <a name="event-grid"></a>Griglia di eventi

* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Spostamento dei comandi `az eventgrid topic event-subscription` in `eventgrid event-subscription`
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Spostamento dei comandi `az eventgrid resource event-subscription` in `eventgrid event-subscription`
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Rimozione del comando `eventgrid event-subscription show-endpoint-url` e sostituzione con `eventgrid event-subscription show --include-full-endpoint-url`
* Aggiunta del comando `eventgrid topic update`
* Aggiunta del comando `eventgrid event-subscription update`
* Aggiunta del parametro `--ids` per i comandi `eventgrid topic`
* Aggiunta del supporto del completamento con tasto TAB per i nomi di argomento

### <a name="interactive"></a>Interattività

* Risoluzione del problema relativo al mancato funzionamento della modalità interattiva con Python 2.x
* Correzione degli errori all'avvio
* Risoluzione del problema con alcuni comandi non eseguiti in modalità interattiva

### <a name="iot"></a>IoT

* Aggiunta del supporto per il servizio di provisioning di dispositivi
* Aggiunta di messaggi di funzionalità deprecata nei comandi e nella guida per i comandi
* Aggiunta della verifica IoT per informare gli utenti dell'estensione IoT

### <a name="monitor"></a>Monitorare

* Aggiunta del supporto di impostazioni di multidiagnostica. Il parametro `--name` è ora necessario per `az monitor diagnostic-settings create`
* Aggiunta del comando `monitor diagnostic-settings categories` per ottenere la categoria delle impostazioni di diagnostica

### <a name="network"></a>Rete

* Risoluzione del problema che si verificava nel tentativo di passare dalla modalità attiva alla modalità standby e viceversa con `vnet-gateway update`
* Aggiunta del supporto per HTTP2 a `application-gateway [create|update]`

### <a name="profile"></a>Profilo

* Aggiunta del supporto per l'accesso con identità assegnate dall'utente

### <a name="role"></a>Ruolo

* Aggiunta dell'argomento `--assignee-object-id` a `role assignment create` per ignorare la query per grafi

### <a name="service-fabric"></a>Service Fabric

* Aggiunta di errori dettagliati alla risposta di convalida durante la creazione di un cluster
* Risoluzione del problema relativo al client mancante con diversi comandi

### <a name="vm"></a>VM

* [ANTEPRIMA] Supporto del bilanciamento del carico tra zone per `vmss`
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Modifica del valore predefinito di `vmss` per zona singola in bilanciamento del carico "Standard"
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Modifica di `externalIdentities` in `userAssignedIdentities` per EMSI
* [ANTEPRIMA] Aggiunta del supporto per lo scambio di dischi del sistema operativo
* Aggiunta del supporto per l'uso di immagini di macchina virtuale di altre sottoscrizioni
* Aggiunta degli argomenti `--plan-name`, `--plan-product`, `--plan-promotion-code` e `--plan-publisher` a `[vm|vmss] create`
* Risoluzione degli errori con `[vm|vmss] create`
* Risoluzione dell'uso eccessivo di risorse provocato da `vm image list --all`

## <a name="december-19-2017"></a>19 dicembre 2017

Versione 2.0.23

* Aggiunta del supporto per l'accesso con identità assegnate dall'utente

### <a name="container"></a>Contenitore

* Correzione dell'ordine errato dei parametri per i log dei contenitori

### <a name="network"></a>Rete

* Aggiunta dell'argomento `--disable-bgp-route-propagation` a `route-table [create|update]`
* Aggiunta dell'argomento `--ip-tags` a `public-ip [create|update]`

### <a name="storage"></a>Archiviazione

* Aggiunta del supporto per le risorse di archiviazione V2

### <a name="vm"></a>VM

* [ANTEPRIMA] Aggiunta del supporto per le identità assegnate dall'utente per macchine virtuali e VMSS


## <a name="december-5-2017"></a>5 dicembre 2017

Versione 2.0.22

* Rimozione dei comandi `az component` e sostituzione con `az extension`

### <a name="core"></a>Core
* Modifica dell'endpoint dell'autorità AAD `AZURE_US_GOV_CLOUD` da login.microsoftonline.com a login.microsoftonline.us
* Risoluzione del problema relativo al continuo reinvio della telemetria

### <a name="acs"></a>ACS

* Aggiunta dei comandi `aks install-connector` e `aks remove-connector`
* Miglioramento della segnalazione di errori per `acs create`
* Risoluzione del problema relativo all'utilizzo di `aks get-credentials -f` senza percorso completo

### <a name="advisor"></a>Advisor

* Versione iniziale

### <a name="appservice"></a>Servizio app

* Risoluzione del problema relativo alla generazione del nome del certificato con `webapp config ssl upload`
* Risoluzione del problema relativo alla visualizzazione delle app corrette con `webapp [list|show]` e `functionapp [list|show]`
* Aggiunta del valore predefinito per `WEBSITE_NODE_DEFAULT_VERSION`

### <a name="consumption"></a>Consumo

* Aggiunta del supporto per l'API versione 2017-11-30

### <a name="container"></a>Contenitore

* Risoluzione del problema relativo alla regressione delle porte predefinite

### <a name="monitor"></a>Monitorare

* Aggiunta del supporto di più dimensioni al comando per le metriche

### <a name="resource"></a>Risorsa

* Aggiunta dell'argomento `--include-response-body` a `resource show`

### <a name="role"></a>Ruolo

* Aggiunta della visualizzazione delle assegnazioni predefinite per gli amministratori "classici" a `role assignment list`
* Aggiunta ad `ad sp reset-credentials` del supporto per l'aggiunta anziché la sovrascrittura delle credenziali
* Miglioramento della segnalazione di errori per `ad sp create-for-rbac`

### <a name="sql"></a>SQL

* Aggiunta dei comandi `sql db list-usages` e `sql db show-usage`
* Aggiunta dei comandi `sql server conn-policy show` e `sql server conn-policy update`

### <a name="vm"></a>VM

* Aggiunta delle informazioni sulla zona ad `az vm list-skus`


## <a name="november-14-2017"></a>14 novembre 2017

Versione 2.0.21

### <a name="acr"></a>ACR

* Aggiunta del supporto per la creazione di webhook nelle aree di replica


### <a name="acs"></a>ACS

* Sostituzione del termine "agente" con "nodo" nel servizio Azure Container
* Deprecazione dell'opzione `--orchestrator-release` per `acs create`
* Modifica in `Standard_D1_v2` delle dimensioni di VM predefinite per il servizio Azure Container
* Correzione di `az aks browse` in Windows
* Correzione di `az aks get-credentials` in Windows

### <a name="appservice"></a>Servizio app

* Aggiunta dell'origine di distribuzione `config-zip` per le app Web e le app per le funzioni
* Aggiunta dell'opzione `--docker-container-logging` a `az webapp log config`
* Rimozione dell'opzione `storage` dal parametro `--web-server-logging` di `az webapp log config`
* Miglioramento dei messaggi di errore per `deployment user set`
* Aggiunta del supporto per la creazione di app per le funzioni Linux
* `list-locations` fisso

### <a name="batch"></a>Batch

* Correzione di un bug nel comando di creazione di pool in caso di uso di un ID risorsa con il flag `--image`

### <a name="batchai"></a>Batch AI

* Aggiunta dell'opzione breve `-s` per `--vm-size` per specificare le dimensioni di VM nel comando `file-server create`
* Aggiunta degli argomenti del nome e della chiave dell'account di archiviazione ai parametri di `cluster create`
* Correzione della documentazione per `job list-files` e `job stream-file`
* Aggiunta dell'opzione breve `-r` per `--cluster-name` per specificare il nome del cluster nel comando `job create`

### <a name="cloud"></a>Cloud

* Modifica di `cloud [register|update]` per impedire la registrazione di cloud privi di endpoint obbligatori

### <a name="container"></a>Contenitore

* Aggiunta del supporto per l'apertura di più porte
* Aggiunta di criteri di avvio per i gruppi di contenitori
* Aggiunta del supporto per il montaggio di una condivisione file di Azure come volume
* Aggiornamento della documentazione relativa agli helper

### <a name="data-lake-analytics"></a>Data Lake Analytics

* Modifica di `[job|account] list` per la restituzione di informazioni più concise

### <a name="data-lake-store"></a>Data Lake Store

* Modifica di `account list` per la restituzione di informazioni più concise

### <a name="extension"></a>Estensione

* Aggiunta di `extension list-available` per consentire la visualizzazione di un elenco delle estensioni Microsoft ufficiali
* Aggiunta di `--name` a `extension [add|update]` per consentire l'installazione di estensioni in base al nome

### <a name="iot"></a>IoT

* Aggiunta del supporto per autorità di certificazione (CA) e catene di certificati

### <a name="monitor"></a>Monitorare

* Aggiunta dei comandi `activity-log alert`

### <a name="network"></a>Rete

* Aggiunta del supporto per record DNS CAA
* Risoluzione del problema relativo all'impossibilità di aggiornare gli endpoint con `traffic-manager profile update`
* Risoluzione del problema relativo al mancato funzionamento di `vnet update --dns-servers` a seconda della modalità di creazione della rete virtuale
* Risoluzione del problema relativo all'importazione non corretta dei nomi DNS relativi da parte di `dns zone import`

### <a name="reservations"></a>Prenotazioni

* Versione di anteprima iniziale

### <a name="resource"></a>Risorsa

* Aggiunta del supporto per ID risorsa al parametro `--resource` e ai blocchi a livello di risorsa

### <a name="sql"></a>SQL

* Aggiunta del parametro `--ignore-missing-vnet-service-endpoint` a `sql server vnet-rule [create|update]`

### <a name="storage"></a>Archiviazione

* Modifica di `storage account create` per usare lo SKU `Standard_RAGRS` come impostazione predefinita
* Correzione dei bug relativi alla gestione di nomi file/BLOB contenenti caratteri non ASCII
* Correzione del bug che impediva di usare `--source-uri` con `storage [blob|file] copy start-batch`
* Aggiunta di comandi per l'uso di caratteri jolly e l'eliminazione di più oggetti `storage [blob|file] delete-batch`
* Risoluzione del problema relativo all'abilitazione delle metriche con `storage metrics update`
* Risoluzione del problema relativo ai file di dimensioni superiori a 200 GB quando si usa `storage blob upload-batch`
* Risoluzione del problema per cui `--bypass` e `--default-action` venivano ignorati da `storage account [create|update]`

### <a name="vm"></a>VM

* Correzione di un bug relativo a `vmss create` che impediva l'uso del livello di dimensione `Basic`
* Aggiunta degli argomenti `--plan` a `[vm|vmss] create` per immagini personalizzate con informazioni di fatturazione
* Aggiunta dei comandi `vm secret `[add|remove|list]`
* Rinominato `vm format-secret` in `vm secret format`
* Aggiunta dell'argomento `--encrypt format` a `vm encryption enable`

## <a name="october-24-2017"></a>24 ottobre 2017

Versione 2.0.20

### <a name="core"></a>Core

* Aggiornamento di `2017-03-09-profile` per l'utilizzo dell'API `MGMT_STORAGE` versione `2016-01-01`

### <a name="acr"></a>ACR

* Aggiornamento della gestione delle risorse in modo da fare riferimento alla versione `2017-10-01` dell'API
* Modifica di 'Bring Your Own Storage' SKU nella versione classica
* Ridenominazione degli SKU del registro su Basic, Standard e Premium

### <a name="acs"></a>ACS

* [ANTEPRIMA] Aggiunta dei comandi `az aks`
* Correzione di `get-credentials` di Kubernetes

### <a name="appservice"></a>Servizio app

* Correzione del problema relativo ai log di `webapp` non validi

### <a name="component"></a>Componente

* Aggiunta di un messaggio di deprecazione più chiaro per tutti i programmi di installazione e di una richiesta di conferma

### <a name="monitor"></a>Monitorare

* Aggiunta dei comandi `action-group`

### <a name="resource"></a>Risorsa

* Risoluzione dell'incompatibilità con la versione più recente della dipendenza msrest in `group export`
* Correzione di `policy assignment create` per consentire il funzionamento con le definizioni dei criteri predefinite e con le definizioni dei set di criteri

### <a name="vm"></a>VM

* Aggiunta dell'argomento `--accelerated-networking` a `vmss create`


## <a name="october-9-2017"></a>9 ottobre 2017

Versione 2.0.19

### <a name="core"></a>Core

* Aggiunta della gestione di URL dell'autorità ADFS con una barra finale ad Azure Stack

### <a name="appservice"></a>Servizio app

* Aggiunta di un aggiornamento generico con il nuovo comando `webapp update`

### <a name="batch"></a>Batch

* Aggiornamento a Batch SDK 4.0.0
* Aggiornamento dell'opzione `--image` di VirtualMachineConfiguration per il supporto di riferimenti a immagini ARM in aggiunta a publish:offer:sku:version
* Aggiunta del supporto per il nuovo modello di estensione dell'interfaccia della riga di comando per i comandi di Estensione Batch
* Rimozione del supporto di Batch dal modello di componente

### <a name="batchai"></a>Batch AI

* Rilascio iniziale del modulo Batch AI

### <a name="keyvault"></a>Key Vault

* Risoluzione di un problema di autenticazione dell'insieme di credenziali delle chiavi quando si usa ADFS in Azure Stack. [(#4448)](https://github.com/Azure/azure-cli/issues/4448)

### <a name="network"></a>Rete

* Modifica dell'argomento `--server` di `application-gateway address-pool create` in argomento facoltativo, consentendo pool di indirizzi vuoti
* Aggiornamento di `traffic-manager` per il supporto delle funzionalità più recenti

### <a name="resource"></a>Risorsa

* Aggiunta del supporto per le opzioni `--resource-group/-g` per il nome del gruppo di risorse a `group`
* Aggiunta di comandi per far sì che `account lock` funzioni con i blocchi a livello di sottoscrizione
* Aggiunta di comandi per far sì che `group lock` funzioni con i blocchi a livello di gruppo
* Aggiunta di comandi per far sì che `resource lock` funzioni con i blocchi a livello di risorsa

### <a name="sql"></a>Sql

* Aggiunta del supporto per SQL Transparent Data Encryption (TDE) e Transparent Data Encryption con Bring Your Own Key
* Aggiunta del comando `db list-deleted` e del parametro `db restore --deleted-time` per trovare e ripristinare i database eliminati
* Aggiunta di `db op list` e `db op cancel` per elencare e annullare le operazioni in corso nel database

### <a name="storage"></a>Archiviazione

* Aggiunta del supporto per snapshot di condivisione file

### <a name="vm"></a>Vm

* Correzione di un bug in `vm show` in cui l'uso di `-d` provocava un arresto anomalo in caso di indirizzi IP privati mancanti
* [ANTEPRIMA] Aggiunta del supporto per l'aggiornamento in sequenza a `vmss create`
* Aggiunta del supporto per l'aggiornamento delle impostazioni di crittografia con `vm encryption enable`
* Aggiunta del parametro `--os-disk-size-gb` a `vm create`
* Aggiunta del parametro `--license-type` per Windows a `vmss create`


## <a name="september-22-2017"></a>22 settembre 2017

Versione 2.0.18

### <a name="resource"></a>Risorsa

* Aggiunta del supporto per la visualizzazione delle definizioni di criteri predefinite
* Aggiunta del supporto del parametro mode per la creazione delle definizioni di criteri
* Aggiunta del supporto per modelli e definizioni dell'interfaccia utente a `managedapp definition create`
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Modifica del tipo di risorsa di `managedapp` da `appliances` ad `applications` e da `applianceDefinitions` ad `applicationDefinitions`

### <a name="network"></a>Rete

* Aggiunta del supporto per la zona di disponibilità ai sottocomandi `network lb` e `network public-ip`
* Aggiunta del supporto per il peering Microsoft IPv6 a `express-route`
* Aggiunta dei comandi `asg` per i gruppi di sicurezza delle applicazioni
* Aggiunta dell'argomento `--application-security-groups` a `nic [create|ip-config create|ip-config update]`
* Aggiunta degli argomenti `--source-asgs` e `--destination-asgs` a `nsg rule [create|update]`
* Aggiunta degli argomenti `--ddos-protection` e `--vm-protection` a `vnet [create|update]`
* Aggiunta dei comandi `network [vnet-gateway|vpn-client|show-url]`

### <a name="storage"></a>Archiviazione

* Risoluzione del problema relativo all'esito negativo dei comandi `storage account network-rule` dopo l'aggiornamento dell'SDK

### <a name="eventgrid"></a>Eventgrid

* Aggiornamento di Python SDK per Griglia di eventi di Azure per l'uso della versione più recente dell'API, "2017-09-15-preview"

### <a name="sql"></a>SQL

* Modifica in facoltativo dell'argomento `--resource-group` di `sql server list`. Se non viene specificato, verranno restituite tutte le istanze di SQL Server nella sottoscrizione
* Aggiunta del parametro `--no-wait` a `db [create|copy|restore|update|replica create|create|update]` e `dw [create|update]`

### <a name="keyvault"></a>Key Vault

* Aggiunta del supporto per i comandi keyvault in presenza di un proxy

### <a name="vm"></a>VM

* Aggiunta del supporto per la zona di disponibilità a `[vm|vmss|disk] create`
* Risoluzione del problema relativo alla generazione di un errore quando si usa `--app-gateway ID` con `vmss create`
* Aggiunta dell'argomento `--asgs` a `vm create`
* Aggiunta del supporto per l'esecuzione di comandi sulle VM con `vm run-command`
* [ANTEPRIMA] Aggiunta del supporto per la crittografia dei dischi di set di scalabilità di macchine virtuali con `vmss encryption`
* Aggiunta del supporto per l'esecuzione della manutenzione sulle VM con `vm perform-maintenance`

### <a name="acs"></a>ACS

* [ANTEPRIMA] Aggiunta dell'argomento `--orchestrator-release` a `acs create` per le aree di anteprima del servizio contenitore di Azure

### <a name="appservice"></a>Servizio app

* Aggiunta della possibilità di aggiornare e visualizzare le impostazioni di autenticazione con `webapp auth [update|show]`

### <a name="backup"></a>Backup

* Versione di anteprima


## <a name="september-11-2017"></a>11 settembre 2017

Versione 2.0.17

### <a name="core"></a>Core

* Abilitazione del modulo di comandi per impostare l'ID correlazione nei dati di telemetria
* Risoluzione del problema di dump JSON quando la telemetria viene impostata sulla modalità diagnostica

### <a name="acs"></a>ACS

* Aggiunta del comando `acs list-locations`
* Impostazione di `ssh-key-file` con il valore predefinito previsto

### <a name="appservice"></a>Servizio app

* Possibilità di creare un'app Web in un gruppo di risorse diverso da quello del piano di servizio attivo

### <a name="cdn"></a>RETE CDN

* Correzione del bug "CustomDomain is not interable" per `cdn custom-domain create`

### <a name="extension"></a>Estensione

* Versione iniziale

### <a name="keyvault"></a>Key Vault

* Risoluzione del problema relativo alla distinzione tra maiuscole e minuscole nelle autorizzazioni per `keyvault set-policy`

### <a name="network"></a>Rete

* Rinominato `vnet list-private-access-services` in `vnet list-endpoint-services`
* Ridenominazione dell'argomento `--private-access-services` in `--service-endpoints` per `vnet subnet create/update`
* Aggiunta del supporto per più intervalli IP e intervalli di porte in `nsg rule create/update`
* Aggiunta del supporto per SKU a `lb create`
* Aggiunta del supporto per SKU a `public-ip create`

### <a name="resource"></a>Risorsa

* È stato consentito il passaggio delle definizioni dei parametri dei criteri delle risorse in `policy definition create` e `policy definition update`
* È stato consentito il passaggio dei valori dei parametri per `policy assignment create`
* È stato consentito il passaggio di JSON o del file per tutti i parametri
* Incremento della versione API

### <a name="sql"></a>SQL

* Aggiunta dei comandi `sql server vnet-rule`

### <a name="vm"></a>VM

* Correzione: l'accesso non viene assegnato se `--scope` non è specificato
* Correzione: uso della stessa denominazione di estensione usata dal portale
* Rimozione di `subscription` dall'output `[vm|vmss] create`
* Correzione: lo SKU di archiviazione `[vm|vmss] create` non viene applicato nei dischi dati con un'immagine
* Correzione: `vm format-secret --secrets` non accetta ID separati di nuova riga

## <a name="august-31-2017"></a>31 agosto 2017

Versione 2.0.16

### <a name="keyvault"></a>Key Vault

* Risoluzione del bug relativo al tentativo di risolvere automaticamente la codifica di un segreto con `secret download`

### <a name="sf"></a>SF

* Tutti i comandi deprecati a favore dell'interfaccia della riga di comando di Service Fabric (sfctl)

### <a name="storage"></a>Archiviazione

* Risoluzione del problema relativo all'impossibilità di creare account di archiviazione in aree che non supportano la funzionalità NetworkACLs
* Determinazione del tipo di contenuto e della codifica del contenuto durante il caricamento di BLOB e file se non vengono specificati né il tipo di contenuto né la codifica del contenuto

## <a name="august-28-2017"></a>28 agosto 2017

Versione 2.0.15

### <a name="cli"></a>CLI

* Aggiunta di una nota legale a `--version`

### <a name="acs"></a>ACS

* Correzione delle aree di anteprima
* Formattazione corretta del valore `dns_name_prefix` predefinito
* Ottimizzazione dell'output dei comandi acs

### <a name="appservice"></a>Servizio app

* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Correzione delle incoerenze nell'output di `az webapp config appsettings [delete|set]`
* Aggiunta di un nuovo alias di `-i` per `az webapp config container set --docker-custom-image-name`
* Esposizione di `az webapp log show`
* Esposizione di nuovi argomenti da `az webapp delete` per mantenere il piano di servizio app, le metriche o la registrazione DNS
* Correzione: rilevamento corretto delle impostazioni dello slot

### <a name="iot"></a>IoT

* Correzione #3934: la creazione di criteri non cancella più i criteri esistenti

### <a name="network"></a>Rete

* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Ridenominazione di `vnet list-private-access-services` in `vnet list-endpoint-services`
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Ridenominazione dell'opzione `--private-access-services` in `--service-endpoints` per `vnet subnet [create|update]`
* Aggiunta del supporto per più indirizzi IP e intervalli di porte in `nsg rule [create|update]`
* Aggiunta del supporto per SKU a `lb create`
* Aggiunta del supporto per SKU a `public-ip create`

### <a name="profile"></a>Profilo

* Esposizione di `--msi` e `--msi-port` per l'accesso tramite l'identità della macchina virtuale

### <a name="service-fabric"></a>Service Fabric

* Versione di anteprima
* Semplificazione delle regole relative a utente/password del Registro di sistema per il comando
* Correzione del problema relativo alla richiesta della password per l'utente anche dopo il passaggio del parametro
* Aggiunta del supporto per il valore `registry_cred` vuoto

### <a name="storage"></a>Archiviazione

* Abilitazione della configurazione del livello BLOB
* Aggiunta degli argomenti `--bypass` e `--default-action` a `storage account [create|update]` per supportare il tunneling del servizio
* Aggiunta di comandi per aggiungere regole della rete virtuale e regole basate su indirizzi IP a `storage account network-rule`
* Abilitazione della crittografia del servizio tramite chiave gestita dal cliente
* [MODIFICA CHE CAUSA UN'INTERRUZIONE] Ridenominazione dell'opzione `--encryption` in `--encryption-services` per il comando `az storage account create and az storage account update`
* Correzione #4220: `az storage account update encryption`: Mancata corrispondenza della sintassi

### <a name="vm"></a>VM

* Risoluzione del problema relativo alla visualizzazione di informazioni aggiuntive errate per `vmss get-instance-view` quando si usa `--instance-id *`
* Aggiunta del supporto per `--lb-sku` a `vmss create`:
* Rimozione dei nomi di persone dall'elenco di nomi non consentiti dell'amministratore per `[vm|vmss] create`
* Risoluzione del problema relativo alla generazione di un errore da parte di `[vm|vmss] create` in caso di impossibilità di estrazione delle informazioni del piano da un'immagine
* Risoluzione del problema relativo all'arresto anomalo in caso di creazione di un set di scalabilità vmms con un servizio di bilanciamento del carico interno
* Risoluzione del problema relativo al mancato funzionamento dell'argomento `--no-wait` con `vm availability-set create`


## <a name="august-15-2017"></a>15 agosto 2017

Versione 2.0.14

### <a name="acs"></a>ACS

* Correzione del numero di porta sshMaster0 per kubernetes

### <a name="appservice"></a>Servizio app

* Correzione dell'eccezione generata durante la creazione di una nuova app Web Linux basata su Git

### <a name="event-grid"></a>Griglia di eventi

* Aggiunta di dipendenze di SDK

## <a name="august-11-2017"></a>11 agosto 2017

Versione 2.0.13

### <a name="acs"></a>ACS

* Aggiunta di altre aree di anteprima

### <a name="batch"></a>Batch

* Aggiornamento a Batch SDK 3.1.0 e Batch Management SDK 4.1.0
* Aggiunta di un nuovo comando per visualizzare il numero di attività di un processo
* Correzione di un bug nell'elaborazione dell'URL della firma di accesso condiviso del file di risorse
* L'endpoint dell'account Batch supporta ora il prefisso facoltativo 'https://'
* Supporto per l'aggiunta di elenchi di più di 100 attività a un processo
* Aggiunta della registrazione di debug per il caricamento del modulo di comandi delle estensioni

### <a name="component"></a>Componente

* Aggiunta dell'avviso relativo a elementi deprecati ai comandi 'az component'

### <a name="container"></a>Contenitore

* `create`: risoluzione del problema relativo all'impossibilità di usare il segno di uguale all'interno di una variabile di ambiente


### <a name="data-lake-store"></a>Data Lake Store

* Abilitazione del controllo dell'avanzamento

### <a name="event-grid"></a>Griglia di eventi

* Versione iniziale

### <a name="network"></a>Rete

* `lb`: correzione del problema relativo alla risoluzione non corretta di determinati nomi di risorse figlio in caso di omissione
* `application-gateway {subresource} delete`: correzione del problema relativo al mancato rispetto di `--no-wait`
* `application-gateway http-settings update`: risoluzione del problema relativo all'impossibilità di disattivare `--connection-draining-timeout`
* Risoluzione del problema relativo all'argomento imprevisto `sa_data_size_kilobyes` della parola chiave con `az network vpn-connection ipsec-policy add`

### <a name="profile"></a>Profilo

* `account list`: aggiunta di `--refresh` per la sincronizzazione delle sottoscrizioni più recenti dal server

### <a name="storage"></a>Archiviazione

* Abilitazione dell'aggiornamento dell'account di archiviazione con l'identità assegnata dal sistema

### <a name="vm"></a>VM

* `availability-set`: esposizione del numero di domini di errore in fase di conversione
* Esposizione del comando `list-skus`
* Supporto per l'assegnazione di identità senza la creazione di assegnazioni di ruolo
* Applicazione di SKU di archiviazione in fase di collegamento del dischi dati
* Rimozione del nome del disco del sistema operativo predefinito e dello SKU di archiviazione in caso di uso di Managed Disks


## <a name="july-28-2017"></a>28 luglio 2017

Versione 2.0.12

* Aggiunta di comandi del contenitore
* Aggiunta di moduli relativi a fatturazione e consumo

```text
azure-cli (2.0.12)

acr (2.0.9)
acs (2.0.11)
appservice (0.1.11)
batch (3.0.3)
billing (0.1.3)
cdn (0.0.6)
cloud (2.0.7)
cognitiveservices (0.1.6)
command-modules-nspkg (2.0.1)
component (2.0.6)
configure (2.0.10)
consumption (0.1.3)
container (0.1.7)
core (2.0.12)
cosmosdb (0.1.11)
dla (0.0.10)
dls (0.0.11)
feedback (2.0.6)
find (0.2.6)
interactive (0.3.7)
iot (0.1.10)
keyvault (2.0.8)
lab (0.0.9)
monitor (0.0.8)
network (2.0.11)
nspkg (3.0.1)
profile (2.0.9)
rdbms (0.0.5)
redis (0.2.7)
resource (2.0.11)
role (2.0.9)
sf (1.0.5)
sql (2.0.8)
storage (2.0.11)
vm (2.0.11)
```

### <a name="core"></a>Core

* Restituzione di informazioni di autorizzazioni dell'SDK per entità servizio con certificati
* Risoluzione del problema relativo alle eccezioni nell'avanzamento delle distribuzioni
* Uso dell'endpoint ARM dal cloud corrente per la creazione del client di sottoscrizione
* Miglioramento della gestione simultanea del file clouds.config (#3636)
* Aggiornamento dell'ID della richiesta client per ogni esecuzione del comando
* Creazione dei client di sottoscrizione con il profilo SDK corretto (#3635)
* Creazione di report relativi all'avanzamento per le distribuzioni dei modelli (#3510)
* Aggiunta del supporto per la selezione di campi di output della tabella tramite una query jmespath (#3581)
* Miglioramento della disattivazione dell'audio degli argomenti di analisi e dell'aggiunta di cronologia con gesti (#3434)
* Creazione di client di sottoscrizione con il profilo SDK corretto
* Spostamento di tutti i file di registrazione esistenti nella cartella più recente
* Risoluzione del problema relativo all'idempotenza per la creazione di VM/VMSS (#3586)
* I percorsi dei comandi non rispettano più la distinzione tra maiuscole/minuscole
* Alcuni parametri di tipo booleano non rispettano più la distinzione tra maiuscole/minuscole
* Supporto dell'accesso ad AD FS nel server locale come Azure Stack
* Risoluzione del problema relativo alle operazioni di scrittura simultanee nel file clouds.config (#3255)

### <a name="acr"></a>ACR

* Aggiunta del comando `show-usage` per i registri gestiti
* Supporto dell'aggiornamento dello SKU per i registri gestiti
* Aggiunta dei registri gestiti con SKU gestito
* Aggiunta di webhook per registri gestiti con modulo di comandi webhook ACR
* Aggiunta dell'autenticazione AAD con il comando di accesso di ACR
* Aggiunta del comando di eliminazione per repository Docker, manifesti e tag

### <a name="acs"></a>ACS

* Supporto per l'API versione 2017-07-01

### <a name="appservice"></a>Servizio app

* Risoluzione del bug relativo alla mancata restituzione di valori per l'elenco di app Web Linux
* Supporto per il recupero di credenziali da ACR
* Rimozione di tutti i comandi in `appservice web`
* Mascheramento delle password del registro Docker dall'output del comando (#3656)
* Conferma dell'uso del browser predefinito in macOS senza errori (#3623)
* Miglioramento della Guida di `webapp log tail` e `webapp log download` (#3624)
* Esposizione del comando `traffic-routing` per la configurazione del routing statico (#3566)
* Aggiunta di correzioni relative all'affidabilità nella configurazione del controllo del codice sorgente (#3245)
* Rimozione dell'argomento `--node-version` non supportato da `webapp config update` per le app Web Windows. Viene usato `webapp config appsettings set --settings WEBSITE_NODE_DEFAULT_VERSION=...`

### <a name="batch"></a>Batch

* Aggiornamento a Batch SDK 3.0.0 con supporto per le macchine virtuali a priorità bassa nei pool
* Ridenominazione per `pool create` dell'opzione `--target-dedicated` in `--target-dedicated-nodes`
* Aggiunta in `pool create` delle opzioni `--target-low-priority-nodes` e `--application-licenses`

### <a name="cdn"></a>RETE CDN

* Miglioramento del messaggio di errore per `cdn endpoint list` nel caso in cui il profilo specificato da `--profile-name` non esista

### <a name="cloud"></a>Cloud

* Modifica della versione dell'API dell'endpoint dei metadati del cloud nel formato AAAA-MM-GG
* L'endpoint della raccolta non è obbligatorio
* Supporto per la registrazione del cloud solo con l'endpoint di gestione delle risorse ARM
* Aggiunta di un'opzione per `cloud set` per la scelta del profilo durante la selezione del cloud corrente
* Esposizione di `endpoint_vm_image_alias_doc`

### <a name="cosmosdb"></a>Cosmos DB

* Correzione dell'errore che consente la creazione di una raccolta con una chiave di partizione personalizzata
* Aggiunta di supporto per la durata TTL predefinita della raccolta

### <a name="data-lake-analytics"></a>Data Lake Analytics

* Aggiunta di comandi per la gestione dei criteri di calcolo sotto l'intestazione `dla account compute-policy`
* Aggiunta di `dla job pipeline show`
* Aggiunta di `dla job recurrence list`

### <a name="data-lake-store"></a>Data Lake Store

* Aggiunta di supporto per la rotazione delle chiavi dell'insieme di credenziali delle chiavi gestito dall'utente in `dls account update`
* Aggiornamento della versione dell'SDK del file system del Data Lake Store sottostante per risolvere un problema di prestazioni
* Aggiunta del comando `dls enable-key-vault`. Questo comando prova ad abilitare un'istanza di Key Vault fornita dall'utente per l'uso per la crittografia dei dati in un account Data Lake Store

### <a name="interactive"></a>Interattività

* Miglioramento del tempo di avvio tramite i comandi memorizzati nella cache
* Miglioramento della copertura dei test
* Miglioramento del gesto '?' per consentire anche l'inserimento nel comando successivo
* Risoluzione degli errori relativi all'interattività con il profilo 2017-03-09-profile-preview (#3587)
* Autorizzazione di `--version` come parametro per la modalità interattiva (#3645)
* Interruzione della generazione di errori da parte della modalità interattiva relativi al completamento delle convalide (#3570)
* Creazione di report relativi all'avanzamento per le distribuzioni dei modelli (#3510)
* Aggiunta del flag `--progress`
* Rimozione di `--debug` e `--verbose` dai completamenti
* Rimozione di `interactive` dai completamenti (#3324)

### <a name="iot"></a>IoT

* Correzione del problema relativo alla creazione di criteri, che non cancella più i criteri esistenti. (#3934)

### <a name="key-vault"></a>Insieme di credenziali delle chiavi

* Aggiunta di comandi per le funzionalità di ripristino di Key Vault:
  * Sottocomandi di `keyvault`: `purge`, `recover`, `keyvault list-deleted`
  * Sottocomandi di `keyvault secret`: `backup`, `restore`, `purge`, `recover`, `list-deleted`
  * Sottocomandi di `keyvault certificate`: `purge`, `recover`, `list-deleted`
  * Sottocomandi di `keyvault key`: `purge`, `recover`, `list-deleted`
* Aggiunta dell'integrazione dell'insieme di credenziali delle chiavi dell'entità servizio (#3133)
* Aggiornamento del piano dati dell'insieme di credenziali delle chiavi a 0.3.2. (#3307)

### <a name="lab"></a>Lab

* Aggiunta del supporto per la rivendicazione di eventuali macchine virtuali nel laboratorio tramite `az lab vm claim`
* Aggiunta del formattatore dell'output della tabella per `az lab vm list` e `az lab vm show`

### <a name="monitor"></a>Monitorare

* Correzione relativa al file del modello con il comando `monitor autoscale-settings get-parameters-template` (#3349)
* Rinominato `monitor alert-rule-incidents list` in `monitor alert list-incidents`
* Rinominato `monitor alert-rule-incidents show` in `monitor alert show-incident`
* Rinominato `monitor metric-defintions list` in `monitor metrics list-definitions`
* Rinominato `monitor alert-rules` in `monitor alert`
* Modifica di `monitor alert create`:
  * I sottocomandi `condition` e `action` non accettano più JSON
  * Aggiunta di numerosi parametri per la semplificazione del processo di creazione delle regole
  * `location` non è più obbligatorio
  * Aggiunta del supporto di nome ID per la destinazione
  * Rimozione di `--alert-rule-resource-name`
  * Ridenominazione di `is-enabled` in `enabled`, non più obbligatorio
  * I valori predefiniti di `description` sono ora basati sulla condizione specificata
  *  Aggiunta di esempi per illustrare in modo più chiaro il nuovo formato
* Supporto di nomi o ID per i comandi `monitor metric`
* Aggiunta di argomenti ed esempi utili a `monitor alert rule update`

### <a name="network"></a>Rete

* Aggiunta del comando `list-private-access-services`
* Aggiunta dell'argomento `--private-access-services` a `vnet subnet create` e `vnet subnet update`
* Risoluzione del problema relativo all'esito negativo di `application-gateway redirect-config create`
* Risoluzione del problema relativo al mancato funzionamento di `application-gateway redirect-config update` con `--no-wait`
* Risoluzione del bug relativo all'uso dell'argomento `--servers` con `application-gateway address-pool create` e `application-gateway address-pool update`
* Aggiunta dei comandi `application-gateway redirect-config`
* Aggiunta di comandi ad `application-gateway ssl-policy`: `list-options`, `predefined list`, `predefined show`
* Aggiunta di comandi ad `application-gateway ssl-policy set`: `--name`, `--cipher-suites`, `--min-protocol-version`
* Aggiunta di argomenti ad `application-gateway http-settings create` e `application-gateway http-settings update`: `--host-name-from-backend-pool`, `--affinity-cookie-name`, `--enable-probe`, `--path`
* Aggiunta di argomenti ad `application-gateway url-path-map create` e `application-gateway url-path-map update`: `--default-redirect-config`, `--redirect-config`
* Aggiunta dell'argomento `--redirect-config` a `application-gateway url-path-map rule create`
* Aggiunta del supporto per `--no-wait` a `application-gateway url-path-map rule delete`
* Aggiunta di argomenti ad `application-gateway probe create` e `application-gateway probe update`: `--host-name-from-http-settings`, `--min-servers`, `--match-body`, `--match-status-codes`
* Aggiunta dell'argomento `--redirect-config` a `application-gateway rule create` e `application-gateway rule update`
* Aggiunta del supporto per `--accelerated-networking` a `nic create` e `nic update`
* Rimozione dell'argomento `--internal-dns-name-suffix` da `nic create`
* Aggiunta del supporto per `--dns-servers` a `nic update` e `nic create`: Aggiunta del supporto per --dns-servers
* Risoluzione del bug relativo al fatto che `local-gateway create` ignora `--local-address-prefixes`
* Aggiunta del supporto per `--dns-servers` a `vnet update`
* Risoluzione del bug relativo alla creazione di un peering senza filtro delle route con `express-route peering create`
* Risoluzione del bug relativo al mancato funzionamento di `--provider` e `--bandwidth` con `express-route update`
* Risoluzione del bug relativo alla logica delle impostazioni predefinite di `network watcher show-topology`
* Miglioramento della formattazione dell'output per `network list-usages`
* Uso dell'indirizzo IP del front-end predefinito per `application-gateway http-listener create` se ne esiste solo uno
* Uso del pool di indirizzi, delle impostazioni HTTP e del listener HTTP predefiniti per `application-gateway rule create` se ne esiste solo uno
* Uso dell'indirizzo IP del front-end e del pool back-end predefiniti per `lb rule create` se ne esiste solo uno
* Uso dell'indirizzo IP del front-end predefinito per `lb inbound-nat-rule create` se ne esiste solo uno

### <a name="profile"></a>Profilo

* Supporto dell'accesso all'interno di una VM con un'identità gestita
* Supporto dell'output per `account show` nel formato di file di autorizzazione di SDK
* Visualizzazione degli avvisi relativi a elementi deprecati quando si usa '--expanded-view'
* Aggiunta del comando `get-access-token` per fornire il token AAD non elaborato
* Supporto dell'accesso con un account utente senza sottoscrizioni associate

### <a name="rdbms"></a>RDBMS

* Supporto della creazione di elenchi di server in una sottoscrizione (#3417)
* Risoluzione del problema relativo alla mancata elaborazione di `%s` a causa dell'assenza del valore `% server_type` (#3393)
* Risoluzione del problema relativo alla mappa di origine del documento e aggiunta di un'attività di integrazione continua per la verifica (#3361)
* Risoluzione dei problemi della Guida di MySQL e PostgreSQL (#3369)

### <a name="resource"></a>Risorsa

* Miglioramento dei prompt relativi a parametri mancanti per `group deployment create`
* Miglioramento dell'analisi della sintassi di `--parameters KEY=VALUE`
* Risoluzione dei problemi relativi al mancato riconoscimento dei file di parametri di `group deployment create` con la sintassi `@<file>`
* Supporto dell'argomento `--ids` per i comandi `resource` e `managedapp`
* Risoluzione di alcuni messaggi relativi ad analisi ed errori (#3584)
* Risoluzione dei problemi relativi all'analisi di `--resource-type` per consentire al comando `lock` di accettare `<resource-namespace>` e `<resource-type>`
* Aggiunta della verifica del parametro per i modelli di collegamenti ai modelli (#3629)
* Aggiunta del supporto per specificare i parametri di distribuzione tramite la sintassi `KEY=VALUE`

### <a name="role"></a>Ruolo

* Supporto dell'output nel formato del file di autenticazione di SDK per `create-for-rbac`
* Pulizia delle assegnazioni di ruolo e dell'applicazione AAD correlata in caso di eliminazione di un'entità servizio (#3610)
* Inclusione del formato dell'ora negli argomenti `app create` per le descrizioni `--start-date` ed `--end-date`
* Visualizzazione degli avvisi relativi a elementi deprecati quando si usa `--expanded-view`
* Aggiunta dell'integrazione dell'insieme di credenziali delle chiavi ai comandi `create-for-rbac` e `reset-credentials`

### <a name="service-fabric"></a>Service Fabric
* Risoluzione di un problema relativo al troncamento dei file di grandi dimensioni nelle applicazioni in fase di caricamento (#3666)
* Aggiunta dei test per i comandi di Service Fabric (#3424)
* Risoluzione dei problemi di numerosi comandi di Service Fabric (#3234)

### <a name="sql"></a>SQL

* Rimozione del parametro `sql server create` `--identity` non funzionante
* Rimozione dei valori della password dall'output dei comandi `sql server create` e `sql server update`
* Aggiunta dei comandi `sql db list-editions` e `sql elastic-pool list-editions`

### <a name="storage"></a>Archiviazione

* Rimozione dell'opzione `--marker` dai comandi `storage blob list`, `storage container list` e `storage share list` (#3745)
* Abilitazione della creazione di un account di archiviazione solo HTTPS
* Aggiornamento delle metriche di archiviazione, della registrazione e dei comandi CORS (#3495)
* Nuova formulazione del messaggio di eccezione dall'aggiunta di CORS (#3638) (#3362)
* Conversione del generatore in elenco in modalità di test controllato del comando Batch di download (#3592)
* Risoluzione del problema relativo al test controllato di Batch del download di BLOB (#3640) (#3592)

### <a name="vm"></a>VM

* Supporto per la configurazione di nsg
* Risoluzione di un bug relativo alla configurazione non corretta del server DNS
* Supporto per le identità del servizio gestito
* Risoluzione del problema per cui `cmss create` con un servizio di bilanciamento del carico esistente richiedeva `--backend-pool-name`
* I dischi dati creati con LUN `vm image create` iniziano con 0


## <a name="may-10-2017"></a>10 maggio 2017

Versione 2.0.6

* documentdb rinominato come cosmosdb
* Aggiunge rdbms (mysql, postgres)
* Inclusione dei moduli Data Lake Analytics e Data Lake Store
* Inclusione del modulo Servizi cognitivi
* Inclusione del modulo Service Fabric
* Inclusione del modulo per l'interattività (ridenominazione di az-shell)
* Aggiunta del supporto per i comandi per la rete CDN
* Rimozione del modulo per i contenitori
* Aggiunge "az -v" come collegamento per "az -- version" ([#2926](https://github.com/Azure/azure-cli/issues/2926))
* Migliora le prestazioni di carico del pacchetto e l'esecuzione del comando ([#2819](https://github.com/Azure/azure-cli/issues/2819))

```text
azure-cli (2.0.6)

acr (2.0.4)
acs (2.0.6)
appservice (0.1.6)
batch (2.0.4)
cdn (0.0.2)
cloud (2.0.2)
cognitiveservices (0.1.2)
command-modules-nspkg (2.0.0)
component (2.0.4)
configure (2.0.6)
core (2.0.6)
cosmosdb (0.1.6)
dla (0.0.6)
dls (0.0.6)
feedback (2.0.2)
find (0.2.2)
interactive (0.3.1)
iot (0.1.5)
keyvault (2.0.4)
lab (0.0.4)
monitor (0.0.4)
network (2.0.6)
nspkg (3.0.0)
profile (2.0.4)
rdbms (0.0.1)
redis (0.2.3)
resource (2.0.6)
role (2.0.4)
sf (1.0.1)
sql (2.0.3)
storage (2.0.6)
vm (2.0.6)
```

### <a name="core"></a>Core

* core: consente di acquisire le eccezioni causate dall'annullamento della registrazione e dalla registrazione automatica del provider
* perf: consente di mantenere la cache del token ADAL in memoria fino alla chiusura di processo ([#2603](https://github.com/Azure/azure-cli/issues/2603))
* Consente di correggere i byte restituiti da impronta digitale hex -o tsv ([#3053](https://github.com/Azure/azure-cli/issues/3053))
* Download dei certificati del Key Vault e dell'integrazione AAD SP migliorati ([#3003](https://github.com/Azure/azure-cli/issues/3003))
* Aggiunge il percorso di Python per "az —version" ([#2986](https://github.com/Azure/azure-cli/issues/2986))
* login: supporta l'accesso quando non sono presenti sottoscrizioni ([#2929](https://github.com/Azure/azure-cli/issues/2929))
* core: consente di correggere un errore quando si effettua l'accesso usando un'entità servizio due volte ([#2800](https://github.com/Azure/azure-cli/issues/2800))
* core: consente al percorso del file accessTokens.json di poter essere configurato tramite un env var ([#2605](https://github.com/Azure/azure-cli/issues/2605))
* core: consente ai valori predefiniti configurati di essere applicati solo ad argomenti facoltativi ([#2703](https://github.com/Azure/azure-cli/issues/2703))
* core: prestazioni migliorate
* core: certificati CA personalizzati: supporta l'impostazione della variabile di ambiente REQUESTS_CA_BUNDLE
* core: configurazione del cloud: usa l'endpoint "gestione risorse" se l'endpoint "gestione" non è impostato

### <a name="acs"></a>ACS

* consente di correggere il conteggio di master e agenti affinché sia un numero intero anziché una stringa
* espone "az acs create --no-wait" e "az acs wait" alla creazione asincrona
* espone "az acs create --validate" alle convalide del test controllato
* rimuove il profilo di Windows prima di INSERIRE la chiamata del comando scale ([#2755](https://github.com/Azure/azure-cli/issues/2755))

### <a name="appservice"></a>AppService

* functionapp: aggiunge supporti functionapp completi, tra cui create, show, list, delete, hostname, ssl e così via
* Aggiunta di servizi di Team (vsts) come opzione di consegna continua a "appservice web source-control config"
* Consente di creare "az webapp" per sostituire "az appservice web". Per la compatibilità con le versioni precedenti, "az appservice web" viene mantenuta per 2 versioni
* Espone gli argomenti per configurare la distribuzione e "stack di runtime" su webapp
* Espone "webapp list-runtimes"
* supporta le stringhe di connessione per la configurazione ([#2647](https://github.com/Azure/azure-cli/issues/2647))
* supporta lo scambio di slot con anteprima
* Errori di polacco neii comandi appservice ([#2948](https://github.com/Azure/azure-cli/issues/2948))
* Usa il gruppo di risorse del piano del servizio app per le operazioni cert ([#2750](https://github.com/Azure/azure-cli/issues/2750))

### <a name="cosmosdb"></a>Cosmos DB

* Ridenominazione del modulo documentdb in cosmosdb
* Aggiunto il supporto per le API del piano di dati documentdb: gestione di database e raccolta
* Aggiunto il supporto per abilitare il failover automatico negli account del database
* Aggiunto il supporto per nuovi criteri di coerenza ConsistentPrefix

### <a name="data-lake-analytics"></a>Data Lake Analytics

* Correzione di un bug a causa del quale l'applicazione di filtri in base al risultato e allo stato negli elenchi dei processi genererebbe un errore
* Aggiunge il supporto per il tipo di elementi del nuovo catalogo: pacchetto. accessibili tramite: `az dla catalog package`
* È possibile elencare gli elementi seguenti del catalogo da un database, senza necessità delle specifiche dello schema:

  * Tabella
  * Funzione con valori di tabella
  * Visualizzazione
  * Statistiche tabelle. Questo elemento può essere elencato anche con uno schema, ma senza specificare un nome di tabella

### <a name="data-lake-store"></a>Data Lake Store

* Aggiornamento della versione dell'SDK del file system sottostante, che offre un supporto migliore per la gestione di scenari con limitazione lato server
* Migliora le prestazioni di carico del pacchetto e l'esecuzione del comando ([#2819](https://github.com/Azure/azure-cli/issues/2819))
* guida mancate per la presentazione di accesso. aggiunta. ([#2743](https://github.com/Azure/azure-cli/issues/2743))

### <a name="find"></a>Find

* migliora i risultati di ricerca e consente di controllare le versioni dell'indice di ricerca

### <a name="keyvault"></a>Insieme di credenziali delle chiavi

* BC: `az keyvault certificate download` modifica -e dalla stringa o dal binario in PEM o DER per rappresentare meglio le opzioni
* BC: rimozione di --expires e --not-before da `keyvault certificate create` perché questi parametri non sono supportati dal servizio
* Aggiunge il parametro --validity a `keyvault certificate create` per eseguire l'override in modo selettivo del valore in --policy
* Risoluzione del problema in `keyvault certificate get-default-policy` che determina l'esposizione di "expires" e "not_before" ma non di "validity_in_months"
* correzione di keyvault per l'importazione di pem e pfx ([#2754](https://github.com/Azure/azure-cli/issues/2754))

### <a name="lab"></a>Lab

* Aggiunta dei comandi create, show, delete e list per l'ambiente nel lab
* Aggiunta dei comandi show e list per la visualizzazione dei modelli di Azure Resource Manager nel lab
* Aggiunta del flag --environment in `az lab vm list` per filtrare le VM in base all'ambiente nel lab
* Aggiunta dell'utile comando `az lab formula export-artifacts` per esportare lo scaffolding di artefatti all'interno di una formula del lab
* Aggiunta dei comandi per la gestione dei segreti in un lab

### <a name="monitor"></a>Monitorare

* Correzione di bug: modellazione di `--actions` in `az alert-rules create` per usare una stringa JSON ([#3009](https://github.com/Azure/azure-cli/issues/3009))
* Correzione di bug: diagnostic settings create non accetta i registri/le metriche dei comandi show ([#2913](https://github.com/Azure/azure-cli/issues/2913))

### <a name="network"></a>Rete

* Aggiunta del comando `network watcher test-connectivity`
* Aggiunta del supporto per il parametro `--filters` per `network watcher packet-capture create`
* Aggiunta del supporto per l'esaurimento delle connessioni del gateway applicazione
* Aggiunta del supporto per la configurazione di set di regole WAF del gateway applicazione
* Aggiunta del supporto per regole e filtri di route ExpressRoute
* Aggiunta del supporto per il routing geografico di Gestione traffico
* Aggiunta del supporto per i selettori di traffico basati su criteri per la connessione VPN
* Aggiunta del supporto per criteri IPSec per la connessione VPN
* Correzione del bug relativo a `vpn-connection create` in caso di uso dei parametri `--no-wait` o `--validate`
* Aggiunge il supporto per i gateway di rete virtuale attivo/attivo
* Rimozione dei valori Null dall'output dei comandi `network vpn-connection list/show`
* BC: correzione di bug nell'output di `vpn-connection create`
* Correzione del bug relativo all'analisi non corretta dell'argomento "--key-length" di "vpn-connection create"
* Correzione del bug in `dns zone import` relativo all'importazione non corretta dei record
* Correzione del bug relativo al mancato funzionamento di `traffic-manager endpoint update`
* Aggiunta dei comandi in anteprima "network watcher"

### <a name="profile"></a>Profilo

* Supporta l'accesso quando non sono state trovate sottoscrizioni ([#2560](https://github.com/Azure/azure-cli/issues/2560))
* Supporta l'abbreviazione del nome dei parametri in az account set --subscription ([#2980](https://github.com/Azure/azure-cli/issues/2980))

### <a name="redis"></a>Redis

* Aggiunta del comando update che offre anche la possibilità di passare alla cache Redis
* Deprecazione del comando "update-settings"

### <a name="resource"></a>Risorsa

* Aggiunge i comandi managedapp e managedapp definition ([#2985](https://github.com/Azure/azure-cli/issues/2985))
* Supporta i comandi "provider operation" ([#2908](https://github.com/Azure/azure-cli/issues/2908))
* Supporta la creazione della risorsa generica ([#2606](https://github.com/Azure/azure-cli/issues/2606))
* Corregge l'analisi della risorsa e la ricerca della versione API. ([#2781](https://github.com/Azure/azure-cli/issues/2781))
* Aggiunge docs ad az lock update. ([#2702](https://github.com/Azure/azure-cli/issues/2702))
* Genera un errore se si tenta di elencare le risorse per un gruppo che non esiste. ([#2769](https://github.com/Azure/azure-cli/issues/2769))
* [Compute] Risolve i problemi con l'aggiornamento dei set di disponibilità VMSS e della macchina virtuale. ([#2773](https://github.com/Azure/azure-cli/issues/2773))
* Correzione della creazione e dell'eliminazione del blocco se parent-resource-path è Nessuno ([#2742](https://github.com/Azure/azure-cli/issues/2742))

### <a name="role"></a>Ruolo

* create-for-rbac: assicura che la data di fine di SP non superi la data di scadenza del certificato ([#2989](https://github.com/Azure/azure-cli/issues/2989))
* RBAC: aggiunge il supporto completo ad "ad group" ([#2016](https://github.com/Azure/azure-cli/issues/2016))
* role: corregge i problemi in role definition update ([#2745](https://github.com/Azure/azure-cli/issues/2745))
* create-for-rbac: garantisce che venga presa la password indicata

### <a name="sql"></a>SQL

* Aggiunta dei comandi az sql server list-usages e az sql db list-usages
* SQL: possibilità di connettersi direttamente al provider di risorse ([#2832](https://github.com/Azure/azure-cli/issues/2832))

### <a name="storage"></a>Archiviazione

* Impostazione della località del gruppo di risorse come località predefinita per `storage account create`
* Aggiunge il supporto per la copia di BLOB incrementale
* Aggiunge il supporto per il upload di BLOB in blocchi di grandi dimensioni
* Modifica le dimensioni del blocco in 100 MB quando il file da caricare supera i 200 GB

### <a name="vm"></a>VM

* avail-set: rende facoltativo il conteggio del dominio UD&FD

  nota: comandi della VM nei cloud sovrani. Evitare le funzionalità correlate al disco gestito, tra cui:
  1. az disk/snapshot/image
  2. az vm/vmss disk
  3. All'interno di "az vm/vmss create" usare "—use-unmanaged-disk" per evitare che il disco gestito. Funziona anche con altri comandi
* vm/vmss: migliora il testo di avviso quando si generano coppie di chiavi SSH
* vm/vmss: supporta la creazione da un'immagine del marketplace che richiede informazioni sul piano ([#1209](https://github.com/Azure/azure-cli/issues/1209))


## <a name="april-3-2017"></a>3 aprile 2017

Versione 2.0.2

In questa versione sono stati rilasciati i componenti ACR, Batch, Key Vault e SQL.

```text
azure-cli (2.0.2)

acr (2.0.0)
acs (2.0.2)
appservice (0.1.2)
batch (2.0.0)
cloud (2.0.0)
component (2.0.0)
configure (2.0.2)
container (0.1.2)
core (2.0.2)
documentdb (0.1.2)
feedback (2.0.0)
find (0.0.1b1)
iot (0.1.2)
keyvault (2.0.0)
lab (0.0.1)
monitor (0.0.1)
network (2.0.2)
nspkg (2.0.0)
profile (2.0.2)
redis (0.1.1b3)
resource (2.0.2)
role (2.0.1)
sql (2.0.0)
storage (2.0.2)
vm (2.0.2)
```

### <a name="core"></a>Core

* Aggiunta dei moduli per il monitoraggio, la ricerca, ACR e lab all'elenco predefinito
* Login: esclusione del tenant errato ([#2634](https://github.com/Azure/azure-cli/pull/2634))
* login: impostazione della sottoscrizione predefinita su una con lo stato "Enabled" ([#2575](https://github.com/Azure/azure-cli/pull/2575))
* Aggiunta di comandi wait e di supporto per --no-wait a più comandi ([#2524](https://github.com/Azure/azure-cli/pull/2524))
* core: supporto per accesso mediante entità servizio con un certificato ([#2457](https://github.com/Azure/azure-cli/pull/2457))
* Aggiunta della richiesta dei parametri di modello mancanti. ([#2364](https://github.com/Azure/azure-cli/pull/2364))
* Supporto per l'impostazione di valori predefiniti per argomenti comuni ad esempio gruppo di risorse predefinito, Web predefinito, macchina virtuale predefinita
* Supporto per l'accesso al tenant specifico

### <a name="acs"></a>ACS

* [ACS] Aggiunta di supporto per la configurazione di un cluster ACS predefinito ([#2554](https://github.com/Azure/azure-cli/pull/2554))
* Aggiunta di supporto per la richiesta di password della chiave SSH. ([#2044](https://github.com/Azure/azure-cli/pull/2044))
* Aggiunta di supporto per i cluster Windows. ([#2211](https://github.com/Azure/azure-cli/pull/2211))
* Passaggio dal ruolo Proprietario al ruolo Collaboratore. ([#2321](https://github.com/Azure/azure-cli/pull/2321))

### <a name="appservice"></a>AppService

* appservice: supporto per ottenere l'indirizzo IP esterno usato per i record DNS A ([#2627](https://github.com/Azure/azure-cli/pull/2627))
* appservice: supporto dell'associazione di certificati jolly ([#2625](https://github.com/Azure/azure-cli/pull/2625))
* appservice: supporto dell'elenco di profili di pubblicazione ([#2504](https://github.com/Azure/azure-cli/pull/2504))
* AppService - attivazione della sincronizzazione del controllo dopo la configurazione ([#2326](https://github.com/Azure/azure-cli/pull/2326))

### <a name="datalake"></a>DataLake

* Versione iniziale del modulo Data Lake Analytics
* Versione iniziale del modulo Data Lake Store

### <a name="docuemntdb"></a>DocuemntDB

* DocumentDB: aggiunta di supporto per l'elenco di stringhe di connessione ([#2580](https://github.com/Azure/azure-cli/pull/2580))

### <a name="vm"></a>VM

* [Compute] Aggiunta di supporto per AppGateway alla creazione di set di scalabilità di macchine virtuali ([#2570](https://github.com/Azure/azure-cli/pull/2570))
* [VM/VMSS] Miglioramento del supporto per memorizzazione nella cache del disco ([#2522](https://github.com/Azure/azure-cli/pull/2522))
* VM/VMSS: inclusione di logica di convalida credenziali usata dal portale ([#2537](https://github.com/Azure/azure-cli/pull/2537))
* Aggiunta di comandi wait e di supporto per --no-wait ([#2524](https://github.com/Azure/azure-cli/pull/2524))
* Virtual machine scale set: supporto * per elencare le visualizzazioni di istanza tra le macchine virtuali ([#2467](https://github.com/Azure/azure-cli/pull/2467))
* Aggiunta di segreti per la VM e il set di scalabilità di macchine virtuali ([#2212}(<https://github.com/Azure/azure-cli/pull/2212>))
* Consentita la creazione di macchine virtuali con disco rigido virtuale specializzato ([#2256](https://github.com/Azure/azure-cli/pull/2256))

## <a name="february-27-2017"></a>27 febbraio 2017

Versione 2.0.0

Questa versione dell'interfaccia della riga di comando di Azure 2.0 è la prima disponibile a livello generale. La disponibilità generale si applica a questi moduli di comandi:
- Servizio contenitore (acs)
- Compute (inclusi Resource Manager, macchina virtuale, set di scalabilità di macchine virtuali, Managed Disks)
- Rete
- Archiviazione

Questi moduli di comandi possono essere usati in produzione e sono supportati dal contratto di servizio Microsoft standard. È possibile segnalare i problemi direttamente al supporto tecnico Microsoft o tramite l'[elenco dei problemi in GitHub](https://github.com/azure/azure-cli/issues/), porre domande in [StackOverflow usando il tag azure-cli](http://stackoverflow.com/questions/tagged/azure-cli), contattare il team del prodotto all'indirizzo [azfeedback@microsoft.com](mailto:azfeedback@microsoft.com) e inviare commenti e suggerimenti dalla riga di comando con il comando `az feedback`.

I comandi in questi moduli sono stabili e non sono previste modifiche della sintassi nei prossimi rilasci di questa versione dell'interfaccia della riga di comando di Azure.

Per verificare la versione dell'interfaccia della riga di comando, usare `az --version`. L'output elenca la versione dell'interfaccia della riga di comando (in questo caso, 2.0.0), i singoli moduli di comandi e le versioni di Python e GCC in uso.

```text
azure-cli (2.0.0)

acs (2.0.0)
appservice (0.1.1b5)
batch (0.1.1b4)
cloud (2.0.0)
component (2.0.0)
configure (2.0.0)
container (0.1.1b4)
core (2.0.0)
documentdb (0.1.1b2)
feedback (2.0.0)
iot (0.1.1b3)
keyvault (0.1.1b5)
network (2.0.0)
nspkg (2.0.0)
profile (2.0.0)
redis (0.1.1b3)
resource (2.0.0)
role (2.0.0)
sql (0.1.1b5)
storage (2.0.0)
vm (2.0.0)

Python (Darwin) 2.7.10 (default, Jul 30 2016, 19:40:32)
[GCC 4.2.1 Compatible Apple LLVM 8.0.0 (clang-800.0.34)]
```

> [!Note]
> Alcuni moduli di comandi hanno il suffisso "b*n*" o "rc*n*". Questi moduli sono ancora in anteprima e saranno disponibili a livello generale in futuro.

Vengono anche offerte build di anteprima notturne dell'interfaccia della riga di comando. Per informazioni, vedere le istruzioni su come [ottenere le build notturne](https://github.com/Azure/azure-cli#nightly-builds) e le istruzioni per [la configurazione e la collaborazione al codice da parte degli sviluppatori](https://github.com/Azure/azure-cli#developer-setup).

È possibile segnalare problemi relative alle build di anteprima notturne nei seguenti modi:
- Segnalare i problemi tramite l'[elenco di problemi github](https://github.com/azure/azure-cli/issues/)
- Contattare il team del prodotto all'indirizzo [azfeedback@microsoft.com](mailto:azfeedback@microsoft.com)
- Inviare commenti e suggerimenti dalla riga di comando con il comando `az feedback`
