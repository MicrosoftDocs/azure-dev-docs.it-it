---
title: Esercitazione - Convalidare una rete hub-spoke in Azure con Terraform
description: Informazioni su come convalidare la topologia di rete Hub e spoke con tutte le reti virtuali connesse tra loro.
ms.topic: tutorial
ms.date: 03/08/2021
ms.custom: devx-track-terraform
ms.openlocfilehash: 71d764da80888326be9625a9e90c1aba164b6f55
ms.sourcegitcommit: 7991f748720673d2dc50baaa8658348ff6cc1044
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/10/2021
ms.locfileid: "102604142"
---
# <a name="tutorial-validate-a-hub-and-spoke-network-in-azure-using-terraform"></a>Esercitazione: convalidare una rete Hub e spoke in Azure usando la bonifica

In questo articolo si eseguono i file Terraform creati nell'articolo precedente di questa serie. Il risultato è una convalida della connettività tra le reti virtuali della demo.

Questa esercitazione illustra le attività seguenti:

> [!div class="checklist"]
> * Usare HCL (HashiCorp Language) per implementare la rete virtuale hub nella topologia hub-spoke
> * Usare terraform plan per verificare le risorse da distribuire
> * Usare terraform apply per creare le risorse in Azure
> * Verificare la connettività tra reti diverse
> * Usare Terraform per eliminare definitivamente tutte le risorse

## <a name="prerequisites"></a>Prerequisiti

1. [Creare una topologia di rete ibrida hub-spoke con Terraform in Azure](./hub-spoke-introduction.md).
1. [Creare una rete virtuale locale con Terraform in Azure](./hub-spoke-on-prem.md).
1. [Creare una rete virtuale hub con Terraform in Azure](./hub-spoke-hub-network.md).
1. [Creare un'appliance di rete virtuale Hub con bonifica in Azure](./hub-spoke-hub-nva.md).
1. [Creare reti virtuali spoke con bonifica in Azure](./hub-spoke-spoke-network.md).

## <a name="verify-your-configuration"></a>Verificare la configurazione

Dopo aver completato le operazioni indicate nei [prerequisiti](#prerequisites), verificare che siano presenti i file di configurazione appropriati.

1. Accedere al [portale di Azure](https://portal.azure.com).

1. Aprire [Azure Cloud Shell](/azure/cloud-shell/overview). Se in precedenza non è stato selezionato un ambiente, selezionare **Bash** come ambiente.

    ![Prompt di Cloud Shell](./media/common/azure-portal-cloud-shell-button-min.png)

1. Passare alla directory `clouddrive`.

    ```bash
    cd clouddrive
    ```

1. Passare alla nuova directory:

    ```bash
    cd hub-spoke
    ```

1. Elencare i file nella directory di lavoro per verificare che i `.tf` file di configurazione creati nelle esercitazioni precedenti siano elencati. (Lo screenshot Visualizza gli altri file che non saranno disponibili fino a quando non si inizializza la bonifica nei passaggi successivi).

    ![File di configurazione per la demo Terraform](./media/hub-and-spoke-tutorial-series/hub-spoke-config-files.png)

## <a name="deploy-the-resources"></a>Distribuire le risorse

1. Inizializzare il provider di Terraform:
    
    ```bash
    terraform init
    ```
    
    ![Risultati di esempio del comando "terraform init"](./media/hub-and-spoke-tutorial-series/hub-spoke-terraform-init.png)
    
1. Eseguire il comando `terraform plan` per vedere l'effetto della distribuzione prima dell'esecuzione:

    ```bash
    terraform plan
    ```
    
    ![Risultati di esempio del comando "terraform plan"](./media/hub-and-spoke-tutorial-series/hub-spoke-terraform-plan.png)

1. Distribuire la soluzione:

    ```bash
    terraform apply
    ```
    
    Immettere `yes` quando viene richiesto di confermare la distribuzione.

    ![Risultati di esempio del comando "terraform apply"](./media/hub-and-spoke-tutorial-series/hub-spoke-terraform-apply.png)
    
## <a name="test-the-hub-vnet-and-spoke-vnets"></a>Testare la rete virtuale hub e le reti virtuali spoke

Questa sezione illustra come testare la connettività dall'ambiente locale simulato alla rete virtuale hub.

1. Nel portale di Azure passare al gruppo di risorse **onprem-vnet-rg**.

1. Nella scheda **onprem-vnet-rg** selezionare la macchina virtuale denominata **onprem-vm**.

1. Prendere nota del valore dell' **indirizzo IP pubblico** .

1. Tornare alla riga di comando ed eseguire `ssh` per connettersi all'ambiente locale simulato. Usare la password specificata nel `variables.tf` file.

   ```bash
   ssh testadmin@<onprem_vm_ip_address>
   ```

1. Dopo essersi connessi **alla macchina virtuale locale** , eseguire il `ping` comando per testare la connettività alla VM JumpBox nell'hub VNet:

   ```bash
   ping 10.0.0.68
   ```

1. Eseguire il comando `ping` per testare la connettività alle macchine virtuali jumpbox in ogni spoke:

   ```bash
   ping 10.1.0.68
   ping 10.2.0.68
   ```

1. Per uscire dalla sessione ssh nella macchina virtuale **onprem-vm**, immettere `exit` e premere &lt;INVIO>.

## <a name="troubleshoot-vpn-issues"></a>Risolvere i problemi relativi alla connessione VPN

Per informazioni sulla risoluzione degli errori di una connessione VPN, vedere l'articolo [Risolvere i problemi relativi a una connessione VPN ibrida](/azure/architecture/reference-architectures/hybrid-networking/troubleshoot-vpn).

## <a name="clean-up-resources"></a>Pulire le risorse

Quando le risorse create in questa serie di esercitazioni non sono più necessarie, eliminarle.

1. Rimuovere le risorse dichiarate nel piano:

    ```bash
    terraform destroy
    ```

    Immettere `yes` quando viene chiesto di confermare la rimozione delle risorse.

1. Passare alla directory padre:

    ```bash
    cd ..
    ```

1. Eliminare la directory `hub-scope` (con tutti i relativi file):

    ```bash
    rm -r hub-spoke
    ```

[!INCLUDE [terraform-troubleshooting.md](includes/terraform-troubleshooting.md)]

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"] 
> [Vedere altre informazioni sull'uso di Terraform in Azure](/azure/terraform)