---
author: edburns
ms.author: edburns
ms.date: 08/09/2020
ms.openlocfilehash: e006f2ec9d6f0d3b2d83a6653d65da19489dc221
ms.sourcegitcommit: b923aee828cd4b309ef92fe1f8d8b3092b2ffc5a
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/10/2020
ms.locfileid: "88052259"
---
### <a name="account-for-authentication-and-authorization"></a>Includere l'autenticazione e l'autorizzazione

La maggior parte delle applicazioni include una forma di autenticazione e autorizzazione.  Se si usa LDAP per l'autenticazione, WebLogic Server in Azure supporta l'integrazione automatica. L'offerta del marketplace usa Azure Active Directory Domain Services (Azure AD DS) con LDAP sicuro.  L'offerta crea l'area autenticazione predefinita per WebLogic Server dai dati in Azure AD DS.  Per altre informazioni, vedere [Autorizzazione e autenticazione degli utenti finali per la migrazione di app Java in WebLogic Server ad Azure](../migrate-weblogic-with-aad-ldap.md).
