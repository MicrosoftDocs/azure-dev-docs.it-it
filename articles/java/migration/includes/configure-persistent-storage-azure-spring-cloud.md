---
author: yevster
ms.author: yebronsh
ms.date: 4/15/2020
ms.openlocfilehash: 6fdf908458eec428f4e01d1b5f20fcbf4e039dbe
ms.sourcegitcommit: 226ebca0d0e3b918928f58a3a7127be49e4aca87
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/08/2020
ms.locfileid: "82990223"
---
### <a name="configure-persistent-storage"></a>Configurare la risorsa di archiviazione persistente

Se una parte dell'applicazione legge o scrive nel file system locale, sarà necessario configurare l'archiviazione persistente in modo da sostituire il file system locale. Per altre informazioni, vedere [Usare l'archiviazione persistente in Azure Spring Cloud](/azure/spring-cloud/spring-cloud-howto-persistent-storage).

È consigliabile scrivere tutti i file temporanei nella directory `/tmp`. Per l'indipendenza del sistema operativo, è possibile accedere a questa directory usando `System.getProperty("java.io.tmpdir")`. È anche possibile usare [`java.nio.Files::createTempFile`](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/nio/file/Files.html#createTempFile(java.lang.String,java.lang.String,java.nio.file.attribute.FileAttribute...)) per creare file temporanei.
