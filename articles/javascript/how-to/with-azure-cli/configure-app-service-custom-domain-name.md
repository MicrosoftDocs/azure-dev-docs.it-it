---
title: Aggiungere un nome di dominio personalizzato all'app Web
description: Aggiungere il nome di dominio personalizzato all'app Web di Azure usando l'interfaccia della riga di comando di Azure.
ms.topic: how-to
ms.date: 01/29/2021
ms.custom: devx-track-js, devx-track-azurecli
ms.openlocfilehash: 977376fda6e7c93390c45196ae5baae751f5ad64
ms.sourcegitcommit: 3f8aa923e4626b31cc533584fe3b66940d384351
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/01/2021
ms.locfileid: "99231551"
---
# <a name="configuring-a-custom-domain-name"></a>Configurazione di un nome di dominio personalizzato

Aggiungere il nome di dominio personalizzato all'app Web di Azure usando l'interfaccia della riga di comando di Azure. 

Il servizio app ha un nome DNS pratico, ideale per i test, sotto forma di `YOUR-RESOURCE-NAME.azurewebsites.net` . A un certo punto potrebbe essere necessario aggiungere un nome di dominio personalizzato all'app Web. 

## <a name="purchase-a-domain-name-and-configure-dns-record"></a>Acquistare un nome di dominio e configurare un record DNS

1. Acquistare un nome di dominio da un registrar. 
1. Per il record DNS aggiungere un `A` record al record DNS che punti all'indirizzo IP esterno dell'app Web, che in realtà è un servizio di bilanciamento del carico. Usare la procedura descritta nella sezione successiva per ottenere l'indirizzo IP esterno.

    Oltre ad aggiungere un record `A` è necessario aggiungere al dominio un record `TXT` che punti al dominio `*.azurewebsites.net` usato finora. La combinazione dei record `A` e `TXT` consente ad Azure di verificare che si è proprietari del dominio.

## <a name="get-web-app-external-ip"></a>Ottenere l'indirizzo IP esterno dell'app Web

Questo indirizzo IP può essere recuperato con il comando seguente:

```azurecli
az webapp config hostname get-external-ip --name
```

<a name="register-a-domain-name-with-your-azure-app"></a>

## <a name="configure-web-app-domain-name"></a>Configurare il nome di dominio dell'app Web 

Dopo aver creato i record e aver propagato le modifiche DNS, registrare il dominio personalizzato con Azure per indicare l'origine corretta del traffico in ingresso.

Usare il comando [AZ webapp config hostname Add](/cli/azure/webapp/config/hostname) :

```azurecli
az webapp config hostname add \
    --hostname YOUR-DOMAIN-NAME
    --webapp-name YOUR-WEBAPP-NAME
    --resource-group YOUR-RESOURCE-GROUP-NAME
```

> [!NOTE]
> Il comando non funziona fino a quando le modifiche DNS non vengono propagate.

Aprire un browser e passare al dominio personalizzato per verificare che ora si risolva nell'app distribuita in Azure.

## <a name="next-steps"></a>Passaggi successivi

* [Creare una risorsa del registro contenitori](create-container-registry-resource.md)