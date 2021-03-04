---
title: Usare archiviazione di Azure per i siti web statici
description: Collegamenti alla documentazione di archiviazione di Azure in cui viene illustrato come caricare i file nella risorsa di archiviazione e gestire direttamente tali file sul Web.
ms.date: 02/17/2021
ms.topic: conceptual
ms.custom: devx-track-python
ms.openlocfilehash: e657a20d2cb5c3d4dde50d5b12fc31a3d9980ece
ms.sourcegitcommit: 576c878c338d286060010646b96f3ad0fdbcb814
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/04/2021
ms.locfileid: "102118612"
---
# <a name="how-to-create-static-websites-on-azure-storage"></a>Come creare siti web statici in archiviazione di Azure

Un sito Web statico è composto da file HTML, CSS, JavaScript e altri file statici come immagini o tipi di carattere. Un sito statico è in genere un'applicazione a singola pagina (SPA) scritta con un numero qualsiasi di Framework JavaScript, ad esempio angolare, React o VME.

Tuttavia, si progetta l'app, è possibile gestire questi file direttamente da archiviazione di Azure anziché usare un server Web. L'hosting nell'archiviazione è più semplice e molto meno costoso rispetto alla gestione di un server Web; L'hosting statico in genere costa solo il mese di penne. In quale misura potrebbe essere necessaria l'elaborazione sul lato server, spesso è possibile soddisfare tali esigenze tramite funzioni senza server come supportato da funzioni di Azure.

Le risorse seguenti forniscono tutti i dettagli sulla creazione di siti web statici.

- [Hosting di siti web statici in archiviazione di Azure](/azure/storage/blobs/storage-blob-static-website): Panoramica della configurazione di archiviazione di Azure per l'hosting statico.

- [Ospitare un sito Web statico in archiviazione di Azure](/azure/storage/blobs/storage-blob-static-website-how-to?tabs=azure-cli): una procedura dettagliata su come abilitare l'hosting statico, caricare i file ed eseguire altre attività usando il portale di Azure, l'interfaccia della riga di comando di azure o Azure PowerShell.

- [Come usare le azioni di GitHub per distribuire un sito Web statico in archiviazione di Azure](/azure/storage/blobs/storage-blobs-static-site-github-actions): una procedura dettagliata sulla configurazione delle azioni di GitHub per distribuire automaticamente i file aggiornati da un repository di origine in archiviazione di Azure.

- [Distribuire un sito Web statico in Azure da Visual Studio Code](/azure/developer/javascript/tutorial/tutorial-vscode-static-website-node/tutorial-vscode-static-website-node-01): un'esercitazione che illustra la creazione di una semplice Spa in angolare, React, VME e svelte e quindi la distribuzione dell'app in archiviazione di Azure dall'interno Visual Studio Code.
