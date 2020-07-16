---
title: Gestione delle Cache Redis con Azure Explorer per IntelliJ
description: Informazioni su come gestire le Cache Redis di Azure con Azure Explorer per IntelliJ.
documentationcenter: java
ms.date: 02/01/2018
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.custom: devx-track-java
ms.openlocfilehash: 17aa931e10f68e08a1d87017a4cdd1238baa7f1f
ms.sourcegitcommit: 44016b81a15b1625c464e6a7b2bfb55938df20b6
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/14/2020
ms.locfileid: "86379935"
---
# <a name="managing-redis-caches-using-the-azure-explorer-for-intellij"></a>Gestione delle Cache Redis con Azure Explorer per IntelliJ

Azure Explorer, che fa parte del Toolkit di Azure per IntelliJ, offre agli sviluppatori Java una soluzione di facile uso per la gestione delle Cache Redis con il proprio account Azure nell'ambiente IDE di IntelliJ.

[!INCLUDE [prerequisites](includes/prerequisites.md)]

[!INCLUDE [show-azure-explorer](includes/show-azure-explorer.md)]

## <a name="create-a-redis-cache-by-using-intellij"></a>Creare una Cache Redis tramite IntelliJ

I passaggi seguenti illustrano come creare una Cache Redis con Azure Explorer.

1. Accedere al proprio account Azure tramite la procedura descritta nell'articolo [Istruzioni di accesso ad Azure per il Toolkit di Azure per IntelliJ].

1. Nella finestra degli strumenti **Azure Explorer** espandere il nodo **Azure**, fare clic con il pulsante destro del mouse su **Cache Redis** e quindi fare clic su **Crea Cache Redis**.

   ![Creare un menu di Cache Redis][CR01]

1. Quando viene visualizzata la finestra di dialogo **Nuova Cache Redis**, specificare le opzioni seguenti:

   ![Finestra di dialogo Crea nuova Cache Redis][CR02]

   a. **Nome DNS**: specifica il sottodominio DNS per la nuova Cache Redis che verrà anteposto a ".redis.cache.windows.net"; ad esempio: *wingtiptoys.redis.cache.windows.net*.

   b. **Sottoscrizione**: specifica la sottoscrizione di Azure da usare per la nuova Cache Redis.

   c. **Gruppo di risorse**: specifica il gruppo di risorse per la Cache Redis. Scegliere una delle opzioni indicate di seguito: 
      * **Crea nuovo**: specifica che si intende creare un nuovo gruppo di risorse. 
      * **Usa esistente**: specifica che sarà possibile scegliere in un elenco i gruppi di risorse associati all'account di Azure. 

   d. **Posizione**: specifica la località in cui verrà creata la Cache Redis, ad esempio *Stati Uniti occidentali*.

   e. **Piano tariffario**: specifica il piano tariffario usato dalla Cache Redis. Questa impostazione determina il numero di connessioni client (Per altre informazioni, vedere [Prezzi di Cache Redis].)

   f. **Porta non SSL**: specifica se la Cache Redis consente le connessioni non SSL. Per impostazione predefinita, sono consentite solo le connessioni SSL.

1. Dopo aver specificato tutte le impostazioni della Cache Redis, fare clic su **OK**.

Dopo aver creato la Cache Redis, quest'ultima verrà visualizzata in Azure Explorer.

   ![Cache Redis in Azure Explorer][CR03]

> [!NOTE]
>
> Per altre informazioni sulla configurazione delle impostazioni di Cache Redis di Azure, vedere [Come configurare Cache Redis di Azure].
>

## <a name="display-the-properties-for-your-redis-cache-in-intellij"></a>Visualizzare le proprietà della Cache Redis in IntelliJ

1. In Azure Explorer, fare clic con il pulsante destro sulla Cache Redis e quindi su **Mostra proprietà**.

   ![Menu di scelta rapida Azure Explorer per mostrare le proprietà della Cache Redis][SP01]

1. Azure Explorer consente di visualizzare le proprietà della Cache Redis.

   ![Proprietà della Cache Redis][SP02]

## <a name="delete-your-redis-cache-by-using-intellij"></a>Eliminare la Cache Redis tramite IntelliJ

1. In Azure Explorer, fare clic con il pulsante destro sulla Cache Redis e quindi su **Elimina**.

   ![Menu di scelta rapida Azure Explorer per eliminare una Cache Redis][DE01]

1. Quando viene richiesto di eliminare la Cache Redis, fare clic su **Sì**.

   ![Eliminare il prompt della Cache Redis][DE02]

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sulle Cache Redis di Azure, le impostazioni di configurazione e i prezzi, vedere i collegamenti seguenti:

* [Cache Redis di Azure]
* [Documentazione di Cache Redis]
* [Prezzi di Cache Redis]
* [Come configurare Cache Redis di Azure]

[!INCLUDE [additional-resources](includes/additional-resources.md)]

<!-- URL List -->

[Prezzi di Cache Redis]: https://azure.microsoft.com/pricing/details/cache/
[Cache Redis di Azure]: https://azure.microsoft.com/services/cache/
[Documentazione di Cache Redis]: /azure/redis-cache
[Come configurare Cache Redis di Azure]: /azure/redis-cache/cache-configure
[Istruzioni di accesso ad Azure per il Toolkit di Azure per IntelliJ]: ./sign-in-instructions.md

<!-- IMG List -->

[CR01]: media/managing-redis-caches-using-azure-explorer/CR01.png
[CR02]: media/managing-redis-caches-using-azure-explorer/CR02.png
[CR03]: media/managing-redis-caches-using-azure-explorer/CR03.png

[SP01]: media/managing-redis-caches-using-azure-explorer/SP01.png
[SP02]: media/managing-redis-caches-using-azure-explorer/SP02.png

[DE01]: media/managing-redis-caches-using-azure-explorer/DE01.png
[DE02]: media/managing-redis-caches-using-azure-explorer/DE02.png
