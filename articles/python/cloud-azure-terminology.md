---
title: Scheda di riferimento rapido per la terminologia di Azure
description: Breve elenco dei termini e concetti più importanti che è necessario conoscere quando si usa Microsoft Azure.
ms.date: 12/07/2020
ms.topic: conceptual
ms.custom: devx-track-python
ms.openlocfilehash: f79864b39bb70c7ae703468f205e420417698200
ms.sourcegitcommit: c8330128d5d6a71859933a890ecdf047cb950996
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/15/2020
ms.locfileid: "97521982"
---
# <a name="azure-terminology-in-brief"></a>Terminologia di Azure in breve

| Termine | Breve descrizione |
| --- | --- |
| [Account e sottoscrizioni](#account-and-subscriptions) | Informazioni di fatturazione e struttura di base dell'organizzazione per gestire le risorse in Azure. [Altro...](#account-and-subscriptions)
| [Risorsa](#resource) | Nome generale di qualsiasi allocazione specifica di funzionalità in un data center di Azure. [Altro...](#resource) |
| [Gruppo di risorse](#resource-group) | Contenitore logico per altre risorse che possono essere gestite successivamente come unità. [Altro...](#resource-group) |
| [Area](#region-location) | Riferimento a un data center di Azure specifico in cui sono allocate le risorse. [Altro...](#region-location) |
| [Servizio app di Azure](#azure-app-service) | Servizio di hosting gestito di Azure per applicazioni Web. [Altro...](#azure-app-service) |
| [Piano di servizio app](#app-service-plan) | Risorsa che definisce la macchina virtuale usata dal Servizio app di Azure. [Altro...](#app-service-plan) |
| [Azure portal](#azure-portal) | Interfaccia utente basata sul Web per la creazione e la gestione di risorse di Azure. [Altro...](#azure-portal) |
| [Interfaccia della riga di comando di Azure](#azure-command-line-interface-cli) | Set di comandi basati su testo usati per creare e gestire risorse di Azure. [Altro...](#azure-command-line-interface-cli) |
| [az webapp, az webapp up](#az-webapp-az-webapp-up) | Comandi dell'interfaccia della riga di comando di Azure per usare il Servizio app di Azure. [Altro...](#az-webapp-az-webapp-up) |

## <a name="account-and-subscriptions"></a>Account e sottoscrizioni

Un **account Azure** contiene le informazioni di contatto di base (numero di telefono, indirizzo di posta elettronica) e le informazioni di fatturazione (carta di credito) dell'utente.

In un singolo account di fatturazione è possibile organizzare le attività in una o più **sottoscrizioni**. Ogni sottoscrizione ha autorizzazioni specifiche ed è quindi possibile creare sottoscrizioni separate per singoli utenti o reparti che rimangono nello stesso account. Un singolo utente può accedere a qualsiasi numero di sottoscrizioni.

La creazione di una risorsa in Azure viene sempre eseguita nel contesto di una sottoscrizione. È possibile [spostare le risorse tra le sottoscrizioni](/azure/azure-resource-manager/management/move-resource-group-and-subscription) ma non tra gli account.

## <a name="resource"></a>Risorsa

Una **risorsa** è il nome generale di qualsiasi allocazione specifica di funzionalità in un data center di Azure. Una risorsa può essere una macchina virtuale, una rete virtuale, diversi livelli di archiviazione, un database, un modello di Machine Learning, un hub di inserimento IoT e così via.

Poiché una risorsa alloca capacità di calcolo effettive, ogni risorsa comporta potenzialmente un costo continuo in base al livello di prestazioni necessario. Per lo sviluppo e il test è possibile creare molte risorse con un livello gratuito. Per altre informazioni, vedere il [Calcolatore dei prezzi](https://azure.microsoft.com/pricing/calculator/).

## <a name="resource-group"></a>Resource group

Un *gruppo di risorse* in una sottoscrizione è un contenitore logico per altre risorse che possono essere gestite successivamente come unità.

Un gruppo di risorse è in genere correlato a un progetto specifico ed è necessario specificare sempre un gruppo di risorse quando si effettua il provisioning di una risorsa. Il primo passaggio con un nuovo progetto consiste in genere nel creare un gruppo di risorse appropriato.

Se si elimina un gruppo di risorse, tutte le risorse contenute correlate verranno deallocate ed eliminate. Questo approccio risulta più semplice rispetto a eliminare individualmente ogni risorsa.

## <a name="region-location"></a>Area (posizione)

Un'**area** identifica la posizione specifica del data center di Azure in cui è stato effettuato il provisioning di una risorsa.

Diverse risorse possono sempre comunicare tra le aree, ma la comunicazione risulta più efficiente quando le risorse sono posizionate nella stessa area.

Le applicazioni rivolte a clienti globali possono consentire ad Azure di replicare automaticamente le risorse in più aree per migliorare la velocità di risposta e le prestazioni complessive dell'applicazione.

## <a name="azure-app-service"></a>Servizio app di Azure

Il **Servizio app** è il servizio di hosting gestito di Azure per applicazioni Web. Azure gestisce tutta l'infrastruttura hardware e server sottostante. L'utente fornisce il codice e la configurazione.

Il Servizio app consente di effettuare il provisioning dell'host, definito **app Web** del Servizio app, e di caricare il codice. È anche possibile configurare diverse caratteristiche dell'host, tra cui il bilanciamento del carico, la scalabilità, le variabili di ambiente lato server e altro ancora.

L'URL diretto per un'app Web del Servizio app è sempre `<web-app-name>.azurewebsites.net`. È anche possibile configurare l'app Web per l'uso di un dominio personalizzato.

## <a name="app-service-plan"></a>Piano di servizio app

Un **piano di servizio app** è una risorsa che definisce la macchina virtuale usata dal Servizio app di Azure, che determina quindi il costo di base per l'hosting dell'applicazione. Il piano di servizio app viene definito come parte del provisioning del Servizio app prima della distribuzione del codice.

## <a name="azure-portal"></a>Portale di Azure

Il [portale di Azure](https://portal.azure.com) è l'interfaccia utente basata sul Web che consente di usare l'account, le sottoscrizioni e le risorse di Azure.

## <a name="azure-command-line-interface-cli"></a>interfaccia della riga di comando di Azure (CLI)

L'[interfaccia della riga di comando di Azure](/cli/azure/what-is-azure-cli) è un set di comandi che consente di creare e gestire le risorse di Azure ed è particolarmente utile per l'automazione. L'interfaccia della riga di comando di Azure è disponibile in tutti i sistemi operativi ed è compatibile con la maggior parte dei servizi di Azure.

Se si preferisce usare PowerShell, è possibile usare in alternativa il [modulo di Azure PowerShell](/powershell/azure).

## <a name="az-webapp-az-webapp-up"></a>az webapp, az webapp up

Il comando **az webapp** consente di interagire con tutti gli aspetti del Servizio app di Azure tramite l'interfaccia della riga di comando di Azure.

Il comando **az webapp up**, in particolare, semplifica la distribuzione di applicazioni Web. Questo singolo comando consente di effettuare il provisioning di un gruppo di risorse, di un piano di servizio app e dell'app Web del Servizio app e quindi di caricare il codice, tutto in un solo passaggio.

## <a name="next-step"></a>Passaggio successivo

> [!div class="nextstepaction"]
> [Panoramica dello sviluppo cloud >>>](cloud-development-overview.md)
