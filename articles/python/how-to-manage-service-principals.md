---
title: Gestire le entità servizio locali per lo sviluppo di Azure
description: Come gestire le entità servizio create per lo sviluppo locale usando il portale di Azure o l'interfaccia della riga di comando di Azure.
ms.date: 08/18/2020
ms.topic: conceptual
ms.custom: devx-track-python, devx-track-azurecli
ms.openlocfilehash: 9d090a4615621c60485b64fac22929472c0cd175
ms.sourcegitcommit: 29b161c450479e5d264473482d31e8d3bf29c7c0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/06/2020
ms.locfileid: "91764787"
---
# <a name="how-to-manage-service-principals"></a>Come gestire le entità servizio

Come illustrato nella sezione [Come autenticare un'app](azure-sdk-authenticate.md), spesso si usano le entità servizio per identificare un'app con Azure, tranne quando si usa l'identità gestita.

Nel tempo è in genere necessario eliminare, rinominare o gestire in altro modo queste entità servizio, operazione che è possibile eseguire attraverso il portale di Azure o tramite l'interfaccia della riga di comando di Azure.

## <a name="manage-service-principals-using-the-azure-portal"></a>Gestire le entità servizio usando il portale di Azure

1. Accedere al [portale di Azure](https://portal.azure.com).

1. Passare alla pagina **Azure Active Directory**, usando l'icona nella home page del portale oppure cercando "Azure Active Directory" nella barra di ricerca del portale.

    ![Ricerca di Azure Active Directory nel portale di Azure](media/how-to-manage-service-principals/azure-ad-portal-search.png)

1. Selezionare **Gestisci** > **Registrazioni app** nel menu di spostamento a sinistra. Le entità servizio di sviluppo locale vengono visualizzate nell'elenco:

    ![Registrazioni app in Azure Active Directory](media/how-to-manage-service-principals/azure-ad-app-registrations.png)

1. Selezionare una delle entità servizio per passare alla pagina delle proprietà in cui è possibile esaminare i valori ID, rinominare o eliminare l'entità servizio e ottenere diversi URL dell'endpoint.

1. Il processo di autorizzazione di un'entità servizio per l'accesso a una risorsa specifica dipende in genere dal servizio in questione. Per altre informazioni, vedere la documentazione relativa a tale servizio. Ad esempio, gli articoli [Autorizzazione per l'archiviazione BLOB](/azure/storage/common/storage-auth-aad-rbac-portal) e [Autorizzazione per l'archiviazione code](/azure/storage/common/storage-auth-aad-rbac-portal) descrivono in parte il processo di Archiviazione di Azure.

## <a name="manage-service-principals-using-the-azure-cli"></a>Gestire le entità servizio usando l'interfaccia della riga di comando di Azure

Usando l'interfaccia della riga di comando di Azure, è possibile eseguire molte delle stesse operazioni sulle entità servizio che è possibile eseguire nel portale di Azure:

- Creare, visualizzare, aggiornare ed eliminare entità servizio: comando [az ad sp](/cli/azure/ad/sp). Vedere anche [Creare un'entità servizio di Azure con l'interfaccia della riga di comando di Azure](/cli/azure/create-an-azure-service-principal-azure-cli).
- Gestire le assegnazioni di ruolo: comando [az role assignment](/cli/azure/role/assignment).

Vedere anche la pagina relativa alla

- [Eseguire l'autenticazione con Azure usando le librerie di Azure](azure-sdk-authenticate.md)
