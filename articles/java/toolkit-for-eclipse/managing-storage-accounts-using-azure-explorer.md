---
title: Gestire gli account di archiviazione con Azure Explorer per Eclipse
description: Informazioni su come gestire gli account di archiviazione di Azure con Azure Explorer per Eclipse.
documentationcenter: java
ms.date: 08/25/2020
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.custom: devx-track-java
ms.openlocfilehash: 7a2a3406e43e1bc65ac61b7197399cdce08ef29b
ms.sourcegitcommit: 723441eda0eb4ff893123201a9e029b7becf5ecc
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/08/2020
ms.locfileid: "91846412"
---
# <a name="manage-storage-accounts-by-using-the-azure-explorer-for-eclipse"></a>Gestire gli account di archiviazione con Azure Explorer per Eclipse

> [!NOTE]
> La funzionalità Account di archiviazione in Azure Explorer è deprecata. È possibile usare il portale di Azure per creare e gestire account di archiviazione e contenitori. Per le guide di avvio rapido relative alla gestione degli account di archiviazione, vedere la documentazione di [Archiviazione di Azure](/azure/storage/blobs/storage-quickstart-blobs-portal).

Incluso in Azure Toolkit for Eclipse, Azure Explorer offre agli sviluppatori Java una soluzione di facile uso per la gestione degli account di archiviazione con il proprio account Azure nell'IDE (ambiente di sviluppo integrato) di Eclipse.

[!INCLUDE [prerequisites](includes/prerequisites.md)]

[!INCLUDE [show-azure-explorer](includes/show-azure-explorer.md)]

## <a name="create-a-storage-account"></a>Creare un account di archiviazione

1. Accedere al proprio account Azure seguendo le [Istruzioni di accesso ad Azure per il Toolkit di Azure per Eclipse](./sign-in-instructions.md).

1. Nella visualizzazione **Azure Explorer** espandere il nodo **Azure**, fare clic con il pulsante destro del mouse su **Account di archiviazione** e quindi fare clic su **Crea account di archiviazione**.

1. Nella finestra di dialogo **Crea account di archiviazione** specificare le opzioni seguenti:

   * **Name**: specifica il nome del nuovo account di archiviazione.

   * **Sottoscrizione** specifica la sottoscrizione di Azure da usare per il nuovo account di archiviazione.

   * **Gruppo di risorse**: specifica il gruppo di risorse per la macchina virtuale. Selezionare una delle opzioni seguenti:
      * **Create New** (Crea nuovo): specifica che si vuole creare un nuovo gruppo di risorse.
      * **Use Existing** (Usa esistente): specifica che verrà effettuata una selezione da un elenco di gruppi di risorse associati all'account Azure.

   * **Area**: specifica la località in cui verrà creato l'account di archiviazione, ad esempio "West US" (Stati Uniti occidentali).

   * **Account kind** (Tipologia account): specifica il tipo di account di archiviazione da creare, ad esempio "Utilizzo generico v1". Per altre informazioni, vedere [Informazioni sugli account di archiviazione di Azure].

   * **Prestazioni**: specifica l'offerta di account di archiviazione dell'editore selezionato da usare, ad esempio "Standard". Per altre informazioni, vedere [Obiettivi di scalabilità e prestazioni per Archiviazione di Azure].

   * **Replica**: specifica la replica per l'account di archiviazione, ad esempio "Ridondanza locale". Per altre informazioni, vedere [Replica di Archiviazione di Azure].

1. Dopo avere specificato tutte le opzioni precedenti, fare clic su **Crea**.

## <a name="create-and-manage-storage-containers"></a>Creare e gestire contenitori di archiviazione

Per creare e gestire i contenitori di archiviazione, visitare il portale di Azure oppure effettuare il provisioning delle risorse a livello di codice.

Vedere [Caricare, scaricare ed elencare BLOB con il portale di Azure](/azure/storage/blobs/storage-quickstart-blobs-portal) per un'esercitazione dettagliata su come usare il portale di Azure per creare un contenitore in Archiviazione di Azure e per caricare e scaricare BLOB in blocchi in tale contenitore.

## <a name="delete-a-storage-account"></a>Eliminare un account di archiviazione

1. Nella visualizzazione **Azure Explorer** fare clic con il pulsante destro del mouse sull'account di archiviazione e quindi selezionare **Elimina**.

1. Nella finestra di conferma fare clic su **OK**.


## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sugli account di archiviazione, sulle dimensioni e sui prezzi di Azure, vedere le risorse seguenti:

* [Introduzione ad Archiviazione di Microsoft Azure]
* [Informazioni sugli account di archiviazione di Azure]
* Dimensioni degli account di archiviazione di Azure
  * [Sizes for Windows storage accounts in Azure] (Dimensioni degli account di archiviazione Windows in Azure)
  * [Sizes for Linux storage accounts in Azure] (Dimensioni degli account di archiviazione Linux in Azure)
* Prezzi per gli account di archiviazione di Azure
  * [Prezzi per gli account di archiviazione Windows]
  * [Prezzi per gli account di archiviazione Linux]

[!INCLUDE [additional-resources](includes/additional-resources.md)]

<!-- URL List -->

[Introduzione ad Archiviazione di Microsoft Azure]: /azure/storage/common/storage-introduction
[Informazioni sugli account di archiviazione di Azure]: /azure/storage/storage-create-storage-account
[Replica di Archiviazione di Azure]: /azure/storage/storage-redundancy
[Obiettivi di scalabilità e prestazioni per Archiviazione di Azure]: /azure/storage/storage-scalability-targets
[Naming and referencing containers, blobs, and metadata]: /rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata

[Sizes for Windows storage accounts in Azure]: /azure/virtual-machines/sizes (Dimensioni degli account di archiviazione Windows in Azure)
[Sizes for Linux storage accounts in Azure]: /azure/virtual-machines/sizes (Dimensioni degli account di archiviazione Linux in Azure)
[Prezzi per gli account di archiviazione Windows]: https://azure.microsoft.com/pricing/details/virtual-machines/windows/
[Prezzi per gli account di archiviazione Linux]: https://azure.microsoft.com/pricing/details/virtual-machines/linux/

<!-- IMG List -->

[CS01]: media/managing-storage-accounts-using-azure-explorer/CS01.png
[CS02]: media/managing-storage-accounts-using-azure-explorer/CS02.png
[CC01]: media/managing-storage-accounts-using-azure-explorer/CC01.png
[CC02]: media/managing-storage-accounts-using-azure-explorer/CC02.png

[DS01]: media/managing-storage-accounts-using-azure-explorer/DS01.png
[DS02]: media/managing-storage-accounts-using-azure-explorer/DS02.png
[DC01]: media/managing-storage-accounts-using-azure-explorer/DC01.png
[DC02]: media/managing-storage-accounts-using-azure-explorer/DC02.png