---
title: Gestire le entità servizio locali per lo sviluppo di Azure
description: Come gestire le entità servizio create per lo sviluppo locale usando il portale di Azure o l'interfaccia della riga di comando di Azure.
ms.date: 05/12/2020
ms.topic: conceptual
ms.openlocfilehash: b2e0c913b08c98994226d7a9de39ae83ae50ec21
ms.sourcegitcommit: 2cdf597e5368a870b0c51b598add91c129f4e0e2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/14/2020
ms.locfileid: "83404884"
---
# <a name="how-to-manage-service-principals"></a>Come gestire le entità servizio

Per motivi di sicurezza, è sempre opportuno gestire con attenzione il modo in cui il codice dell'app è autorizzato ad accedere e modificare le risorse di Azure. Quando si esegue il test del codice localmente, è consigliabile usare sempre un'*entità servizio* locale invece di eseguire il test come utente con privilegi completi, come descritto in [Configurare l'ambiente di sviluppo Python locale - Autenticazione](configure-local-development-environment.md#configure-authentication).

Nel tempo probabilmente sarà necessario eliminare, rinominare o gestire in altro modo queste entità servizio, operazione che è possibile eseguire tramite il portale di Azure o tramite l'interfaccia della riga di comando di Azure.

## <a name="basics-of-azure-authorization"></a>Nozioni di base sull'autorizzazione di Azure

Ogni volta che il codice tenta di eseguire qualsiasi operazione sulle risorse di Azure (tramite le classi in Azure SDK), Azure garantisce che l'applicazione sia autorizzata a eseguire tale azione. Usare il [portale di Azure](https://portal.azure.com) o l'interfaccia della riga di comando di Azure per concedere autorizzazioni specifiche basate sui ruoli o sulle risorse all'identità dell'applicazione. Questa procedura evita di concedere autorizzazioni in eccesso all'applicazione che potrebbero essere sfruttate se la sicurezza dell'applicazione viene compromessa.

Quando viene distribuita in Azure, l'identità dell'applicazione è in genere identica al nome assegnato all'app all'interno del servizio che la ospita, ad esempio Servizio app di Azure, Funzioni di Azure, una macchina virtuale e così via, quando è abilitata l'identità gestita. Quando si esegue il codice localmente, tuttavia, non è incluso alcun servizio di hosting di questo tipo, pertanto è necessario usare con Azure un sostituto appropriato.

A questo scopo, si usa un'*entità servizio* locale, ovvero un altro nome per un'identità dell'app, anziché un'identità utente. L'entità servizio ha un nome, un identificatore "tenant" (essenzialmente un ID per l'organizzazione), un identificatore app o "client" e un segreto o una password. Queste credenziali sono sufficienti per autenticare l'identità con Azure, che può quindi verificare se tale identità è autorizzata ad accedere a una determinata risorsa.

Ogni sviluppatore deve avere la propria entità servizio che è protetta all'interno del proprio account utente nella propria workstation e che non deve mai essere archiviata in un repository del controllo del codice sorgente. Se un'entità servizio viene rubata o compromessa, è possibile eliminarla facilmente nel portale di Azure per revocare tutte le relative autorizzazioni e quindi ricreare l'entità servizio per tale sviluppatore.

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

- Creare, visualizzare, aggiornare ed eliminare entità servizio: comando [az ad sp](/cli/azure/ad/sp?view=azure-cli-latest). Vedere anche [Creare un'entità servizio di Azure con l'interfaccia della riga di comando di Azure](/cli/azure/create-an-azure-service-principal-azure-cli?view=azure-cli-latest).
- Gestire le assegnazioni di ruolo: comando [az role assignment](/cli/azure/role/assignment?view=azure-cli-latest).

Vedere anche la pagina relativa alla

- [Eseguire l'autenticazione con Azure con Azure SDK](azure-sdk-authenticate.md)
