---
author: yevster
ms.author: yebronsh
ms.date: 1/22/2020
ms.openlocfilehash: 2fc4e3b43a051103e7aea6aa77f66666ca454910
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/05/2020
ms.locfileid: "81672137"
---
<!-- Included in "### Switch to a supported platform" sections that have different (required) intro paragraphs. For example:

### Switch to a supported platform

App Service offers specific versions of Java SE. To ensure compatibility, migrate your application to one of the supported versions of in its current environment before you proceed with any of the remaining steps. Be sure to fully test the resulting configuration. Use the latest stable release of your Linux distribution in such tests.

-->

> [!NOTE]
> Questa convalida è particolarmente importante se il server corrente è in esecuzione in un JDK non supportato, ad esempio Oracle JDK o IBM OpenJ9.

Per ottenere la versione corrente di Java, accedere al server di produzione ed eseguire il comando seguente:

```bash
java -version
```

Per ottenere la versione corrente usata dal servizio app di Azure, scaricare [Zulu 8](https://www.azul.com/downloads/azure-only/zulu/?&version=java-8-lts&architecture=x86-64-bit&package=jdk) se si intende usare Java Runtime Environment 8 o [Zulu 11](https://www.azul.com/downloads/azure-only/zulu/?&version=java-11-lts&architecture=x86-64-bit&package=jdk) se si intende usare Java Runtime Environment 11.
