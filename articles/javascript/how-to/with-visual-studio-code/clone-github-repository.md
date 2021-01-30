---
title: Clonare il repository GitHub con VSCode
description: Clonare un repository pubblico da GitHub nel computer locale usando Visual Studio Code.
ms.topic: how-to
ms.date: 01/28/2021
ms.custom: devx-track-js
ms.openlocfilehash: d47dfe7a7f0cee694e04d5f88098543db1e0ee0a
ms.sourcegitcommit: 3843092e47691fbd32452c93d51f894a0cab31db
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2021
ms.locfileid: "99072975"
---
# <a name="clone-and-work-with-a-github-repository-with-visual-studio-code"></a>Clonare e usare un repository GitHub con Visual Studio Code

Informazioni sui passaggi per clonare un repository pubblico da GitHub nel computer locale usando Visual Studio Code.

L'uso di Visual Studio Code con un repository usa strumenti distinti:

|Icona|Informazioni|Accesso da|
|--|--|--|
|| [INTERFACCIA della riga di comando git](https://code.visualstudio.com/docs/getstarted/userinterface#_command-palette)|riquadro comandi-F1|
|:::image type="content" source="../../media/how-to-clone-github-repo/git-commit-icon-activity-bar.png" alt-text="Icona per il controllo del codice sorgente.":::|Estensione del controllo del codice sorgente|Barra attività|
|:::image type="content" source="../../media/how-to-clone-github-repo/github-icon-activity-bar.png" alt-text="Icona per le richieste pull e i problemi di GitHub":::|Estensione GitHub|Barra attività|

Questi strumenti sono concepiti per eseguire rapidamente attività comuni. Nelle procedure riportate di seguito vengono utilizzate le parti denominate dell' [interfaccia utente di Visual Studio Code](https://code.visualstudio.com/docs/getstarted/userinterface). 

## <a name="use-command-palette-to-clone-repository"></a>Usare il riquadro comandi per clonare il repository

Per iniziare, scaricare il progetto di esempio usando la procedura seguente:

1. Premere **F1** per visualizzare il riquadro comandi.

1. Al prompt del riquadro comandi immettere `gitcl`, selezionare il comando **Git: Clone** e premere **INVIO**.

    ![Comando gitcl nel prompt del riquadro comandi di Visual Studio Code](../../media/how-to-clone-github-repo/visual-studio-code-git-clone.png)

1. Quando viene richiesto l' **URL del repository**, immettere un URL del repository GitHub, quindi premere **invio**.

1. Selezionare o creare la directory locale in cui si vuole clonare il progetto.

    ![Esplora risorse di Visual Studio Code](../../media/how-to-clone-github-repo/visual-studio-code-explorer.png)

## <a name="create-a-branch-for-changes-with-git-cl"></a>Creare un ramo per le modifiche con git CL

Usare git nel riquadro comandi per creare un nuovo ramo.

1. Premere **F1** per visualizzare il riquadro comandi.
1. Cercare `git branch` e selezionare `Git: Create Branch` .

    :::image type="content" source="../../media/how-to-clone-github-repo/git-cli-branch-list.png" alt-text="Cercare &quot;git branch&quot; e selezionare &quot;git: create Branch&quot;.":::

1. Immettere un nuovo nome per il ramo. Il nome del ramo è visibile nella barra di stato. 

    :::image type="content" source="../../media/how-to-clone-github-repo/git-branch-status-bar-visual-studio-code.png" alt-text="Il nome del ramo è visibile nella barra di stato.":::

## <a name="create-a-branch-from-status-bar"></a>Creare un ramo dalla barra di stato

1. Selezionare il nome del ramo nella barra di stato. 

    La barra di stato si trova in genere nella parte inferiore di Visual Studio Code. 

1. Nel riquadro comandi selezionare **+ + Crea un nuovo ramo**.
1. Immettere il nome del nuovo ramo. 

1. Immettere un nuovo nome per il ramo. Il nome del ramo è visibile nella barra di stato. 

    :::image type="content" source="../../media/how-to-clone-github-repo/git-branch-status-bar-visual-studio-code.png" alt-text="Il nome del ramo è visibile nella barra di stato.":::

## <a name="commit-changes-with-git"></a>Eseguire il commit delle modifiche con git 

Dopo aver apportato modifiche al ramo, eseguire il commit delle modifiche

1. Passare alla barra attività e selezionare l'icona git.

1. Nella finestra di **messaggio** immettere un messaggio di commit e premere **CTRL** + **invio** oppure selezionare il segno di spunta nella barra di controllo del codice sorgente.

    ![Aggiunta del file yarn.lock a Git](../../media/how-to-clone-github-repo/visual-studio-code-add-yarn-lock.png)

## <a name="push-a-local-branch-to-remote-from-status-bar"></a>Eseguire il push di un ramo locale in remoto dalla barra di stato

1. Selezionare l'icona di push a destra del nome del ramo. 
1. Selezionare il nome remoto nella casella popup. Se si dispone di un solo remoto, non verrà chiesto di selezionare il nome remoto. 

## <a name="push-a-local-branch-to-remote-from-the-source-control-extension"></a>Eseguire il push di un branch locale in remoto dall'estensione del controllo del codice sorgente
1. Selezionare l'icona del controllo del codice sorgente nella barra attività. 
1. Selezionare i puntini di sospensione (...), quindi selezionare **pull, push**, quindi selezionare **push in...*. 
1. Selezionare il nome remoto nella casella popup. Se si dispone di un solo remoto, non verrà chiesto di selezionare il nome remoto. 

## <a name="next-steps"></a>Passaggi successivi

* Come [distribuire un'app Web](../deploy-web-app.md)
