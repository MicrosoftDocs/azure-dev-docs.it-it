---
title: Usare Azure Managed Disks tramite le librerie di Azure per Python
description: Usare Azure SDK per creare, ridimensionare e aggiornare i dischi gestiti.
ms.topic: conceptual
ms.date: 11/18/2020
ms.custom: devx-track-python
ms.openlocfilehash: b8d45f3d4b5ccd2c8a1c2850d496b9f68625ef46
ms.sourcegitcommit: 6fbf9e489b194586887a2c11152044be5b3a2b99
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/25/2021
ms.locfileid: "98759324"
---
# <a name="use-azure-managed-disks-with-the-azure-libraries-sdk-for-python"></a>Usare Azure Managed Disks con le librerie di Azure (SDK) per Python

Azure Managed Disks fornisce una gestione semplificata dei dischi, offre una scalabilità avanzata, una maggiore sicurezza e una scalabilità migliore per usare direttamente con gli account di archiviazione.

Usare la libreria [`azure-mgmt-compute`](/python/api/overview/azure/virtualmachines) per amministrare Managed Disks. Per un esempio del provisioning di una macchina virtuale con la libreria `azure-mgmt-compute`, vedere [Esempio - Effettuare il provisioning di una macchina virtuale](azure-sdk-example-virtual-machines.md).

## <a name="standalone-managed-disks"></a>Dischi gestiti autonomi

È possibile creare dischi gestiti autonomi in diversi modi, come illustrato nelle sezioni riportate di seguito.

### <a name="create-an-empty-managed-disk"></a>Creare un disco gestito vuoto

```python
from azure.mgmt.compute.models import DiskCreateOption

poller = compute_client.disks.begin_create_or_update(
    'my_resource_group',
    'my_disk_name',
    {
        'location': 'eastus',
        'disk_size_gb': 20,
        'creation_data': {
            'create_option': DiskCreateOption.empty
        }
    }
)
disk_resource = poller.result()
```

### <a name="create-a-managed-disk-from-blob-storage"></a>Creare un disco gestito dall'archiviazione BLOB

```python
from azure.mgmt.compute.models import DiskCreateOption

poller = compute_client.disks.begin_create_or_update(
    'my_resource_group',
    'my_disk_name',
    {
        'location': 'eastus',
        'creation_data': {
            'create_option': DiskCreateOption.import_enum,
            'source_uri': 'https://bg09.blob.core.windows.net/vm-images/non-existent.vhd'
        }
    }
)
disk_resource = poller.result()
```

### <a name="create-a-managed-disk-image-from-blob-storage"></a>Creare un'immagine di disco gestito dall'archiviazione BLOB

```python
from azure.mgmt.compute.models import DiskCreateOption

poller = compute_client.images.begin_create_or_update(
    'my_resource_group',
    'my_image_name',
    {
        'location': 'eastus',
        'storage_profile': {
           'os_disk': {
              'os_type': 'Linux',
              'os_state': "Generalized",
              'blob_uri': 'https://bg09.blob.core.windows.net/vm-images/non-existent.vhd',
              'caching': "ReadWrite",
           }
        }
    }
)
image_resource = poller.result()
```

### <a name="create-a-managed-disk-from-your-own-image"></a>Creare un disco gestito da un'immagine personalizzata

```python
from azure.mgmt.compute.models import DiskCreateOption

# If you don't know the id, do a 'get' like this to obtain it
managed_disk = compute_client.disks.get(self.group_name, 'myImageDisk')

poller = compute_client.disks.begin_create_or_update(
    'my_resource_group',
    'my_disk_name',
    {
        'location': 'eastus',
        'creation_data': {
            'create_option': DiskCreateOption.copy,
            'source_resource_id': managed_disk.id
        }
    }
)

disk_resource = poller.result()
```

## <a name="virtual-machine-with-managed-disks"></a>Macchina virtuale con dischi gestiti

È possibile creare una macchina virtuale con un disco gestito implicito per un'immagine del disco specifica, che evita di dover specificare tutti i dettagli.

Un disco gestito viene creato implicitamente durante la creazione di una VM da un'immagine del sistema operativo in Azure. Nel parametro `storage_profile` il valore `os_disk` è facoltativo e non è necessario creare un account di archiviazione come precondizione obbligatoria per la creazione di una macchina virtuale.

```python
storage_profile = azure.mgmt.compute.models.StorageProfile(
    image_reference = azure.mgmt.compute.models.ImageReference(
        publisher='Canonical',
        offer='UbuntuServer',
        sku='16.04-LTS',
        version='latest'
    )
)
```

Per un esempio completo su come creare una macchina virtuale usando le librerie di gestione di Azure per Python, vedere [Esempio - Effettuare il provisioning di una macchina virtuale](azure-sdk-example-virtual-machines.md).

È anche possibile creare `storage_profile` dalla propria immagine:

```python
# If you don't know the id, do a 'get' like this to obtain it
image = compute_client.images.get(self.group_name, 'myImageDisk')

storage_profile = azure.mgmt.compute.models.StorageProfile(
    image_reference = azure.mgmt.compute.models.ImageReference(
        id = image.id
    )
)
```

È possibile collegare con facilità un disco gestito sottoposto in precedenza a provisioning:

```python
vm = compute.virtual_machines.get(
    'my_resource_group',
    'my_vm'
)
managed_disk = compute_client.disks.get('my_resource_group', 'myDisk')

vm.storage_profile.data_disks.append({
    'lun': 12, # You choose the value, depending of what is available for you
    'name': managed_disk.name,
    'create_option': DiskCreateOptionTypes.attach,
    'managed_disk': {
        'id': managed_disk.id
    }
})

async_update = compute_client.virtual_machines.begin_create_or_update(
    'my_resource_group',
    vm.name,
    vm,
)
async_update.wait()
```

## <a name="virtual-machine-scale-sets-with-managed-disks"></a>Set di scalabilità di macchine virtuali con Managed Disks

Senza Managed Disks, è necessario creare manualmente un account di archiviazione per tutte le VM da includere nel set di scalabilità e quindi usare il parametro di elenco `vhd_containers` per specificare i nomi di tutti gli account di archiviazione per l'API REST del set di scalabilità. Per una guida alla migrazione, vedere [Convertire un modello di set di scalabilità in un modello di set di scalabilità per i dischi gestiti](/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-convert-template-to-md).

Poiché non è necessario gestire gli account di archiviazione con Azure Managed Disks, il `storage_profile` ora può essere esattamente uguale a quello usato nella creazione della macchina virtuale:

```python
'storage_profile': {
    'image_reference': {
        "publisher": "Canonical",
        "offer": "UbuntuServer",
        "sku": "16.04-LTS",
        "version": "latest"
    }
},
```

L'esempio completo è il seguente:

```python
naming_infix = "PyTestInfix"

vmss_parameters = {
    'location': self.region,
    "overprovision": True,
    "upgrade_policy": {
        "mode": "Manual"
    },
    'sku': {
        'name': 'Standard_A1',
        'tier': 'Standard',
        'capacity': 5
    },
    'virtual_machine_profile': {
        'storage_profile': {
            'image_reference': {
                "publisher": "Canonical",
                "offer": "UbuntuServer",
                "sku": "16.04-LTS",
                "version": "latest"
            }
        },
        'os_profile': {
            'computer_name_prefix': naming_infix,
            'admin_username': 'Foo12',
            'admin_password': 'BaR@123!!!!',
        },
        'network_profile': {
            'network_interface_configurations' : [{
                'name': naming_infix + 'nic',
                "primary": True,
                'ip_configurations': [{
                    'name': naming_infix + 'ipconfig',
                    'subnet': {
                        'id': subnet.id
                    }
                }]
            }]
        }
    }
}

# Create VMSS test
result_create = compute_client.virtual_machine_scale_sets.begin_create_or_update(
    'my_resource_group',
    'my_scale_set',
    vmss_parameters,
)
vmss_result = result_create.result()
```

## <a name="other-operations-with-managed-disks"></a>Altre operazioni con Managed Disks

### <a name="resizing-a-managed-disk"></a>Ridimensionamento di un disco gestito

```python
managed_disk = compute_client.disks.get('my_resource_group', 'myDisk')
managed_disk.disk_size_gb = 25

async_update = self.compute_client.disks.begin_create_or_update(
    'my_resource_group',
    'myDisk',
    managed_disk
)
async_update.wait()
```

### <a name="update-the-storage-account-type-of-the-managed-disks"></a>Aggiornare il tipo di account di archiviazione dei dischi gestiti

```python
from azure.mgmt.compute.models import StorageAccountTypes

managed_disk = compute_client.disks.get('my_resource_group', 'myDisk')
managed_disk.account_type = StorageAccountTypes.standard_lrs

async_update = self.compute_client.disks.begin_create_or_update(
    'my_resource_group',
    'myDisk',
    managed_disk
)
async_update.wait()
```

### <a name="create-an-image-from-blob-storage"></a>Creare un'immagine dall'archiviazione BLOB

```python
async_create_image = compute_client.images.create_or_update(
    'my_resource_group',
    'myImage',
    {
        'location': 'westus',
        'storage_profile': {
            'os_disk': {
                'os_type': 'Linux',
                'os_state': "Generalized",
                'blob_uri': 'https://bg09.blob.core.windows.net/vm-images/non-existent.vhd',
                'caching': "ReadWrite",
            }
        }
    }
)
image = async_create_image.result()
```

### <a name="create-a-snapshot-of-a-managed-disk-that-is-currently-attached-to-a-virtual-machine"></a>Creare uno snapshot di un disco gestito attualmente collegato a una macchina virtuale

```python
managed_disk = compute_client.disks.get('my_resource_group', 'myDisk')

async_snapshot_creation = self.compute_client.snapshots.begin_create_or_update(
        'my_resource_group',
        'mySnapshot',
        {
            'location': 'westus',
            'creation_data': {
                'create_option': 'Copy',
                'source_uri': managed_disk.id
            }
        }
    )
snapshot = async_snapshot_creation.result()
```

## <a name="see-also"></a>Vedi anche

- [Esempio: Effettuare il provisioning di una macchina virtuale](azure-sdk-example-virtual-machines.md)
- [Esempio: Effettuare il provisioning di un gruppo di risorse](azure-sdk-example-resource-group.md)
- [Esempio: Elencare i gruppi di risorse in una sottoscrizione](azure-sdk-example-list-resource-groups.md)
- [Esempio: Effettuare il provisioning di Archiviazione di Azure](azure-sdk-example-storage.md)
- [Esempio: Usare Archiviazione di Azure](azure-sdk-example-storage-use.md)
- [Esempio: Effettuare il provisioning e usare un database MySQL](azure-sdk-example-database.md)
- [Completa un breve sondaggio su Azure SDK per Python](https://microsoft.qualtrics.com/jfe/form/SV_bNFX0HECjzPWMiG?Q_CHL=docs)
