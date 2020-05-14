---
author: edburns
ms.author: edburns
ms.date: 1/21/2020
ms.openlocfilehash: 73f3893f22764280ed5d7c030e7c8eab7d2bf6b1
ms.sourcegitcommit: 226ebca0d0e3b918928f58a3a7127be49e4aca87
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/08/2020
ms.locfileid: "82988784"
---
Se l'applicazione contiene codice con dipendenze dal sistema operativo host, sarà necessario effettuarne il refactoring per rimuovere tali dipendenze. Ad esempio, potrebbe essere necessario sostituire qualsiasi utilizzo di `/` o `\` nei percorsi del file system con [`File.Separator`](https://docs.oracle.com/javase/8/docs/api/java/io/File.html#separator) o [`Paths.get`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Paths.html#get-java.lang.String-java.lang.String...-).
