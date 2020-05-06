---
title: Usare entità servizio di Azure con l'interfaccia della riga di comando di Azure
description: Informazioni su come creare e usare entità servizio con l'interfaccia della riga di comando di Azure.
author: dbradish-microsoft
ms.author: dbradish
manager: barbkess
ms.date: 02/15/2019
ms.topic: conceptual
ms.service: azure-cli
ms.devlang: azurecli
ms.openlocfilehash: c18adbee84fd3e5c73367b07bbd0b03ac61008cd
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/05/2020
ms.locfileid: "82030733"
---
# <a name="create-an-azure-service-principal-with-the-azure-cli"></a>Creare un'entità servizio di Azure con l'interfaccia della riga di comando di Azure

Gli strumenti automatici che usano i servizi di Azure devono sempre avere autorizzazioni limitate. Invece di consentire alle applicazioni l'accesso come utenti con privilegi completi, Azure offre le identità servizio.

Un'entità servizio di Azure è un'identità creata per l'uso con applicazioni, servizi in hosting e strumenti automatici per l'accesso alle risorse di Azure. Questo accesso è limitato dai ruoli assegnati all'entità servizio e consente quindi di definire quali risorse siano accessibili e a quale livello. Per motivi di sicurezza è sempre consigliabile usare le identità servizio per gli strumenti automatici, invece di consentire loro di accedere con un'identità utente.

Questo articolo illustra i passaggi per la creazione, l'acquisizione di informazioni correlate e il ripristino di un'entità servizio con l'interfaccia della riga di comando di Azure.

## <a name="create-a-service-principal"></a>Creare un'entità servizio

Creare un'entità servizio con il comando [az ad sp create-for-rbac](/cli/azure/ad/sp#az-ad-sp-create-for-rbac). Quando si crea un'entità servizio, si sceglie il tipo di autenticazione per l'accesso che verrà usata dall'entità.

> [!NOTE]
>
> Se l'account in uso non è autorizzato a creare un'entità servizio, `az ad sp create-for-rbac` restituirà un messaggio di errore contenente "I privilegi non sono sufficienti per completare l'operazione". Per creare un'entità servizio, contattare l'amministratore di Azure Active Directory.

Sono disponibili due tipi di autenticazione per le entità servizio: autenticazione basata su password e autenticazione basata su certificato.

### <a name="password-based-authentication"></a>Autenticazione basata su password

Senza parametri di autenticazione, l'autenticazione basata su password prevede una password casuale creata automaticamente.

  ```azurecli-interactive
  az ad sp create-for-rbac --name ServicePrincipalName
  ```

> [!IMPORTANT]
> A partire dalla versione 2.0.68 dell'interfaccia della riga di comando di Azure, il parametro `--password` per creare un'entità servizio con una password definita dall'utente __non è più supportato__ allo scopo di impedire l'uso accidentale di password vulnerabili.

L'output per un'entità servizio con autenticazione della password include la chiave `password`. __Assicurarsi__ di copiare questo valore perché non può essere recuperato. Se si dimentica la password, [reimpostare le credenziali dell'entità servizio](#reset-credentials).

Le chiavi `appId` e `tenant` vengono visualizzate nell'output di `az ad sp create-for-rbac` e usate nell'autenticazione dell'entità servizio.
Registrare i valori, che possono essere tuttavia recuperati in qualsiasi momento con [az ad sp list](/cli/azure/ad/sp#az-ad-sp-list).

### <a name="certificate-based-authentication"></a>Autenticazione basata su certificati

Per l'autenticazione basata su certificato, usare l'argomento `--cert`. Questo argomento richiede un certificato esistente. Assicurarsi che tutti gli strumenti che usano questa entità servizio abbiano accesso alla chiave privata del certificato. I certificati devono essere in un formato ASCII come PEM, CER o DER. Passare il certificato come stringa oppure usare il formato `@path` per caricare il certificato da un file.

> [!NOTE]
> Quando si usa un file PEM, è necessario aggiungere un **CERTIFICATO** alla **CHIAVE PRIVATA** nel file.

```azurecli-interactive
az ad sp create-for-rbac --name ServicePrincipalName --cert "-----BEGIN CERTIFICATE-----
...
-----END CERTIFICATE-----"
```

```azurecli-interactive
az ad sp create-for-rbac --name ServicePrincipalName --cert @/path/to/cert.pem
```

È possibile aggiungere l'argomento `--keyvault` per usare un certificato in Azure Key Vault. In questo caso, il valore `--cert` è il nome del certificato.

```azurecli-interactive
az ad sp create-for-rbac --name ServicePrincipalName --cert CertName --keyvault VaultName
```

Per creare un certificato _autofirmato_ per l'autenticazione, usare l'argomento `--create-cert`:

```azurecli-interactive
az ad sp create-for-rbac --name ServicePrincipalName --create-cert
```

Output della console:

```
Creating a role assignment under the scope of "/subscriptions/myId"
Please copy C:\myPath\myNewFile.pem to a safe place.
When you run 'az login', provide the file path in the --password argument
{
  "appId": "myAppId",
  "displayName": "myDisplayName",
  "fileWithCertAndPrivateKey": "C:\\myPath\\myNewFile.pem",
  "name": "http://myName",
  "password": null,
  "tenant": "myTenantId"
}
```

Contenuto del nuovo file PEM:
```
-----BEGIN PRIVATE KEY-----
myPrivateKeyValue
-----END PRIVATE KEY-----
-----BEGIN CERTIFICATE-----
myCertificateValue
-----END CERTIFICATE-----
```

> [!NOTE]
> Il comando `az ad sp create-for-rbac --create-cert` crea l'entità servizio e un file PEM. Il file PEM contiene una **CHIAVE PRIVATA** e un **CERTIFICATO** formattati correttamente.

È possibile aggiungere l'argomento `--keyvault` per archiviare il certificato in Azure Key Vault. Quando si usa `--keyvault`, l'argomento `--cert` è __obbligatorio__.

```azurecli-interactive
az ad sp create-for-rbac --name ServicePrincipalName --create-cert --cert CertName --keyvault VaultName
```

A meno che non si archivi il certificato in Key Vault, l'output include la chiave `fileWithCertAndPrivateKey`. Il valore di questa chiave indica la posizione di archiviazione del certificato generato.
__Assicurarsi__ di copiare il certificato in una posizione sicura, in caso contrario non sarà possibile accedere con questa entità servizio.

Per i certificati archiviati in Key Vault, recuperare la chiave privata del certificato con [az keyvault secret show](/cli/azure/keyvault/secret#az-keyvault-secret-show). In Key Vault, il nome del segreto del certificato è uguale al nome del certificato. Se si perde l'accesso alla chiave privata del certificato, [reimpostare le credenziali dell'entità servizio](#reset-credentials).

Le chiavi `appId` e `tenant` vengono visualizzate nell'output di `az ad sp create-for-rbac` e usate nell'autenticazione dell'entità servizio.
Registrare i valori, che possono essere tuttavia recuperati in qualsiasi momento con [az ad sp list](/cli/azure/ad/sp#az-ad-sp-list).

## <a name="get-an-existing-service-principal"></a>Ottenere un'entità servizio esistente

È possibile recuperare un elenco delle entità servizio presenti in un tenant con [az ad sp list](/cli/azure/ad/sp#az-ad-sp-list). Per impostazione predefinita, questo comando restituisce le prime 100 entità servizio per il tenant. Per ottenere tutte le entità servizio del tenant, usare l'argomento `--all`. L'ottenimento di questo elenco può richiedere molto tempo, quindi è consigliabile filtrare l'elenco con uno degli argomenti seguenti:

* `--display-name` richiede entità di servizio che hanno un _prefisso_ corrispondente al nome specificato. Il nome visualizzato di un'entità servizio è il valore impostato con il parametro `--name` durante la creazione. Se `--name` non è stato impostato durante la creazione dell'entità servizio, il prefisso del nome è `azure-cli-`.
* `--spn` filtra in base alla corrispondenza esatta del nome dell'entità servizio. Il nome dell'entità servizio inizia sempre con `https://`.
  Se il valore usato per `--name` non era un URI, questo valore è `https://` seguito dal nome visualizzato.
* `--show-mine` richiede solo entità servizio create dall'utente che ha eseguito l'accesso.
* `--filter` accetta un filtro OData ed esegue il filtro _sul lato server_. Questo metodo è preferibile all'uso del filtro sul lato client con l'argomento `--query` dell'interfaccia della riga di comando. Per informazioni sui filtri OData, vedere [Sintassi delle espressioni OData per filtri e ordinamento](/rest/api/searchservice/odata-expression-syntax-for-azure-search).

Le informazioni restituite per gli oggetti dell'entità servizio sono dettagliate. Per ottenere solo le informazioni necessarie per l'accesso, usare la stringa di query `[].{id:appId, tenant:appOwnerTenantId}`. Ad esempio, per ottenere le informazioni di accesso per tutte le entità servizio create dall'utente attualmente connesso:

```azurecli-interactive
az ad sp list --show-mine --query "[].{id:appId, tenant:appOwnerTenantId}"
```

> [!IMPORTANT]
>
> `az ad sp list` o [az ad sp show](/cli/azure/ad/sp#az-ad-sp-show) consente di ottenere utente e tenant, ma non i segreti di autenticazione _oppure_ il metodo di autenticazione.
> I segreti dei certificati presenti in Key Vault possono essere recuperati con [az keyvault secret show](/cli/azure/keyvault/secret#az-keyvault-secret-show), ma per impostazione predefinita non vengono archiviati altri segreti.
> Se si dimentica un metodo di autenticazione o un segreto, [reimpostare le credenziali dell'entità servizio](#reset-credentials).

## <a name="manage-service-principal-roles"></a>Gestire i ruoli delle entità servizio

Per gestire le assegnazioni di ruolo, l'interfaccia della riga di comando di Azure offre i comandi seguenti:

* [az role assignment list](/cli/azure/role/assignment#az-role-assignment-list)
* [az role assignment create](/cli/azure/role/assignment#az-role-assignment-create)
* [az role assignment delete](/cli/azure/role/assignment#az-role-assignment-delete)

Il ruolo predefinito per un'entità servizio è quello **Collaboratore**. Questo ruolo ha autorizzazioni complete per la lettura e la scrittura in un account di Azure. Il ruolo **Lettore** è più restrittivo e offre l'accesso in sola lettura.  Per altre informazioni sul controllo degli accessi in base al ruolo e i ruoli, vedere [Controllo degli accessi in base al ruolo: ruoli predefiniti](/azure/active-directory/role-based-access-built-in-roles).

Questo esempio aggiunge il ruolo **Lettore** e rimuove il ruolo **Collaboratore**:

```azurecli-interactive
az role assignment create --assignee APP_ID --role Reader
az role assignment delete --assignee APP_ID --role Contributor
```

> [!NOTE]
> Se l'account non ha le autorizzazioni per assegnare un ruolo, verrà visualizzato un messaggio di errore che segnala che l'account non è autorizzato a eseguire l'azione 'Microsoft.Authorization/roleAssignments/write'. Per gestire i ruoli, contattare l'amministratore di Azure Active Directory.

L'aggiunta di un ruolo _non_ limita le autorizzazioni assegnate in precedenza. Quando si limitano le autorizzazioni di un'entità servizio, il ruolo __Collaboratore__ deve essere rimosso.

È possibile verificare le modifiche visualizzando l'elenco dei ruoli assegnati:

```azurecli-interactive
az role assignment list --assignee APP_ID
```

## <a name="sign-in-using-a-service-principal"></a>Accedere con un'entità servizio

Testare le credenziali e le autorizzazioni della nuova entità servizio eseguendo l'accesso. Per accedere con un'entità servizio, sono necessari `appId`, `tenant` e le credenziali.

Per accedere con un'entità servizio usando una password:

```azurecli-interactive
az login --service-principal --username APP_ID --password PASSWORD --tenant TENANT_ID
```

Per accedere con un certificato, il certificato deve essere disponibile in locale come file PEM o DER in formato ASCII. Quando si usa un file PEM, è necessario aggiungere nel file sia il **CERTIFICATO** che la **CHIAVE PRIVATA**.

```azurecli-interactive
az login --service-principal --username APP_ID --tenant TENANT_ID --password /path/to/cert
```

Per altre informazioni sull'accesso con un'entità servizio, vedere [Accedere tramite l'interfaccia della riga di comando di Azure](authenticate-azure-cli.md).

## <a name="reset-credentials"></a>Reimpostare le credenziali

Se si dimenticano le credenziali per un'entità di servizio, usare [az ad sp credential reset](/cli/azure/ad/sp/credential#az-ad-sp-credential-reset). Il comando reset accetta gli stessi argomenti di `az ad sp create-for-rbac`.

```azurecli-interactive
az ad sp credential reset --name APP_ID
```
