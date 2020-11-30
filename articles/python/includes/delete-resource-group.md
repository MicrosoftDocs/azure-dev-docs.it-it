---
ms.openlocfilehash: c45bda8b08ec963febbc6497136cda71086423a3
ms.sourcegitcommit: 418e446e6ada5d50df283401df4f6b6370a356b9
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/25/2020
ms.locfileid: "96035491"
---
È possibile eliminare il gruppo di risorse usando il [portale di Azure](https://portal.azure.com) o l'interfaccia della riga di comando di Azure:

- Nel [portale di Azure](https://portal.azure.com) selezionare **Gruppi di risorse** nel riquadro di spostamento sinistro, selezionare il gruppo di risorse creato nella procedura di questa esercitazione e quindi usare il comando **Elimina gruppo di risorse**.

- Eseguire il comando dell'interfaccia della riga di comando di Azure seguente (in locale o usando tramite [Cloud Shell](/azure/cloud-shell/overview)), sostituendo `<resource_group>` con il nome del gruppo usato in questa esercitazione:

    ```azurecli
    az group delete --no-wait --name <resource_group>
    ```
