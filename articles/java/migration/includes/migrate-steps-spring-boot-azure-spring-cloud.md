---
author: yevster
ms.author: yebronsh
ms.date: 8/25/2020
ms.openlocfilehash: 7970cae2b9ac39f7012e74b1334d12b2c5006af0
ms.sourcegitcommit: 4036ac08edd7fc6edf8d11527444061b0e4531ef
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/28/2020
ms.locfileid: "89062056"
---
### <a name="create-an-azure-spring-cloud-instance-and-apps"></a>Creare un'istanza di Azure Spring Cloud e le app

Effettuare il provisioning di un'istanza di Azure Spring Cloud nella sottoscrizione di Azure, se non ne esiste già una. Quindi, crearvi un'applicazione. Per altre informazioni, vedere [Avvio rapido: Avviare un'applicazione Azure Spring Cloud esistente con il portale di Azure](/azure/spring-cloud/spring-cloud-quickstart-launch-app-portal).

[!INCLUDE [ensure-console-logging-and-configure-diagnostic-settings-azure-spring-cloud](ensure-console-logging-and-configure-diagnostic-settings-azure-spring-cloud.md)]

[!INCLUDE [configure-persistent-storage-azure-spring-cloud](configure-persistent-storage-azure-spring-cloud.md)]

[!INCLUDE [migrate-all-certificates-to-keyvault-azure-spring-cloud](migrate-all-certificates-to-keyvault-azure-spring-cloud.md)]

### <a name="remove-application-performance-management-apm-integrations"></a>Rimuovere le integrazioni di gestione delle prestazioni delle applicazioni

Eliminare tutte le integrazioni con gli strumenti o gli agenti di gestione delle prestazioni delle applicazioni. Per informazioni sulla configurazione della gestione delle prestazioni con Monitoraggio di Azure, vedere la sezione [Post-migrazione](#post-migration).

### <a name="disable-metrics-clients-and-endpoints-in-your-applications"></a>Disabilitare i client e gli endpoint delle metriche nelle applicazioni

Rimuovere tutti i client o gli endpoint delle metriche esposti nelle applicazioni.

### <a name="deploy-the-application"></a>Distribuire l'applicazione

Distribuire tutti i microservizi sottoposti a migrazione, esclusi i server Spring Cloud Config e Registry, come descritto in [Avvio rapido: Avviare un'applicazione Azure Spring Cloud esistente con il portale di Azure](/azure/spring-cloud/spring-cloud-quickstart-launch-app-portal).

### <a name="configure-per-service-secrets-and-externalized-settings"></a>Configurare i segreti per servizio e le impostazioni esternalizzate

È possibile inserire tutte le impostazioni di configurazione per servizio in ogni servizio come variabili di ambiente. Nel portale di Azure seguire questa procedura:

1. Passare all'istanza di Azure Spring Cloud e selezionare **App**.
1. Selezionare il servizio da configurare.
1. Selezionare **Configurazione**.
1. Immettere le variabili da configurare.
1. Selezionare **Salva**.

![Impostazioni di configurazione app Spring Cloud](../media/migrate-spring-cloud-to-azure-spring-cloud/spring-cloud-app-configuration-settings.png)

### <a name="migrate-and-enable-the-identity-provider"></a>Eseguire la migrazione e abilitare il provider di identità

Se le applicazioni Spring Cloud richiedono l'autenticazione o l'autorizzazione, assicurarsi che siano configurate per l'accesso al provider di identità:

* Se il provider di identità è Azure Active Directory, non è necessario apportare alcuna modifica.
* Se il provider di identità è una foresta Active Directory locale, provare a implementare una soluzione di gestione delle identità ibrida con Azure Active Directory. Per altre informazioni, vedere la [documentazione delle identità ibride](/azure/active-directory/hybrid/).
* Se il provider di identità è un'altra soluzione locale, ad esempio PingFederate, vedere l'argomento [Installazione personalizzata di Azure AD Connect](/azure/active-directory/hybrid/how-to-connect-install-custom) per configurare la federazione con Azure Active Directory. In alternativa, valutare l'uso di Spring Security per usare il provider di identità personalizzato tramite [OAuth2/OpenID Connect](https://docs.spring.io/spring-security/site/docs/current/reference/html5/#oauth2) o [SAML](https://docs.spring.io/spring-security/site/docs/current/reference/html5/#servlet-saml2).

### <a name="expose-the-application"></a>Esporre l'applicazione

Per impostazione predefinita, le applicazioni distribuite in Azure Spring Cloud non sono visibili esternamente. È possibile esporre l'applicazione rendendola pubblica con il comando seguente:

```azurecli
az spring-cloud app update -n <application name> --is-public true
```

Ignorare questo passaggio se si usa o si intende usare Spring Cloud Gateway. Per altre informazioni, vedere la sezione seguente.
