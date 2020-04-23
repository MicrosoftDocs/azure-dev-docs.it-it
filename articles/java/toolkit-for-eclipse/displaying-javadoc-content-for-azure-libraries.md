---
title: Visualizzare il contenuto Javadoc in Eclipse
titleSuffix: Azure Libraries for Java
description: Come visualizzare il contenuto Javadoc per le librerie di Azure in Eclipse.
documentationcenter: java
ms.assetid: 30f8b6a1-1d76-4d1c-861b-1db478c46e6b
ms.date: 02/01/2018
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.openlocfilehash: 1d7aaafe790efc867d7ca609a3c7a4e37af02314
ms.sourcegitcommit: 0af39ee9ff27c37ceeeb28ea9d51e32995989591
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/21/2020
ms.locfileid: "81671077"
---
# <a name="displaying-javadoc-content-in-eclipse-for-the-azure-libraries-package-for-java"></a>Visualizzare il contenuto Javadoc in Eclipse per il pacchetto di librerie di Azure per Java

Il contenuto Javadoc per le librerie di Azure per Java può essere visualizzato nell'ambiente Eclipse associando il contenuto Javadoc per le librerie di Azure per Java. La procedura seguente mostra come usare questa funzionalità in Eclipse.

Questa procedura presuppone che la libreria di Azure per Java sia già stata aggiunta al percorso di compilazione.

## <a name="to-display-javadoc-content-in-eclipse-for-the-azure-libraries-for-java"></a>Visualizzare il contenuto Javadoc in Eclipse per le librerie di Azure per Java

1. In Project Explorer di Eclipse, nella sezione del progetto **Librerie a cui si fa riferimento** , aprire il menu di scelta rapida per la libreria di Azure per Java JAR. Ad esempio, **microsoft-windowsazure-api-0.1.0.jar** (il numero della versione può essere diverso, a seconda della versione installata).

1. Scegliere **Proprietà**.

1. Nella finestra di dialogo **Properties** (Proprietà) nel riquadro sinistro, fare clic su **Javadoc Location** (Posizione Javadoc). Viene visualizzata la finestra di dialogo **Posizione Javadoc** .

1. È possibile specificare un **URL Javadoc** o un **Javadoc nell'archivio**.

   * Se si sceglie di specificare un **URL Javadoc**, usare URL come **https://dl.windowsazure.com/javadoc** o **https://dl.windowsazure.com/storage/javadoc** .

   * Se si sceglie di utilizzare **Javadoc nell'archivio**, è possibile specificare un file esterno o un file dell’area di lavoro.

   Effettuare la selezione e sfogliare/convalidare in base alle esigenze. L'esempio seguente associa le librerie di Azure per Java con il corrispondente file JAR di Javadoc scaricato localmente in una cartella denominata **c:\MyAzureJARs**.

   ![Visualizzare il contenuto Javadoc.][ic553487]

1. *Passaggio facoltativo*: Fare clic su **Convalida**. Potrebbero essere visualizzati dei potenziali problemi con il file JAR di Javadoc.

1. Fare clic su **OK**.

Una volta associato alla libreria, il contenuto Javadoc deve essere visualizzato nell'IDE di Eclipse. Ad esempio, se `blob` è definito come tipo `CloudBlockBlob` nel codice, quello che segue è un esempio del contenuto Javadoc che viene visualizzato quando si digita `blob.acquireLease` nel codice:

![Visualizzare il contenuto Javadoc.][ic553488]

## <a name="next-steps"></a>Passaggi successivi

[!INCLUDE [additional-resources](includes/additional-resources.md)]

<!-- URL List -->

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh698319.aspx -->

<!-- IMG List -->

[ic553487]: media/displaying-javadoc-content-for-azure-libraries/ic553487.png
[ic553488]: media/displaying-javadoc-content-for-azure-libraries/ic553488.png
