---
title: Esercitazione - Creare una rete virtuale locale in Azure con Terraform
description: Informazioni su come implementare una rete virtuale locale (VNet) in Azure che ospita le risorse locali.
ms.topic: tutorial
ms.date: 03/08/2021
ms.custom: devx-track-terraform
ms.openlocfilehash: 08f6f7b9c3c3e73f0d240d568b4049c47d7c5796
ms.sourcegitcommit: 7991f748720673d2dc50baaa8658348ff6cc1044
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/10/2021
ms.locfileid: "102604112"
---
# <a name="tutorial-create-on-premises-virtual-network-in-azure-using-terraform"></a>Esercitazione: creare una rete virtuale locale in Azure usando la bonifica

Questa esercitazione illustra come implementare una rete locale in Azure. È possibile sostituire la rete di esempio con una rete virtuale privata. A tale scopo, modificare gli indirizzi IP della subnet in base all'ambiente in uso.

Vengono illustrate le attività seguenti:

> [!div class="checklist"]
> * Usare HCL (HashiCorp Language) per implementare la rete virtuale locale nella topologia hub-spoke
> * Usare Terraform per creare risorse per l'appliance di rete hub
> * Usare Terraform per creare una macchina virtuale locale
> * Usare Terraform per creare un gateway di rete virtuale privata locale

## <a name="prerequisites"></a>Prerequisiti

1. [Creare una topologia di rete ibrida hub-spoke con Terraform in Azure](./hub-spoke-introduction.md).

## <a name="create-the-directory-structure"></a>Creare la struttura di directory

Per simulare una rete locale, creare una rete virtuale di Azure. La rete virtuale demo sostituisce una rete privata locale reale. Per eseguire la stessa operazione con la rete locale esistente, mappare indirizzi IP appropriati nelle subnet.

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

## <a name="declare-the-on-premises-vnet"></a>Dichiarare la rete virtuale locale

Creare il file di configurazione Terraform che dichiara una rete virtuale locale.

1. In Cloud Shell aprire un nuovo file denominato `on-prem.tf`.

    ```bash
    code on-prem.tf
    ```

1. Incollare il codice seguente nell'editor:

    ```hcl
    locals {
      onprem-location       = "eastus"
      onprem-resource-group = "onprem-vnet-rg"
      prefix-onprem         = "onprem"
    }

    resource "azurerm_resource_group" "onprem-vnet-rg" {
      name     = local.onprem-resource-group
      location = local.onprem-location
    }

    resource "azurerm_virtual_network" "onprem-vnet" {
      name                = "onprem-vnet"
      location            = azurerm_resource_group.onprem-vnet-rg.location
      resource_group_name = azurerm_resource_group.onprem-vnet-rg.name
      address_space       = ["192.168.0.0/16"]

      tags = {
        environment = local.prefix-onprem
      }
    }

    resource "azurerm_subnet" "onprem-gateway-subnet" {
      name                 = "GatewaySubnet"
      resource_group_name  = azurerm_resource_group.onprem-vnet-rg.name
      virtual_network_name = azurerm_virtual_network.onprem-vnet.name
      address_prefixes     = ["192.168.255.224/27"]
    }

    resource "azurerm_subnet" "onprem-mgmt" {
      name                 = "mgmt"
      resource_group_name  = azurerm_resource_group.onprem-vnet-rg.name
      virtual_network_name = azurerm_virtual_network.onprem-vnet.name
      address_prefixes     = ["192.168.1.128/25"]
    }

    resource "azurerm_public_ip" "onprem-pip" {
        name                         = "${local.prefix-onprem}-pip"
        location            = azurerm_resource_group.onprem-vnet-rg.location
        resource_group_name = azurerm_resource_group.onprem-vnet-rg.name
        allocation_method   = "Dynamic"

        tags = {
            environment = local.prefix-onprem
        }
    }

    resource "azurerm_network_interface" "onprem-nic" {
      name                 = "${local.prefix-onprem}-nic"
      location             = azurerm_resource_group.onprem-vnet-rg.location
      resource_group_name  = azurerm_resource_group.onprem-vnet-rg.name
      enable_ip_forwarding = true

      ip_configuration {
        name                          = local.prefix-onprem
        subnet_id                     = azurerm_subnet.onprem-mgmt.id
        private_ip_address_allocation = "Dynamic"
        public_ip_address_id          = azurerm_public_ip.onprem-pip.id
      }
    }

    # Create Network Security Group and rule
    resource "azurerm_network_security_group" "onprem-nsg" {
        name                = "${local.prefix-onprem}-nsg"
        location            = azurerm_resource_group.onprem-vnet-rg.location
        resource_group_name = azurerm_resource_group.onprem-vnet-rg.name

        security_rule {
            name                       = "SSH"
            priority                   = 1001
            direction                  = "Inbound"
            access                     = "Allow"
            protocol                   = "Tcp"
            source_port_range          = "*"
            destination_port_range     = "22"
          source_address_prefix      = "*"
            destination_address_prefix = "*"
        }

        tags = {
            environment = "onprem"
        }
    }

    resource "azurerm_subnet_network_security_group_association" "mgmt-nsg-association" {
      subnet_id                 = azurerm_subnet.onprem-mgmt.id
      network_security_group_id = azurerm_network_security_group.onprem-nsg.id
    }

    resource "azurerm_virtual_machine" "onprem-vm" {
      name                  = "${local.prefix-onprem}-vm"
      location              = azurerm_resource_group.onprem-vnet-rg.location
      resource_group_name   = azurerm_resource_group.onprem-vnet-rg.name
      network_interface_ids = [azurerm_network_interface.onprem-nic.id]
      vm_size               = var.vmsize

      storage_image_reference {
        publisher = "Canonical"
        offer     = "UbuntuServer"
        sku       = "16.04-LTS"
        version   = "latest"
      }

      storage_os_disk {
        name              = "myosdisk1"
        caching           = "ReadWrite"
        create_option     = "FromImage"
        managed_disk_type = "Standard_LRS"
      }

      os_profile {
        computer_name  = "${local.prefix-onprem}-vm"
        admin_username = var.username
        admin_password = var.password
      }

      os_profile_linux_config {
        disable_password_authentication = false
      }

      tags = {
        environment = local.prefix-onprem
      }
    }

    resource "azurerm_public_ip" "onprem-vpn-gateway1-pip" {
      name                = "${local.prefix-onprem}-vpn-gateway1-pip"
      location            = azurerm_resource_group.onprem-vnet-rg.location
      resource_group_name = azurerm_resource_group.onprem-vnet-rg.name

      allocation_method = "Dynamic"
    }

    resource "azurerm_virtual_network_gateway" "onprem-vpn-gateway" {
      name                = "onprem-vpn-gateway1"
      location            = azurerm_resource_group.onprem-vnet-rg.location
      resource_group_name = azurerm_resource_group.onprem-vnet-rg.name

      type     = "Vpn"
      vpn_type = "RouteBased"

      active_active = false
      enable_bgp    = false
      sku           = "VpnGw1"

      ip_configuration {
        name                          = "vnetGatewayConfig"
        public_ip_address_id          = azurerm_public_ip.onprem-vpn-gateway1-pip.id
        private_ip_address_allocation = "Dynamic"
        subnet_id                     = azurerm_subnet.onprem-gateway-subnet.id
      }
      depends_on = [azurerm_public_ip.onprem-vpn-gateway1-pip]

    }
    ```

1. Salvare il file e uscire dall'editor.

[!INCLUDE [terraform-troubleshooting.md](includes/terraform-troubleshooting.md)]

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"]
> [Creare una rete virtuale Hub con bonifica in Azure](./hub-spoke-hub-network.md)