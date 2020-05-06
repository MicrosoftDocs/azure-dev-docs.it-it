---
author: tomarchermsft
ms.service: ansible
ms.topic: include
ms.date: 04/09/2020
ms.author: tarcher
ms.openlocfilehash: 4f1fbe75f974aadc9e92e518e0e84d49ee73ed3d
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/05/2020
ms.locfileid: "81755224"
---
- **Configurare la raccolta di Azure**: Eseguire il comando seguente in una finestra del terminale per installare la raccolta di Azure. Se la raccolta di Azure è già installata, il flag `--force` la aggiornerà alla versione più recente.

    ```bash
    ansible-galaxy collection install azure.azcollection --force
    ```
