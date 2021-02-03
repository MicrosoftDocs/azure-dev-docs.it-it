---
title: Usare Azure SDK per Java
description: Panoramica delle funzionalità e delle funzionalità di Azure SDK per Java che consentono di aumentare la produttività durante il provisioning, l'uso e la gestione delle risorse di Azure.
author: jonathangiles
ms.date: 02/02/2021
ms.topic: conceptual
ms.custom: devx-track-java
ms.author: jogiles
ms.openlocfilehash: 64a13c99f58b109a066acd322aa90f39a74ea0e9
ms.sourcegitcommit: 71847ee0a1fee3f3320503629d9a8c82319a1f6a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/03/2021
ms.locfileid: "99522106"
---
# <a name="use-the-azure-sdk-for-java"></a>Usare Azure SDK per Java

Azure SDK per Java open source semplifica il provisioning, la gestione e l'uso delle risorse di Azure dal codice dell'applicazione Java.

## <a name="important-details"></a>Dettagli importanti

* Le librerie di Azure rappresentano la modalità di comunicazione con i servizi di Azure dal codice Java eseguito localmente o nel cloud.
* Le librerie supportano Java 8 e versioni successive e sono testate sia per la baseline Java 8 che per la versione più recente del supporto a lungo termine ' Java '.
* Le librerie includono il supporto completo dei moduli Java, il che significa che sono completamente conformi ai requisiti di un modulo Java ed esportano tutti i pacchetti rilevanti da usare.
* Azure SDK per Java è costituito esclusivamente da molte librerie Java diverse correlate a servizi specifici di Azure. Non sono presenti altri strumenti nell'SDK.
* Esistono librerie di "gestione" e "client" distinte (talvolta denominate librerie del "piano di gestione" e del "piano dati"). Ogni set serve a scopi diversi e viene usato da tipi di codice diversi. Per ulteriori informazioni, vedere le sezioni seguenti più avanti in questo articolo:
  * [Connettersi e usare le risorse di Azure con le librerie client.](#connect-to-and-use-azure-resources-with-client-libraries)
  * [Provisioning e gestione delle risorse di Azure con le librerie di gestione.](#provision-and-manage-azure-resources-with-management-libraries)
* È possibile trovare la documentazione per le librerie nei [riferimenti di Azure per Java](/java/api/overview/azure/) organizzati dal servizio di Azure o il [browser API Java](/java/api/) organizzato in base al nome del pacchetto.

## <a name="other-details"></a>Altri dettagli

* Le librerie di Azure SDK per Java si basano sull'API REST di Azure sottostante, consentendo di usare tali API tramite paradigmi Java noti. Tuttavia, se si preferisce, è sempre possibile usare l'API REST direttamente dal codice Java.
* È possibile trovare il codice sorgente per le librerie di Azure nel [repository GitHub](https://github.com/Azure/azure-sdk-for-java). Essendo un progetto open source, i contributi sono benvenuti.
* È in corso l'aggiornamento di Azure SDK per le librerie Java per condividere modelli Cloud comuni come protocolli di autenticazione, registrazione, traccia, protocolli di trasporto, risposte memorizzate nel buffer e tentativi.
  * Questa funzionalità condivida è contenuta nella libreria [azure-core](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/core/azure-core).
* Per altre informazioni sulle linee guida applicabili alle librerie, vedere le [linee guida di progettazione di Java Azure SDK](https://azure.github.io/azure-sdk/java_introduction.html).

## <a name="connect-to-and-use-azure-resources-with-client-libraries"></a>Connettersi e usare le risorse di Azure con le librerie client

Le librerie client (o "piano dati") consentono di scrivere il codice dell'applicazione Java per interagire con i servizi già sottoposti a provisioning. Le librerie client sono disponibili solo per i servizi che supportano un'API client. È possibile identificarli perché l'ID del gruppo Maven è `com.azure` .

Tutte le librerie client Java di Azure seguono lo stesso schema di progettazione dell'API per offrire una classe del compilatore Java responsabile della creazione di un'istanza di un client. Questo modello separa la definizione e la creazione di istanze del client dalla relativa operazione, consentendo al client di essere immutabile e quindi più semplice da usare. Inoltre, tutte le librerie client seguono alcuni modelli importanti:

* Le librerie client che supportano API sincrone e asincrone devono offrire queste API in classi separate. Ciò significa che in questi casi, ad esempio, per le `KeyVaultClient` API di sincronizzazione e `KeyVaultAsyncClient` per le API asincrone.

* Esiste una singola classe del generatore che si assume la responsabilità di creare sia la sincronizzazione sia le API asincrone. Il generatore è denominato in modo analogo alla classe del client di sincronizzazione, con `Builder` incluso. Ad esempio: `KeyVaultClientBuilder`. Questo generatore dispone `buildClient()` `buildAsyncClient()` di metodi e per creare istanze client, a seconda delle esigenze.

A causa di queste convenzioni, tutte le classi che terminano in non `Client` sono modificabili e forniscono operazioni per interagire con un servizio di Azure. Tutte le classi che terminano in `ClientBuilder` forniscono operazioni per la configurazione e la creazione di un'istanza di un determinato tipo di client.

### <a name="client-libraries-example"></a>Esempio di librerie client

Nell'esempio di codice seguente viene illustrato come creare un Key Vault sincrono `KeyClient` :

```java
KeyClient client = new KeyClientBuilder()
        .endpoint(<your Key Vault URL>)
        .credential(new DefaultAzureCredentialBuilder().build())
        .buildClient();
```

Nell'esempio di codice seguente viene illustrato come creare un Key Vault asincrono `KeyAsyncClient` :

```java
KeyAsyncClient client = new KeyClientBuilder()
        .endpoint(<your Key Vault URL>)
        .credential(new DefaultAzureCredentialBuilder().build())
        .buildAsyncClient();
```

Per altre informazioni sull'uso di ogni libreria client, vedere il file *Readme.MD* che si trova nella directory del progetto della libreria nel [repository GitHub dell'SDK](https://github.com/Azure/azure-sdk-for-java). È anche possibile trovare altri frammenti di codice nella [documentazione di riferimento](/java/api) e negli [esempi di Azure](/samples/browse/?products=azure&languages=java).

## <a name="provision-and-manage-azure-resources-with-management-libraries"></a>Effettuare il provisioning e la gestione di risorse di Azure con le librerie di gestione

Le librerie di gestione (o "piano di gestione") consentono di creare, effettuare il provisioning e gestire in altro modo le risorse di Azure dal codice dell'applicazione Java. È possibile trovare queste librerie nell' `com.azure.resourcemanager` ID gruppo Maven. Per tutti i servizi di Azure sono disponibili librerie di gestione corrispondenti.

Con le librerie di gestione è possibile scrivere script di configurazione e distribuzione per eseguire le stesse attività eseguibili tramite il [portale di Azure](https://portal.azure.com/) o l'[interfaccia della riga di comando di Azure](/cli/azure/install-azure-cli).

Tutte le librerie di gestione Java di Azure forniscono una `*Manager` classe come API del servizio, ad esempio per `ComputeManager` il servizio di calcolo di Azure o `AzureResourceManager` per l'aggregazione dei servizi più diffusi.

### <a name="management-libraries-example"></a>Esempio di librerie di gestione

Nell'esempio di codice seguente viene illustrato come creare un oggetto `ComputeManager` :

```java
ComputeManager computeManager = ComputeManager
    .authenticate(
        new DefaultAzureCredentialBuilder().build(),
        new AzureProfile(AzureEnvironment.AZURE));
```

Nell'esempio di codice seguente viene illustrato come eseguire il provisioning di una nuova macchina virtuale:

```java
VirtualMachine virtualMachine = computeManager.virtualMachines()
    .define(<your virtual machine>)
    .withRegion(Region.US_WEST)
    .withExistingResourceGroup(<your resource group>)
    .withNewPrimaryNetwork("10.0.0.0/28")
    .withPrimaryPrivateIPAddressDynamic()
    .withoutPrimaryPublicIPAddress()
    .withPopularLinuxImage(KnownLinuxVirtualMachineImage.UBUNTU_SERVER_18_04_LTS)
    .withRootUsername(<virtual-machine username>)
    .withSsh(<virtual-machine SSH key>)
    .create();
```

Nell'esempio di codice seguente viene illustrato come ottenere una macchina virtuale esistente:

```java
VirtualMachine virtualMachine = computeManager.virtualMachines()
    .getByResourceGroup(<your resource group>, <your virtual machine>);
```

Nell'esempio di codice seguente viene illustrato come aggiornare la macchina virtuale e aggiungere un nuovo disco dati:

```java
virtualMachine.update()
    .withNewDataDisk(10)
    .apply();
```

Per altre informazioni sull'uso di ogni libreria di gestione, vedere il file *Readme.MD* che si trova nella directory del progetto della libreria nel [repository GitHub dell'SDK](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/resourcemanager#readme). È anche possibile trovare altri frammenti di codice nella [documentazione di riferimento](/java/api) e negli [esempi di Azure](/samples/browse/?products=azure&languages=java).

## <a name="get-help-and-connect-with-the-sdk-team"></a>Ottenere assistenza e contattare il team dell'SDK

* Visitare la [documentazione di Azure SDK per Java](https://azure.github.io/azure-sdk-for-java/).
* Pubblicare le domande per la community in [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-sdk-for-java).
* Aprire i problemi con l'SDK nel [repository GitHub](https://github.com/Azure/azure-sdk-for-java/issues).
* Menzione [@AzureSDK](https://twitter.com/AzureSdk/) su Twitter.

## <a name="next-steps"></a>Passaggi successivi

Ora che si è appreso come è installato Azure SDK per Java, è possibile approfondire molti dei concetti trasversali esistenti per migliorare la produttività quando si usano le librerie. Gli articoli seguenti forniscono ottimi punti di partenza:

* [Pipeline e client HTTP](http-client-pipeline.md)
* [Programmazione asincrona](async-programming.md)
* [Impaginazione e iterazione](pagination.md)
* [Operazioni a esecuzione prolungata](lro.md)
* [Configurare i proxy](proxying.md)
* [Configurare la traccia](tracing.md)
