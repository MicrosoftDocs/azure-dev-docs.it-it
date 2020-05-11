---
ms.openlocfilehash: 5d005f5a4906e5e2a55189d073383ab77f6c1860
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/05/2020
ms.locfileid: "81674187"
---
| **Creare macchine virtuali** || 
|---|---|
| [Gestire le macchine virtuali][1] | Creare, modificare, avviare, arrestare ed eliminare le macchine virtuali. |
| [Creare una macchina virtuale da un'immagine personalizzata][2] | Creare un'immagine personalizzata di macchina virtuale e usarla per creare nuove macchine virtuali. | 
| [Creare una macchina virtuale che usa un disco rigido virtuale specializzato da uno snapshot][3] | Creare snapshot dal sistema operativo e dai dischi dati della macchina virtuale, creare dischi gestiti dagli snapshot e quindi creare una macchina virtuale collegando i dischi gestiti. |  
| [Creare macchine virtuali in parallelo nella stessa rete][4] | Creare macchine virtuali nella stessa area sulla stessa rete virtuale con due subnet in parallelo. |
| [Creare macchine virtuali in diverse aree in parallelo][5] | Creare e applicare il bilanciamento del carico a un set di macchine virtuali in diverse aree di Azure. |
| **Macchine virtuali di rete** || 
| [Gestire le reti virtuali][6] | Configurare una rete virtuale con due subnet e limitare l'accesso Internet a tali subnet. |
| **Creare set di scalabilità** ||
| [Creare un set di scalabilità di macchine virtuali con un servizio di bilanciamento del carico][7] | Creare un set di scalabilità di macchine virtuali, configurare un servizio di bilanciamento del carico e ottenere le stringhe di connessione SSH per le VM del set di scalabilità. |

[1]: ../java-sdk-manage-virtual-machines.md
[2]: https://github.com/Azure-Samples/managed-disk-java-create-virtual-machine-using-custom-image/
[3]: https://github.com/Azure-Samples/managed-disk-java-create-virtual-machine-using-specialized-disk-from-vhd/
[4]: https://github.com/Azure-Samples/compute-java-manage-virtual-machines-in-parallel/
[5]: ../java-sdk-virtual-machines-in-parallel.md
[6]: ../java-sdk-manage-virtual-networks.md
[7]: ../java-sdk-manage-vm-scalesets.md