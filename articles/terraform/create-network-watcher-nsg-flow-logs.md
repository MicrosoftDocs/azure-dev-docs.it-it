---
title: Configurare Network Watcher di Azure usando la bonifica per l'uso con analisi del traffico
description: Informazioni su come usare la bonifica per configurare un Network Watcher e i log di flusso NSG in Azure.
keywords: Azure DevOps per la bonifica del traffico di Network Watcher NSG
ms.topic: how-to
ms.date: 11/18/2020
ms.custom: devx-track-terraform
ms.openlocfilehash: 4191f7311426ddcfd05b0f4e4cc243ae2c33afb2
ms.sourcegitcommit: 3d906f265b748fbc0a070fce252098675674c8d9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/22/2021
ms.locfileid: "98707181"
---
# <a name="configure-an-azure-network-watcher-connection-using-terraform"></a>Configurare una connessione di Azure Network Watcher usando la bonifica

Questo articolo illustra un esempio di codice di bonifica per la configurazione di [Network Watcher](/azure/network-watcher/network-watcher-monitoring-overview) in Azure per monitorare l'integrità della rete per un gruppo di sicurezza di rete.

## <a name="prerequisites"></a>Prerequisiti

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../includes/open-source-devops-prereqs-azure-subscription.md)]

[!INCLUDE [terraform-configure-environment.md](includes/terraform-configure-environment.md)]

## <a name="configure-an-azure-network-watcher-flow-logs-and-networking-pre-requisites"></a>Configurare un Network Watcher di Azure, i log di flusso e i prerequisiti di rete 

```hcl
provider azurerm {
  version = "~>2.0"

  features {}
}

resource "azurerm_resource_group" "application1" {
  name                        = "app1_rg"
  location                    = "northcentralus"
}

# Networking components to be monitored
resource "azurerm_network_security_group" "application1" {
  name                = "application1"
  location            = azurerm_resource_group.application1.location
  resource_group_name = azurerm_resource_group.application1.name

  security_rule {
    name                       = "test123"
    priority                   = 110
    direction                  = "Inbound"
    access                     = "Deny"
    protocol                   = "Tcp"
    source_port_range          = "*"
    destination_port_range     = "22"
    source_address_prefix      = "*"
    destination_address_prefix = "*"
  }
}

# Log collection components
resource "azurerm_storage_account" "network_log_data" {
  name                      = "app1logdata"
  resource_group_name       = azurerm_resource_group.application1.name
  location                  = azurerm_resource_group.application1.location

  account_tier              = "Standard"
  account_replication_type  = "GRS"
  min_tls_version           = "TLS1_2"
}

resource "azurerm_log_analytics_workspace" "traffic_analytics" {
  name                = "app007-traffic-analytics"
  location            = azurerm_resource_group.application1.location
  resource_group_name = azurerm_resource_group.application1.name
  retention_in_days   = 90
  daily_quota_gb      = 10
}

# The Network Watcher Instance & network log flow
# There can only be one Network Watcher per subscription and region

resource "azurerm_network_watcher" "app1_traffic" {
  name                = "NetworkWatcher_northcentralus"
  location            = azurerm_resource_group.application1.location
  resource_group_name = azurerm_resource_group.application1.name
}

resource "azurerm_network_watcher_flow_log" "app1_network_logs" {
  network_watcher_name = azurerm_network_watcher.app1_traffic.name
  resource_group_name  = azurerm_network_watcher.app1_traffic.resource_group_name

  network_security_group_id = azurerm_network_security_group.application1.id
  storage_account_id        = azurerm_storage_account.network_log_data.id
  enabled                   = true

  retention_policy {
    enabled = true
    days    = 90
  }

  traffic_analytics {
    enabled               = true
    workspace_id          = azurerm_log_analytics_workspace.traffic_analytics.workspace_id
    workspace_region      = azurerm_log_analytics_workspace.traffic_analytics.location
    workspace_resource_id = azurerm_log_analytics_workspace.traffic_analytics.id
    interval_in_minutes   = 10
  }
}
```


[!INCLUDE [terraform-troubleshooting.md](includes/terraform-troubleshooting.md)]

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"] 
> [Registrazione dei flussi dei gruppi di sicurezza di rete](/azure/network-watcher/network-watcher-nsg-flow-logging-overview)
