---
title: Usare la sostituzione delle variabili con GitHub Actions per Azure
description: GitHub Actions per la sostituzione delle variabili nei file con parametri
author: juliakm
ms.author: jukullam
ms.topic: conceptual
ms.service: azure
ms.date: 01/25/2021
ms.custom: github-actions-azure
ms.openlocfilehash: e2a82fbcbe48269339dc672d46aca4cc3601ae12
ms.sourcegitcommit: 6fbf9e489b194586887a2c11152044be5b3a2b99
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/25/2021
ms.locfileid: "98759494"
---
# <a name="use-variable-substitution-with-github-actions"></a>Usare la sostituzione delle variabili con GitHub Actions

Informazioni su come usare l'[azione di sostituzione delle variabili](https://github.com/marketplace/actions/variable-substitution) per sostituire i valori nei file di configurazione e dei parametri XML, JSON e YAML.

La sostituzione delle variabili consente di inserire i valori, inclusi i [segreti GitHub](https://docs.github.com/en/actions/reference/encrypted-secrets), nei file nel repository durante l'esecuzione del flusso di lavoro. Ad esempio, è possibile inserire credenziali accesso e password dell'API in un file JSON durante l'esecuzione del flusso di lavoro.

La sostituzione delle variabili funziona solo per le chiavi predefinite nella gerarchia di oggetti. Non è possibile creare nuove chiavi con la sostituzione delle variabili. Inoltre, solo le variabili definite come [variabili di ambiente](https://docs.github.com/en/actions/reference/environment-variables) nel flusso di lavoro o variabili di sistema già disponibili possono essere usate per la sostituzione.

## <a name="prerequisites"></a>Prerequisiti

- Un account GitHub. Se non è disponibile, iscriversi per riceverne uno [gratuito](https://github.com/join).  

## <a name="use-the-variable-substitution-action"></a>Usare l'azione di sostituzione delle variabili

Questo esempio illustra come sostituire i valori in `employee.json` usando l'[azione di sostituzione delle variabili](https://github.com/marketplace/actions/variable-substitution).

1. Creare `employee.json` al livello radice del repository.

    ```json
    {
        "first-name": "Toni",
        "last-name": "Cranz",
        "username": "",
        "password": "",
        "url": ""
    }
    ```

2. Aprire il repository GitHub e passare a **Impostazioni**.

    :::image type="content" source="media/github-repo-settings.png" alt-text="Selezionare Impostazioni nel riquadro di spostamento":::

3. Selezionare **Segreti** e quindi **Nuovo segreto**.

    :::image type="content" source="media/select-secrets.png" alt-text="Scegliere di aggiungere un segreto":::

4. Aggiungere un nuovo segreto `PASSWORD` con il valore `5v{W<$2B<GR2=t4#` (o una password selezionata). Salvare il segreto. 

5. Passare ad **Actions** (Azioni) e selezionare **Set up a workflow yourself** (Configurare manualmente un flusso di lavoro).

6. Aggiungere un file del flusso di lavoro. Il valore del nome utente nel file JSON verrà sostituito con `tcranz`. La password verrà sostituita con il segreto GitHub. Il campo URL verrà popolato con un URL che include la variabile GitHub `github.repository`.

    ```yaml
    on: [push]
    name: variable substitution in json

    jobs:
    build:
        runs-on: ubuntu-latest
        steps:
        - uses: actions/checkout@v2
        - uses: microsoft/variable-substitution@v1 
        with:
            files: 'employee.json'
        env:
            username: tcranz
            password: ${{ secrets.PASSWORD }}
            url: https://github.com/${{github.repository}}

    ```

7. Passare ad **Actions** per visualizzare l'esecuzione del flusso di lavoro. Aprire l'azione di sostituzione delle variabili. Si noterà che ogni variabile è stata sostituita.

    ```text
    SubstitutingValueonKeyWithString username tcranz
    SubstitutingValueonKeyWithString password ***
    SubstitutingValueonKeyWithString url https://github.com/account/variable-sub
    Successfully updated file: employee.json
    ```

## <a name="clean-up-resources"></a>Pulire le risorse

Eliminare il repository GitHub quando non è più necessario.

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"]
> [Distribuire in App Web di Azure con GitHub Actions](/azure/app-service/deploy-github-actions)
