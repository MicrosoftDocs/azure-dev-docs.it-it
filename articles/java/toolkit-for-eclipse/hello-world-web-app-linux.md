---
title: Distribuire un'app Hello World in un contenitore cloud Linux
titleSuffix: Azure Toolkit for Eclipse
description: Eseguire un'app Web Hello World di base in un contenitore Linux e distribuirla nel cloud con Azure Toolkit for Eclipse.
services: app-service\web
documentationcenter: java
ms.date: 12/20/2018
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.openlocfilehash: 9048fe69be5f12b19b547e275fd801733849a87f
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/05/2020
ms.locfileid: "82209824"
---
# <a name="deploy-a-hello-world-web-app-to-a-linux-container-in-the-cloud-using-the-azure-toolkit-for-eclipse"></a>Distribuire un'app Web Hello World in un contenitore Linux nel cloud con Azure Toolkit for Eclipse

I contenitori [Docker] sono un metodo molto diffuso per distribuire applicazioni Web. L'uso dei contenitori Docker permette agli sviluppatori di consolidare tutti i file di progetto e le dipendenze in un unico pacchetto per la distribuzione in un server. Azure Toolkit for Eclipse semplifica questo processo per gli sviluppatori Java aggiungendo funzionalità per la distribuzione di contenitori in Microsoft Azure.

Questo articolo illustra i passaggi necessari per creare un'app Web Hello World di base e pubblicarla in un contenitore Linux in Azure usando Azure Toolkit for Eclipse.

[!INCLUDE [prerequisites](includes/prerequisites.md)]
* Un client [Docker].

> [!NOTE]
>
> Per completare i passaggi di questa esercitazione, è necessario configurare [Docker] in modo da esporre il daemon sulla porta 2375 senza TLS. È possibile configurare questa impostazione durante l'installazione di Docker o tramite il menu delle impostazioni di Docker.
>
> ![Menu delle impostazioni di Docker][docker-settings-menu]
>

## <a name="create-a-new-web-app-project"></a>Creare un nuovo progetto di app Web

1. Avviare Eclipse e accedere al proprio account Azure usando la procedura descritta nelle [istruzioni di accesso per Azure Toolkit for Eclipse](/azure/developer/java/toolkit-for-eclipse/sign-in-instructions).

1. Fare clic sul menu **File**, quindi su **New** (Nuovo) e infine su **Dynamic Web Project** (Progetto Web dinamico).
   
   ![Creare un nuovo progetto][file-new-project]

1. Nella finestra di dialogo **New Dynamic Web Project** (Nuovo progetto Web dinamico) specificare il nome e il percorso del progetto e quindi fare clic su **Finish** (Fine).
   
   ![Specificare il nome del progetto][project-name]

## <a name="create-an-azure-container-registry-to-use-as-a-private-docker-registry"></a>Creare un'istanza di Registro Azure Container da usare come registro Docker privato

La procedura seguente illustra come usare il portale di Azure per creare un'istanza di Registro Azure Container.

> [!NOTE]
>
> Se si vuole usare l'interfaccia della riga di comando di Azure anziché il portale di Azure, seguire i passaggi in [Creare un registro per contenitori Docker privati usando l'interfaccia della riga di comando di Azure 2.0][Create Docker Registry using Azure CLI].
>

1. Aprire il [Azure portal] ed effettuare l'accesso.

   Dopo aver effettuato l'accesso all'account nel portale di Azure, è possibile seguire la procedura illustrata nell'articolo [Creare un registro per contenitori Docker privati con il portale di Azure], parafrasata per semplicità nei passaggi seguenti.

1. Fare clic sull'icona di menu **+ Crea una risorsa**, quindi su **Contenitori** e infine su **Registro Container**.
   
   ![Creare una nuova istanza di Registro Azure Container][create-container-registry-01]

1. Quando viene visualizzata la pagina **Crea registro contenitori**, immettere **Nome registro** e **Gruppo di risorse**, scegliere **Abilita** per **Utente amministratore** e quindi fare clic su **Crea**.

   ![Configurare le impostazioni di Registro Azure Container][create-container-registry-02]

## <a name="deploy-your-web-app-in-a-docker-container"></a>Distribuire l'app Web in un contenitore Docker

1. Fare clic con il pulsante destro del mouse sul progetto in Project Explorer (Esplora progetti) e scegliere **Azure**, quindi fare clic su **Add Docker Support** (Aggiungi supporto per Docker).

   Verrà automaticamente creato un file Docker con una configurazione predefinita.

   ![Aggiungere il supporto di Docker][add-docker-support]

1. Dopo aver aggiunto il supporto per Docker, fare clic con il pulsante destro del mouse in Project Explorer (Esplora progetti), scegliere **Azure** e quindi fare clic su **Publish to Web App for Containers** (Pubblica in app Web per contenitori).

   ![Pubblicazione in un'app Web per contenitori][run-on-web-app-for-containers]

1. Quando viene visualizzata la finestra di dialogo **Run on Web App for Containers** (Esegui in app Web per contenitori), inserire le informazioni necessarie.

   * **Docker File** (File Docker): specifica il percorso del file Docker che è stato creato quando è stato aggiunto il supporto per Docker al progetto. 

   * **Container Registry** (Registro Container): scegliere dal menu di scelta rapida l'istanza di Registro Container creata nella sezione precedente di questo articolo. I campi **Server URL** (URL server), **Username** (Nome utente) e **Password** verranno popolati automaticamente.

   * **Image and tag** (Immagine e tag): specifica il nome dell'immagine del contenitore. In genere viene usata la sintassi "*registro*.azurecr.io/*nomeapp*:latest", dove: 
      * *registro* è il registro contenitori creato nella sezione precedente di questo articolo 
      * *nomeapp* è il nome dell'app Web 

   * **Web App for Container** (App Web per contenitore): scegliere **Use Existing** (Usa esistente) o **Create New** (Crea nuova) per specificare se il contenitore verrà distribuito in un'app Web esistente o ne verrà creata una nuova.  Con il valore specificato in **App name** (Nome app) verrà creato l'URL dell'app Web, ad esempio *wingtiptoys.azurewebsites.net*.

   * **Gruppo di risorse**: specifica se usare un gruppo di risorse esistente o crearne uno nuovo. 

   * **App Service Plan** (Piano di servizio app): specifica se usare un piano di servizio app esistente o crearne uno nuovo. 

   ![Esecuzione in un'app Web per contenitori][run-on-web-app-linux]

1. Al termine della configurazione delle impostazioni indicate sopra, fare clic su **OK** per pubblicare l'app Web in Azure.

1. Dopo la pubblicazione dell'app Web, è possibile passare all'URL specificato in precedenza per l'app Web, ad esempio *wingtiptoys.azurewebsites.net*.

   ![Passaggio all'app Web][browsing-to-web-app]

## <a name="next-steps"></a>Passaggi successivi

Per altre risorse per Docker, vedere il [sito Web Docker][Docker] ufficiale.

[!INCLUDE [additional-resources](includes/additional-resources.md)]

<!-- URL List -->

[Azure portal]: https://portal.azure.com/
[Creare un registro per contenitori Docker privati con il portale di Azure]: /azure/container-registry/container-registry-get-started-portal
[Azure for Java Developers]: /azure/developer/java/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[Create Docker Registry using Azure CLI]: /azure/container-registry/container-registry-get-started-azure-cli

[Docker]: https://www.docker.com/
[Configuring artifacts]: https://www.jetbrains.com/help/idea/2016.1/configuring-artifacts.html

<!-- IMG List -->

[add-docker-support]: media/hello-world-web-app-linux/add-docker-support.png
[browsing-to-web-app]:  media/hello-world-web-app-linux/browsing-to-web-app.png
[create-container-registry-01]: media/hello-world-web-app-linux/create-container-registry-01.png
[create-container-registry-02]: media/hello-world-web-app-linux/create-container-registry-02.png
[docker-settings-menu]: media/hello-world-web-app-linux/docker-settings-menu.png
[file-new-project]: media/hello-world-web-app-linux/file-new-project.png
[project-name]: media/hello-world-web-app-linux/project-name.png
[run-on-web-app-for-containers]: media/hello-world-web-app-linux/run-on-web-app-for-containers.png
[run-on-web-app-linux]: media/hello-world-web-app-linux/run-on-web-app-linux.png