---
title: Distribuire un'app Web Hello World in un contenitore Linux
titleSuffix: Azure Toolkit for IntelliJ
description: Eseguire un'app Web Hello World di base in un contenitore Linux e distribuirla sul cloud tramite Azure Toolkit for IntelliJ.
services: app-service\web
documentationcenter: java
ms.date: 09/09/2020
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.custom: devx-track-java
ms.openlocfilehash: 080ea6565a05860d930d8dd22bca5328fd34a545
ms.sourcegitcommit: cbcde17e91e7262a596d813243fd713ce5e97d06
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/06/2020
ms.locfileid: "93405840"
---
# <a name="deploy-java-app-to-azure-web-apps-for-containers-using-azure-toolkit-for-intellij"></a>Distribuire un'app Java in app Web di Azure per contenitori tramite Azure Toolkit for IntelliJ

I contenitori [Docker] sono un metodo molto diffuso per distribuire applicazioni Web. L'uso dei contenitori Docker permette agli sviluppatori di consolidare tutti i file di progetto e le dipendenze in un unico pacchetto per la distribuzione in un server. Azure Toolkit for IntelliJ semplifica questo processo per gli sviluppatori Java aggiungendo funzionalità per la distribuzione di contenitori in Microsoft Azure.

Questo articolo indica i passaggi necessari per creare un'app Web Hello World di base e pubblicarla in un contenitore Linux in Azure usando Azure Toolkit for IntelliJ.

[!INCLUDE [prerequisites](includes/prerequisites.md)]
* Un client [Docker].

> [!NOTE]
>
> Per completare i passaggi di questa esercitazione, è necessario configurare [Docker] in modo da esporre il daemon sulla porta 2375 senza TLS. È possibile configurare questa impostazione durante l'installazione di Docker o tramite il menu delle impostazioni di Docker.
>
> ![Menu delle impostazioni di Docker][docker-settings-menu]
>

## <a name="installation-and-sign-in"></a>Installazione e accesso

La procedura seguente illustra il processo di accesso ad Azure nell'ambiente di sviluppo IntelliJ.

1. Se non è stato installato il plug-in, vedere [Installazione di Azure Toolkit for IntelliJ](./index.yml).

1. Per accedere all'account Azure, passare alla barra laterale sinistra di **Azure Explorer** e quindi fare clic sull'icona **Azure Sign In** (Accesso ad Azure). In alternativa, è possibile passare a **Tools** (Strumenti), espandere **Azure** e fare clic su **Azure Sign in** (Accesso ad Azure).

   :::image type="content" source="media/sign-in-instructions/I01.png" alt-text="Accesso ad Azure in IntelliJ."::: 

1. Nella finestra **Azure Sign In** (Accesso ad Azure) selezionare **Device Login** (Accesso dispositivo) e quindi fare clic su **Sign in** (Accedi) ( [altre opzioni di accesso](sign-in-instructions.md)).

1. Nella finestra di dialogo **Azure Device Login** (Accesso dispositivo Azure) fare clic su **Copy&Open** (Copia e apri).

1. Nel browser incollare il codice dispositivo (copiato facendo clic su **Copy&Open** nell'ultimo passaggio) e quindi fare clic su **Avanti**.

1. Selezionare l'account Azure e completare le procedure di autenticazione necessarie per eseguire l'accesso.

1. Dopo l'accesso, chiudere il browser e tornare all'IDE di IntelliJ. Nella finestra di dialogo **Select Subscriptions** (Seleziona sottoscrizioni) selezionare le sottoscrizioni da usare e quindi fare clic su **Select** (Seleziona).

## <a name="creating-a-new-web-app-project"></a>Creazione di un nuovo progetto di app Web

1. Fare clic su **File** , espandere **New** (Nuovo) e quindi fare clic su **Project** (Progetto).

1. Nella finestra di dialogo **New Project** (Nuovo progetto) selezionare **Maven** e assicurarsi che l'opzione **Create from Archetype** (Crea da archetipo) sia selezionata. Nell'elenco selezionare **maven-archetype-webapp** e quindi fare clic su **Next** (Avanti).

   :::image type="content" source="media/create-hello-world-web-app/maven-archetype-webapp.png" alt-text="Selezionare l'opzione maven-archetype-webapp."::: 

1. Espandere l'elenco a discesa **Artifact Coordinates** (Coordinate artefatto) per visualizzare tutti i campi di input, specificare le informazioni seguenti per la nuova app Web e fare clic su **Next** (Avanti):

   * **Name** : nome dell'app Web. Questo valore verrà inserito automaticamente nel campo **ArtifactId** dell'app Web.
   * **GroupId** (ID gruppo): nome del gruppo di artefatti, in genere un dominio aziendale, ad esempio *com.microsoft.azure*.
   * **Versione** : mantenere la versione predefinita, ovvero *1.0-SNAPSHOT*.

1. Personalizzare eventuali impostazioni di Maven o accettare quelle predefinite e quindi fare clic su **Finish** (Fine).

1. Passare al progetto nella scheda **Project** (Progetto) a sinistra e aprire il file **src/main/webapp/index.jsp**. Sostituire il codice con quello seguente e **salvare le modifiche** :

   ```html
   <html>
    <body>
      <b><% out.println("Hello World!"); %></b>
    </body>
   </html>
   ```
   :::image type="content" source="media/create-hello-world-web-app/open-index-page.png" alt-text="Aprire il file index.jsp.":::

## <a name="create-an-azure-container-registry-to-use-as-a-private-docker-registry"></a>Creare un'istanza di Registro Azure Container da usare come registro Docker privato

La procedura seguente illustra come usare il portale di Azure per creare un'istanza di Registro Azure Container.

> [!NOTE]
>
> Se si vuole usare l'interfaccia della riga di comando di Azure anziché il portale di Azure, seguire i passaggi in [Creare un registro per contenitori Docker privati usando l'interfaccia della riga di comando di Azure 2.0][Create Docker Registry using Azure CLI].
>

1. Aprire il [Azure portal] ed effettuare l'accesso.

   Dopo aver effettuato l'accesso all'account nel portale di Azure, è possibile seguire la procedura illustrata nell'articolo [Creare un registro per contenitori Docker privati con il portale di Azure], parafrasata per semplicità nei passaggi seguenti.

1. Fare clic sull'icona di menu **+ Crea una risorsa** , quindi sulla categoria **Contenitori** e infine su **Registro contenitori**.

1. Quando viene visualizzata la pagina **Crea registro contenitori** specificare le informazioni seguenti:

   * **Sottoscrizione** specifica la sottoscrizione di Azure da usare per il nuovo registro contenitori.

   * **Gruppo di risorse** : specifica il gruppo di risorse per il registro contenitori. Selezionare una delle opzioni seguenti:
      * **Create New** (Crea nuovo): specifica che si vuole creare un nuovo gruppo di risorse.
      * **Use Existing** (Usa esistente): specifica che verrà effettuata una selezione da un elenco di gruppi di risorse associati all'account Azure.

   * **Nome registro** : specifica il nome del nuovo registro contenitori.

   * **Località** : specifica l'area in cui verrà creato il registro contenitori, ad esempio "Stati Uniti occidentali".

   * **SKU** : specifica il livello di servizio per il registro contenitori. Per questa esercitazione selezionare *Basic*. Per altre informazioni, vedere [Livelli di servizio di Registro Azure Container](/azure/container-registry/container-registry-skus).

1. Fare clic su **Rivedi e crea** e verificare che le informazioni siano corrette. Per finire, fare clic su **Crea**.

## <a name="deploy-your-web-app-in-a-docker-container"></a>Distribuire l'app Web in un contenitore Docker

La procedura seguente consente di configurare il supporto Docker per l'app Web e di distribuire l'app Web in un contenitore Docker.

1. Passare al progetto nella scheda **Project** (Progetto) a sinistra e fare clic con il pulsante destro del mouse sul progetto. Espandere **Azure** e fare clic su **Add Docker Support** (Aggiungi supporto Docker).

   Verrà automaticamente creato un file Docker con una configurazione predefinita.

   :::image type="content" source="media/hello-world-web-app-linux/docker-support-file.png" alt-text="File di supporto Docker.":::

1. Dopo aver aggiunto il supporto per Docker, fare clic con il pulsante destro del mouse in Project Explorer (Esplora progetti), espandere **Azure** e quindi fare clic su **Run on Web App for Containers** (Esegui in app Web per contenitori).

1. Nella finestra di dialogo **Run on Web App for Containers** (Esegui in app Web per contenitori) immettere le informazioni seguenti:

   * **Name** : specifica il nome descrittivo visualizzato in Azure Toolkit. 

   * **Container Registry** (Registro Container): scegliere dal menu di scelta rapida l'istanza di Registro Container creata nella sezione precedente di questo articolo. I campi **Server URL** (URL server), **Username** (Nome utente) e **Password** verranno popolati automaticamente.

   * **Image and tag** (Immagine e tag): specifica il nome dell'immagine del contenitore. In genere viene usata la sintassi " *registro*.azurecr.io/ *nomeapp* :latest", dove: 
      * *registro* è il registro contenitori creato nella sezione precedente di questo articolo 
      * *nomeapp* è il nome dell'app Web 

   * **Use Existing Web App** (Usa app Web esistente) o **Create New Web App** (Crea nuova app Web): specifica se il contenitore verrà distribuito in un'app Web esistente o ne verrà creata una nuova. Con il valore specificato in **App name** (Nome app) verrà creato l'URL dell'app Web, ad esempio *wingtiptoys.azurewebsites.net*.

   * **Gruppo di risorse** : specifica se usare un gruppo di risorse esistente o crearne uno nuovo. 

   * **App Service Plan** (Piano di servizio app): specifica se usare un piano di servizio app esistente o crearne uno nuovo. 

1. Al termine della configurazione delle impostazioni indicate sopra, fare clic su **Run** (Esegui). Al completamento della distribuzione dell'app Web, lo stato verrà visualizzato nella finestra **Run** (Esegui).

1. Dopo la pubblicazione dell'app Web, è possibile passare all'URL specificato in precedenza per l'app Web, ad esempio *wingtiptoys.azurewebsites.net*.

   ![Passaggio all'app Web][browsing-to-web-app]

## <a name="optional-modify-your-web-app-publish-settings"></a>Facoltativo: modificare le impostazioni di pubblicazione dell'app Web

1. Dopo aver pubblicato l'app Web, le impostazioni verranno salvate come predefinite e sarà possibile eseguire l'applicazione su Azure facendo clic sulla freccia verde sulla barra degli strumenti. È possibile modificare queste impostazioni facendo clic sul menu a discesa per l'app Web e quindi scegliendo **Edit Configurations** (Modifica configurazioni).

    :::image type="content" source="media/create-hello-world-web-app/edit-configuration-menu.png" alt-text="Menu Edit Configurations (Modifica configurazioni).":::

1. Quando viene visualizzata la finestra di dialogo **Run/Debug Configurations** (Esecuzione/debug configurazioni), è possibile modificare qualsiasi impostazione predefinita e quindi fare clic su **OK**.

## <a name="next-steps"></a>Passaggi successivi

Per altre risorse per Docker, vedere il [sito Web Docker][Docker] ufficiale.

[!INCLUDE [additional-resources](includes/additional-resources.md)]

<!-- URL List -->

[Azure portal]: https://portal.azure.com/
[Creare un registro per contenitori Docker privati con il portale di Azure]: /azure/container-registry/container-registry-get-started-portal
[Azure for Java Developers]: ../index.yml
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[Create Docker Registry using Azure CLI]: /azure/container-registry/container-registry-get-started-azure-cli

[Docker]: https://www.docker.com/
[Configuring artifacts]: https://www.jetbrains.com/help/idea/2016.1/configuring-artifacts.html

<!-- IMG List -->

[browsing-to-web-app]:  media/hello-world-web-app-linux/browsing-to-web-app.png
[docker-settings-menu]: media/hello-world-web-app-linux/docker-settings-menu.png