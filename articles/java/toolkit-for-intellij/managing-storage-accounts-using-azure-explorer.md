---
title: Gestire gli account di archiviazione con Azure Explorer per IntelliJ
description: Informazioni su come gestire gli account di archiviazione di Azure con Azure Explorer per IntelliJ.
documentationcenter: java
ms.date: 09/09/2020
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.custom: devx-track-java
ms.openlocfilehash: 5152b1bfedd02c821d2a9138fa3e20325b5ea086
ms.sourcegitcommit: a139e25190960ba89c9e31f861f0996a6067cd6c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/15/2020
ms.locfileid: "90534319"
---
# <a name="manage-storage-accounts-by-using-the-azure-explorer-for-intellij"></a>Gestire gli account di archiviazione usando Azure Explorer per IntelliJ

> [!NOTE]
> La funzionalità Account di archiviazione in Azure Explorer è deprecata. È possibile usare il portale di Azure per creare e gestire account di archiviazione e contenitori. Per le guide di avvio rapido relative alla gestione degli account di archiviazione, vedere la documentazione di [Archiviazione di Azure](/azure/storage/blobs/storage-quickstart-blobs-portal).

Incluso in Azure Toolkit for IntelliJ, Azure Explorer offre agli sviluppatori Java una soluzione di facile uso per la gestione degli account di archiviazione con il proprio account Azure nell'ambiente di sviluppo integrato (IDE) di IntelliJ.

[!INCLUDE [prerequisites](includes/prerequisites.md)]

[!INCLUDE [show-azure-explorer](includes/show-azure-explorer.md)]

## <a name="create-a-storage-account"></a>Creare un account di archiviazione

Per creare un account di archiviazione con Azure Explorer, eseguire queste operazioni:

1. Accedere al proprio account Azure seguendo le [Istruzioni di accesso per Azure Toolkit for IntelliJ]. 

2. Nella visualizzazione **Azure Explorer** espandere il nodo **Azure**, fare clic con il pulsante destro del mouse su **Account di archiviazione** e quindi fare clic su **Crea account di archiviazione**.

3. Nella finestra di dialogo **Crea account di archiviazione** specificare le opzioni seguenti:

   * **Name**: specifica il nome del nuovo account di archiviazione.

   * **Account kind** (Tipologia account): specifica il tipo di account di archiviazione da creare, ad esempio "Blob storage" (Archiviazione BLOB). Per altre informazioni, vedere [Informazioni sugli account di archiviazione di Azure]. 

   * **Prestazioni**: specifica l'offerta di account di archiviazione dell'editore selezionato da usare, ad esempio "Premium". Per altre informazioni, vedere [Obiettivi di scalabilità e prestazioni per Archiviazione di Azure]. 

   * **Replica**: specifica la replica per l'account di archiviazione, ad esempio "Zone-Redundant" (Con ridondanza della zona). Per altre informazioni, vedere [Replica di Archiviazione di Azure]. 

   * **Sottoscrizione** specifica la sottoscrizione di Azure da usare per il nuovo account di archiviazione.

   * **Località**: specifica la località in cui verrà creato l'account di archiviazione, ad esempio "West US" (Stati Uniti occidentali).

   * **Gruppo di risorse**: specifica il gruppo di risorse per la macchina virtuale. Selezionare una delle opzioni seguenti:
      * **Crea nuovo**: specifica che si vuole creare un nuovo gruppo di risorse.
      * **Use existing** (Usa esistente): specifica che verrà effettuata una selezione da un elenco di gruppi di risorse associati all'account Azure.

4. Dopo avere specificato tutte le opzioni precedenti, fare clic su **OK**.

## <a name="delete-a-storage-account"></a>Eliminare un account di archiviazione

Per eliminare un account di archiviazione con Azure Explorer, eseguire queste operazioni:

1. Nella visualizzazione **Azure Explorer** fare clic con il pulsante destro del mouse sull'account di archiviazione e quindi selezionare **Elimina**.

2. Nella finestra di conferma fare clic su **Sì**.


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

[Istruzioni di accesso per Azure Toolkit for IntelliJ]: ./sign-in-instructions.md
[Introduzione ad Archiviazione di Microsoft Azure]: /azure/storage/common/storage-introduction
[Informazioni sugli account di archiviazione di Azure]: /azure/storage/storage-create-storage-account
[Replica di Archiviazione di Azure]: /azure/storage/storage-redundancy
[Obiettivi di scalabilità e prestazioni per Archiviazione di Azure]: /azure/storage/storage-scalability-targets
[Naming and referencing containers, blobs, and metadata]: https://go.microsoft.com/fwlink/?LinkId=255555

[Sizes for Windows storage accounts in Azure]: https://docs.microsoft.com/azure/virtual-machines/sizes (Dimensioni degli account di archiviazione Windows in Azure)
[Sizes for Linux storage accounts in Azure]: https://docs.microsoft.com/azure/virtual-machines/sizes (Dimensioni degli account di archiviazione Linux in Azure)
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
