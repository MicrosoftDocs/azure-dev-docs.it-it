---
title: Selezionare i cloud con l'interfaccia della riga di comando di Azure
description: Creare, accedere e gestire più cloud con l'interfaccia della riga di comando di Azure.
author: dbradish-microsoft
manager: barbkess
ms.author: dbradish
ms.date: 09/09/2018
ms.topic: conceptual
ms.service: azure-cli
ms.devlang: azurecli
ms.openlocfilehash: 8e24a4740d97ddf67f81e60fef9217a4e72daab0
ms.sourcegitcommit: 36e02e96b955ed0531f98b9c0f623f4acb508661
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/22/2020
ms.locfileid: "82031223"
---
# <a name="select-clouds-with-the-azure-cli"></a>Selezionare i cloud con l'interfaccia della riga di comando di Azure

Se si lavora in diverse aree o si usa [Azure Stack](https://docs.microsoft.com/azure/azure-stack/user/), potrebbe essere necessario usare più cloud. Microsoft offre cloud disponibili per l'uso in conformità alle leggi dell'area specifica. Questo articolo illustra come ottenere informazioni sui cloud, cambiare il cloud attivo ed eseguire o annullare la registrazione di nuovi cloud.

## <a name="list-available-clouds"></a>Elencare i cloud disponibili

È possibile ottenere un elenco dei cloud disponibili con il comando [az cloud list](/cli/azure/cloud#az-cloud-list). Questo comando mostra il cloud attualmente attivo, il relativo profilo corrente e informazioni sui suffissi e sui nomi host specifici per l'area.

Per recuperare il cloud attivo e un elenco di tutti i cloud disponibili:

```azurecli-interactive
az cloud list --output table
```

```output
IsActive    Name               Profile
----------  -----------------  ---------
True        AzureCloud         latest
            AzureChinaCloud    latest
            AzureUSGovernment  latest
            AzureGermanCloud   latest
```

Il cloud attualmente attivo è indicato da `True` nella colonna `IsActive`. Può essere attivo un solo cloud alla volta. Per ottenere informazioni più dettagliate su un cloud, tra cui gli endpoint usati per i servizi di Azure, usare il comando `cloud show`:

```azurecli-interactive
az cloud show --name AzureChinaCloud --output json
```

```json
{
  "endpoints": {
    "activeDirectory": "https://login.chinacloudapi.cn",
    "activeDirectoryDataLakeResourceId": null,
    "activeDirectoryGraphResourceId": "https://graph.chinacloudapi.cn/",
    "activeDirectoryResourceId": "https://management.core.chinacloudapi.cn/",
    "batchResourceId": "https://batch.chinacloudapi.cn/",
    "gallery": "https://gallery.chinacloudapi.cn/",
    "management": "https://management.core.chinacloudapi.cn/",
    "resourceManager": "https://management.chinacloudapi.cn",
    "sqlManagement": "https://management.core.chinacloudapi.cn:8443/",
    "vmImageAliasDoc": "https://raw.githubusercontent.com/Azure/azure-rest-api-specs/master/arm-compute/quickstart-templates/aliases.json"
  },
  "isActive": false,
  "name": "AzureChinaCloud",
  "profile": "latest",
  "suffixes": {
    "azureDatalakeAnalyticsCatalogAndJobEndpoint": null,
    "azureDatalakeStoreFileSystemEndpoint": null,
    "keyvaultDns": ".vault.azure.cn",
    "sqlServerHostname": ".database.chinacloudapi.cn",
    "storageEndpoint": "core.chinacloudapi.cn"
  }
}
```

## <a name="switch-the-active-cloud"></a>Cambiare il cloud attivo

Per impostare il cloud predefinito usando un file di configurazione, vedere [i valori di configurazione e le variabili di ambiente dell'interfaccia della riga di comando](/cli/azure/azure-cli-configuration?view=azure-cli-latest#cli-configuration-values-and-environment-variables).  Per cambiare il cloud attivo, eseguire il comando [az cloud set](/cli/azure/cloud#az-cloud-set). Questo comando accetta un argomento obbligatorio, ovvero il nome del cloud.

```azurecli-interactive
az cloud set --name AzureChinaCloud
```

> [!IMPORTANT]
> Se l'autenticazione per il cloud attivato è scaduta, è necessario ripetere l'autenticazione prima di eseguire qualsiasi altra attività dell'interfaccia della riga di comando. Se si passa al nuovo cloud per la prima volta, è anche necessario impostare la sottoscrizione attiva.
> Per istruzioni relative all'autenticazione, vedere [Accedere con l'interfaccia della riga di comando di Azure](authenticate-azure-cli.md). Per informazioni sulla gestione delle sottoscrizioni, vedere [Gestire le sottoscrizioni di Azure con l'interfaccia della riga di comando di Azure](manage-azure-subscriptions-azure-cli.md).

## <a name="register-a-new-cloud"></a>Registrare un nuovo cloud

Registrare un nuovo cloud se sono disponibili endpoint personalizzati per Azure Stack. Per creare un cloud, usare il comando [az cloud register](/cli/azure/cloud#az-cloud-register). Questo comando richiede un nome e un set di endpoint di servizio. Per informazioni su come registrare un cloud da usare con Azure Stack, vedere [Usare i profili delle versioni dell'API con l'interfaccia della riga di comando di Azure in Azure Stack](/azure/azure-stack/user/azure-stack-version-profiles-azurecli2#connect-to-azure-stack).

Non è necessario registrare informazioni per le aree Cina, US Government o Germania, perché questi cloud sono gestiti da Microsoft e disponibili per impostazione predefinita.  Per altre informazioni su tutte le impostazioni per gli endpoint disponibili, vedere la [documentazione per `az cloud register`](/cli/azure/cloud#az-cloud-register).

La registrazione di un cloud non comporta automaticamente il passaggio a tale cloud. Usare il comando `az cloud set` per selezionare il cloud appena creato.

## <a name="update-an-existing-cloud"></a>Aggiornare un cloud esistente

Se si hanno le autorizzazioni necessarie, è anche possibile aggiornare un cloud esistente. Con l'aggiornamento di un cloud si passa a un diverso profilo di servizi di Azure oppure si modificano gli endpoint di connessione.
Aggiornare un cloud con il comando [az cloud update](/cli/azure/cloud#az-cloud-update), che accetta gli stessi argomenti di `az cloud register`.

## <a name="unregister-a-cloud"></a>Annullare la registrazione di un cloud

Se il cloud creato non è più necessario, è possibile annullarne la registrazione con il comando [az cloud unregister](/cli/azure/cloud#az-cloud-unregister):

```azurecli-interactive
az cloud unregister --name MyCloud
```
