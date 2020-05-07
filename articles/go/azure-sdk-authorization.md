---
title: Autenticazione con Azure SDK per Go
description: Informazioni sui metodi di autenticazione disponibili in Azure SDK per Go e su come usarli.
ms.date: 09/05/2018
ms.topic: conceptual
ms.openlocfilehash: b4bf09dbb3f59c77c2914ae9c9091dc0af31b093
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/05/2020
ms.locfileid: "80319678"
---
# <a name="authentication-methods-in-the-azure-sdk-for-go"></a>Metodi di autenticazione in Azure SDK per Go

Azure SDK per Go offre più modalità di autenticazione in Azure. Questi _tipi_ di autenticazione vengono richiamati tramite diversi _metodi_ di autenticazione. Questo articolo illustra i tipi e i metodi disponibili e mostra come scegliere l'approccio ottimale per l'applicazione.

## <a name="available-authentication-types-and-methods"></a>Metodi e tipi di autenticazione disponibili

Azure SDK per Go offre diversi tipi di autenticazione che usano set di credenziali diversi. Ogni tipo di autenticazione è disponibile tramite metodi di autenticazione diversi, ovvero secondo il modo in cui l'SDK riceve le credenziali come input. La tabella seguente descrive i tipi di autenticazione e le situazioni in cui sono consigliati per l'uso da parte dell'applicazione.

| Tipo di autenticazione | Consigliato quando... |
|---------------------|---------------------|
| Autenticazione basata su certificati | È presente un certificato X509 configurato per un utente o un'entità servizio di Azure Active Directory (AAD). Per altre informazioni, vedere [Introduzione all'autenticazione basata su certificati di Azure Active Directory]. |
| Credenziali del client | Si dispone di un'entità servizio configurata per l'applicazione o una classe di applicazioni a cui questa appartiene. Per altre informazioni, vedere [Creare un'entità servizio con l'interfaccia della riga di comando di Azure]. |
| Identità gestite per le risorse di Azure | L'applicazione viene eseguita in una risorsa di Azure che è stata configurata con un'identità gestita. Per altre informazioni, vedere l'articolo relativo alle [identità gestite per le risorse di Azure]. |
| Token dispositivo | L'applicazione è stata progettata __solo__ per l'utilizzo interattivo. È possibile che l'utente abbia abilitato Multi-Factor Authentication. Gli utenti hanno accesso a un Web browser per eseguire l'accesso. Per altre informazioni, vedere [Uso dell'autenticazione tramite token dispositivo](#use-device-token-authentication).|
| Nome utente/password | È disponibile un'applicazione interattiva che non può usare alcun altro metodo di autenticazione. Gli utenti non hanno abilitato Multi-Factor Authentication per l'accesso ad AAD. |

> [!IMPORTANT]
> Se si usa un tipo di autenticazione diverso dalle credenziali del client, l'applicazione deve essere registrata in Azure Active Directory. Per altri dettagli, vedere [Integrazione di applicazioni con Azure Active Directory](/azure/active-directory/develop/active-directory-integrating-applications).
>
> [!NOTE]
> A meno che non si abbiano esigenze particolari, evitare l'autenticazione con nome utente/password. Nelle situazioni in cui è opportuno eseguire l'accesso basato sull'utente è possibile usare l'autenticazione tramite token.

[Introduzione all'autenticazione basata su certificati di Azure Active Directory]: /azure/active-directory/active-directory-certificate-based-authentication-get-started
[Creare un'entità servizio con l'interfaccia della riga di comando di Azure]: /cli/azure/create-an-azure-service-principal-azure-cli
[Identità gestite per le risorse di Azure]: /azure/active-directory/managed-identities-azure-resources/overview

Questi tipi di autenticazione sono disponibili tramite metodi diversi.

* L'[_autenticazione basata su ambiente_](#use-environment-based-authentication) legge le credenziali direttamente dall'ambiente del programma.
* L'[_autenticazione basata su file_](#use-file-based-authentication) carica un file contenente le credenziali dell'entità servizio.
* L'[_autenticazione basata su client_](#use-an-authentication-client) usa un oggetto nel codice e chiede all'utente di fornire le credenziali durante l'esecuzione del programma.
* L'[_autenticazione tramite token dispositivo_](#use-device-token-authentication) richiede l'accesso interattivo degli utenti tramite un Web browser con un token.

Tutti i tipi e le funzioni di autenticazione sono disponibili nel pacchetto `github.com/Azure/go-autorest/autorest/azure/auth`.

> [!NOTE]
> A meno che non si abbiano esigenze particolari, evitare l'autenticazione basata su client. Questo metodo di autenticazione incoraggia procedure non valide. In particolare, l'autenticazione basata su client spinge a impostare le credenziali come hardcoded. La scrittura di codici personalizzati per l'autenticazione potrà essere anche interrotta in versioni future di SDK qualora i requisiti di autenticazione cambino.

## <a name="use-environment-based-authentication"></a>Usare l'autenticazione basata su ambiente

Se si esegue l'applicazione in un contesto controllato, l'autenticazione basata su ambiente è una scelta naturale. Con questo metodo di autenticazione è possibile configurare l'ambiente shell prima di eseguire l'applicazione. In fase di esecuzione Go SDK legge tali variabili di ambiente per l'autenticazione in Azure.

L'autenticazione basata su ambiente è supportata per tutti i metodi di autenticazione ad eccezione dei token dispositivo, valutati nell'ordine seguente:

* Credenziali del client
* Certificati X509
* Nome utente/password
* Identità gestite per le risorse di Azure

Se un tipo di autenticazione ha valori non configurati o viene rifiutato, l'SDK prova automaticamente il tipo di autenticazione successivo. Quando non sono disponibili altri tipi, l'SDK restituisce un errore.

La tabella seguente illustra in dettaglio le variabili di ambiente che devono essere impostate per ogni tipo di autenticazione supportato dall'autenticazione basata su ambiente.


|  Tipo di autenticazione   |     Variabile di ambiente     |                                                                                                     Descrizione                                                                                                      |
|------------------------|------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Credenziali del client** |      `AZURE_TENANT_ID`       |                                                                    ID del tenant di Active Directory a cui appartiene l'entità servizio.                                                                     |
|                        |      `AZURE_CLIENT_ID`       |                                                                                       Nome o ID dell'entità servizio.                                                                                       |
|                        |    `AZURE_CLIENT_SECRET`     |                                                                                  Segreto associato all'entità servizio.                                                                                   |
|    **Certificate**     |      `AZURE_TENANT_ID`       |                                                                   ID del tenant di Active Directory che viene registrato con il certificato.                                                                    |
|                        |      `AZURE_CLIENT_ID`       |                                                                              ID client dell'applicazione associato al certificato.                                                                              |
|                        |   `AZURE_CERTIFICATE_PATH`   |                                                                                       Percorso del file di certificato client.                                                                                       |
|                        | `AZURE_CERTIFICATE_PASSWORD` |                                                                                       Password del certificato client.                                                                                       |
| **Nome utente/password**  |      `AZURE_TENANT_ID`       |                                                                           ID del tenant di Active Directory a cui appartiene l'utente.                                                                           |
|                        |      `AZURE_CLIENT_ID`       |                                                                                              ID client dell'applicazione.                                                                                              |
|                        |       `AZURE_USERNAME`       |                                                                                            Nome utente con cui accedere.                                                                                             |
|                        |       `AZURE_PASSWORD`       |                                                                                            Password con cui accedere.                                                                                             |
|  **Identità gestita**  |                              | Non sono necessarie credenziali per l'autenticazione con identità gestita. L'applicazione deve essere eseguita in una risorsa di Azure configurata per l'uso delle identità gestite. Per informazioni dettagliate, vedere l'articolo relativo alle [identità gestite per le risorse di Azure]. |

Per connettersi a un endpoint di gestione o cloud diverso dal cloud pubblico di Azure, impostare anche le variabili di ambiente seguenti. I motivi più comuni per impostarle includono l'uso di Azure Stack, un cloud in un'area geografica diversa o il modello di distribuzione classica.

| Variabile di ambiente | Descrizione  |
|----------------------|--------------|
| `AZURE_ENVIRONMENT` | Nome dell'ambiente cloud a cui connettersi. |
| `AZURE_AD_RESOURCE` | ID di risorsa di Active Directory da usare durante la connessione, come URI per l'endpoint di gestione. |

Quando si usa l'autenticazione basata su ambiente, chiamare la funzione [NewAuthorizerFromEnvironment](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromEnvironment) per ottenere l'oggetto provider di autorizzazioni. Questo oggetto viene quindi impostato sulla proprietà `Authorizer` dei client per consentire loro l'accesso ad Azure.

```go
import "github.com/Azure/go-autorest/autorest/azure/auth"
authorizer, err := auth.NewAuthorizerFromEnvironment()
```

### <a name="authentication-on-azure-stack"></a>Autenticazione in Azure Stack

Per autenticarsi in Azure Stack, è necessario impostare le variabili seguenti:

| Variabile di ambiente | Descrizione  |
|----------------------|--------------|
| `AZURE_AD_ENDPOINT` | Endpoint di Active Directory. |
| `AZURE_AD_RESOURCE` | ID risorsa di Active Directory. |

Queste variabili possono essere recuperate dalle informazioni sui metadati di Azure Stack. Per recuperare i metadati, aprire un Web browser nel proprio ambiente di Azure Stack e utilizzare l'URL: `(ResourceManagerURL)/metadata/endpoints?api-version=1.0`

`ResourceManagerURL` varia in base al nome dell'area, al nome del computer e al nome di dominio completo esterno (FQDN) della distribuzione di Azure Stack:

| Environment | ResourceManagerURL |
|----------------------|--------------|
| Kit di sviluppo | `https://management.local.azurestack.external/` |
| Sistemi integrati | `https://management.(region).ext-(machine-name).(FQDN)` |

Per altre informazioni su come usare Azure SDK per Go in Azure Stack, vedere [Usare profili di versioni delle API con Go in Azure Stack](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-version-profiles-go)

## <a name="use-file-based-authentication"></a>Usare l'autenticazione basata su file

L'autenticazione basata su file usa un formato di file generato dall'[interfaccia della riga di comando di Azure](/cli/azure). È possibile creare facilmente questo file quando si crea una nuova entità servizio con il parametro `--sdk-auth`. Se si intende usare l'autenticazione basata su file, assicurarsi che questo argomento venga fornito quando si crea un'entità servizio. Poiché l'interfaccia della riga di comando stampa l'output in `stdout`, reindirizzare l'output verso un file.

```azurecli
az ad sp create-for-rbac --sdk-auth > azure.auth
```

Impostare la variabile di ambiente `AZURE_AUTH_LOCATION` in cui si trova il file di autorizzazione. Questa variabile di ambiente viene letta dall'applicazione e le credenziali contenute in essa vengono analizzate. Se è necessario selezionare il file di autorizzazione in fase di runtime, modificare l'ambiente del programma tramite la funzione [os.Setenv](https://golang.org/pkg/os/#Setenv).

Per caricare le informazioni di autenticazione, chiamare la funzione [NewAuthorizerFromFile](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromFile). A differenza dell'autorizzazione basata su ambiente, l'autorizzazione basata su file richiede un endpoint di risorse.

```go
import "github.com/Azure/go-autorest/autorest/azure/auth"
authorizer, err := NewAuthorizerFromFile(azure.PublicCloud.ResourceManagerEndpoint)
```

Per altre informazioni sull'uso delle entità servizio e sulla gestione delle relative autorizzazioni di accesso, vedere [Creare un'entità servizio con l'interfaccia della riga di comando di Azure].

## <a name="use-device-token-authentication"></a>Uso dell'autenticazione tramite token dispositivo

Se si vuole che gli utenti accedano in modo interattivo, l'autenticazione tramite token dispositivo è l'approccio ottimale. Questo flusso di autenticazione fornisce all'utente un token da incollare in un sito di accesso di Microsoft, in cui l'utente può quindi autenticarsi con un account di Azure Active Directory (AAD). Questo metodo di autenticazione supporta gli account con autenticazione a più fattori abilitata, a differenza dell'autenticazione standard tramite nome utente/password.

Per usare l'autenticazione tramite token dispositivo, creare un provider di autorizzazioni [DeviceFlowConfig](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#DeviceFlowConfig) con la funzione [NewDeviceFlowConfig](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewDeviceFlowConfig). Chiamare il [provider di autorizzazioni](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#DeviceFlowConfig.Authorizer) nell'oggetto risultante per avviare il processo di autenticazione. L'autenticazione tramite flusso del dispositivo blocca l'esecuzione del programma fino a quando non è stato completato l'intero flusso di autenticazione.

```go
import "github.com/Azure/go-autorest/autorest/azure/auth"
deviceConfig := auth.NewDeviceFlowConfig(applicationID, tenantID)
authorizer, err := deviceConfig.Authorizer()
```

## <a name="use-an-authentication-client"></a>Usare un client di autenticazione

Se è necessario un tipo di autenticazione specifico e si è disposti a far eseguire al programma il caricamento delle informazioni di autenticazione da parte dell'utente, è possibile usare qualsiasi client conforme all'interfaccia [auth.AuthorizerConfig](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#AuthorizerConfig). Usare un tipo che implementa questa interfaccia nelle situazioni seguenti:

* Scrittura di un programma interattivo
* Uso di file di configurazione specializzati
* Presenza di un requisito che impedisce l'uso di un metodo di autenticazione predefinito

> [!WARNING]
> Mai impostare come hardcoded le credenziali di Azure in un'applicazione. L'inserimento di informazioni riservate in un file binario dell'applicazione rende più semplice per un utente malintenzionato estrarle, indipendentemente dal fatto che l'applicazione sia in esecuzione o meno. In questo modo vengono messe a rischio tutte le risorse di Azure per cui sono autorizzate le credenziali!

Nella tabella seguente sono elencati i tipi conformi all'interfaccia `AuthorizerConfig` nell'SDK.

| Tipo di autenticazione | Tipo di provider di autorizzazioni |
|---------------------|-----------------------|
| Autenticazione basata su certificati | [ClientCertificateConfig] |
| Credenziali del client | [ClientCredentialsConfig] |
| Identità gestite per le risorse di Azure | [MSIConfig] |
| Nome utente/password | [UsernamePasswordConfig] |

[ClientCertificateConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#ClientCertificateConfig
[ClientCredentialsConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#ClientCredentialsConfig
[MSIConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#MSIConfig
[DeviceFlowConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#DeviceFlowConfig
[UsernamePasswordConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#UsernamePasswordConfig

Creare un autenticatore con la rispettiva funzione `New` associata e quindi chiamare `Authorize` nell'oggetto risultante per eseguire l'autenticazione. Ad esempio, per usare l'autenticazione basata su certificati:

```go
import "github.com/Azure/go-autorest/autorest/azure/auth"
certificateAuthorizer := auth.NewClientCertificateConfig(certificatePath, certificatePassword, clientID, tenantID)
authorizerToken, err := certificateAuthorizer.Authorize()
```
