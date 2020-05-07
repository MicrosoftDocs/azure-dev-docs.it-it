---
title: Estensioni disponibili per l'interfaccia della riga di comando di Azure
description: Un elenco completo delle estensioni supportate ufficialmente per l'interfaccia della riga di comando di Azure.
author: haroldrandom
ms.author: jianzen
manager: yonzhan,yungezz
ms.date: 04/16/2020
ms.topic: article
ms.prod: azure
ms.technology: azure-cli
ms.devlang: azure-cli
ms.openlocfilehash: 531ea056b276cc12d5807ed62baa4d3c6044c26e
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/05/2020
ms.locfileid: "82030953"
---
# <a name="available-extensions-for-the-azure-cli"></a>Estensioni disponibili per l'interfaccia della riga di comando di Azure

Questo articolo offre un elenco completo delle estensioni disponibili per l'interfaccia della riga di comando di Azure che sono supportate da Microsoft.

L'elenco delle estensioni è accessibile anche dall'interfaccia della riga di comando. Per recuperarlo, eseguire [az extension list-available](/cli/azure/extension?view=azure-cli-latest#az-extension-list-available):

```azurecli-interactive
az extension list-available --output table
```

| Nome | Versione | Summary | Anteprima |
|------|---------|---------|---------|
| [aem](https://github.com/Azure/azure-cli-extensions) | 0.1.1 | Gestione delle estensioni di monitoraggio avanzato di Azure per SAP |  |
| [ai-examples](https://github.com/Azure/azure-cli-extensions/tree/master/src/ai-examples) | 0.2.0 | Aggiunta di esempi basati sull'intelligenza artificiale al contenuto della Guida. | Sì |
| [aks-preview](https://github.com/Azure/azure-cli-extensions/tree/master/src/aks-preview) | 0.4.43 | Fornisce un'anteprima per le funzionalità servizio Azure Kubernetes presto disponibili | Sì |
| [alertsmanagement](https://github.com/Azure/azure-cli-extensions) | 0.1.0 | Estensione Alerts degli strumenti da riga di comando di Microsoft Azure |  |
| [alias](https://github.com/Azure/azure-cli-extensions) | 0.5.2 | Supporto per gli alias dei comandi | Sì |
| [application-insights](https://github.com/Azure/azure-cli-extensions/tree/master/src/application-insights) | 0.1.6 | Supporto per la gestione dei componenti di Application Insights e l'esecuzione di query per metriche, eventi e log da tali componenti. | Sì |
| [azure-batch-cli-extensions](https://github.com/Azure/azure-batch-cli-extensions) | 5.0.1 | Comandi aggiuntivi per l'uso del servizio Azure Batch |  |
| [azure-cli-iot-ext](https://github.com/azure/azure-iot-cli-extension) | 0.8.9 | L'estensione IoT di Azure per l'interfaccia della riga di comando di Azure. |  |
| [azure-cli-ml](https://docs.microsoft.com/azure/machine-learning/service/) | 1.3.0 | Modulo comandi AzureML degli strumenti da riga di comando di Microsoft Azure |  |
| [azure-devops](https://github.com/Microsoft/azure-devops-cli-extension) | 0.18.0 | Strumenti per la gestione di Azure DevOps. |  |
| [azure-firewall](https://github.com/Azure/azure-cli-extensions/tree/master/src/azure-firewall) | 0.3.1 | Gestione delle risorse di Firewall di Azure. | Sì |
| [azure-iot](https://github.com/azure/azure-iot-cli-extension) | 0.9.1 | L'estensione IoT di Azure per l'interfaccia della riga di comando di Azure. |  |
| [blueprint](https://github.com/Azure/azure-cli-extensions) | 0.1.0 | Estensione Blueprint degli strumenti da riga di comando di Microsoft Azure |  |
| [connectedmachine](https://github.com/Azure/azure-cli-extensions) | 0.1.1 | Estensione connectedmachine degli strumenti da riga di comando di Microsoft Azure | Sì |
| [connection-monitor-preview](https://github.com/Azure/azure-cli-extensions/tree/master/src/connection-monitor-preview) | 0.1.0 | Estensione Monitoraggio connessione da riga di comando di Microsoft Azure v2 | Sì |
| [csvmware](https://github.com/Azure/az-vmware-cli) | 0.3.0 | Gestire la soluzione Azure VMware di CloudSimple. | Sì |
| [databox](https://github.com/Azure/azure-cli-extensions) | 0.1.0 | Estensione DataBox degli strumenti da riga di comando di Microsoft Azure |  |
| [databricks](https://github.com/Azure/azure-cli-extensions) | 0.2.0 | Estensione DatabricksClient degli strumenti da riga di comando di Microsoft Azure |  |
| [db-up](https://github.com/Azure/azure-cli-extensions/tree/master/src/db-up) | 0.1.13 | Altri comandi per semplificare i flussi di lavoro del database di Azure. | Sì |
| [deploy-to-azure](https://github.com/Azure/deploy-to-azure-cli-extension) | 0.2.0 | Distribuire in Azure con azioni di GitHub. | Sì |
| [dev-spaces](https://github.com/Azure/azure-cli-extensions) | 1.0.5 | Dev Spaces offre un'esperienza di sviluppo Kubernetes rapida e iterativa per i team. |  |
| [dev-spaces-preview](https://github.com/Azure/azure-cli-extensions) | 0.1.6 | Dev Spaces offre un'esperienza di sviluppo Kubernetes rapida e iterativa per i team. | Sì |
| [dms-preview](https://github.com/Azure/azure-cli-extensions/tree/master/src/dms-preview) | 0.11.0 | Supporto per i nuovi scenari del Servizio Migrazione del database. | Sì |
| [eventgrid](https://github.com/Azure/azure-cli-extensions) | 0.4.7 | Modulo di comandi EventGrid degli strumenti della riga di comando di Microsoft Azure. | Sì |
| [express-route](https://github.com/Azure/azure-cli-extensions/tree/master/src/express-route) | 0.1.3 | Gestione di ExpressRoute con funzionalità di anteprima. | Sì |
| [express-route-cross-connection](https://github.com/Azure/azure-cli-extensions/tree/master/src/express-route-cross-connection) | 0.1.1 | Gestire circuiti ExpressRoute dei clienti con una cross connection ExpressRoute. |  |
| [front-door](https://github.com/Azure/azure-cli-extensions/tree/master/src/front-door) | 1.0.6 | Gestione delle frontdoor di rete. |  |
| [hack](https://github.com/Azure/azure-cli-extensions) | 0.4.2 | Estensione Hack degli strumenti da riga di comando di Microsoft Azure | Sì |
| [healthcareapis](https://github.com/Azure/azure-cli-extensions) | 0.1.3 | Estensione HealthCareApis degli strumenti da riga di comando di Microsoft Azure |  |
| [hpc-cache](https://github.com/Azure/azure-cli-extensions) | 0.1.0 | Estensione StorageCache degli strumenti da riga di comando di Microsoft Azure | Sì |
| [image-copy-extension](https://github.com/Azure/azure-cli-extensions) | 0.2.3 | Supporto per la copia di immagini di macchina virtuale gestita tra aree |  |
| [interactive](https://github.com/Azure/azure-cli) | 0.4.4 | Shell interattiva della riga di comando di Microsoft Azure | Sì |
| [internet-analyzer](https://github.com/Azure/azure-cli-extensions) | 0.1.0rc5 | Estensione internet-analyzer degli strumenti da riga di comando di Microsoft Azure | Sì |
| [ip-group](https://github.com/Azure/azure-cli-extensions) | 0.1.1 | Estensione ip-group degli strumenti da riga di comando di Microsoft Azure |  |
| [keyvault-preview](https://github.com/Azure/azure-keyvault-cli-extension) | 0.1.3 | Anteprima dei comandi di Azure Key Vault. | Sì |
| [log-analytics](https://github.com/Azure/azure-cli-extensions/tree/master/src/log-analytics) | 0.1.4 | Supporto per le funzionalità di query di Azure Log Analytics. | Sì |
| [maintenance](https://github.com/Azure/azure-cli-extensions) | 1.0.1 | Supporto per la gestione della manutenzione di Azure. |  |
| [managementpartner](https://github.com/Azure/azure-cli-extensions) | 0.1.2 | Supporto per l'anteprima di Management Partner |  |
| [mesh](https://github.com/Azure/azure-cli-extensions) | 0.10.6 | Supporto per Microsoft Azure Service Fabric Mesh - Anteprima pubblica | Sì |
| [mixed-reality](https://github.com/Azure/azure-cli-extensions) | 0.0.1 | Estensione dell'interfaccia della riga di comando di Azure per la realtà mista. |  |
| [netappfiles-preview](https://github.com/Azure/azure-cli-extensions/tree/master/src/netappfiles-preview) | 0.3.2 | Fornisce un'anteprima per le funzionalità di Azure NetApp Files (ANF) presto disponibili. | Sì |
| [notification-hub](https://github.com/Azure/azure-cli-extensions) | 0.2.0 | Estensione Hub di notifica degli strumenti da riga di comando di Microsoft Azure |  |
| [peering](https://github.com/Azure/azure-cli-extensions) | 0.1.0rc2 | Estensione peering degli strumenti da riga di comando di Microsoft Azure | Sì |
| [powerbidedicated](https://github.com/Azure/azure-cli-extensions) | 0.1.1 | Estensione PowerBIDedicated degli strumenti da riga di comando di Microsoft Azure | Sì |
| [privatedns](https://github.com/Azure/azure-cli-extensions) | 0.1.1 | Comandi per gestire zone DNS private | Sì |
| [resource-graph](https://github.com/Azure/azure-cli-extensions/tree/master/src/resource-graph) | 1.1.0 | Supporto per l'esecuzione di query sulle risorse di Azure con Resource Graph. | Sì |
| [sap-hana](https://github.com/Azure/azure-hanaonazure-cli-extension) | 0.5.9 | Comandi aggiuntivi per l'uso delle istanze di SAP Hana in Azure. |  |
| [spring-cloud](https://github.com/Azure/azure-cli-extensions) | 0.2.2 | Estensione spring-cloud degli strumenti da riga di comando di Microsoft Azure | Sì |
| [storage-preview](https://github.com/Azure/azure-cli-extensions/tree/master/src/storage-preview) | 0.2.10 | Fornisce un'anteprima per le funzionalità di archiviazione presto disponibili. | Sì |
| [storagesync](https://github.com/Azure/azure-cli-extensions) | 0.1.0 | Estensione MicrosoftStorageSync degli strumenti da riga di comando di Microsoft Azure |  |
| [stream-analytics](https://github.com/Azure/azure-cli-extensions) | 0.1.0 | Estensione stream-analytics degli strumenti da riga di comando di Microsoft Azure |  |
| [sottoscrizione](https://github.com/Azure/azure-cli-extensions) | 0.1.3 | Supporto per l'anteprima della gestione delle sottoscrizioni |  |
| [support](https://github.com/azure/azure-cli-extensions/tree/master/src/support) | 1.0.1 | Estensione Supporto degli strumenti da riga di comando di Microsoft Azure |  |
| [synapse](https://github.com/Azure/azure-cli-extensions) | 0.1.0 | Estensione Synapse degli strumenti da riga di comando di Microsoft Azure | Sì |
| [virtual-network-tap](https://github.com/Azure/azure-cli-extensions/tree/master/src/virtual-network-tap) | 0.1.0 | Gestione di TAP di rete virtuale (VTAP). | Sì |
| [virtual-wan](https://github.com/Azure/azure-cli-extensions/tree/master/src/virtual-wan) | 0.1.3 | Gestione di WAN virtuale, hub, gateway VPN e siti VPN. | Sì |
| [vm-repair](https://github.com/Azure/azure-cli-extensions/tree/master/src/vm-repair) | 0.2.6 | Comandi di correzione automatica per le macchine virtuali. |  |
| [vmware](https://github.com/virtustream/azure-vmware-virtustream-cli-extension) | 0.5.5 | Anteprima dei comandi della soluzione Azure VMware di Virtustream. | Sì |
| [webapp](https://github.com/Azure/azure-cli-extensions/tree/master/src/webapp) | 0.2.24 | Comandi aggiuntivi per il servizio app di Azure. | Sì |