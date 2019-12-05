---
title: Creare un flusso di lavoro di app per la logica
description: Creare un flusso di lavoro di app per la logica
ms.topic: conceptual
ms.date: 6/15/2017
ms.openlocfilehash: e561d57427b4a8e52f1d13bf3a0ff36fc0ed047c
ms.sourcegitcommit: e77f8f652128b798dbf972078a7b460ed21fb5f8
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/25/2019
ms.locfileid: "74466319"
---
# <a name="create-a-logic-app-workflow"></a>Creare un flusso di lavoro di app per la logica

Il codice seguente consente di creare un flusso di lavoro di app per la logica.

```python
from azure.mgmt.logic.models import Workflow

group_name = 'myresourcegroup'
workflow_name = '12HourHeartBeat'
logic_client.workflows.create_or_update(
    group_name,
    workflow_name,
    Workflow(
        location = 'East US',
        definition={
            "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
            "contentVersion": "1.0.0.0",
            "parameters": {},
            "triggers": {},
            "actions": {},
            "outputs": {}
        }
    )
)
```

