---
title: Creare una risorsa di Monitoraggio di Azure
description: Creare un gruppo di risorse di Azure per tutte le risorse di Azure e una risorsa di Monitoraggio di Azure per raccogliere i file di log dell'app Web nel cloud di Azure. Monitoraggio di Azure è il nome del servizio di Azure, mentre Application Insights è il nome della libreria client utilizzata nell'esercitazione.
ms.topic: tutorial
ms.date: 11/13/2020
ms.custom: devx-track-js
ms.openlocfilehash: 0b1b634756a5188cbd9233274205005f62b25026
ms.sourcegitcommit: 593d177cfb5f56f236ea59389e43a984da30f104
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/18/2021
ms.locfileid: "98561047"
---
# <a name="2-create-application-insights-resource-for-web-pages"></a>2. Creare una risorsa di Application Insights per le pagine Web

In questo passaggio dell'esercitazione, creare un gruppo di risorse di Azure per tutte le risorse di Azure e una risorsa di Monitoraggio di Azure per raccogliere i file di log dell'app Web nel cloud di Azure. Monitoraggio di Azure è il nome del servizio di Azure, mentre Application Insights è il nome della libreria client utilizzata nell'esercitazione. 

## <a name="create-a-resource-group-for-your-virtual-machine-resources"></a>Creare un gruppo di risorse per le risorse della macchina virtuale

Questa esercitazione include diverse risorse di Azure. La creazione di un gruppo di risorse consente di trovare facilmente le risorse ed eliminarle al termine dell'operazione.

1. In un terminale o in una shell Bash, immettere il [comando dell'interfaccia della riga di comando di Azure per creare un gruppo di risorse di Azure](/cli/azure/group#az_group_create) con il nome `rg-demo-vm-eastus`:

    ```azurecli
    az group create \
        --location eastus \
        --name rg-demo-vm-eastus 
    ```

## <a name="create-azure-monitor-resource-with-azure-cli"></a>Creare una risorsa di Monitoraggio di Azure con l'interfaccia della riga di comando di Azure

1. Installare l'estensione di Application Insights nell'interfaccia della riga di comando di Azure.

    ```azurecli
    az extension add -n application-insights
    ```

1. Usare il comando seguente per [creare una risorsa di monitoraggio](/cli/azure/ext/application-insights/monitor/app-insights/component#ext_application_insights_az_monitor_app_insights_component_create):


    ```azurecli
    az monitor app-insights component create \
      --app demoWebAppMonitor \
      --location eastus \
      --resource-group rg-demo-vm-eastus
    ```

    Nei risultati trovare e copiare il valore della `instrumentationKey`. che sarà necessario più avanti. 

1. Lasciare il terminale aperto: verrà usato nel passaggio successivo.

## <a name="next-step"></a>Passaggio successivo

> [!div class="nextstepaction"]
> [Creare una macchina virtuale Linux](create-linux-virtual-machine-azure-cli.md) 
