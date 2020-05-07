---
ms.openlocfilehash: 8f757c030849cb89eea36d74b55867dcc2ff98a6
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/05/2020
ms.locfileid: "80441356"
---
È possibile eliminare il gruppo di risorse usando il [portale di Azure](https://portal.azure.com) o l'interfaccia della riga di comando di Azure:

- Nel portale selezionare **Gruppi di risorse** nel riquadro di spostamento sinistro, selezionare il gruppo di risorse creato nella procedura di questa esercitazione e quindi usare il comando **Elimina gruppo di risorse**.

- Eseguire il comando dell'interfaccia della riga di comando di Azure seguente (in locale o usando tramite [Cloud Shell](/azure/cloud-shell/overview)), sostituendo `<resource_group>` con il nome del gruppo usato in questa esercitazione:

    ```azurecli
    az group delete --name <resource_group>
    ```
