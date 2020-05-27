---
title: Flusso di sviluppo di Azure
description: Panoramica del ciclo di sviluppo per il cloud in Azure, che include provisioning, scrittura di codice, test, distribuzione e gestione.
ms.date: 05/12/2020
ms.topic: conceptual
ms.openlocfilehash: 577b813e2b4ccd8d2cae3d7999d9bb959f88adc2
ms.sourcegitcommit: 2cdf597e5368a870b0c51b598add91c129f4e0e2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/14/2020
ms.locfileid: "83404964"
---
# <a name="the-azure-development-flow-provision-code-test-deploy-and-manage"></a>Flusso di sviluppo di Azure: effettuare il provisioning, scrivere codice, testare, distribuire e gestire

[Articolo precedente: provisioning, accesso e gestione delle risorse](cloud-development-provisioning.md)

Ora che si conosce il modello di servizi e risorse di Azure, è possibile capire il flusso complessivo dello sviluppo di applicazioni cloud con Azure: **provisioning**, **scrittura di codice**, **test**, **distribuzione**e **gestione**.

## <a name="step-1-provision-and-configure-resources"></a>Passaggio 1: Effettuare il provisioning e configurare le risorse

Come descritto nell'[articolo precedente di questa serie](cloud-development-provisioning.md), il primo passaggio per lo sviluppo di un'applicazione consiste nell'effettuare il provisioning e la configurazione delle risorse che costituiscono l'ambiente di destinazione.

Il provisioning inizia con la creazione di un gruppo di risorse in un'area di Azure appropriata. È possibile creare un gruppo di risorse tramite il portale di Azure o l'interfaccia della riga di comando di Azure oppure con uno script personalizzato che usa Azure SDK (o API REST).

All'interno di tale gruppo di risorse, è quindi possibile effettuare il provisioning e la configurazione delle singole risorse necessarie, sempre usando il portale, l'interfaccia della riga di comando o Azure SDK. La configurazione include l'impostazione di criteri di accesso che controllano quali identità (entità servizio e/o ID applicazione) sono in grado di accedere a tali risorse.

Per la maggior parte degli scenari di sviluppo, è probabile che si creino script di provisioning con l'interfaccia della riga di comando di Azure e/o con codice Python usando Azure SDK. Tali script descrivono la totalità delle esigenze di risorse dell'applicazione e consentono di ricreare facilmente tali risorse all'interno di ambienti di sviluppo, test e produzione diversi, invece di eseguire manualmente molti passaggi ripetuti nel portale di Azure. Gli script facilitano inoltre il provisioning di un ambiente in un'area diversa o l'uso di gruppi di risorse diversi. È anche possibile gestire questi script nei repository del controllo del codice sorgente, in modo da avere il controllo completo e la cronologia delle modifiche.

## <a name="step-2-write-your-app-code-to-use-resources"></a>Passaggio 2: Scrivere il codice dell'app per usare le risorse

Una volta effettuato il provisioning delle risorse necessarie per l'applicazione, scrivere il codice per usare queste risorse, ad eccezione di quelle in cui si distribuisce il codice stesso.

Nel passaggio di provisioning, ad esempio, è possibile che sia stato creato un account di archiviazione di Azure con un contenitore BLOB al suo interno e che siano stati impostati i criteri di accesso per l'applicazione in tale contenitore. Dal codice è ora possibile eseguire l'autenticazione con l'account di archiviazione e quindi creare, aggiornare o eliminare BLOB all'interno del contenitore. Questo processo è illustrato in [Esempio - Usare Archiviazione di Azure](azure-sdk-example-storage.md). Analogamente, è possibile che sia stato effettuato il provisioning di un database con uno schema e le autorizzazioni appropriate, per cui il codice dell'applicazione può connettersi al database ed eseguire le consuete operazioni di creazione, lettura, aggiornamento ed eliminazione.

Gli sviluppatori Python possono in genere scrivere il codice dell'applicazione in Python usando Azure SDK per Python. Detto questo, qualsiasi parte indipendente di un'applicazione cloud può essere scritta in qualsiasi linguaggio supportato. Se si lavora in un team con varie competenze a livello di linguaggio, ad esempio, è possibile che alcune parti dell'applicazione vengano scritte in Python, altre in JavaScript, altre in Java e altre ancora in C#.

Si noti che il codice dell'applicazione può usare Azure SDK per le operazioni di provisioning e gestione, se necessario. Gli script di provisioning, analogamente, possono usare l'SDK per inizializzare le risorse con dati specifici o eseguire attività di manutenzione sulle risorse cloud anche quando tali script vengono eseguiti localmente.

## <a name="step-3-test-and-debug-your-app-code-locally"></a>Passaggio 3: Eseguire test e debug del codice dell'app in locale

Gli sviluppatori in genere preferiscono testare il codice dell'app nelle workstation locali prima di distribuirlo nel cloud. Per eseguire i test del codice dell'app in locale solitamente si accede ad altre risorse di cui è già stato effettuato il provisioning nel cloud, come risorse di archiviazione, database e così via. La differenza è che non si esegue ancora il codice dell'app stesso all'interno di un servizio cloud.

Eseguendo il codice localmente, è anche possibile sfruttare appieno le funzionalità di debug offerte da strumenti come Visual Studio Code e gestire il codice in un repository del controllo del codice sorgente.

Non è necessario modificare il codice per i test locali: Azure include il supporto completo per lo sviluppo e il debug in locale con lo stesso codice distribuito nel cloud. Le variabili di ambiente sono la chiave: nel cloud il codice può accedere alle impostazioni delle risorse di hosting come variabili di ambiente. Creando queste stesse variabili di ambiente in locale, lo stesso codice viene eseguito senza modifiche. Questo modello è valido per le credenziali di autenticazione, gli URL delle risorse, le stringhe di connessione e un numero qualsiasi di altre impostazioni, semplificando l'uso delle risorse in un ambiente di sviluppo quando si esegue il codice in locale e quello delle risorse di produzione una volta distribuito il codice nel cloud.

## <a name="step-4-deploy-your-app-code-to-azure"></a>Passaggio 4: Distribuire il codice dell'app in Azure

Dopo aver testato il codice in locale, è possibile distribuirlo per ospitarlo nella risorsa di Azure di cui è stato effettuato il provisioning. Se ad esempio si scrive un'applicazione Web Django, è possibile distribuire il codice in una macchina virtuale (per cui si fornisce il proprio server Web) o nel servizio app di Azure (che fornisce automaticamente il server Web). Una volta distribuito, il codice viene eseguito nel server invece che nel computer locale ed è possibile accedere a tutte le risorse di Azure per cui è autorizzato.

Come indicato nella sezione precedente, nei tipici processi di sviluppo il codice viene prima distribuito alle risorse di cui è stato effettuare il provisioning in un ambiente di sviluppo. Dopo un ciclo di test, il codice viene distribuito alle risorse in un ambiente di staging, rendendo l'applicazione disponibile ai team di test e magari ai clienti dell'anteprima. Quando si è soddisfatti delle prestazioni dell'applicazione, è possibile distribuire il codice nell'ambiente di produzione. Tutte queste distribuzioni possono anche essere automatizzate tramite integrazione continua e distribuzione continua usando Azure DevOps.

Comunque si esegua questa operazione, una volta distribuito nel cloud il codice diventa una vera e propria applicazione cloud, eseguita interamente nei computer server dei data center di Azure.

## <a name="step-5-manage-monitor-and-revise"></a>Passaggio 5: Gestire, monitorare e ottimizzare

Dopo la distribuzione, è necessario assicurarsi che l'applicazione venga eseguita come dovrebbe, rispondendo alle richieste dei clienti e usando in modo efficiente le risorse (e al costo più basso possibile). È possibile gestire il modo in cui Azure ridimensiona la distribuzione secondo necessità ed è possibile raccogliere e monitorare i dati sulle prestazioni tramite il portale di Azure, l'interfaccia della riga di comando di Azure o script personalizzati scritti con Azure SDK. È quindi possibile apportare modifiche in tempo reale alle risorse di cui è stato effettuato il provisioning per ottimizzarne le prestazioni, sempre usando uno di questi stessi strumenti.

Il monitoraggio offre informazioni dettagliate su come ristrutturare eventualmente l'applicazione cloud. Ad esempio, si potrebbe notare che alcune parti di un'app Web, come un gruppo di endpoint API, vengono usate solo occasionalmente rispetto alle parti principali. È quindi possibile scegliere di distribuire tali API separatamente come Funzioni di Azure serverless, in cui avranno risorse di calcolo di supporto specifiche senza entrare in conflitto con l'applicazione principale ma disponibili a un costo mensile minimo. L'applicazione principale diventa quindi più reattiva per più clienti, senza la necessità di passare a un livello di costo più alto.

## <a name="next-steps"></a>Passaggi successivi

A questo punto si ha familiarità con la struttura di base di Azure e con il flusso di sviluppo generale: effettuare il provisioning di risorse, scrivere e testare il codice, distribuire il codice in Azure e quindi monitorare e gestire tali risorse.

Il passaggio successivo consiste nel fare in modo che la workstation sia completamente configurata per funzionare con tale flusso, dopodiché si è pronti per iniziare a usare Azure SDK.

> [!div class="nextstepaction"]
> [Configurare l'ambiente di sviluppo locale >>>](configure-local-development-environment.md)
