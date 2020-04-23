---
author: mriem
ms.author: manriem
ms.date: 2/28/2020
ms.openlocfilehash: 59e277b0ae4a05566e2dbcd0d7a0a4a15c22eed9
ms.sourcegitcommit: 0af39ee9ff27c37ceeeb28ea9d51e32995989591
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/21/2020
ms.locfileid: "81672987"
---
### <a name="build-and-push-the-docker-image-to-azure-container-registry"></a>Creare l'immagine Docker ed eseguirne il push in Registro Azure Container

Dopo aver creato il Dockerfile, è necessario creare l'immagine Docker e pubblicarla nel registro contenitori di Azure.

Se è stato [usato il repository GitHub di avvio rapido dei contenitori WildFly](https://github.com/Azure/wildfly-container-quickstart), il processo di creazione dell'immagine e push nel registro contenitori di Azure equivale a richiamare i tre comandi seguenti.

In questi esempi la variabile di ambiente `MY_ACR` include il nome del registro contenitori di Azure e la variabile `MY_APP_NAME` il nome dell'applicazione Web che si vuole usare al suo interno.

Compilare il file WAR:

```shell
mvn package
```

Accedere al registro contenitori di Azure:

```shell
az acr login -n ${MY_ACR}
```

Creare l'immagine ed eseguirne il push:

```shell
az acr build -t ${MY_ACR}.azurecr.io/${MY_APP_NAME} -f src/main/docker/Dockerfile .
```

In alternativa, è possibile usare l'interfaccia della riga di comando di Docker per creare e testare l'immagine in locale, come illustrato nei comandi seguenti. Questo approccio può semplificare il test e il perfezionamento dell'immagine prima della distribuzione iniziale in Registro Azure Container. Tuttavia, è necessario installare l'interfaccia della riga di comando di Docker e assicurarsi che il daemon Docker sia in esecuzione.

Creare l'immagine:

```shell
docker build -t ${MY_ACR}.azurecr.io/${MY_APP_NAME}
```

Eseguire l'immagine in locale:

```shell
docker run -it -p 8080:8080 ${MY_ACR}.azurecr.io/${MY_APP_NAME}
```

È ora possibile accedere all'applicazione all'indirizzo [http://localhost:8080](http://localhost:8080).

Accedere al registro contenitori di Azure:

```shell
az acr login -n ${MY_ACR}
```

Eseguire il push delle immagini nel registro contenitori di Azure:

```shell
docker push ${MY_ACR}.azurecr.io/${MY_APP_NAME}
```

Per informazioni più dettagliate sulla creazione e l'archiviazione di immagini di contenitore in Azure, vedere il modulo Learn [Compilare e archiviare immagini del contenitore con Registro Azure Container](/learn/modules/build-and-store-container-images/).
