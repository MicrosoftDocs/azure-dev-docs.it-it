---
title: Utilizzo di Terraform con Azure
description: Informazioni su come usare Terraform per semplificare la distribuzione e il controllo delle versioni dell'infrastruttura in Azure.
ms.topic: overview
ms.date: 10/26/2019
ms.custom: devx-track-terraform
adobe-target: true
ms.openlocfilehash: 0857586db7fdac0f4decd515c3a6a71026d2d375
ms.sourcegitcommit: b380f6e637b47e6e3822b364136853e1d342d5cd
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/14/2021
ms.locfileid: "100395316"
---
# <a name="terraform-with-azure"></a>Utilizzo di Terraform con Azure

[Hashicorp Terraform](https://www.terraform.io/) è uno strumento open source per il provisioning e la gestione dell'infrastruttura cloud. Codifica l'infrastruttura nei file di configurazione che descrivono la topologia delle risorse cloud. Queste risorse includono macchine virtuali, account di archiviazione e interfacce di rete. L'interfaccia della riga di comando di Terraform offre un meccanismo semplice per la distribuzione e il controllo della versione dei file di configurazione in Azure.

Questo articolo descrive i vantaggi dell'utilizzo di Terraform nella gestione dell'infrastruttura di Azure.

## <a name="automate-infrastructure-management"></a>Automatizzare la gestione dell'infrastruttura

I file di configurazione basati su modello di Terraform consentono di definire, eseguire il provisioning e configurare le risorse di Azure in modo prevedibile e ripetibile. L'automazione dell'infrastruttura ha diversi vantaggi:

- Riduce il rischio di errori umani durante la distribuzione e la gestione dell'infrastruttura.
- Distribuisce lo stesso modello più volte per creare ambienti di sviluppo, test e produzione identici.
- Riduce i costi degli ambienti di sviluppo e test creandoli su richiesta.

## <a name="understand-infrastructure-changes-before-being-applied"></a>Comprendere le modifiche apportate all'infrastruttura prima che vengano applicate

Quando una topologia di risorsa diventa complessa, comprendere il significato e l'impatto delle modifiche apportate all'infrastruttura può essere difficile.

L'interfaccia della riga di comando di Terraform consente agli utenti di convalidare e visualizzare in anteprima le modifiche apportate all'infrastruttura prima dell'applicazione. La visualizzazione in anteprima delle modifiche all'infrastruttura in modo sicuro offre numerosi vantaggi:
- I membri del team possono collaborare efficacemente conoscendo le modifiche proposte e il relativo impatto.
- È possibile rilevare le modifiche non intenzionali nelle prime fasi del processo di sviluppo.

## <a name="deploy-infrastructure-to-multiple-clouds"></a>Distribuire l'infrastruttura su più cloud

Terraform consente di distribuire un'infrastruttura tra più provider di servizi cloud. Permette inoltre agli sviluppatori di usare strumenti coerenti per gestire ogni definizione di infrastruttura.

## <a name="next-steps"></a>Passaggi successivi

Dopo aver letto la panoramica su Terraform e i relativi vantaggi, ecco i passaggi successivi suggeriti:

A seconda dell'ambiente, installare e configurare Terraform:

- [Configurare Terraform con Azure Cloud Shell e l'interfaccia della riga di comando di Azure](get-started-cloud-shell.md)
- [Configurare Terraform con Azure PowerShell](get-started-powershell.md)
