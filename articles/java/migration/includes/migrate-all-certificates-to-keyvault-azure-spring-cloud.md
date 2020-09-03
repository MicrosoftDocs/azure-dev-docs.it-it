---
author: yevster
ms.author: yebronsh
ms.date: 7/17/2020
ms.openlocfilehash: 1e7957a03cb96254a0318774a1620c7143ae6fc4
ms.sourcegitcommit: 4036ac08edd7fc6edf8d11527444061b0e4531ef
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/28/2020
ms.locfileid: "89062057"
---
### <a name="migrate-all-certificates-to-keyvault"></a>Eseguire la migrazione di tutti i certificati a Key Vault

Azure Spring Cloud non fornisce l'accesso all'archivio chiavi JRE, quindi è necessario eseguire la migrazione dei certificati ad Azure Key Vault e modificare il codice dell'applicazione per accedere ai certificati in Key Vault. Per altre informazioni, vedere [Introduzione ai certificati Key Vault](/azure/key-vault/certificates/certificate-scenarios) e [Libreria client dei certificati di Azure Key Vault per Java](/java/api/overview/azure/security-keyvault-certificates-readme).