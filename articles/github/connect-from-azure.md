---
title: Connettersi a GitHub e Azure
description: Risorse per connettersi a GitHub da Azure e da altri servizi
author: N-Usha
ms.author: ushan
ms.topic: reference
ms.service: azure
ms.date: 08/31/2020
ms.openlocfilehash: 4f6a7d09c0adba2fc55e94ab652c5228a742e67d
ms.sourcegitcommit: 5ab6e90e20a87f9a8baea652befc74158a9b6613
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/09/2020
ms.locfileid: "89614268"
---
# <a name="use-github-actions-to-connect-to-azure"></a>Usare GitHub Actions per connettersi ad Azure

Informazioni su come usare un [account di accesso di Azure](https://github.com/Azure/login) con [Azure PowerShell](https://github.com/Azure/PowerShell) o l'[interfaccia della riga di comando di Azure](https://github.com/Azure/CLI) per interagire con le risorse di Azure.

Per usare Azure PowerShell o l'interfaccia della riga di comando di Azure, è prima di tutto necessario accedere con l'[account di accesso di Azure](https://github.com/marketplace/actions/azure-login). L'azione di accesso di Azure connette la sottoscrizione di Azure a GitHub usando un'entità servizio.

Dopo aver configurato l'azione di accesso, è possibile usare l'interfaccia della riga di comando di Azure o Azure PowerShell.  
L'interfaccia della riga di comando di Azure configura l'ambiente dello strumento di esecuzione dell'azione GitHub per l'interfaccia della riga di comando di Azure. Azure PowerShell configura l'ambiente dello strumento di esecuzione dell'azione GitHub con il modulo di Azure PowerShell.


## <a name="create-a-service-principal-and-add-it-to-github-secret"></a>Creare un'entità servizio e aggiungerla al segreto GitHub

Per usare l'[account di accesso di Azure](https://github.com/marketplace/actions/azure-login), è prima necessario aggiungere l'entità servizio di Azure come segreto al repository GitHub.

In questo esempio verrà creato un segreto denominato `AZURE_CREDENTIALS` che è possibile usare per l'autenticazione con Azure.  

1. Se non si dispone di un'applicazione esistente, registrare una [nuova applicazione di Active Directory](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#register-an-application-with-azure-ad-and-create-a-service-principal&preserve-view=true) da usare con l'entità servizio.

    ```azurecli-interactive
        appName="myApp"

        az ad app create \
        --display-name $appName \
        --homepage "http://localhost/$appName" \
        --identifier-uris http://localhost/$appName
    ```

1. [Creare una nuova entità servizio](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli?view=azure-cli-latest) nel portale di Azure per l'app. 

    ```azurecli-interactive
        az ad sp create-for-rbac --name "myApp" --role contributor \
                                    --scopes /subscriptions/{subscription-id}/resourceGroups/{resource-group} \
                                    --sdk-auth
    ```

1. Copiare l'oggetto JSON per l'entità servizio.

    ```json
    {
        "clientId": "<GUID>",
        "clientSecret": "<GUID>",
        "subscriptionId": "<GUID>",
        "tenantId": "<GUID>",
        (...)
    }
    ```

1. Aprire il repository GitHub e passare a **Impostazioni**.

    :::image type="content" source="media/github-repo-settings.png" alt-text="Selezionare Impostazioni nel riquadro di spostamento":::

1. Selezionare **Segreti** e quindi **Nuovo segreto**.

    :::image type="content" source="media/select-secrets.png" alt-text="Scegliere di aggiungere un segreto":::

1. Incollare l'oggetto JSON per l'entità servizio con il nome `AZURE_CREDENTIALS`. 

    :::image type="content" source="media/azure-secret-add.png" alt-text="Aggiungere un segreto in GitHub":::

1. Salvare selezionando **Aggiungi segreto**.

## <a name="use-the-azure-login-action"></a>Usare l'azione di accesso di Azure

Usare il segreto dell'entità servizio con l'[azione di accesso di Azure](https://github.com/Azure/login) per l'autenticazione con Azure.

In questo flusso di lavoro è possibile eseguire l'autenticazione con `secrets.AZURE_CREDENTIALS` e quindi eseguire un'azione dell'interfaccia della riga di comando di Azure.

Quando si ha un account di accesso di Azure funzionante, è possibile usare le azioni di Azure PowerShell o dell'interfaccia della riga di comando di Azure. È anche possibile usare altre azioni di Azure come [distribuzione app Web di Azure](https://github.com/Azure/webapps-deploy) e [funzioni di Azure](https://github.com/Azure/functions-action).

```yaml
on: [push]

name: AzureLoginSample

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Log in with Azure
        uses: azure/login@v1
        with:
          creds: '${{ secrets.AZURE_CREDENTIALS }}'
```

## <a name="use-the-azure-powershell-action"></a>Usare l'azione di Azure PowerShell

In questo esempio si accede con l'[azione di accesso di Azure](https://github.com/Azure/login) e quindi si recupera un gruppo di risorse con l'[azione dell'interfaccia della riga di comando di Azure](https://github.com/azure/powershell).

```yaml
on: [push]

name: AzureLoginSample

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Log in with Azure
        uses: azure/login@v1
        with:
          creds: '${{ secrets.AZURE_CREDENTIALS }}'
          enable-AzPSSession: true
      - name: Azure PowerShell Action
        uses: Azure/powershell@v1
        with:
          inlineScript: Get-AzVM -ResourceGroupName "< YOUR RESOURCE GROUP >"
          azPSVersion: 3.1.0
```

## <a name="use-the-azure-cli-action"></a>Usare l'azione dell'interfaccia della riga di comando di Azure

In questo esempio si accede con l'[azione di accesso di Azure](https://github.com/Azure/login) e quindi si recupera un gruppo di risorse con l'[azione dell'interfaccia della riga di comando di Azure](https://github.com/Azure/CLI).


```yaml
on: [push]

name: AzureLoginSample

jobs:
build-and-deploy:
    runs-on: ubuntu-latest
    steps:

    - name: Log in with Azure
        uses: azure/login@v1
        with:
        creds: ${{ secrets.AZURE_CREDENTIALS }}

    - name: Azure CLI script
        uses: azure/CLI@v1
        with:
        azcliversion: 2.0.72
        inlineScript: |
            az account show
            az storage -h
```

## <a name="connect-with-other-azure-services"></a>Connettersi con altri servizi di Azure

Gli articoli seguenti forniscono informazioni dettagliate sulla connessione a GitHub da Azure e da altri servizi.  

### <a name="azure-active-directory"></a>Azure Active Directory 

- [Accedere a GitHub Enterprise con Azure AD (Single Sign-On)](https://docs.microsoft.com/azure/active-directory/saas-apps/github-tutorial)   

### <a name="power-bi"></a>Power BI

- [Connettere Power BI a GitHub](https://docs.microsoft.com/power-bi/service-connect-to-github)   

### <a name="connectors"></a>Connettori

- [Connettore GitHub per App per la logica di Azure, Power Automate e Power Apps](https://docs.microsoft.com/connectors/github/)   

### <a name="azure-databricks"></a>Azure Databricks

- [Usare GitHub come controllo della versione per i notebook](https://docs.microsoft.com/azure/databricks/notebooks/github-version-control) 

> [!div class="nextstepaction"]
> [Distribuire app da GitHub ad Azure](deploy-to-azure.md)
