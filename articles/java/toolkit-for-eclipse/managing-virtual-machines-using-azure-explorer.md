---
title: Gestire macchine virtuali con Azure Explorer per Eclipse
description: Informazioni su come gestire macchine virtuali di Azure con Azure Explorer per Eclipse.
documentationcenter: java
ms.date: 11/13/2018
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.openlocfilehash: 89e40ebd28ffc792430ba73b192a1aec1a7c49c3
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/05/2020
ms.locfileid: "82209804"
---
# <a name="manage-virtual-machines-by-using-the-azure-explorer-for-eclipse"></a>Gestire macchine virtuali con Azure Explorer per Eclipse

Incluso in Toolkit di Azure per Eclipse, Azure Explorer offre agli sviluppatori Java una soluzione di facile uso per la gestione delle macchine virtuali con il proprio account Azure nell'ambiente di sviluppo integrato (IDE) di Eclipse.

[!INCLUDE [prerequisites](includes/prerequisites.md)]

[!INCLUDE [show-azure-explorer](includes/show-azure-explorer.md)]

## <a name="create-a-virtual-machine-in-eclipse"></a>Creare una macchina virtuale in Eclipse

Per creare una macchina virtuale con Azure Explorer, eseguire queste operazioni:

1. Accedere al proprio account Azure seguendo le [Istruzioni di accesso ad Azure per il Toolkit di Azure per Eclipse](/azure/developer/java/toolkit-for-eclipse/sign-in-instructions).

2. Nella visualizzazione **Azure Explorer** espandere il nodo **Azure**, fare clic con il pulsante destro del mouse su **Macchine virtuali** e quindi fare clic su **Crea macchina virtuale**.

   ![Comando Crea macchina virtuale][CR01]  

   Viene visualizzata la procedura guidata **Creare una nuova macchina virtuale**.

3. Nella finestra **Scegliere una sottoscrizione** selezionare la sottoscrizione e fare clic su **Avanti**.

   ![Finestra di dialogo Scegliere una sottoscrizione][CR02]

4. Nella finestra **Selezionare un'immagine di macchina virtuale** immettere le informazioni seguenti:

   * **Località**: specifica dove verrà creata la macchina virtuale, ad esempio *Stati Uniti occidentali*.

   * **Autore**: specifica l'autore che ha creato l'immagine che verrà usata per creare la macchina virtuale, ad esempio *Microsoft*.

   * **Offerta**: specifica la macchina virtuale offerta da usare dall'autore selezionato, ad esempio *JDK*.

   * **SKU**: specifica la SKU da usare nell'offerta selezionata, ad esempio *JDK_8*.

   * **Versione #** (N. versione): specifica la versione della SKU selezionata.

   ![Finestra Selezionare un'immagine di macchina virtuale][CR03]

5. Fare clic su **Avanti**.

6. Nella finestra **Impostazioni di base della macchina virtuale** immettere le informazioni seguenti:

   * **Nome macchina virtuale**: specifica il nome della nuova macchina virtuale, che deve iniziare con una lettera e contenere solo lettere, numeri e trattini.

   * **Dimensioni**: specifica il numero di core e la quantità di memoria da allocare per la macchina virtuale.

   * **Nome utente**: specifica l'account amministratore da creare per la gestione della macchina virtuale.

   * **Password** e **Conferma**: specifica la password per l'account di amministratore.

   ![Finestra Impostazioni di base della macchina virtuale][CR04]

7. Fare clic su **Avanti**.

8. Nella finestra **Crea un nuovo account di archiviazione** immettere le informazioni seguenti:

   * **Gruppo di risorse**: specifica il gruppo di risorse per la macchina virtuale. Selezionare una delle opzioni seguenti:
     * **Crea nuovo**: specifica che si intende creare un nuovo gruppo di risorse.
     * **Usa esistente**: specifica che si vuole selezionare un gruppo di risorse già associato all'account di Azure.

       ![Finestra di dialogo Crea un nuovo account di archiviazione][CR05]

   * **Account di archiviazione**: specifica l'account di archiviazione da usare per archiviare la macchina virtuale. È possibile usare un account di archiviazione esistente o crearne uno nuovo.

   * **Rete virtuale** e **Subnet**: specifica la rete virtuale e la subnet a cui si connetterà la macchina virtuale. È possibile usare una subnet e una rete esistente oppure creare una rete e una subnet nuove. Se si seleziona **Crea nuovo**, verrà visualizzata la finestra di dialogo seguente:

      ![Finestra di dialogo Crea una nuova rete virtuale][CR06]

9. Nella finestra **Risorse associate** immettere le informazioni seguenti:

   * **Indirizzo IP pubblico**: specifica un indirizzo IP con connessione esterna per la macchina virtuale. È possibile scegliere di creare un nuovo indirizzo IP o, se la macchina virtuale non avrà un indirizzo IP pubblico, è possibile selezionare **(Nessuno)** .

   * **Gruppo di sicurezza di rete**: specifica un firewall di rete facoltativo per la macchina virtuale. È possibile selezionare un firewall esistente oppure, se la macchina virtuale non usa un firewall di rete, è possibile selezionare **(Nessuno)** .

   * **Set di disponibilità**: specifica un set di disponibilità facoltativo a cui può appartenere la macchina virtuale. È possibile selezionare un set di disponibilità esistente, creare un nuovo set di disponibilità o, se la macchina virtuale non apparterrà a un set di disponibilità, selezionare **(Nessuno)** .

   ![Finestra Risorse associate][CR07]

10. Fare clic su **Fine**.  

    La nuova macchina virtuale viene visualizzata nella finestra dello strumento Azure Explorer.

    ![Nuova macchina virtuale][CR08]

## <a name="restart-a-virtual-machine-in-eclipse"></a>Riavvio di una macchina virtuale in Eclipse

Per riavviare una macchina virtuale con Azure Explorer in Eclipse, eseguire queste operazioni:

1. Nella visualizzazione **Azure Explorer** fare clic con il pulsante destro del mouse sulla macchina virtuale e quindi selezionare **Riavvia**.

   ![Comando di riavvio della macchina virtuale][RE01]

1. Nella finestra di conferma fare clic su **Sì**.

   ![Finestra di conferma del riavvio][RE02]

## <a name="shut-down-a-virtual-machine-in-eclipse"></a>Arrestare una macchina virtuale in Eclipse

Per arrestare una macchina virtuale in esecuzione con Azure Explorer in Eclipse, eseguire queste operazioni:

1. Nella visualizzazione **Azure Explorer** fare clic con il pulsante destro del mouse sulla macchina virtuale e quindi selezionare **Chiudi**.

   ![Comando di arresto della macchina virtuale][SH01]

1. Nella finestra di conferma fare clic su **Sì**.

   ![Finestra di conferma dell'arresto della macchina virtuale][SH02]

## <a name="delete-a-virtual-machine-in-eclipse"></a>Eliminare una macchina virtuale in Eclipse

Per eliminare una macchina virtuale con Azure Explorer in Eclipse, eseguire queste operazioni:

1. Nella visualizzazione **Azure Explorer** fare clic con il pulsante destro del mouse sulla macchina virtuale e quindi selezionare **Elimina**.

   ![Comando di eliminazione della macchina virtuale][DE01]

1. Nella finestra di conferma fare clic su **Sì**.

   ![Finestra di conferma dell'eliminazione della macchina virtuale][DE02]

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

[Dimensioni per le macchine virtuali Windows in Azure]: /azure/virtual-machines/virtual-machines-windows-sizes
[Dimensioni delle macchine virtuali Linux in Azure]: /azure/virtual-machines/virtual-machines-linux-sizes
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