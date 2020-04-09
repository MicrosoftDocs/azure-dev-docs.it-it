---
title: Avvio rapido - Distribuire il modello di soluzione Ansible per Azure in CentOS
description: In questo argomento di avvio rapido viene illustrato come distribuire il modello di soluzione Ansible in una macchina virtuale CentOS ospitata in Azure, insieme agli strumenti configurati per l'uso con Azure.
keywords: ansible, azure, devops, modello di soluzione, macchina virtuale, Identità gestite per le risorse di azure, centos, red hat
ms.topic: quickstart
ms.date: 04/30/2019
ms.openlocfilehash: ac6d5f550447c11a463fb2d002c95a1242c08afb
ms.sourcegitcommit: f89c59f772364ec717e751fb59105039e6fab60c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2020
ms.locfileid: "80741059"
---
# <a name="quickstart-deploy-the-ansible-solution-template-for-azure-to-centos"></a>Guida introduttiva: Distribuire il modello di soluzione Ansible per Azure in CentOS

Il modello di soluzione Ansible per Azure è progettato per configurare un'istanza di Ansible in una macchina virtuale CentOS insieme ad Ansible e a una suite di strumenti configurati per funzionare con Azure. Gli strumenti comprendono:

- **Moduli Ansible per Azure**: i [moduli Ansible per Azure](./module-version-matrix.md) sono una suite di moduli che consente di creare e gestire l'infrastruttura in Azure. Per impostazione predefinita viene restituita la versione più recente di questi moduli. Tuttavia, durante il processo di distribuzione del modello di soluzione, è possibile specificare un numero di versione appropriato per l'ambiente.
- **Interfaccia della riga di comando di Azure 2.0 (CLI)** : l'[interfaccia della riga di comando di Azure 2.0](/cli/azure/?view=azure-cli-latest) è un comando multipiattaforma per la gestione delle risorse di Azure. 
- **Identità gestite per le risorse di Azure**: la funzionalità delle [identità gestite per le risorse di Azure](/azure/active-directory/managed-identities-azure-resources/overview) risolve il problema della sicurezza delle credenziali dell'applicazione cloud.

## <a name="prerequisites"></a>Prerequisiti

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../../includes/open-source-devops-prereqs-azure-subscription.md)]

## <a name="deploy-the-ansible-solution-template"></a>Distribuire il modello di soluzione Ansible

1. Passare a al [modello di soluzione Ansible in Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/azure-oss.ansible?tab=Overview).

1. Selezionare **SCARICA ADESSO**.

1. Viene visualizzata una finestra che descrive in dettaglio le condizioni per l'utilizzo, l'informativa sulla privacy e le condizioni per l'utilizzo di Azure Marketplace. Selezionare **Continua**.

1. Nel portale di Azure verrà visualizzata la pagina di Ansible che descrive il modello di soluzione. Selezionare **Create** (Crea).

1. Nella pagina **Crea Ansible** verranno visualizzate diverse schede. Nella scheda **Informazioni di base** immettere le informazioni necessarie:

   - **Nome**: specificare il nome dell'istanza di Ansible. A scopo dimostrativo, viene usato il nome `ansiblehost`.
   - **Nome utente**: specificare il nome dell'utente che avrà accesso all'istanza di Ansible. A scopo dimostrativo, viene usato il nome `ansibleuser`.
   - **Tipo di autenticazione**: selezionare **Password** o **Chiave pubblica SSH**. A scopo dimostrativo, è selezionata l'opzione **Chiave pubblica SSH**.
   - **Password** e **Conferma password**: se si seleziona **Password** per **Tipo di autenticazione**, immettere la password per questi valori.
   - **Chiave pubblica SSH**: se si seleziona **Chiave pubblica SSH** per **Tipo di autenticazione** immettere la chiave pubblica RSA nel formato a riga singola che inizia con `ssh-rsa`.
   - **Sottoscrizione**: selezionare la sottoscrizione di Azure nell'elenco a discesa.
   - **Gruppo di risorse**: selezionare un gruppo di risorse esistente nell'elenco a discesa oppure selezionare **Crea nuovo** e specificare un nome per un nuovo gruppo di risorse. A scopo dimostrativo, viene usato un nuovo gruppo di risorse denominato `ansiblerg`.
   - **Percorso**: selezionare il percorso appropriato per lo scenario specifico nell'elenco a discesa.

     ![Scheda del portale di Azure per le impostazioni di base di Ansible](./media/solution-template-deploy/portal-ansible-setup-tab-1.png)

1. Selezionare **OK**.

1. Nella scheda **Impostazioni aggiuntive** immettere le informazioni necessarie:

   - **Dimensioni** -per impostazione predefinita, le dimensioni del portale di Azure sono standard. Per specificare una dimensione diversa che si adatti allo scenario specifico, selezionare la freccia per visualizzare un elenco di dimensioni diverse.
   - **VM disk type** (Tipo disco VM): selezionare **SSD** (Premium Solid-State Drive) o **HDD** (Hard Disk Drive). A scopo dimostrativo, è selezionato **SSD** per i vantaggi offerti in termini di prestazioni. Per altre informazioni su questi tipi di archiviazione su disco, vedere gli articoli seguenti:
       - [Archiviazione Premium a prestazioni elevate e dischi gestiti per le macchine virtuali](/azure/virtual-machines/windows/premium-storage)
       - [Managed Disks SSD Standard per carichi di lavoro delle macchine virtuali di Azure](/azure/virtual-machines/windows/disks-standard-ssd)
   - **Indirizzo IP pubblico**: specificare questa impostazione se si vuole comunicare con la macchina virtuale dall'esterno della stessa. Il valore predefinito è un nuovo indirizzo IP pubblico con il nome `ansible-pip`. Per specificare un indirizzo IP diverso, selezionare la freccia e specificare gli attributi, ad esempio nome, SKU e assegnazione, di quell'indirizzo IP. 
   - **Etichetta del nome di dominio**: immettere il nome di dominio pubblico della macchina virtuale. Il nome deve essere univoco e soddisfare i requisiti di denominazione. Per altre informazioni su come specificare un nome per la macchina virtuale, vedere [Convenzioni di denominazione per le risorse di Azure](/azure/architecture/best-practices/resource-naming).
   - **Versione di Ansible**: specificare un numero di versione o il valore `latest` per distribuire la versione più recente. Selezionare l'icona informazioni accanto a **Versione di Ansible** per visualizzare altre informazioni sulle versioni disponibili.

     ![Scheda del portale di Azure per le impostazioni aggiuntive di Ansible](./media/solution-template-deploy/portal-ansible-setup-tab-2.png)

1. Selezionare **OK**.

1. Nella scheda **Impostazioni di integrazione di Ansible** specificare il tipo di autenticazione. Per altre informazioni sulla protezione delle risorse di Azure, vedere [Informazioni sulle identità gestite per le risorse di Azure](/azure/active-directory/managed-identities-azure-resources/overview).

    ![Scheda del portale di Azure per le impostazioni di integrazione](./media/solution-template-deploy/portal-ansible-setup-tab-3.png)

1. Selezionare **OK**.

1. La pagina **Riepilogo** mostra il processo di convalida ed elenca i criteri specificati per la distribuzione di Ansible. Un collegamento nella parte inferiore della scheda consente di **scaricare il modello e i parametri** da usare con i linguaggi e le piattaforme di Azure supportati. 

     ![Scheda Riepilogo di Ansible nel portale di Azure](./media/solution-template-deploy/portal-ansible-setup-tab-4.png)

1. Selezionare **OK**.

1. Quando viene visualizzata la scheda **Crea** selezionare **OK** per distribuire Ansible.

1. Selezionare l'icona **Notifiche** in alto nella pagina del sito per tenere traccia della distribuzione di Ansible. Una volta completata la distribuzione, selezionare **Vai al gruppo di risorse**. 

     ![Scheda Riepilogo di Ansible nel portale di Azure](./media/solution-template-deploy/portal-ansible-setup-complete.png)

1. Nella pagina del gruppo di risorse, ottenere l'indirizzo IP dell'host di Ansible e accedere per gestire le risorse di Azure con Ansible.

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"] 
> [Avvio rapido: Configurare una macchina virtuale Linux in Azure tramite Ansible](./vm-configure.md)
