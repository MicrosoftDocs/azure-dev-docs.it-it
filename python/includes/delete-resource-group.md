---
ms.openlocfilehash: 2f54e918e7126123ada68b696ea96f4d5f3dbb83
ms.sourcegitcommit: a8073315f751631ab983618fa9f812eb95d8b2dc
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/16/2020
ms.locfileid: "76125239"
---
È possibile eliminare il gruppo di risorse usando il [portale di Azure](https://portal.azure.com) o l'interfaccia della riga di comando di Azure:

- Nel portale selezionare **Gruppi di risorse** nel riquadro di spostamento sinistro, selezionare il gruppo di risorse creato nella procedura di questa esercitazione e quindi usare il comando **Elimina gruppo di risorse**.

- Eseguire il comando dell'interfaccia della riga di comando di Azure seguente (in locale o usando tramite [Cloud Shell](/cloud-shell/overview)), sostituendo `<resource_group>` con il nome del gruppo usato in questa esercitazione:

    ```azurecli
    az group delete --name <resource_group>
    ```
