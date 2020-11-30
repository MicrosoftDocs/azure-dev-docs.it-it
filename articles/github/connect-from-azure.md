---
title: Connettersi a GitHub e Azure
description: Risorse per connettersi a GitHub da Azure e da altri servizi
author: N-Usha
ms.author: ushan
ms.topic: reference
ms.service: azure
ms.date: 11/17/2020
ms.custom: github-actions-azure, devx-track-azurecli
ms.openlocfilehash: 5462d7ca3618869232296a9a6739ebe5adcefdb1
ms.sourcegitcommit: 4dac39849ba2e48034ecc91ef578d11aab796e58
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/20/2020
ms.locfileid: "94983630"
---
# <a name="use-github-actions-to-connect-to-azure"></a>Usare GitHub Actions per connettersi ad Azure

Informazioni su come usare un [account di accesso di Azure](https://github.com/Azure/login) con [Azure PowerShell](https://github.com/Azure/PowerShell) o l'[interfaccia della riga di comando di Azure](https://github.com/Azure/CLI) per interagire con le risorse di Azure.

Per usare Azure PowerShell o l'interfaccia della riga di comando di Azure in un flusso di lavoro di GitHub Actions, è necessario prima accedere con l'azione di [accesso di Azure](https://github.com/marketplace/actions/azure-login).
L'azione di accesso di Azure consente di eseguire comandi in un flusso di lavoro nel contesto di un'[entità servizio di Azure AD](/azure/active-directory/develop/app-objects-and-service-principals#service-principal-object).

Dopo aver configurato l'azione di accesso, è possibile usare l'interfaccia della riga di comando di Azure o Azure PowerShell.

Per impostazione predefinita, l'azione effettua l'accesso con l'interfaccia della riga di comando di Azure, configurando l'ambiente dello strumento di esecuzione di GitHub Actions per l'interfaccia della riga di comando di Azure. È possibile usare Azure PowerShell con la proprietà `enable-AzPSSession` dell'azione di accesso di Azure. In questo modo l'ambiente dello strumento di esecuzione di GitHub Action viene configurato con il modulo di Azure PowerShell.

## <a name="create-a-service-principal-and-add-it-to-github-secret"></a>Creare un'entità servizio e aggiungerla al segreto GitHub

Per usare l'[account di accesso di Azure](https://github.com/marketplace/actions/azure-login), è prima necessario aggiungere l'entità servizio di Azure come segreto al repository GitHub.

In questo esempio verrà creato un segreto denominato `AZURE_CREDENTIALS` che è possibile usare per l'autenticazione con Azure.  

1. Se non si dispone di un'applicazione esistente, registrare una [nuova applicazione di Active Directory](/azure/active-directory/develop/howto-create-service-principal-portal#register-an-application-with-azure-ad-and-create-a-service-principal&preserve-view=true) da usare con l'entità servizio.

    ```azurecli-interactive
        appName="myApp"

        az ad app create \
        --display-name $appName \
        --homepage "http://localhost/$appName" \
        --identifier-uris http://localhost/$appName
    ```

1. [Creare una nuova entità servizio](/cli/azure/create-an-azure-service-principal-azure-cli?view=azure-cli-latest&preserve-view=true) nel portale di Azure per l'app. 

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

Usare il segreto dell'entità servizio con l'[azione di accesso di Azure](https://github.com/Azure/login) per eseguire l'autenticazione con Azure.

In questo flusso di lavoro l'autenticazione viene eseguita usando l'azione di accesso di Azure con i dettagli dell'entità servizio archiviati in `secrets.AZURE_CREDENTIALS`. Quindi, eseguire un'azione dell'interfaccia della riga di comando di Azure. Per altre informazioni su come fare riferimento ai segreti di GitHub in un file del flusso di lavoro, vedere [Uso di segreti crittografati in un flusso di lavoro](https://docs.github.com/en/free-pro-team@latest/actions/reference/encrypted-secrets#using-encrypted-secrets-in-a-workflow) nella documentazione di GitHub.

Una volta configurato un passaggio di accesso di Azure funzionante, è possibile usare le azioni di [Azure PowerShell](https://github.com/Azure/PowerShell) o dell'[interfaccia della riga di comando di Azure](https://github.com/Azure/CLI). È anche possibile usare altre azioni di Azure, ad esempio la [distribuzione in app Web di Azure](https://github.com/Azure/webapps-deploy) e [Funzioni di Azure](https://github.com/Azure/functions-action).

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

In questo esempio si accede con l'[azione di accesso di Azure](https://github.com/Azure/login) e quindi si recupera un gruppo di risorse con l'[azione di Azure PowerShell](https://github.com/azure/powershell).

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

- [Accedere a GitHub Enterprise con Azure AD (Single Sign-On)](/azure/active-directory/saas-apps/github-tutorial)   

### <a name="power-bi"></a>Power BI

- [Connettere Power BI a GitHub](/power-bi/service-connect-to-github)   

### <a name="connectors"></a>Connettori

- [Connettore GitHub per App per la logica di Azure, Power Automate e Power Apps](/connectors/github/)   

### <a name="azure-databricks"></a>Azure Databricks

- [Usare GitHub come controllo della versione per i notebook](/azure/databricks/notebooks/github-version-control) 

> [!div class="nextstepaction"]
> [Distribuire app da GitHub ad Azure](deploy-to-azure.md)
