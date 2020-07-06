---
author: yevster
ms.author: yebronsh
ms.date: 5/19/2020
ms.openlocfilehash: 51ed106fde71e37467571baab2b327b092dddc15
ms.sourcegitcommit: 81577378a4c570ced1e9c6765f4a9eee8453c889
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/08/2020
ms.locfileid: "84507633"
---
### <a name="migrate-and-enable-the-identity-provider"></a>Eseguire la migrazione e abilitare il provider di identità

Se l'applicazione richiede l'autenticazione o l'autorizzazione, assicurarsi che sia configurata per l'accesso al provider di identità seguendo queste istruzioni:

* Se il provider di identità è Azure Active Directory, non è necessario apportare alcuna modifica.
* Se il provider di identità è una foresta Active Directory locale, provare a implementare una soluzione di gestione delle identità ibrida con Azure Active Directory. Per altre informazioni, vedere la [documentazione delle identità ibride](/azure/active-directory/hybrid/).
* Se il provider di identità è un'altra soluzione locale, ad esempio PingFederate, vedere l'argomento [Installazione personalizzata di Azure AD Connect](/azure/active-directory/hybrid/how-to-connect-install-custom) per configurare la federazione con Azure Active Directory. In alternativa, valutare l'uso di Spring Security per usare il provider di identità personalizzato tramite [OAuth2/OpenID Connect](https://docs.spring.io/spring-security/site/docs/current/reference/html5/#oauth2) o [SAML](https://docs.spring.io/spring-security/site/docs/current/reference/html5/#servlet-saml2).
