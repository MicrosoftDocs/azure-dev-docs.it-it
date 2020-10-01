---
title: Uso di Ansible con Azure
description: Introduzione all'uso di Ansible per automatizzare il provisioning cloud, la gestione della configurazione e le distribuzioni di applicazioni.
keywords: ansible, azure, devops, panoramica, provisioning cloud, gestione della configurazione, distribuzione di applicazioni, moduli ansible, playbook ansible
ms.topic: overview
ms.date: 08/13/2020
ms.custom: devx-track-ansible
ms.openlocfilehash: 9eb90921a0d44e138c331eb716700feb85e8aa9d
ms.sourcegitcommit: 717e32b68fc5f4c986f16b2790f4211967c0524b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/30/2020
ms.locfileid: "91586102"
---
# <a name="using-ansible-with-azure"></a>Uso di Ansible con Azure

[Ansible](https://www.ansible.com) è un prodotto open source che consente di automatizzare il provisioning cloud, la gestione della configurazione e le distribuzioni di applicazioni. Tramite Ansible è possibile eseguire il provisioning di macchine virtuali, contenitori e reti, nonché completare le infrastrutture cloud. Inoltre, Ansible consente di automatizzare la distribuzione e la configurazione delle risorse nell'ambiente.

In questo articolo viene presentata una panoramica di base di alcuni dei vantaggi derivanti dall'uso di Ansible con Azure.

## <a name="ansible-playbooks"></a>Playbook Ansible

I [playbook Ansible](https://docs.ansible.com/ansible/latest/playbooks.html) consentono di usare Ansible per configurare l'ambiente. I playbook vengono codificati usando YAML in modo che siano leggibili dall'utente. La sezione Esercitazioni offre numerosi esempi dell'uso dei playbook per installare e configurare le risorse di Azure. 

## <a name="ansible-modules"></a>Moduli Ansible

Ansible include un gruppo di [moduli Ansible](https://docs.ansible.com/ansible/2.9/modules/modules_by_category.html) che vengono eseguiti direttamente su host remoti o tramite [playbook](https://docs.ansible.com/ansible/latest/playbooks.html). Gli utenti possono creare moduli personalizzati. I moduli vengono usati per controllare le risorse di sistema, ad esempio servizi, pacchetti o file, oppure per eseguire i comandi di sistema.

Per interagire con i servizi di Azure, Ansible include una suite di [moduli cloud Ansible](https://docs.ansible.com/ansible/2.9/modules/list_of_cloud_modules.html#azure). Questi moduli consentono di creare e orchestrare l'infrastruttura in Azure. 

## <a name="migrate-existing-workload-to-azure"></a>Eseguire la migrazione del carico di lavoro esistente in Azure

Dopo aver usato Ansible per definire l'infrastruttura, è possibile applicare il playbook dell'applicazione in modo da consentire ad Azure di ridimensionare automaticamente l'ambiente in base alle esigenze. 

## <a name="automate-cloud-native-application-in-azure"></a>Automatizzare l'applicazione nativa del cloud in Azure

Ansible consente di automatizzare le applicazioni native del cloud in Azure tramite microservizi di Azure quali, ad esempio, [Funzioni di Azure](https://azure.microsoft.com//services/functions/) e [Kubernetes in Azure](https://azure.microsoft.com/services/container-service/kubernetes/).  

## <a name="manage-deployments-with-dynamic-inventory"></a>Gestire le distribuzioni con un inventario dinamico

Tramite il relativo [inventario dinamico](https://docs.ansible.com/ansible/intro_dynamic_inventory.html) Ansible consente di eseguire il pull dell'inventario dalle risorse di Azure. È quindi possibile contrassegnare le distribuzioni di Azure esistenti e gestire tali le distribuzioni con tag tramite Ansible.

## <a name="additional-azure-marketplace-options"></a>Opzioni aggiuntive di Azure Marketplace

[Ansible Tower](https://azuremarketplace.microsoft.com/marketplace/apps/redhat.ansible-tower) è un'immagine di Azure Marketplace fornita da Red Hat. 

Ansible Tower è costituito da un'interfaccia utente e un dashboard basati sul Web per Ansible con le funzionalità seguenti:

* Consente di definire il controllo degli accessi in base al ruolo, la pianificazione dei processi e la gestione dell'inventario grafico. 
* Include un'API REST e un'interfaccia della riga di comando che consentono di inserire Tower negli strumenti e processi esistenti. 
* Supporta l'output in tempo reale delle esecuzioni dei playbook. 
* Crittografa credenziali, ad esempio le chiavi di Azure e SSH, ed è quindi possibile delegare le attività senza esporre le credenziali.

## <a name="ansible-module-and-version-matrix-for-azure"></a>Modulo Ansible e matrice della versione per Azure

Ansible include una suite di moduli da usare per il provisioning e la configurazione delle risorse di Azure. Queste risorse includono macchine virtuali, set di scalabilità, servizi di rete e servizi contenitore. La [matrice di Ansible](./module-version-matrix.md) elenca i moduli di Ansible per Azure e le versioni di Ansible in cui sono disponibili.

## <a name="next-steps"></a>Passaggi successivi

- [Avvio rapido: Configurare Ansible con Azure Cloud Shell](getting-started-cloud-shell.md)
- [Avvio rapido: Configurare Ansible con l'interfaccia della riga di comando di Azure](install-on-linux-vm.md)
