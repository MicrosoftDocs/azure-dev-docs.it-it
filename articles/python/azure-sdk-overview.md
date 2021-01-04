---
title: Usare le librerie di Azure (SDK) per Python
description: Panoramica delle caratteristiche e delle funzionalità delle librerie di Azure per Python che consentono agli sviluppatori di aumentare la produttività per il provisioning, l'uso e la gestione di risorse di Azure.
ms.date: 09/19/2020
ms.topic: conceptual
ms.custom: devx-track-python
ms.openlocfilehash: d610099b3b877f0916079ca2000a5268f3f08c2a
ms.sourcegitcommit: c8330128d5d6a71859933a890ecdf047cb950996
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/15/2020
ms.locfileid: "97522005"
---
# <a name="use-the-azure-libraries-sdk-for-python"></a>Usare le librerie di Azure (SDK) per Python

Le librerie di Azure open source per Python semplificano il provisioning, la gestione e l'uso di risorse di Azure dal codice applicativo Python.

## <a name="the-details-you-really-want-to-know"></a>Informazioni che è necessario conoscere

- Le librerie di Azure stabiliscono la modalità di comunicazione con i servizi di Azure *dal* codice Python eseguito localmente o nel cloud. Tenere presente che è possibile eseguire codice Python nell'ambito di un determinato servizio solo se tale servizio supporta attualmente Python.

- Le librerie supportano Python 2.7 e Python 3.5.3 o versioni successive e vengono testate anche con PyPy 5.4 e versioni successive.

- Azure SDK per Python è costituito da oltre 180 singole librerie Python correlate a servizi specifici di Azure. Non sono presenti altri strumenti nell'SDK.

- Quando si esegue il codice localmente, l'autenticazione con Azure si basa su variabili di ambiente, come descritto in [Configurare l'ambiente di sviluppo Python locale per Azure](configure-local-development-environment.md). 

- Installare i pacchetti di librerie necessari con `pip install <library_name>`, usando i nomi delle librerie disponibili nell'[indice dei pacchetti delle librerie di Azure SDK per Python](azure-sdk-library-package-index.md). Per altre informazioni, vedere [Installare le librerie di Azure](azure-sdk-install.md).

- Esistono librerie di **gestione** e **client** distinte (talvolta denominate librerie del "piano di gestione" e del "piano dati"). Ogni set serve a scopi diversi e viene usato da tipi di codice diversi. Per altre informazioni, vedere le sezioni seguenti più avanti in questo articolo:
  - [Effettuare il provisioning e la gestione di risorse di Azure con le librerie di gestione](#provision-and-manage-azure-resources-with-management-libraries)
  - [Connettersi e usare le risorse di Azure con le librerie client](#connect-to-and-use-azure-resources-with-client-libraries)

- La documentazione relativa alle librerie è disponibile nelle [informazioni di riferimento di Azure per Python](/python/api/overview/azure/), organizzate in base a servizio di Azure, oppure nel [browser delle API Python](/python/api/), organizzato in base al nome del pacchetto. Al momento, è spesso necessario fare clic su diversi livelli per ottenere le classi e i metodi a cui si è interessati. Ci scusiamo in anticipo per questa esperienza poco soddisfacente. Stiamo lavorando per migliorarla.

- Per provare le librerie, è consigliabile prima di tutto [configurare l'ambiente di sviluppo locale](configure-local-development-environment.md). È quindi possibile provare uno dei seguenti esempi autonomi (in qualsiasi ordine): [Esempio: Effettuare il provisioning di un gruppo di risorse](azure-sdk-example-resource-group.md), [Esempio: Effettuare il provisioning e usare Archiviazione di Azure](azure-sdk-example-storage.md), [Esempio: Effettuare il provisioning di un app Web e distribuire il codice](azure-sdk-example-web-app.md), [Esempio: Effettuare il provisioning e usare un database MySQL](azure-sdk-example-database.md) e [Esempio: Effettuare il provisioning di una macchina virtuale](azure-sdk-example-virtual-machines.md).

- Per una dimostrazione, vedere il video che spiega come <a href="https://www.youtube.com/watch?v=M1pVxItg2Mg&feature=youtu.be&ocid=AID3006292" target="_blank">usare gli SDK di Azure per interagire con le risorse di Azure</a> (youtube.com) tratto dalla conferenza virtuale PyCon 2020.

### <a name="non-essential-but-still-interesting-details"></a>Dettagli non essenziali ma comunque interessanti

- Poiché l'interfaccia della riga di comando di Azure è scritta in Python usando le librerie di gestione, qualsiasi operazione eseguibile con i comandi dell'interfaccia della riga di comando di Azure può essere completata anche con uno script Python. Detto questo, i comandi dell'interfaccia della riga di comando offrono molte funzionalità utili, ad esempio l'esecuzione simultanea di più attività, la gestione automatica di operazioni asincrone, la formattazione dell'output come stringhe di connessione e così via. Di conseguenza, l'uso dell'interfaccia della riga di comando (o dell'equivalente Azure PowerShell) per gli script automatizzati di provisioning e gestione può risultare notevolmente più agevole rispetto alla scrittura di codice Python equivalente, a meno che non serva un grado di controllo molto più rigoroso sul processo.

- Le librerie di Azure per Python si basano sull'API REST di Azure sottostante, consentendo di usare tali API tramite paradigmi Python ben noti. Tuttavia, se si preferisce, è sempre possibile usare l'API REST direttamente dal codice Python.

- Il codice sorgente delle librerie di Azure è disponibile all'indirizzo [https://github.com/Azure/azure-sdk-for-python](https://github.com/Azure/azure-sdk-for-python). Essendo un progetto open source, i contributi sono benvenuti.

- Anche se è possibile usare le librerie con interpreti come IronPython e Jython che non vengono testati, si potrebbero riscontrare incompatibilità e problemi isolati.

- Il repository di origine per la documentazione di riferimento delle API delle librerie si trova all'indirizzo [https://github.com/MicrosoftDocs/azure-docs-sdk-python/](https://github.com/MicrosoftDocs/azure-docs-sdk-python/).

- È in corso l'aggiornamento delle librerie di Azure per Python per condividere modelli cloud comuni come protocolli di autenticazione, registrazione, traccia, protocolli di trasporto, risposte memorizzate nel buffer e tentativi di ripetizione.

  - Questa funzionalità condivida è contenuta nella libreria [azure-core](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/core/azure-core).

  - Le librerie attualmente compatibili con la libreria Core sono riportate nella pagina [Versioni più recenti di Azure SDK per Python](azure-sdk-library-package-index.md#libraries-using-azurecore). Queste librerie, principalmente le librerie client, sono a volte identificate come "track 2".

  - Le librerie di gestione, oltre a eventuali altre non ancora aggiornate, sono a volte identificate come "track 1".

- Per informazioni dettagliate sulle linee guida applicabili alle librerie, vedere [Linee guida di Python: introduzione](https://azure.github.io/azure-sdk/python_introduction.html).

## <a name="provision-and-manage-azure-resources-with-management-libraries"></a>Effettuare il provisioning e la gestione di risorse di Azure con le librerie di gestione

Le librerie di *gestione* (o "piano di gestione") dell'SDK, i cui nomi iniziano con `azure-mgmt-`, consentono di creare, sottoporre a provisioning e altrimenti gestire le risorse di Azure da script Python. Per tutti i servizi di Azure sono disponibili librerie di gestione corrispondenti.

Con le librerie di gestione è possibile scrivere script di configurazione e distribuzione per eseguire le stesse attività eseguibili tramite il [portale di Azure](https://portal.azure.com) o l'[interfaccia della riga di comando di Azure](/cli/azure/install-azure-cli). Come indicato in precedenza, l'interfaccia della riga di comando di Azure è scritta in Python e usa le librerie di gestione per implementare i vari comandi.

I seguenti esempi illustrano come usare alcune delle librerie di gestione primarie:

- [Effettuare il provisioning di un gruppo di risorse](azure-sdk-example-resource-group.md)
- [Elencare i gruppi di risorse in una sottoscrizione](azure-sdk-example-list-resource-groups.md)
- [Effettuare il provisioning di Archiviazione di Azure](azure-sdk-example-storage.md)
- [Effettuare il provisioning di un'app Web e distribuire il codice](azure-sdk-example-web-app.md)
- [Effettuare il provisioning ed eseguire query su un database](azure-sdk-example-database.md)
- [Effettuare il provisioning di una macchina virtuale](azure-sdk-example-virtual-machines.md)

Per informazioni dettagliate sull'uso di ogni libreria di gestione, vedere il file *README.md* o *README.rst* disponibile nella cartella di progetto della libreria nel [repository GitHub dell'SDK](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk). È anche possibile trovare frammenti di codice aggiuntivi nella [documentazione di riferimento](/python/api) e negli [esempi di Azure](/samples/browse/?languages=python&term=Getting%20started%20-%20Managing).

### <a name="migrating-from-older-management-libraries"></a>Migrazione dalle librerie di gestione precedenti

Se si esegue la migrazione del codice da versioni precedenti delle librerie di gestione, vedere i dettagli seguenti:

- Se si usa la classe `ServicePrincipalCredentials`, vedere [Come eseguire l'autenticazione - Usare le credenziali del token](azure-sdk-authenticate.md#authenticate-with-token-credentials).
- I nomi delle API asincrone sono stati modificati come descritto in [Modelli di utilizzo delle librerie - Operazioni asincrone](azure-sdk-library-usage-patterns.md#asynchronous-operations). Più semplicemente, i nomi delle API asincrone nelle librerie più recenti iniziano con `begin_`. Nella maggior parte dei casi, la firma dell'API rimane invariata.

## <a name="connect-to-and-use-azure-resources-with-client-libraries"></a>Connettersi e usare le risorse di Azure con le librerie client

Le librerie *client* (o "piano dati") dell'SDK consentono di scrivere il codice applicativo Python per interagire con servizi di cui è già stato effettuato il provisioning. Le librerie client sono disponibili solo per i servizi che supportano un'API client.

L'articolo [Esempio: Usare Archiviazione di Azure](azure-sdk-example-storage-use.md) illustra in modo semplice l'uso di una libreria client.

Anche diversi servizi di Azure forniscono esempi di uso di queste librerie. Per vedere collegamenti, vedere le pagine di indice seguenti:

- [Hosting di app](quickstarts-app-hosting.md)
- [Servizi cognitivi](quickstarts-cognitive-services.md)
- [Soluzioni per i dati](quickstarts-data-solutions.md)
- [Identità e sicurezza](quickstarts-identity-security.md)
- [Machine Learning](quickstarts-machine-learning.md)
- [Messaggistica e IoT](quickstarts-messaging-iot.md)
- [Altri servizi](quickstarts-other-services.md)

Per informazioni dettagliate sull'uso di ogni libreria client, vedere il file *README.md* o *README.rst* disponibile nella cartella di progetto della libreria nel [repository GitHub dell'SDK](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk). È anche possibile trovare frammenti di codice aggiuntivi nella [documentazione di riferimento](/python/api) e negli [esempi di Azure](/samples/browse/?languages=python&products=azure).

## <a name="get-help-and-connect-with-the-sdk-team"></a>Ottenere assistenza e contattare il team dell'SDK

- Vedere la [documentazione delle librerie di Azure per Python](https://aka.ms/python-docs)
- Pubblicare le domande per la community in [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-sdk-python).
- Aprire i problemi relativi all'SDK in [GitHub](https://github.com/Azure/azure-sdk-for-python/issues)
- Menzionare [@AzureSDK](https://twitter.com/AzureSdk/) su Twitter

## <a name="next-step"></a>Passaggio successivo

È consigliabile eseguire una configurazione una tantum dell'ambiente di sviluppo locale in modo da usare facilmente qualsiasi libreria di Azure per Python.

> [!div class="nextstepaction"]
> [Configurare l'ambiente di sviluppo locale >>>](configure-local-development-environment.md)
