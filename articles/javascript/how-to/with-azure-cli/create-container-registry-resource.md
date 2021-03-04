---
title: Crea registro contenitori personalizzato
description: Un registro contenitori è ideale per le immagini del contenitore che si vuole distribuire in Azure. Il registro di sistema consente di gestire i repository del contenitore e le versioni.
ms.topic: how-to
ms.date: 01/28/2021
ms.custom: devx-track-js, devx-track-azurecli
ms.openlocfilehash: c4f02f04203b467cf166e07be16d5884c9a4e8d4
ms.sourcegitcommit: 576c878c338d286060010646b96f3ad0fdbcb814
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/04/2021
ms.locfileid: "102118533"
---
# <a name="create-and-use-container-registry"></a>Creare e usare il registro contenitori

Un registro contenitori è ideale per le immagini del contenitore che si vuole distribuire in Azure.

Il registro di sistema consente di gestire i repository del contenitore e le versioni.  

## <a name="create-a-container-registry"></a>Creare un registro contenitori

Creare un registro con un nome di risorsa. Il nome della risorsa diventerà parte del nome del server di accesso per la risorsa. 

Usare il comando [AZ ACR create](/cli/azure/acr#az_acr_create) per creare un registro di sistema. 

```azurecli
az acr create \
    --resource-group YOUR-RESOURCE-GROUP
    --name YOUR-REGISTRY-NAME 
    --location westus 
    --admin-enabled
    --sku Basic
    --public-network-enabled false
```

## <a name="get-container-registry-credentials"></a>Ottenere le credenziali del registro contenitori

Per recuperare le credenziali, eseguire il comando [AZ ACR Credential Show](/cli/azure/acr/credential#az_acr_credential_show) e prendere nota del nome utente e della password visualizzati:

```azurecli
az acr credential show \
    --resource-group YOUR-RESOURCE-GROUP \
    --name YOUR-REGISTRY-NAME
```

## <a name="login-to-container-registry-with-docker-cli"></a>Accedere al registro contenitori con l'interfaccia della riga di comando di Docker

Usando le credenziali del passaggio precedente e il proprio server di accesso è possibile accedere al registro tramite il flusso di lavoro standard dell'interfaccia della riga di comando di Docker.

```bash
docker login YOUR-LOGIN_SERVER \
    --username USERNAME
    --password PASSWORD
```

## <a name="tag-your-local-image"></a>Contrassegnare l'immagine locale

È ora possibile contrassegnare il contenitore Docker per indicare che è associato al registro privato usando il comando seguente, in cui è necessario sostituire `YOURALIAS/IMAGENAME` con il nome assegnato all'immagine del contenitore.

```bash
docker tag YOURALIAS/IMAGENAME \
    YOUR-LOGIN_SERVER/YOURALIAS/IMAGENAME:v1
```

## <a name="push-your-local-image-to-your-container-registry"></a>Eseguire il push dell'immagine locale nel registro contenitori

```bash
docker push YOUR-LOGIN_SERVER/YOURALIAS/IMAGENAME:v1
```

## <a name="configure-web-app-to-use-container"></a>Configurare l'app Web per l'uso del contenitore 

In configurare l'app Web del servizio app per estrarre l'immagine dal registro di sistema, eseguire il comando [AZ Appservice Web config container set](/cli/azure/webapp/config/container#az_webapp_config_container_set) seguente:

```azurecli
az appservice web config container set \
    --resource-group YOUR-RESOURCE-GROUP \
    --name YOUR-WEBAPP-NAME
    --docker-registry-server-url YOUR-LOGIN_SERVER \
    --docker-custom-image-name YOUR-LOGIN_SERVER/YOURALIAS/IMAGENAME:v1 \
    -u USERNAME \
    -p PASSWORD
```

Aggiungere il prefisso `https://` all'inizio dell'opzione `--docker-registry-server-url`. Non aggiungere tuttavia il prefisso al nome dell'immagine del contenitore.

## <a name="next-steps"></a>Passaggi successivi

* [Creare una risorsa Cosmos DB MongoDB](create-mongodb-cosmosdb.md)