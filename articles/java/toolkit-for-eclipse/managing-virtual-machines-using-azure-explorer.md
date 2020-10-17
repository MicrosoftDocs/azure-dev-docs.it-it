---
title: Gestire macchine virtuali con Azure Explorer per Eclipse
description: Informazioni su come gestire macchine virtuali di Azure con Azure Explorer per Eclipse.
documentationcenter: java
ms.date: 08/25/2020
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.custom: devx-track-java
ms.openlocfilehash: 82dce0ada2824a00e75e9bb00d8943458e09e44f
ms.sourcegitcommit: f460914ac5843eb7392869a08e3a80af68ab227b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/13/2020
ms.locfileid: "92010172"
---
# <a name="manage-virtual-machines-by-using-the-azure-explorer-for-eclipse"></a>Gestire macchine virtuali con Azure Explorer per Eclipse

Incluso in Toolkit di Azure per Eclipse, Azure Explorer offre agli sviluppatori Java una soluzione di facile uso per la gestione delle macchine virtuali con il proprio account Azure nell'ambiente di sviluppo integrato (IDE) di Eclipse.

[!INCLUDE [prerequisites](includes/prerequisites.md)]

[!INCLUDE [show-azure-explorer](includes/show-azure-explorer.md)]

## <a name="create-a-virtual-machine"></a>Creare una macchina virtuale

1. Accedere al proprio account Azure seguendo le [Istruzioni di accesso ad Azure per il Toolkit di Azure per Eclipse](./sign-in-instructions.md).

1. Nella visualizzazione **Azure Explorer** espandere il nodo **Azure**, fare clic con il pulsante destro del mouse su **Macchine virtuali** e quindi fare clic su **Crea macchina virtuale**.

   :::image type="content" source="media/managing-virtual-machines-using-azure-explorer/CR01.png" alt-text="Opzione Crea macchina virtuale in Azure Explorer.":::

1. Nella finestra **Scegliere una sottoscrizione** selezionare la sottoscrizione e fare clic su **Avanti**.

1. Nella finestra **Selezionare un'immagine per la macchina virtuale** selezionare un valore per **Località**, ad esempio *Stati Uniti occidentali*. Sarà possibile scegliere se procedere con un'immagine consigliata o selezionare un'immagine personalizzata. Per questa guida di avvio rapido si procederà con l'immagine consigliata. 

   Se si sceglie di selezionare un'immagine personalizzata, immettere le informazioni seguenti:
   * **Publisher** (Editore): specifica l'editore che ha creato l'immagine che verrà usata per creare la macchina virtuale, ad esempio *Microsoft*.

   * **Offer** (Offerta): specifica l'offerta di macchina virtuale dell'editore selezionato da usare, ad esempio *JDK*.

   * **Sku**: specifica lo SKU dell'offerta selezionata da usare, ad esempio *JDK_8*.

   * **Version #** (N. versione): specifica la versione dello SKU selezionato da usare.

1. Fare clic su **Avanti**.

1. Nella finestra **Impostazioni di base della macchina virtuale** immettere le informazioni seguenti:

   * **Virtual Machine Name** (Nome macchina virtuale): specifica il nome della nuova macchina virtuale, che deve iniziare con una lettera e contenere solo lettere, numeri e trattini.

   * **Size**: specifica il numero di core e la quantità di memoria da allocare per la macchina virtuale.

   * **Nome utente**: specifica l'account amministratore da creare per la gestione della macchina virtuale.

   * **Password**: specificano la password per l'account amministratore. Immettere di nuovo la password nella casella **Conferma** per convalidare le credenziali.

1. Fare clic su **Avanti**.

1. Nella finestra **Risorse associate** immettere le informazioni seguenti:
   * **Gruppo di risorse**: specifica il gruppo di risorse per la macchina virtuale. Selezionare una delle opzioni seguenti:
      * **Crea nuovo**: specifica che si vuole creare un nuovo gruppo di risorse.
      * **Use existing** (Usa esistente): specifica che si vuole selezionare un gruppo di risorse già associato all'account Azure.

   * **Account di archiviazione**: specifica l'account di archiviazione da usare per archiviare la macchina virtuale. È possibile usare un account di archiviazione esistente o crearne uno nuovo.

   * **Virtual Network** (Rete virtuale) e **Subnet**: specificano la rete virtuale e la subnet a cui si connetterà la macchina virtuale. È possibile usare una subnet e una rete esistente oppure creare una rete e una subnet nuove. Se si seleziona **Crea nuovo**, verrà visualizzata la finestra di dialogo seguente:

   * **Indirizzo IP pubblico**: specifica un indirizzo IP con accesso all'esterno per la macchina virtuale. È possibile scegliere di creare un nuovo indirizzo IP o, se la macchina virtuale non avrà un indirizzo IP pubblico, è possibile selezionare **(Nessuno)** .

   * **Network security group** (Gruppo di sicurezza di rete): specifica un firewall di rete facoltativo per la macchina virtuale. È possibile selezionare un firewall esistente oppure, se la macchina virtuale non usa un firewall di rete, è possibile selezionare **(Nessuno)** .

   * **Set di disponibilità**: specifica un set di disponibilità facoltativo a cui può appartenere la macchina virtuale. È possibile selezionare un set di disponibilità esistente, creare un nuovo set di disponibilità o, se la macchina virtuale non apparterrà a un set di disponibilità, selezionare **(Nessuno)** .

10. Fare clic su **Fine**.  

      > [!NOTE]
      > È possibile controllare lo stato di avanzamento della creazione nell'angolo in basso a destra dell'area di lavoro di Eclipse.

## <a name="restart-a-virtual-machine"></a>Riavviare una macchina virtuale

Per riavviare una macchina virtuale con Azure Explorer in Eclipse, eseguire queste operazioni:

1. Nella visualizzazione **Azure Explorer** fare clic con il pulsante destro del mouse sulla macchina virtuale e quindi selezionare **Riavvia**.

1. Nella finestra di conferma fare clic su **OK**.

   ![Finestra di conferma del riavvio della macchina virtuale](media/managing-virtual-machines-using-azure-explorer/RE02.png)

## <a name="shut-down-a-virtual-machine"></a>Arrestare una macchina virtuale

Per arrestare una macchina virtuale in esecuzione con Azure Explorer in Eclipse, eseguire queste operazioni:

1. Nella visualizzazione **Azure Explorer** fare clic con il pulsante destro del mouse sulla macchina virtuale e quindi selezionare **Chiudi**.

1. Nella finestra di conferma fare clic su **OK**.

   ![Finestra di conferma dell'arresto della macchina virtuale](media/managing-virtual-machines-using-azure-explorer/SH02.png)

## <a name="delete-a-virtual-machine"></a>Eliminare una macchina virtuale

Per eliminare una macchina virtuale con Azure Explorer in Eclipse, eseguire queste operazioni:

1. Nella visualizzazione **Azure Explorer** fare clic con il pulsante destro del mouse sulla macchina virtuale e quindi selezionare **Elimina**.

1. Nella finestra di conferma fare clic su **OK**.

   ![Finestra di conferma dell'eliminazione della macchina virtuale](media/managing-virtual-machines-using-azure-explorer/DE02.png)

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sulle dimensioni e sui prezzi delle macchine virtuali in Azure, vedere i collegamenti seguenti:

* Dimensioni delle macchine virtuali in Azure
  * [Dimensioni per le macchine virtuali Windows in Azure]
  * [Dimensioni delle macchine virtuali Linux in Azure]
* Prezzi delle macchine virtuali in Azure
  * [Prezzi delle macchine virtuali in Windows]
  * [Prezzi delle macchine virtuali in Linux]

[!INCLUDE [additional-resources](includes/additional-resources.md)]

<!-- URL List -->

[Dimensioni per le macchine virtuali Windows in Azure]: /azure/virtual-machines/sizes
[Dimensioni delle macchine virtuali Linux in Azure]: /azure/virtual-machines/sizes
[Prezzi delle macchine virtuali in Windows]: https://azure.microsoft.com/pricing/details/virtual-machines/windows/
[Prezzi delle macchine virtuali in Linux]: https://azure.microsoft.com/pricing/details/virtual-machines/linux/

<!-- IMG List -->

[RE01]: media/managing-virtual-machines-using-azure-explorer/RE01.png
[RE02]: media/managing-virtual-machines-using-azure-explorer/RE02.png

[SH01]: media/managing-virtual-machines-using-azure-explorer/SH01.png
[SH02]: media/managing-virtual-machines-using-azure-explorer/SH02.png

[DE01]: media/managing-virtual-machines-using-azure-explorer/DE01.png
[DE02]: media/managing-virtual-machines-using-azure-explorer/DE02.png

[CR01]: media/managing-virtual-machines-using-azure-explorer/CR01.png
[CR02]: media/managing-virtual-machines-using-azure-explorer/CR02.png
[CR03]: media/managing-virtual-machines-using-azure-explorer/CR03.png
[CR04]: media/managing-virtual-machines-using-azure-explorer/CR04.png
[CR05]: media/managing-virtual-machines-using-azure-explorer/CR05.png
[CR06]: media/managing-virtual-machines-using-azure-explorer/CR06.png
[CR07]: media/managing-virtual-machines-using-azure-explorer/CR07.png
[CR08]: media/managing-virtual-machines-using-azure-explorer/CR08.png
