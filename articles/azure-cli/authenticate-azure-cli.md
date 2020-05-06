---
title: Accedere tramite l'interfaccia della riga di comando di Azure
description: Accedere con l'interfaccia della riga di comando di Azure in modo interattivo oppure usando le credenziali locali
author: dbradish-microsoft
ms.author: dbradish
manager: barbkess
ms.date: 02/22/2019
ms.topic: conceptual
ms.service: azure-cli
ms.devlang: azurecli
ms.openlocfilehash: 26112c9a7f338a7f178dc627d89f6e1f33d97714
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/05/2020
ms.locfileid: "82030863"
---
# <a name="sign-in-with-azure-cli"></a>Accedere tramite l'interfaccia della riga di comando di Azure 

Esistono diversi tipi di autenticazione per l'interfaccia della riga di comando di Azure. Il modo più semplice per iniziare è con [Azure Cloud Shell](/azure/cloud-shell/overview), che esegue automaticamente l'accesso.
A livello locale è possibile accedere in modo interattivo tramite il browser con il comando [az login](/cli/azure/reference-index#az-login). Quando si scrivono script, l'approccio consigliato è l'uso di entità servizio. Concedendo a un'entità servizio solo le autorizzazioni necessarie, è possibile preservare la sicurezza dell'automazione.

Nessuna informazione di accesso viene archiviata dall'interfaccia della riga di comando. Azure genera e archivia invece un [token di aggiornamento di autenticazione](https://docs.microsoft.com/azure/active-directory/develop/v1-id-and-access-tokens#refresh-tokens). A partire da agosto 2018, il token viene revocato dopo 90 giorni di inattività, ma questo valore può essere modificato da Microsoft o dall'amministratore del tenant. Dopo la revoca del token, un messaggio dell'interfaccia della riga di comando informa che è necessario eseguire di nuovo l'accesso.

Dopo l'accesso, i comandi dell'interfaccia della riga di comando vengono eseguiti sulla sottoscrizione predefinita. Se si hanno più sottoscrizioni, è possibile [modificare la sottoscrizione predefinita](manage-azure-subscriptions-azure-cli.md).

## <a name="sign-in-interactively"></a>Accedere in modo interattivo

Il metodo di autenticazione predefinito dell'interfaccia della riga di comando di Azure usa un Web browser e un token di accesso per l'accesso.

[!INCLUDE [interactive_login](includes/interactive-login.md)]

## <a name="sign-in-with-credentials-on-the-command-line"></a>Accedere con le credenziali dalla riga di comando

Specificare le credenziali utente di Azure nella riga di comando.

> [!Note]
> Questo approccio non funziona con gli account Microsoft o gli account in cui è abilitata l'autenticazione a due fattori.

```azurecli-interactive
az login -u <username> -p <password>
```

> [!IMPORTANT]
> Se si vuole evitare che la password venga visualizzata nella console e si usa `az login` in modo interattivo, usare il comando `read -s` in `bash`.
>
> ```bash
> read -sp "Azure password: " AZ_PASS && echo && az login -u <username> -p $AZ_PASS
> ```
>
> In PowerShell usare il cmdlet `Get-Credential`.
>
> ```powershell
> $AzCred = Get-Credential -UserName <username>
> az login -u $AzCred.UserName -p $AzCred.GetNetworkCredential().Password
> ```

## <a name="sign-in-with-a-service-principal"></a>Accedere con un'entità servizio

Le entità servizio sono account non associati a un utente specifico e per i quali è possibile assegnare autorizzazioni tramite ruoli predefiniti. L'autenticazione con un'entità servizio rappresenta il modo ottimale per la scrittura di script o programmi sicuri, consentendo di applicare sia limitazioni delle autorizzazioni che informazioni di credenziali statiche archiviate nell'ambiente locale. Per altre informazioni sulle entità servizio, vedere [Creare un'entità servizio di Azure con l'interfaccia della riga di comando di Azure](/cli/azure/create-an-azure-service-principal-azure-cli#sign-in-using-a-service-principal).

Per accedere con un'entità servizio è necessario:

* L'URL o il nome associato all'entità servizio
* La password dell'entità servizio o il certificato X509 usato per creare l'entità servizio in formato PEM
* Il tenant associato all'entità servizio, sotto forma di dominio `.onmicrosoft.com` o di ID oggetto di Azure

> [!NOTE]
> È necessario aggiungere un **CERTIFICATO** alla **CHIAVE PRIVATA** in un file PEM.  Per un esempio di formato di file PEM, vedere [Creare un'entità servizio di Azure con l'interfaccia della riga di comando di Azure](/cli/azure/create-an-azure-service-principal-azure-cli#sign-in-using-a-service-principal). 
>

> [!IMPORTANT]
>
> Se l'entità servizio usa un certificato archiviato in Key Vault, la chiave privata del certificato deve essere disponibile senza accedere ad Azure. È possibile recuperare una chiave privata da usare offline tramite il comando [az keyvault secret show](/cli/azure/keyvault/secret).

```azurecli-interactive
az login --service-principal -u <app-url> -p <password-or-cert> --tenant <tenant>
```

> [!IMPORTANT]
> Se si vuole evitare che la password venga visualizzata nella console e si usa `az login` in modo interattivo, usare il comando `read -s` in `bash`.
>
> ```bash
> read -sp "Azure password: " AZ_PASS && echo && az login --service-principal -u <app-url> -p $AZ_PASS --tenant <tenant>
> ```
>
> In PowerShell usare il cmdlet `Get-Credential`.
>
> ```powershell
> $AzCred = Get-Credential -UserName <app-url>
> az login --service-principal -u $AzCred.UserName -p $AzCred.GetNetworkCredential().Password --tenant <tenant>
> ```

## <a name="sign-in-with-a-different-tenant"></a>Accedere con un tenant diverso

È possibile selezionare un tenant per l'accesso con l'argomento `--tenant`. Il valore di questo argomento può essere un dominio `.onmicrosoft.com` oppure l'ID oggetto di Azure per il tenant. Con `--tenant` è possibile usare sia l'accesso interattivo che l'accesso da riga di comando.

```azurecli-interactive
az login --tenant <tenant>
```

## <a name="sign-in-with-a-managed-identity"></a>Accedere con un'identità gestita

Per le risorse di Azure configurate per le identità gestite, è possibile effettuare l'accesso usando l'identità gestita. L'accesso con l'identità della risorsa viene eseguito tramite il flag `--identity`.

```azurecli-interactive
az login --identity
```

Per altre informazioni sulle identità gestite per le risorse di Azure, vedere [Configurare le identità gestite per le risorse di Azure](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vm) e [Usare le identità gestite per le risorse di Azure per l'accesso](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/how-to-use-vm-sign-in).
