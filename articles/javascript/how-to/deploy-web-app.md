---
title: Distribuire le applicazioni JavaScript in Azure
description: Le opzioni di hosting e gli scenari di distribuzione includono diversi servizi e strumenti per Azure. Pubblicare l'app e gestirla su Azure.
ms.topic: how-to
ms.date: 12/09/2020
ms.custom: seo-javascript-september2019, seo-javascript-october2019, devx-track-js, contperf-fy21q2
ms.openlocfilehash: e2020d90260af4fbab8d6a37ef475eddb7754b90
ms.sourcegitcommit: 1dfcc022a3098b1a1505e9458eada35f527ef070
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97636526"
---
# <a name="deploy-and-host-your-nodejs-apps-on-azure"></a>Distribuire e ospitare le app Node.js in Azure

Le opzioni di hosting e gli scenari di distribuzione includono diversi servizi e strumenti per Azure. Azure offre molte opzioni per l'hosting e molti strumenti che consentono di spostare l'app da un repository locale o cloud in Azure. 

## <a name="choose-a-recommended-azure-host-provider"></a>Scegliere un provider di hosting di Azure consigliato

Se si ospita il client, il server o l'app per le attività in background in Azure è possibile scegliere in un'ampia gamma di soluzioni. Usare la tabella seguente per effettuare una selezione. La soluzione consigliata per la maggior parte dei casi d'uso è [Servizio app di Azure](/azure/app-service/overview). 

Per una panoramica completa delle diverse opzioni di hosting, vedere [Albero delle decisioni per i servizi di calcolo di Azure](/azure/architecture/guide/technology-choices/compute-decision-tree) e il modulo [Servizi cloud di base - Opzioni di calcolo di Azure](/learn/modules/intro-to-azure-compute) in Microsoft Learn.


 Servizio |Tipo di app supportato| Suggerito per |
|--|--|--|
|[*Servizio app](/azure/app-service/overview) - **consigliato**|Client, Server, Client/Server, API, Rendering su server|Ospitare l'app dal codice o da un contenitore. Ciò consente di gestire il server Web senza la necessità di gestire l'ambiente sottostante.|
|[(Anteprima) App Web statiche](/azure/static-web-apps/)|Front-end statico, pre-rendering, front-end statico con API server|Ospitare l'app client statica, ad esempio Angular, Vue, React. Facoltativamente, aggiungere endpoint di funzione serverless per ospitare un'app di stack completo. Questo semplice servizio rimuove gran parte del server Web, consentendo di concentrarsi sulle funzionalità più importanti per un'applicazione client. |
|[Funzioni](/azure/azure-functions/)|API server|Ospitare gli endpoint dell'API serverless. Azure offre molti modelli noti come trigger per il bootstrap di scenari comuni.|

## <a name="host-web-apps-with-more-control"></a>Ospitare app Web con maggiore controllo

Le opzioni seguenti offrono maggiore controllo sull'ambiente dell'applicazione. 

| Servizio | Suggerito per |
|--|--|
|[Macchine virtuali](/azure/virtual-machines) (VM)|Controllo completo di una VM Windows o Linux. [Trovare una distribuzione Linux approvata](/azure/virtual-machines/linux/endorsed-distros?toc=/azure/virtual-machines/linux/toc.json) o [informazioni su come trovare](/azure/virtual-machines/linux/cli-ps-findimage) immagini di macchine virtuali in Azure Marketplace.|
|[Istanze di Container](/azure/container-instances/)|Configurare rapidamente un singolo contenitore.|
|[Servizio Kubernetes](/azure/aks/)|Orchestrazioni di più contenitori.|

## <a name="alternative-choices-for-web-app-hosting-on-azure"></a>Opzioni alternative per l'hosting di app Web in Azure

Queste opzioni sono personalizzate per casi d'uso specifici. 

| Servizio | Suggerito per |
|--|--|
|[Archiviazione](/azure/storage/blobs/storage-blob-static-website-how-to?tabs=azure-portal)|Archiviazione di Azure può anche ospitare un'app Web statica. Questa operazione è utile se è necessaria una stretta integrazione tra l'archiviazione affidabile e l'applicazione client.|
|[Rete per la distribuzione di contenuti ](/azure/cdn/) (Rete CDN)|Distribuire siti Web con pre-rendering. È possibile memorizzare nella cache gli oggetti statici caricati dall'archivio BLOB di Azure, da un'applicazione Web o da qualsiasi server Web accessibile pubblicamente, tramite il server POP (Point of Presence) più vicino. La rete CDN di Azure consente anche di accelerare il contenuto dinamico, non memorizzabile nella cache, sfruttando varie ottimizzazioni del routing e della rete.|

## <a name="deploy-your-web-app-to-azure"></a>Distribuire l'app Web in Azure

Dopo aver selezionato un servizio per ospitare l'applicazione, selezionare un processo di distribuzione e uno strumento. La distribuzione delle app client e server nei servizi di Azure significa spostare un file o un set di file in Azure per gestirli tramite un endpoint HTTP. 

Sono disponibili molti metodi comuni di spostamento di file sul cloud di Azure, tra cui:

| Metodo | Dettagli |
|--|--|
|[GitHub Actions](/azure/app-service/deploy-github-actions?tabs=applevel)|Usarlo per le distribuzioni continue automatizzate o attivate.|
|[Estensioni per Visual Studio Code](https://marketplace.visualstudio.com/search?term=azure&target=VSCode&category=All%20categories&sortBy=Relevance)|Usarlo per le distribuzioni manuali, di test o rare. Richiede che l'estensione per il servizio sia installata localmente.|
|[Interfaccia della riga di comando di Azure](../tutorial/tutorial-vscode-azure-cli-node/tutorial-vscode-azure-cli-node-04.md)|Usarlo per le distribuzioni manuali o rare. Richiede che l'estensione per il servizio sia installata localmente.|

Potrebbero esistere altri metodi di distribuzione, in base al servizio specifico. Il servizio app di Azure, ad esempio, supporta un'ampia gamma di metodi di distribuzione:
* [Da file ZIP](/azure/app-service/deploy-zip)
* [Con FTP](/azure/app-service/deploy-ftp)
* [Dropbox o OneDrive](/azure/app-service/deploy-content-sync)
* [GIT locale](/azure/app-service/deploy-local-git)
* [cURL](/azure/app-service/deploy-zip#with-curl)
* [SSH](/azure/app-service/configure-linux-open-ssh-session)

## <a name="verify-your-deployment-with-your-http-endpoint"></a>Verificare la distribuzione con l'endpoint HTTP

Per verificare la distribuzione, accedere all'endpoint HTTP. L'endpoint HTTP è visibile in tutti i servizi nella pagina **Panoramica**. 

### <a name="view-http-endpoint-in-azure-portal"></a>Visualizzare l'endpoint HTTP nel portale di Azure

Visualizzare l'endpoint HTTP dalla pagina Panoramica del servizio nel portale di Azure. 

:::image type="content" source="../media/howto-deploy/azure-portal-hosting-url.png" alt-text="Visualizzare l'endpoint HTTP dalla pagina Panoramica del servizio nel portale di Azure.":::

## <a name="next-steps"></a>Passaggi successivi

* [Distribuire con i contenitori](deploy-containers.md)
