---
title: Uso di Chef con Azure
description: Introduzione all'uso di Chef per configurare e testare l'infrastruttura di Azure
keywords: azure, chef, devops, macchine virtuali, panoramica, automazione
ms.date: 02/22/2020
ms.topic: article
ms.openlocfilehash: 94cfe5d57673db151d53133fb8b2ef66c5629b93
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/05/2020
ms.locfileid: "80892990"
---
# <a name="using-chef-with-azure"></a>Uso di Chef con Azure

[Chef](https://www.chef.io) è una potente piattaforma di automazione, che converte in codice l'infrastruttura delle macchine virtuali di Azure. Chef automatizza la configurazione, la distribuzione e la gestione dell'infrastruttura nella rete, indipendentemente dalle dimensioni.

Questo articolo descrive i vantaggi dell'uso di Chef nella gestione dell'infrastruttura di Azure.

## <a name="chef-extension-on-azure"></a>Estensione Chef per Azure

Effettuare il provisioning di una macchina virtuale con un client Chef in esecuzione come servizio in background con l'[estensione Chef](https://docs.microsoft.com/azure/chef/chef-extension-portal) nel [portale di Azure](https://go.microsoft.com/fwlink/p/?LinkID=525040). Al termine del provisioning, queste macchine virtuali sono pronte per essere gestite da un server Chef.

## <a name="chef-cloud-shell"></a>Chef Cloud Shell

Usare la workstation di Chef direttamente in Azure Cloud Shell. Eseguire tutte le utilità di Chef e InSpec direttamente da Cloud Shell. È possibile usare i comandi di Chef da:

* [chef](https://docs.chef.io/ctl_chef.html)
* [kitchen](https://docs.chef.io/ctl_kitchen.html)
* [inspec](https://www.inspec.io/docs/reference/cli/)
* [knife](https://docs.chef.io/knife.html)
* [cookstyle](https://docs.chef.io/cookstyle.html)
* [chef-run](https://www.chef.sh/docs/chef-workstation/getting-started/)

Combinare l'utilità di comando con gli altri strumenti disponibili in Cloud Shell, ad esempio `git`, `az-cli`, e `terraform`; scrivere l'automazione dell'infrastruttura e la conformità dal browser.

## <a name="automate-everything-with-one-platform"></a>Automatizzare tutte le operazioni con un'unica piattaforma

Per competere nei mercati digitali, le aziende richiedono velocità, tempestività e sicurezza nella gestione dei dati. Chef e Microsoft collaborano per offrire a utenti, team e aziende tutte queste caratteristiche. Un'unica piattaforma, Chef Automate, consente ora di automatizzare e implementare in modo continuativo l'infrastruttura, le applicazioni e la conformità nell'intero ambiente Microsoft aziendale.

## <a name="test-drive-chef-automate-on-azure"></a>Test drive di Chef Automate in Azure

La [soluzione Chef Automate per Azure Marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/chef-software.chef-automate), supportata da Chef, consente di creare, distribuire e gestire l'infrastruttura e le applicazioni con un approccio collaborativo. Un solo clic è sufficiente per l'accesso immediato a tutte le funzionalità commerciali di Chef Automate, che garantiscono la visibilità end-to-end sull'intera struttura, l'attivazione della conformità continuativa e la gestione di tutte le modifiche con un flusso di lavoro unificato.

## <a name="next-steps"></a>Passaggi successivi

* [Creare una macchina virtuale Windows in Azure usando Chef](windows-vm-configure.md)
