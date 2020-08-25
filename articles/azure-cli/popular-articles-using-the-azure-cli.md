---
title: Collegamenti a più servizi per l'uso dell'interfaccia della riga di comando di Azure
description: Collegamenti a esercitazioni, guide di avvio rapido, esempi, concetti e guide pratiche, interfaccia della riga di comando di Azure, macchine virtuali, servizio Azure Kubernetes, Batch, interfaccia della riga di comando di Azure (Core), Azure Resource Manager, Key Vault, Azure Stack Hub, Funzioni, Database, Hub eventi, Configurazione app, Germania, sicurezza, governance, Insights, IoT, Internet delle cose, DevOps, rete virtuale, calcolo, rete, strumenti di sviluppo, database, analisi, gestione e governance, ibrido, archiviazione, sicurezza, intelligenza artificiale, intelligenza artificiale + Machine Learning, Linux, Windows, Ubuntu, automazione, applicazione, app Web, script
author: dbradish-microsoft
ms.author: dbradish
manager: barbkess
ms.date: 03/01/2020
ms.topic: conceptual
ms.prod: azure
ms.technology: azure-cli
ms.devlang: azurecli
ms.openlocfilehash: 648f147c8bc3f954f8b41c2fbec4500c3115e66a
ms.sourcegitcommit: 815cf2acff71e849735f7afce54723f03ffa5df3
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/17/2020
ms.locfileid: "88501276"
---
# <a name="popular-articles-using-the-azure-cli"></a>Articoli più diffusi sull'uso dell'interfaccia della riga di comando di Azure

L'interfaccia della riga di comando di Azure viene usata in molti servizi di Azure, quindi gli articoli sono distribuiti tra vari repository di documenti.  Questa pagina contiene i collegamenti a specifici articoli più diffusi.  

## <a name="compute"></a>Calcolo

|Servizio|Tipo di articolo|Titolo dell'articolo|Descrizione|
|-|-|-|-|
|Macchine virtuali | Esercitazione: Linux | [Creare una macchina virtuale Linux con l'interfaccia della riga di comando di Azure](azure-cli-vm-tutorial.yml) | Creare una macchina virtuale.  Informazioni sulle query di output e sull'impostazione delle variabili di ambiente.
|Macchine virtuali | Avvio rapido: Linux | [Creare una macchina virtuale Linux con l'interfaccia della riga di comando di Azure](/azure/virtual-machines/linux/quick-create-cli) | Creare e distribuire una macchina virtuale Linux.  Aprire una porta per il traffico Web e installare un server Web.
|Macchine virtuali | Guida pratica: Linux |[Creare un'immagine Linux di una macchina virtuale o di un disco rigido virtuale](/azure/virtual-machines/linux/capture-image) | Effettuare il deprovisioning di una macchina virtuale esistente, creare un'immagine e creare una nuova macchina virtuale dall'immagine acquisita.
|Macchine virtuali | Guida pratica: Linux | [Caricare un disco rigido virtuale in Azure con l'interfaccia della riga di comando di Azure](/azure/virtual-machines/linux/disks-upload-vhd-to-managed-disk-cli) | Creare un disco gestito vuoto, caricare il file VHD locale e copiare un disco gestito.
|Macchine virtuali | Guida pratica: Linux | [Creare una raccolta immagini condivise con l'interfaccia della riga di comando di Azure](/azure/virtual-machines/linux/shared-images) | Creare una raccolta di immagini di macchine virtuali personalizzate condivise con altri utenti dell'organizzazione, all'interno di un'area o tra aree diverse oppure all'interno di un tenant di Azure Active Directory.
|Macchine virtuali | Guida pratica: Linux | [Distribuire VM spot con l'interfaccia della riga di comando di Azure (anteprima)](/azure/virtual-machines/linux/spot-cli) | Distribuire una macchina virtuale Linux spot che non verrà rimossa in base al prezzo.
|Macchine virtuali | Avvio rapido: Windows | [Creare una macchina virtuale Windows con l'interfaccia della riga di comando di Azure](/azure/virtual-machines/windows/quick-create-cli) | Distribuire una macchina virtuale in Azure che esegue Windows Server 2016.
|Macchine virtuali | Modulo di Learn | [Gestire macchine virtuali con l'interfaccia della riga di comando di Azure](https://docs.microsoft.com/learn/modules/manage-virtual-machines-with-azure-cli/) | Creare, avviare, arrestare ed eseguire altre attività di gestione correlate alle macchine virtuali.
|Servizio Azure Kubernetes| Avvio rapido | [Distribuire un cluster del servizio Azure Kubernetes tramite l'interfaccia della riga di comando di Azure](/azure/aks/kubernetes-walkthrough) | Distribuire e gestire i cluster del servizio Azure Kubernetes.  Informazioni su come monitorare l'integrità del cluster e dei pod che eseguono l'applicazione.
|Azure Batch|Esempio | [Eseguire processi e attività con Azure Batch tramite l'interfaccia della riga di comando di Azure](/azure/batch/scripts/batch-cli-sample-run-job) | Creare un processo Batch e aggiungervi una serie di attività. Monitorare un processo e le relative attività.
|Azure Batch|Esempio | [Creare e gestire un pool Windows in Azure Batch con l'interfaccia della riga di comando di Azure](/azure/batch/scripts/batch-cli-sample-manage-windows-pool) | Creare e gestire un pool di nodi di calcolo di Windows con una configurazione di Servizi cloud.
|Istanza di contenitore di Azure|Avvio rapido | [Distribuire un'istanza di contenitore con l'interfaccia della riga di comando di Azure](/azure/container-instances/container-instances-quickstart) | Distribuire un contenitore Docker isolato e renderne disponibile l'applicazione con un nome di dominio completo (FQDN). Eseguire un singolo comando di distribuzione e quindi passare all'applicazione in esecuzione nel contenitore.
|Funzione di Azure|Avvio rapido |  [Creare una funzione in Azure che risponde a richieste HTTP con l'interfaccia della riga di comando di Azure](/azure/azure-functions/functions-create-first-azure-function-azure-cli) | Usare strumenti da riga di comando per creare una funzione che risponde a richieste HTTP. Dopo aver testato il codice in locale, distribuire la funzione nell'ambiente serverless di Funzioni di Azure.

## <a name="networking"></a>Rete

|Servizio|Tipo di articolo|Titolo dell'articolo|Descrizione|
|-|-|-|-|
|Rete virtuale|Avvio rapido | [Creare una rete virtuale usando l'interfaccia della riga di comando di Azure](/azure/virtual-network/quick-create-cli) | Creare una rete virtuale e distribuirvi due macchine virtuali a cui connettersi da Internet.
|Rete virtuale|Guida pratica | [Abilitare la rete accelerata in una macchina virtuale Linux con l'interfaccia della riga di comando di Azure](/azure/virtual-network/create-vm-accelerated-networking-cli) | Creare una macchina virtuale Linux, gestire il binding dinamico e la revoca della funzione virtuale, quindi abilitare la rete accelerata.

## <a name="internet-of-things"></a>Internet delle cose

|Servizio|Tipo di articolo|Titolo dell'articolo|Descrizione|
|-|-|-|-|
|Hub IoT|Esercitazione | [Configurare il routing di messaggi per l'hub IoT con l'interfaccia della riga di comando di Azure](/azure/iot-hub/tutorial-routing) | Configurare e usare query di routing personalizzate con l'hub IoT.

## <a name="developer-tools"></a>Strumenti di sviluppo

|Servizio|Tipo di articolo|Titolo dell'articolo|Descrizione|
|-|-|-|-|
|Configurazione app di Azure|Esempi |[Esempi di interfaccia della riga di comando di Azure per Configurazione app di Azure](/azure/azure-app-configuration/cli-samples) | Ottenere collegamenti a script bash che usano l'interfaccia della riga di comando di Azure per Configurazione app di Azure.
|Azure DevOps| Introduzione: Pipeline DevOps |[Creare la prima pipeline di Azure con l'interfaccia della riga di comando di Azure](/azure/devops/pipelines/create-first-pipeline-cli) | Creare una nuova pipeline in una directory di GitHub clonata, gestire ed eseguire le pipeline.
|Azure DevOps| Guida pratica: Pipeline DevOps |[Attività di distribuzione di pipeline di Azure con l'interfaccia della riga di comando di Azure](/azure/devops/pipelines/tasks/deploy/azure-cli?view=azure-devops) | In una pipeline di compilazione o versione eseguire uno script batch o della shell contenente l'interfaccia della riga di comando di Azure.  I comandi vengono eseguiti su agenti multipiattaforma in esecuzione in sistemi operativi Linux, macOS o Windows.
|Azure DevOps| Esercitazione: Pipeline Jenkins |[Eseguire la distribuzione nel servizio app di Azure con Jenkins tramite l'interfaccia della riga di comando di Azure](/azure/jenkins/execute-cli-jenkins-pipeline) | Creare e configurare una macchina virtuale Jenkins, creare un'app Web in Azure e preparare un repository GitHub.  Creare ed eseguire la pipeline Jenkins.

## <a name="databases"></a>Database

|Servizio|Tipo di articolo|Titolo dell'articolo|Descrizione|
|-|-|-|-|
|Database SQL| Esempio |[Configurare Database SQL di Azure con l'interfaccia della riga di comando di Azure](/azure/sql-database/sql-database-cli-samples?tabs=single-database) | Esempi dell'interfaccia della riga di comando di Azure per Database SQL di Azure.
|MySQL|Avvio rapido |[Creare un server di Database di Azure per MySQL tramite l'interfaccia della riga di comando di Azure](/azure/mysql/quickstart-create-mysql-server-database-using-azure-cli) | Creare un database di Azure per il server MySQL.  Configurare una regola del firewall e le impostazioni SSL.  Ottenere e usare le informazioni di connessione.
|Cosmos DB |Guida pratica |[Gestire le risorse di Azure Cosmos DB con l'interfaccia della riga di comando di Azure](/azure/cosmos-db/manage-with-cli) | Usare comandi comuni per automatizzare la gestione di account, database e contenitori di Azure Cosmos DB.
|Cosmos DB |Esempio |[Esempi dell'interfaccia della riga di comando di Azure per l'API SQL (Core) di Azure Cosmos DB](/azure/cosmos-db/cli-samples) | Ottenere collegamenti a esempi di script dell'interfaccia della riga di comando di Azure per l'API SQL (Core) di Azure Cosmos DB.

## <a name="analytics"></a>Analytics

|Servizio|Tipo di articolo|Titolo dell'articolo|Descrizione|
|-|-|-|-|
Hub eventi di Azure |Avvio rapido |[Creare un hub eventi con l'interfaccia della riga di comando di Azure](/azure/event-hubs/event-hubs-quickstart-cli) | Creare uno spazio dei nomi di Hub eventi e un hub eventi.
HDInsight |Guida pratica |[Creare cluster HDInsight tramite l'interfaccia della riga di comando di Azure](/azure/hdinsight/hdinsight-hadoop-create-linux-clusters-azure-cli) | Creare un cluster HDInsight 3.6.
HDInsight |Esercitazione |[Gestire cluster HDInsight con l'interfaccia della riga di comando di Azure](/azure/hdinsight/hdinsight-administer-use-command-line) | Elencare, visualizzare, eliminare e ridimensionare i cluster HDInsight.

## <a name="management-and-governance"></a>Gestione e governance

|Servizio|Tipo di articolo|Titolo dell'articolo|Descrizione|
|-|-|-|-|
Modelli di Resource Manager |Guida pratica |[Distribuire le risorse con i modelli di Azure Resource Manager e l'interfaccia della riga di comando di Azure](/azure/azure-resource-manager/templates/deploy-cli) | Distribuire le risorse in Azure usando i modelli.
Gruppi di Resource Manager |Guida pratica |[Gestire gruppi di risorse di Azure Resource Manager con l'interfaccia della riga di comando di Azure](/azure/azure-resource-manager/management/manage-resource-groups-cli) | Usare Azure Resource Manager per gestire i gruppi di risorse di Azure.
Resource Graph |Avvio rapido |[Eseguire la prima query di Resource Graph con l'interfaccia della riga di comando di Azure](/azure/governance/resource-graph/first-query-azurecli) | Aggiungere Azure Resource Graph all'installazione dell'interfaccia della riga di comando di Azure ed eseguire la prima query di Resource Graph.
Assegnazione di criterio |Avvio rapido |[Creare un'assegnazione di criteri per identificare le risorse non conformi con l'interfaccia della riga di comando di Azure](/azure/governance/policy/assign-policy-azurecli) | Creare un'assegnazione di criteri per identificare le macchine virtuali che non usano dischi gestiti.

## <a name="hybrid"></a>Ibrido

|Servizio|Tipo di articolo|Titolo dell'articolo|Descrizione|
|-|-|-|-|
Hub di Azure Stack| Avvio rapido: VM Linux |[Creare una macchina virtuale server Linux nell'hub di Azure Stack con l'interfaccia della riga di comando di Azure](/azure-stack/user/azure-stack-quick-create-vm-linux-cli) | Creare una macchina virtuale Ubuntu Server 16.04 LTS, connettersi con un client remoto e installare un server Web NGINX.
Hub di Azure Stack| Avvio rapido: Macchina virtuale Windows |[Creare una macchina virtuale Windows Server nell'hub di Azure Stack con l'interfaccia della riga di comando di Azure](/azure-stack/user/azure-stack-quick-create-vm-windows-cli) |Creare una macchina virtuale Windows Server 2016, connettersi con un client remoto e installare il server Web IIS.
Hub di Azure Stack| Guida pratica: Risorse per Azure Stack Development Kit |[Gestire e distribuire le risorse nell'hub di Azure Stack con l'interfaccia della riga di comando di Azure](/azure-stack/user/azure-stack-version-profiles-azurecli2) | Configurare l'interfaccia della riga di comando di Azure per gestire le risorse di Azure Stack Development Kit da piattaforme client Linux, Mac e Windows.

## <a name="storage"></a>Archiviazione

|Servizio|Tipo di articolo|Titolo dell'articolo|Descrizione|
|-|-|-|-|
Archiviazione BLOB |Avvio rapido |  [Creare, scaricare ed elencare BLOB con l'interfaccia della riga di comando di Azure](/azure/storage/blobs/storage-quickstart-blobs-cli) | Caricare e scaricare i dati da e verso archiviazione BLOB di Azure.
Archiviazione BLOB |Guida pratica |[Autorizzare l'accesso ai dati di BLOB o code con l'interfaccia della riga di comando di Azure](/azure/storage/common/authorize-data-operations-cli) | Specificare come vengono autorizzate le operazioni sui dati e impostare le variabili di ambiente per i parametri.
Archiviazione BLOB |Guida pratica |[Usare l'interfaccia della riga di comando di Azure per gestire directory, file ed elenchi di controllo di accesso in Azure Data Lake Storage Gen2 (anteprima)](/azure/storage/blobs/data-lake-storage-directory-file-acl-cli) | Creare e gestire directory, file e autorizzazioni negli account di archiviazione dotati di uno spazio dei nomi gerarchico.
Archiviazione file |Avvio rapido |[Creare e gestire condivisioni file di Azure con l'interfaccia della riga di comando di Azure](/azure/storage/files/storage-how-to-use-files-cli) | Creare e usare condivisioni file di Azure.  Creare e gestire gli snapshot di condivisione.

## <a name="security"></a>Sicurezza

|Service|Tipo di articolo|Titolo dell'articolo|Descrizione|
|-|-|-|-|
Entità servizio |Guida pratica |[Creare un'entità servizio di Azure con l'interfaccia della riga di comando di Azure](/cli/azure/create-an-azure-service-principal-azure-cli) | Creare, ottenere informazioni e reimpostare un'entità servizio con l'interfaccia della riga di comando di Azure.
Controllo degli accessi in base al ruolo |Guida pratica |[Aggiungere o rimuovere assegnazioni di ruolo con il controllo degli accessi in base al ruolo di Azure e l'interfaccia della riga di comando di Azure](/azure/role-based-access-control/role-assignments-cli) | Assegnare ruoli al controllo degli accessi in base al ruolo di Azure.
Key Vault |Guida pratica |[Gestire Key Vault tramite l'interfaccia della riga di comando di Azure](/azure/key-vault/key-vault-manage-with-cli2) | Creare e gestire Azure Key Vault.  Registrare e autorizzare un'applicazione, impostare criteri di accesso avanzati e acquisire informazioni sui comandi dell'interfaccia della riga di comando multipiattaforma.
Key Vault |Esercitazione |[Gestire le chiavi degli account di archiviazione con Key Vault e l'interfaccia della riga di comando di Azure](/azure/key-vault/key-vault-ovw-storage-keys) | Gestire le chiavi degli account di archiviazione e generare token con firma di accesso condiviso.

## <a name="ai--machine-learning"></a>Intelligenza artificiale e Machine Learning

|Servizio|Tipo di articolo|Titolo dell'articolo|Descrizione|
|-|-|-|-|
Machine Learning |Riferimento |[Usare l'estensione dell'interfaccia della riga di comando di Azure per Azure Machine Learning](/azure/machine-learning/reference-azure-machine-learning-cli) | Eseguire esperimenti per creare modelli di Machine Learning e registrarli per l'utilizzo dei clienti.  Creare un pacchetto, distribuire e monitorare il ciclo di vita dei modelli di Machine Learning.
Servizi cognitivi |Guida pratica |[Creare una risorsa di Servizi cognitivi con l'interfaccia della riga di comando di Azure](/azure/cognitive-services/cognitive-services-apis-create-account-cli) | Iscriversi a Servizi cognitivi di Azure e creare un account con una sottoscrizione di uno o più servizi.  Usare le chiavi e l'endpoint generati per autenticare le applicazioni.
Monitoraggio di Azure |Guida pratica |[Creare un'area di lavoro Log Analytics con l'interfaccia della riga di comando di Azure](/azure/azure-monitor/learn/quick-create-workspace-cli) | Creare e distribuire un'area di lavoro Log Analytics.

## <a name="geographies"></a>Aree geografiche

|Servizio|Tipo di articolo|Titolo dell'articolo|Descrizione|
|-|-|-|-|
Azure Germania |Introduzione |[Connettersi ad Azure Germania con l'interfaccia della riga di comando di Azure](/azure/germany/germany-get-started-connect-with-cli) | Usando Azure Germania, gestire una grande sottoscrizione tramite script e accedere a funzionalità attualmente non disponibili nel portale di Azure globale.
Azure Government|Introduzione |[Connettersi ad Azure per enti pubblici con l'interfaccia della riga di comando di Azure](/azure/azure-government/documentation-government-get-started-connect-with-cli)|Accedere e iniziare a gestire le risorse in Azure per enti pubblici.

## <a name="see-also"></a>Vedere anche

* [Introduzione all'interfaccia della riga di comando di Azure](get-started-with-azure-cli.md)
* [Elenco di riferimento completo dei comandi per l'interfaccia della riga di comando di Azure](/cli/azure/reference-index)
* [Servizi gestibili con l'interfaccia della riga di comando di Azure](azure-services-the-azure-cli-can-manage.md)
