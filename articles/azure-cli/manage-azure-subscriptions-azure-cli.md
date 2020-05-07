---
title: Gestire le sottoscrizioni di Azure con l'interfaccia della riga di comando di Azure
description: Gestire le sottoscrizioni di Azure con l'interfaccia della riga di comando di Azure
author: dbradish-microsoft
ms.author: dbradish
manager: barbkess
ms.date: 09/09/2018
ms.topic: conceptual
ms.service: azure-cli
ms.devlang: azurecli
ms.openlocfilehash: dad217dff159baa39bd1361258fb308eea872564
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/05/2020
ms.locfileid: "82030903"
---
# <a name="use-multiple-azure-subscriptions"></a>Usare più sottoscrizioni di Azure

La maggior parte degli utenti di Azure non avrà mai più di una sottoscrizione. Si possono tuttavia avere più sottoscrizioni in Azure se si fa parte di più organizzazioni o se l'organizzazione ha suddiviso l'accesso a determinate risorse tra vari gruppi. L'interfaccia della riga di comando supporta la selezione di una sottoscrizione sia a livello globale che per ogni comando.

Per informazioni dettagliate su sottoscrizioni, fatturazione e gestione dei cosi, vedere la [documentazione su fatturazione e gestione dei costi](/azure/billing/).

## <a name="tenants-users-and-subscriptions"></a>Tenant, utenti e sottoscrizioni

La differenza tra tenant, utenti e sottoscrizioni in Azure potrebbe non essere del tutto chiara. Un _tenant_ è l'entità di Azure Active Directory che include un'intera organizzazione. Il tenant ha almeno una _sottoscrizione_ e un _utente_. Un utente è una persona ed è associato a un solo tenant, corrispondente all'organizzazione a cui appartiene. Gli utenti sono gli account che accedono ad Azure per creare, gestire e usare le risorse.
Un utente può avere accesso a più _sottoscrizioni_, che sono contratti con Microsoft per l'uso dei servizi cloud, incluso Azure. Ogni risorsa è associata a una sottoscrizione.

Per altre informazioni sulle differenze tra tenant, utenti e sottoscrizioni, vedere il [dizionario della terminologia cloud di Azure](/azure/azure-glossary-cloud-terminology).  Per informazioni su come aggiungere una sottoscrizione al tenant di Azure Active Directory, vedere [Come aggiungere una sottoscrizione di Azure ad Azure Active Directory](/azure/active-directory/active-directory-how-subscriptions-associated-directory).
Per informazioni su come eseguire l'accesso a un tenant specifico, vedere [Accedere con l'interfaccia della riga di comando di Azure](/cli/azure/authenticate-azure-cli).

## <a name="change-the-active-subscription"></a>Cambiare la sottoscrizione attiva

Per accedere alle risorse di una sottoscrizione, cambiare la sottoscrizione attiva oppure usare l'argomento `--subscription`. Il cambio di sottoscrizione per tutti i comandi viene eseguito con [az account set](/cli/azure/account#az-account-set).

Per cambiare la sottoscrizione attiva:

1. Ottenere un elenco di sottoscrizioni con il comando [az account list](/cli/azure/account#az-account-list):

    ```azurecli-interactive
    az account list --output table
    ```
2. Usare `az account set` con l'ID o il nome della sottoscrizione alla quale si intende passare.

    ```azurecli-interactive
    az account set --subscription "My Demos"
    ```

Per eseguire un solo comando con una sottoscrizione diversa, usare l'argomento `--subscription`. Questo argomento accetta un ID sottoscrizione o un nome di sottoscrizione:

```azurecli-interactive
az vm create --subscription "My Demos" --resource-group MyGroup --name NewVM --image Ubuntu
```
