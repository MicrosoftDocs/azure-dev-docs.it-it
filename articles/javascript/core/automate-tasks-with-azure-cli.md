---
title: Attività di automazione delle app Web con l'interfaccia della riga di comando di Azure
description: L'automazione delle attività di Azure è un requisito comune per la distribuzione continua negli ambienti host. L'interfaccia della riga di comando di Azure è la scelta consigliata per gli sviluppatori JavaScript che gestiscono le attività ed eseguono distribuzioni da qualsiasi posizione.
ms.topic: conceptual
ms.date: 12/16/2020
ms.custom: devx-track-js
ms.openlocfilehash: 7cfce90d8d0daf861dab9ba02e46ce489ae10742
ms.sourcegitcommit: 0d2ea78f18430c845a32e0d2311427ab81033465
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/23/2020
ms.locfileid: "97754057"
---
# <a name="automate-tasks-with-azure-cli"></a>Automatizzare le attività con l'interfaccia della riga di comando di Azure

L'automazione delle attività di Azure è un requisito comune per la distribuzione continua negli ambienti host. L'[interfaccia della riga di comando di Azure](/cli/azure/) è la scelta consigliata per gli sviluppatori JavaScript che gestiscono le attività ed eseguono distribuzioni da qualsiasi posizione.

Questo articolo descrive i comandi per attività comuni per sviluppatori JavaScript. 

## <a name="automation-with-azure-cli"></a>Automazione con l'interfaccia della riga di comando di Azure

Per l'automazione, è necessario che l'interfaccia della riga di comando di Azure sia installata nell'ambiente. I metodi comuni sono: 

* [Installazione dell'interfaccia della riga di comando di Azure in locale](/cli/azure/install-azure-cli)
* [Esecuzione di comandi da un contenitore Docker](/cli/azure/run-azure-cli-docker)

## <a name="using-the-example-commands"></a>Uso dei comandi di esempio 

1. Sostituire le variabili tra parentesi uncinate, `<...>`, con i propri valori. 
1. Il valore del repository GitHub per `<MY_GITHUB_DEFAULT_BRANCH_NAME>` è specifico per il repository usato. Attualmente, i valori tipici sono `main` o `default`. I repository meno recenti potrebbero usare `master`. 

## <a name="log-in-for-automated-tasks-with-azure-cli"></a>Accedere per eseguire attività automatizzate con l'interfaccia della riga di comando di Azure

Una volta installata l'interfaccia della riga di comando di Azure, è necessario accedere per continuare a eseguire i relativi comandi. Per l'automazione, è possibile eseguire l'autenticazione con l'interfaccia della riga di comando di Azure.

**Documentazione di riferimento**: [az login](/cli/azure/reference-index?view=azure-cli-latest#az-login)

L'[identità gestita](/cli/azure/authenticate-azure-cli#sign-in-with-a-managed-identity) è la scelta consigliata per l'autenticazione.

```azurecli
az login --identity
```

[Accesso con l'entità servizio di un utente](/cli/azure/authenticate-azure-cli#sign-in-with-a-service-principal) dopo che [è stata creata](../core/node-sdk-azure-authenticate-principal.md#create-a-service-principal-using-the-azure-cli-20). 

```dotnetcli
read -sp "Azure password: " AZ_PASS && echo && \ 
    az login --service-principal \
    -u <MY-SP-APP-URL> \
    -p $AZ_PASS \
    --tenant <MY-TENANT>
```


[Con credenziali utente: nome utente e password](/cli/azure/authenticate-azure-cli#sign-in-with-credentials-on-the-command-line)

```dotnetcli
az login -u <MY_AZURE_USERNAME> -p <MY_AZURE_PASSWORD>
```    

## <a name="create-resource-group-for-resources"></a>Creare un gruppo di risorse per le risorse

Un gruppo di risorse è una raccolta logica delle risorse di Azure. Il raggruppamento logico è basato sui servizi necessari in un'area specifica per un progetto. Vedere informazioni sulle [convenzioni di denominazione](/azure/cloud-adoption-framework/ready/azure-best-practices/resource-naming).

**Documentazione di riferimento**: [az group create](/cli/azure/group?view=azure-cli-latest#az_group_create)

```azurecli
az group create \
    --name <MY-AZURE-RESOURCE_GROUP_NAME> \
    --location <AZURE_REGION_LOCATION>
```

## <a name="static-web-apps-with-azure-cli"></a>App Web statiche con l'interfaccia della riga di comando di Azure

Un'app Web statica contiene codice per:

* Un'applicazione front-end contenuta in un repository GitHub
* Facoltativamente, un'API di Funzioni di Azure esistente nella directory `/API`. [Altre informazioni](/azure/static-web-apps/add-api#create-the-api)

L'app può usare funzioni di Azure per le API serverless, ma questo non è un requisito per le app Web statiche. 

**Documentazione di riferimento**: [az staticwebapp](/cli/azure/staticwebapp?view=azure-cli-latest)

### <a name="create-azure-static-web-app"></a>Creare l'app Web statica di Azure 

**Documentazione di riferimento**: [az staticwebapp create](/cli/azure/staticwebapp?view=azure-cli-latest#az_staticwebapp_create)

```azurecli
az staticwebapp create \
    --name <MY_AZURE_WEB_APP_NAME> \
    --resource-group <MY-AZURE-RESOURCE_GROUP_NAME> \
    --source https://github.com/<MY_GITHUB_ACCOUNT_NAME>/<MY_AZURE_WEB_APP_NAME> \
    --location <AZURE_REGION_LOCATION> \
    --branch <MY_GITHUB_DEFAULT_BRANCH_NAME> \
    --app-artifact-location "<MY_WEB_APP_BUILD_DIRECTORY_NAME>" \
    --token <MY_GITHUB_PERSONAL_ACCESS_TOKEN>
```

### <a name="deploy-azure-static-web-app"></a>Distribuire un'app Web statica di Azure 

Per distribuire l'app, eseguire il push al repository remoto o al ramo impostati durante la creazione della risorsa in precedenza con Git. 

```bash
git push <REMOTE_NAME> <MY_GITHUB_DEFAULT_BRANCH_NAME>
```

Un esempio di questo comando è:

```bash
git push origin main
```

### <a name="delete-static-web-app"></a>Eliminare un'app Web statica 

**Documentazione di riferimento**: [az staticwebapp delete](/cli/azure/staticwebapp?view=azure-cli-latest#az_staticwebapp_delete)

```azurecli
az staticwebapp delete && \
    --name <MY_AZURE_WEB_APP_NAME> && \
    --resource-group <MY-AZURE-RESOURCE_GROUP_NAME>
```

## <a name="next-steps"></a>Passaggi successivi

* [Esercitazione: Compilare e distribuire un'app Web statica in Azure](../tutorial/static-web-app/introduction.md)
