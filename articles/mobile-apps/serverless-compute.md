---
title: Creare un back-end di applicazioni per dispositivi mobili serverless con Funzioni di Azure e altri servizi
description: Informazioni sui servizi di calcolo usati per creare un back-end per applicazioni per dispositivi mobili affidabile e serverless.
author: codemillmatt
ms.service: mobile-services
ms.assetid: 444f0959-aa7f-472c-a6c7-9eecea3a34b9
ms.topic: conceptual
ms.date: 06/08/2020
ms.author: masoucou
ms.openlocfilehash: 9de5f65045ab7d947902c18be68abf1f4880eeae
ms.sourcegitcommit: 2f832baf90c208a8a69e66badef5f126d23bbaaf
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/21/2020
ms.locfileid: "88725155"
---
# <a name="build-mobile-back-end-components-with-compute-services"></a>Creare i componenti del back-end per dispositivi mobili con i servizi di calcolo

Ogni applicazione per dispositivi mobili necessita di un back-end responsabile dell'archiviazione dei dati, della logica di business e della sicurezza. La gestione dell'infrastruttura per ospitare ed eseguire il codice back-end richiede il dimensionamento, il provisioning e la scalabilità per più server. È anche necessario gestire gli aggiornamenti del sistema operativo e l'hardware previsto e applicare le patch di sicurezza. Occorre inoltre monitorare tutti questi componenti dell'infrastruttura per verificarne le prestazioni, la disponibilità e la tolleranza di errore. 

L'architettura serverless è utile per questo tipo di scenario, perché non occorre gestire alcun server, sistema operativo, software correlato o aggiornamenti hardware. L'architettura serverless consente agli sviluppatori di risparmiare tempo e costi, accelerare il time-to-market e concentrare le energie sulla creazione di applicazioni.

## <a name="benefits-of-compute"></a>Vantaggi del calcolo

- Con l'astrazione dei server non è necessario preoccuparsi di hosting, applicazione di patch e sicurezza ed è possibile concentrarsi esclusivamente sul codice.
- Il dimensionamento istantaneo ed efficiente assicura che il provisioning venga effettuato automaticamente o su richiesta, su larga o piccola scala.
- Disponibilità elevata e tolleranza di errore.
- La micro-fatturazione garantisce l'addebito di costi solo quando il codice è effettivamente in esecuzione.
- Il codice, scritto nel linguaggio scelto, viene eseguito nel cloud.

Usare i servizi seguenti per abilitare le funzionalità di calcolo serverless nelle app per dispositivi mobili.

## <a name="azure-functions"></a>Funzioni di Azure

[Funzioni di Azure](https://azure.microsoft.com/services/functions/) è un'esperienza di calcolo basata su eventi che è possibile usare per eseguire il codice, scritto nel linguaggio di programmazione preferito, senza preoccuparsi dei server. Non è necessario gestire l'applicazione o l'infrastruttura in cui viene eseguita. È possibile dimensionare le funzioni su richiesta e vengono addebitati i costi solo per il tempo di esecuzione del codice. Funzioni di Azure è un ottimo modo per implementare un'API per un'applicazione per dispositivi mobili. Le API sono facili da implementare e gestire e sono accessibili tramite HTTP.

### <a name="azure-functions-key-features"></a>Funzionalità principali di Funzioni di Azure

- Esperienza scalabile e basata su eventi, in cui è possibile usare trigger e associazioni per definire quando una funzione viene richiamata e a quali dati si connette.
- Trasferimento di dipendenze personalizzate perché Funzioni supporta NuGet e NPM e consente quindi l'uso delle librerie preferite.
- Sicurezza integrata che permette di proteggere le funzioni attivate tramite HTTP con provider OAuth, ad esempio Azure Active Directory, Facebook, Google, Twitter e account Microsoft.
- Integrazione semplificata con vari [servizi di Azure](/azure/azure-functions/functions-overview) e offerte di software come un servizio (SaaS).
- Sviluppo flessibile che consente di creare le funzioni direttamente nel portale di Azure oppure configurare l'integrazione continua e distribuire il codice tramite GitHub, Azure DevOps Services e altri strumenti di sviluppo supportati.
- Il runtime di Funzioni è open source e disponibile in [GitHub](https://github.com/azure/azure-webjobs-sdk-script).
- Esperienza di sviluppo avanzata in cui è possibile scrivere codice, testare ed eseguire il debug in locale usando l'editor preferito o l'interfaccia Web di facile utilizzo con il monitoraggio con strumenti integrati e funzionalità DevOps predefinite.
- Ampia varietà di linguaggi di programmazione e opzioni di hosting per lo sviluppo, ad esempio C#, Node.js, Java, JavaScript o Python.
- Modello di determinazione prezzi in base al consumo che consente di pagare solo per il tempo di esecuzione del codice.

### <a name="azure-functions-references"></a>Informazioni di riferimento per Funzioni di Azure

- [Azure portal](https://portal.azure.com)
- [Documentazione di Funzioni di Azure](/azure/azure-functions/)
- [Guida per sviluppatori di Funzioni di Azure](/azure/azure-functions/functions-reference)
- [Guide introduttive](/azure/azure-functions/functions-create-first-function-vs-code)
- [Esempi](/samples/browse/?products=azure-functions&languages=csharp)

## <a name="azure-app-service"></a>Servizio app di Azure

Il [Servizio app di Azure](https://azure.microsoft.com/services/app-service/) consente di creare e ospitare app Web e API RESTful nel linguaggio di programmazione preferito senza gestire l'infrastruttura. Offre la scalabilità automatica e la disponibilità elevata, supporta sia Windows che Linux e consente distribuzioni automatizzate da GitHub, Azure DevOps o qualsiasi repository Git.

### <a name="azure-app-service-key-features"></a>Funzionalità principali del Servizio app di Azure

- Più linguaggi e framework per il supporto per ASP.NET, ASP.NET Core, Java, Ruby, Node.js, PHP o Python. È anche possibile eseguire PowerShell e altri script o eseguibili come servizi in background.
- Ottimizzazione della metodologia DevOps tramite la distribuzione e l'integrazione continua con Azure DevOps, GitHub, BitBucket, Docker Hub o Registro Azure Container, e gestire le app nel servizio app con Azure PowerShell o l'interfaccia della riga di comando multipiattaforma.
- Scalabilità globale con disponibilità elevata per aumentare le prestazioni o il numero di istanze manualmente o automaticamente.
- Connessioni a piattaforme SaaS e dati locali per scegliere tra oltre 50 connettori per sistemi aziendali, ad esempio SAP, servizi SaaS, ad esempio Salesforce, e servizi Internet, come Facebook, nonché accedere ai dati locali con connessioni ibride e reti virtuali di Azure.
- Il Servizio app di Azure garantisce la conformità ISO, SOC e PCI. Autenticare gli utenti con Azure Active Directory o con l'account di accesso ai social media, ad esempio Google, Facebook, Twitter e Microsoft. Creare restrizioni per gli indirizzi IP e gestire le identità dei servizi.
- Modelli di applicazione che consentono di scegliere da un esteso elenco di modelli di applicazione disponibili in Azure Marketplace, come WordPress, Joomla e Drupal.
- Integrazione con Visual Studio che consente di usare strumenti dedicati per semplificare il processo di creazione, distribuzione e debug.

### <a name="azure-app-service-references"></a>Informazioni di riferimento per il Servizio app di Azure

- [Azure portal](https://portal.azure.com/)
- [Documentazione del Servizio app di Azure](/azure/app-service/)
- [Guide introduttive](/azure/app-service/app-service-web-get-started-dotnet)
- [Esempi](/azure/app-service/samples-cli)
- [Esercitazioni](/azure/app-service/app-service-web-tutorial-dotnet-sqldatabase)

## <a name="azure-kubernetes-service"></a>Servizio Azure Kubernetes

Il [servizio Azure Kubernetes](https://azure.microsoft.com/services/kubernetes-service/) gestisce l'ambiente Kubernetes ospitato, consentendo di distribuire e gestire applicazioni in contenitori in modo semplice e rapido senza competenze nell'orchestrazione di contenitori. Elimina inoltre le attività continuative correlate alle operazioni e alla gestione. Il servizio Azure Kubernetes effettua il provisioning, l'aggiornamento e il dimensionamento delle risorse su richiesta, senza portare offline le applicazioni.

### <a name="azure-kubernetes-service-key-features"></a>Funzionalità principali del servizio Azure Kubernetes

- Migrazione semplificata delle applicazioni esistenti ai contenitori per l'esecuzione nel servizio Azure Kubernetes.
- Semplificare la distribuzione e la gestione delle applicazioni basate su microservizi.
- Secure DevOps per il servizio Azure Kubernetes per ottenere un equilibrio tra velocità e sicurezza e recapitare il codice più velocemente su larga scala.
- Dimensionamento facile con il servizio Azure Kubernetes e le istanze di Azure Container per il provisioning dei pod all'interno di Istanze di Container avviate in pochi secondi.
- Distribuire e gestire i dispositivi IoT su richiesta.
- Eseguire il training dei modelli di Machine Learning tramite strumenti come TensorFlow e KubeFlow.

### <a name="azure-kubernetes-service-references"></a>Informazioni di riferimento per il servizio Azure Kubernetes

- [Azure portal](https://portal.azure.com/)
- [Documentazione del servizio Azure Kubernetes](/azure/aks/)
- [Guide introduttive](/azure/aks/kubernetes-walkthrough-portal)
- [Esercitazioni](/azure/aks/tutorial-kubernetes-prepare-app)

## <a name="azure-container-instances"></a>Istanze di Azure Container

[Istanze di Azure Container](https://azure.microsoft.com/services/container-instances/) è un'ottima soluzione per qualsiasi scenario che opera in contenitori isolati, ad esempio processi di compilazione, automazione di attività e applicazioni semplici. Sviluppo rapido di app senza gestire le macchine virtuali.

### <a name="azure-container-instances-key-features"></a>Funzionalità principali di Istanze di Azure Container

- Tempi di avvio rapido, in quanto le Istanze di Container possono avviare i contenitori in Azure in pochi secondi, senza dover effettuare il provisioning delle macchine virtuali o gestirle.
- Connettività tramite indirizzo IP pubblico e nome DNS personalizzato.
- Sicurezza a livello di hypervisor che garantisce all'applicazione in un contenitore lo stesso livello di isolamento di cui usufruirebbe in una macchina virtuale.
- Dimensioni personalizzate per un utilizzo ottimale grazie alla possibilità di specificare esattamente la quantità di memoria e core CPU. Si paga in base alle risorse necessarie con fatturazione al secondo ed è così possibile ottimizzare la spesa in base alle esigenze effettive.
- Archiviazione permanente per il recupero e la persistenza dello stato. Istanze di Container offre il montaggio diretto di condivisioni file di Azure.
- Contenitori Linux e Windows pianificati con la stessa API.

### <a name="azure-container-instances-references"></a>Informazioni di riferimento per Istanze di Azure Container

- [Azure portal](https://portal.azure.com/)
- [Documentazione di Istanze di Azure Container](/azure/container-instances/)
- [Guide introduttive](/azure/container-instances/container-instances-quickstart-portal)
- [Esempi](https://azure.microsoft.com/resources/samples/?sort=0&term=aci)
- [Esercitazioni](/azure/container-instances/container-instances-tutorial-prepare-app)