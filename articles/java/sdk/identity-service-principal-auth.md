---
title: Autenticazione di Azure con entità servizio
description: Panoramica dei concetti di Azure SDK per Java correlati all'autenticazione delle applicazioni tramite l'entità servizio
author: g2vinay
ms.date: 02/02/2021
ms.topic: conceptual
ms.custom: devx-track-java
ms.author: vigera
ms.openlocfilehash: fd8c107d4fb5d37e3f2c2e99ba45ad46a6de34ae
ms.sourcegitcommit: 71847ee0a1fee3f3320503629d9a8c82319a1f6a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/03/2021
ms.locfileid: "99522130"
---
# <a name="azure-authentication-with-service-principal"></a>Autenticazione di Azure con entità servizio

Questo articolo illustra il modo in cui la libreria di identità di Azure supporta l'autenticazione Azure Active Directory token tramite l'entità servizio. Gli argomenti trattati in questo articolo includono:

* [Creare un'entità servizio con l'interfaccia della riga di comando di Azure](#create-a-service-principal-with-the-azure-cli)
* [Credenziali segrete client](#client-secret-credential)
* [Credenziali certificato client](#client-certificate-credential)

Per ulteriori informazioni, vedere [oggetti applicazione e oggetti entità servizio in Azure Active Directory](/azure/active-directory/develop/app-objects-and-service-principals).

## <a name="create-a-service-principal-with-the-azure-cli"></a>Creare un'entità servizio con l'interfaccia della riga di comando di Azure

Usare gli esempi dell'interfaccia della riga di comando di [Azure][azure_cli] seguenti per creare o ottenere le credenziali del segreto client.

Usare il comando seguente per creare un'entità servizio e configurarne l'accesso alle risorse di Azure:

```azurecli
az ad sp create-for-rbac -n <your application name> --skip-assignment
```

Questo comando restituisce un valore simile all'output seguente:

```output
{
"appId": "generated-app-ID",
"displayName": "dummy-app-name",
"name": "http://dummy-app-name",
"password": "random-password",
"tenant": "tenant-ID"
}
```

Usare il comando seguente per creare un'entità servizio insieme a un certificato. Annotare il percorso/percorso del certificato.

```azurecli
az ad sp create-for-rbac -n <your application name> --skip-assignment --cert <certificate name> --create-cert
```

Controllare le credenziali restituite e prendere nota delle informazioni seguenti:

* `AZURE\_CLIENT\_ID` per l'appId.
* `AZURE\_CLIENT\_SECRET` per la password.
* `AZURE\_TENANT\_ID` per il tenant.

## <a name="client-secret-credential"></a>Credenziali segrete client

Questa credenziale autentica l'entità servizio creata tramite il segreto client (password). Questo esempio illustra l'autenticazione `SecretClient` di dalla libreria client [Azure-Security-Vault-Secrets][secrets_client_library] usando `ClientSecretCredential` .

```java
/**
 *  Authenticate with client secret.
 */
ClientSecretCredential clientSecretCredential = new ClientSecretCredentialBuilder()
  .clientId("<your client ID>")
  .clientSecret("<your client secret>")
  .tenantId("<your tenant ID>")
  .build();

// Azure SDK client builders accept the credential as a parameter.
SecretClient client = new SecretClientBuilder()
  .vaultUrl("https://<your Key Vault name>.vault.azure.net")
  .credential(clientSecretCredential)
  .buildClient();
```

## <a name="client-certificate-credential"></a>Credenziali certificato client

Questa credenziale autentica l'entità servizio creata tramite il certificato client. Questo esempio illustra l'autenticazione `SecretClient` di dalla libreria client [Azure-Security-Vault-Secrets][secrets_client_library] usando `ClientCertificateCredential` .

```java
/**
 *  Authenticate with a client certificate.
 */
ClientCertificateCredential clientCertificateCredential = new ClientCertificateCredentialBuilder()
  .clientId("<your client ID>")
  .pemCertificate("<path to PEM certificate>")
  // Choose between either a PEM certificate or a PFX certificate.
  //.pfxCertificate("<path to PFX certificate>", "PFX CERTIFICATE PASSWORD")
  .tenantId("<your tenant ID>")
  .build();

// Azure SDK client builders accept the credential as a parameter.
SecretClient client = new SecretClientBuilder()
  .vaultUrl("https://<your Key Vault name>.vault.azure.net")
  .credential(clientCertificateCredential)
  .buildClient();
```

## <a name="next-steps"></a>Passaggi successivi

Questo articolo ha analizzato l'autenticazione tramite l'entità servizio. Questa forma di autenticazione è uno dei diversi modi in cui è possibile eseguire l'autenticazione in Azure SDK per Java. Gli articoli seguenti descrivono altri modi:

* [Autenticazione di Azure negli ambienti di sviluppo](identity-dev-env-auth.md)
* [Autenticazione di applicazioni ospitate in Azure](identity-azure-hosted-auth.md)
* [Autenticazione con credenziali utente](identity-user-auth.md)

Dopo aver eseguito l'autenticazione, vedere [configurare la registrazione in Azure SDK per Java](logging-overview.md) per informazioni sulla funzionalità di registrazione fornita dall'SDK.

<!-- LINKS -->
[azure_cli]: /cli/azure
[secrets_client_library]: https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/keyvault/azure-security-keyvault-secrets
