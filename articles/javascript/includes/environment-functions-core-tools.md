---
title: file di inclusione completed.md
description: file di inclusione completed.md
ms.topic: include
ms.date: 10/13/2020
ms.custom: devx-track-javascript
ms.openlocfilehash: 09f69293062f52dfc2190cfeb040ad45ea428c67
ms.sourcegitcommit: 801682d3fc9651bf95d44e58574d5a4564be6feb
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/06/2020
ms.locfileid: "94338516"
---
## <a name="install-the-azure-functions-core-tools"></a>Installare gli strumenti di base per Funzioni di Azure

Per abilitare il debug locale, è necessario installare [Azure Functions Core Tools](https://github.com/Azure/azure-functions-core-tools). Questa operazione può essere eseguita direttamente in Visual Studio Code.

1. Avviare Visual Studio Code.

1. Aprire il **riquadro comandi** ( **F1** ) e immettere **Funzioni di Azure: Install or Update Azure Functions Core Tools** (Installa o aggiorna Azure Functions Core Tools), quindi premere **INVIO** per eseguire il comando.

1. Per verificare l'installazione, selezionare il comando di menu **Terminale** > **Nuovo terminale** in VS Code, quindi eseguire il comando `func`. Il comando dovrebbe mostrare un output simile al seguente, insieme alle informazioni sull'utilizzo.

    <pre>
                      %%%%%%
                     %%%%%%
                @   %%%%%%    @
              @@   %%%%%%      @@
           @@@    %%%%%%%%%%%    @@@
         @@      %%%%%%%%%%        @@
           @@         %%%%       @@
             @@      %%%       @@
               @@    %%      @@
                    %%
                    %

    Azure Functions Core Tools (2.4.419 Commit hash: c9c1724d002bd90b2e6b41393915ea3a26bcf0ce)
    Function Runtime Version: 2.0.12332.0
    </pre>