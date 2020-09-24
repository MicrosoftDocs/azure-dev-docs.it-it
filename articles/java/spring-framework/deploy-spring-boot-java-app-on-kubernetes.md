---
title: Distribuire un'applicazione Spring Boot nel servizio Azure Kubernetes
titleSuffix: Azure Kubernetes Service
description: Questa esercitazione illustra in modo dettagliato la procedura per la distribuzione di un'applicazione Spring Boot in un cluster Kubernetes in Microsoft Azure.
services: container-service
documentationcenter: java
ms.date: 11/12/2019
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.custom: mvc, devx-track-java
ms.openlocfilehash: b4b36df09811e8e9e6a9bae944119e280258d751
ms.sourcegitcommit: 39f3f69e3be39e30df28421a30747f6711c37a7b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/21/2020
ms.locfileid: "90830077"
---
# <a name="deploy-spring-boot-application-to-the-azure-kubernetes-service"></a>Distribuire un'applicazione Spring Boot nel servizio Azure Kubernetes

**[Kubernetes]** e **[Docker]** sono soluzioni open source che consentono agli sviluppatori di automatizzare la distribuzione, il ridimensionamento e la gestione delle applicazioni eseguite in contenitori.

Questa esercitazione illustra come combinare queste due diffuse tecnologie open source per sviluppare e distribuire un'applicazione Spring Boot in Microsoft Azure. In particolare, si userà *[Spring Boot]* per lo sviluppo dell'applicazione, *[Kubernetes]* per la distribuzione del contenitore e il [servizio Azure Kubernetes] per l'hosting dell'applicazione.

## <a name="prerequisites"></a>Prerequisiti

* Sottoscrizione di Azure; se non si ha una sottoscrizione di Azure, è possibile attivare i [vantaggi per i sottoscrittori di MSDN] oppure iscriversi per ottenere un [account Azure gratuito].
* [Interfaccia della riga di comando di Azure].
* Java Development Kit (JDK) supportato. Per altre informazioni sulle versioni di JDK utilizzabili per lo sviluppo in Azure, vedere <https://aka.ms/azure-jdks>.
* Lo strumento di compilazione [Maven] di Apache (versione 3).
* Un client [Git].
* Un client [Docker].
* L'[helper per le credenziali Docker di Registro Azure Container](https://github.com/Azure/acr-docker-credential-helper).

> [!NOTE]
> A causa dei requisiti di virtualizzazione di questa esercitazione, non è possibile seguire la procedura illustrata in questo articolo in una macchina virtuale. È necessario usare un computer fisico in cui sono abilitate le funzionalità di virtualizzazione.

## <a name="create-the-spring-boot-on-docker-getting-started-web-app"></a>Creare l'app Web introduttiva di Spring Boot in Docker

La procedura seguente illustra come creare un'applicazione Web di Spring Boot e come testarla in locale.

1. Aprire un prompt dei comandi e creare una directory locale in cui contenere l'applicazione, quindi passare a tale directory. Ad esempio:

   ```bash
   mkdir C:\SpringBoot
   cd C:\SpringBoot
   ```

   -- o --

   ```bash
   mkdir /users/$USER/SpringBoot
   cd /users/$USER/SpringBoot
   ```

1. Clonare il progetto di esempio [Spring Boot on Docker Getting Started] (Introduzione a Spring Boot in Docker) nella directory.

   ```bash
   git clone https://github.com/spring-guides/gs-spring-boot-docker.git
   ```

1. Passare alla directory del progetto completato.

   ```bash
   cd gs-spring-boot-docker
   cd complete
   ```

1. Usare Maven per compilare ed eseguire l'app di esempio.

   ```bash
   mvn package spring-boot:run
   ```

1. Testare l'app Web passando a `http://localhost:8080` oppure con il comando `curl` seguente:

   ```bash
   curl http://localhost:8080
   ```

1. Dovrebbe essere visualizzato il messaggio **Hello Docker World**

   ![Esplorare l'app di esempio in locale][SB01]

## <a name="create-an-azure-container-registry-using-the-azure-cli"></a>Creare un'istanza di Registro Azure Container usando l'interfaccia della riga di comando di Azure

1. Aprire un prompt dei comandi.

1. Accedere all'account di Azure:

   ```azurecli
   az login
   ```

1. Scegliere la sottoscrizione di Azure:

   ```azurecli
   az account set -s <YourSubscriptionID>
   ```

1. Creare un gruppo di risorse per le risorse di Azure usate in questa esercitazione.

   ```azurecli
   az group create --name=wingtiptoys-kubernetes --location=eastus
   ```

1. Creare un Registro Azure Container privato nel gruppo di risorse. L'esercitazione effettua il push dell'app di esempio come immagine Docker in questo registro nei passaggi successivi. Sostituire `wingtiptoysregistry` con un nome univoco per il registro.

   ```azurecli
   az acr create --resource-group wingtiptoys-kubernetes --location eastus \
    --name wingtiptoysregistry --sku Basic
   ```

## <a name="push-your-app-to-the-container-registry-via-jib"></a>Eseguire il push dell'app nel registro contenitori tramite Jib

1. Accedere a Registro Azure Container dall'interfaccia della riga di comando di Azure.

   ```azurecli
   # set the default name for Azure Container Registry, otherwise you will need to specify the name in "az acr login"
   az configure --defaults acr=wingtiptoysregistry
   az acr login
   ```

1. Aprire il file *pom.xml* con un editor di testo, ad esempio [Visual Studio Code](https://code.visualstudio.com/docs).

   ```bash
   code pom.xml
   ```

1. Aggiornare la raccolta `<properties>` nel file *pom.xml* con il nome registro dell'istanza di Registro Azure Container e la versione più recente di [jib-maven-plugin](https://github.com/GoogleContainerTools/jib/tree/master/jib-maven-plugin).

   ```xml
   <properties>
      <!-- Note: If your ACR name contains upper case characters, be sure to convert them to lower case characters. -->
      <docker.image.prefix>wingtiptoysregistry.azurecr.io</docker.image.prefix>
      <jib-maven-plugin.version>2.5.2</jib-maven-plugin.version>
      <java.version>1.8</java.version>
   </properties>
   ```

1. Aggiornare la raccolta `<plugins>` nel file *pom.xml* in modo che l'elemento `<plugin>` contenga una voce per `jib-maven-plugin`, come illustrato nell'esempio seguente. Si noti che si sta usando un'immagine di base di Microsoft Container Registry, `mcr.microsoft.com/java/jdk:8-zulu-alpine`, che contiene una versione di JDK supportata ufficialmente per Azure. Per altre immagini di base di MCR con versioni di JDK supportate ufficialmente, vedere [Java SE JDK](https://hub.docker.com/_/microsoft-java-jdk), [Java SE JRE](https://hub.docker.com/_/microsoft-java-jre), [Java SE Headless JRE](https://hub.docker.com/_/microsoft-java-jre-headless) e [Java SE JDK e Maven](https://hub.docker.com/_/microsoft-java-maven).

   ```xml
   <plugin>
     <artifactId>jib-maven-plugin</artifactId>
     <groupId>com.google.cloud.tools</groupId>
     <version>${jib-maven-plugin.version}</version>
     <configuration>
        <from>
            <image>mcr.microsoft.com/java/jdk:8-zulu-alpine</image>
        </from>
        <to>
            <image>${docker.image.prefix}/${project.artifactId}</image>
        </to>
     </configuration>
   </plugin>
   ```

1. Passare alla directory del progetto completato per l'applicazione Spring Boot ed eseguire questo comando per creare l'immagine ed eseguire il push dell'immagine nel registro:

   ```cmd
   az acr login && mvn compile jib:build
   ```

> [!NOTE]
> Per motivi di sicurezza dell'interfaccia della riga di comando di Azure e Registro Azure Container, la credenziale creata da `az acr login` è valida per un'ora. Se viene visualizzato l'errore *401 Non autorizzato*, è possibile eseguire di nuovo il comando `az acr login -n <your registry name>` per ripetere l'autenticazione.

## <a name="create-a-kubernetes-cluster-on-aks-using-the-azure-cli"></a>Creare un cluster Kubernetes nel servizio Azure Container usando l'interfaccia della riga di comando di Azure

1. Creare un cluster Kubernetes nel servizio Azure Kubernetes. Il comando seguente crea un cluster *kubernetes* nel gruppo di risorse *wingtiptoys-kubernetes*, con *wingtiptoys-akscluster* come nome del cluster, con `wingtiptoysregistry` come Registro Azure Container collegato e con *wingtiptoys-kubernetes* come prefisso DNS:

   ```azurecli
   az aks create --resource-group=wingtiptoys-kubernetes --name=wingtiptoys-akscluster \
    --attach-acr wingtiptoysregistry \
    --dns-name-prefix=wingtiptoys-kubernetes --generate-ssh-keys
   ```

   Il completamento di questo comando può richiedere alcuni minuti.

1. Installare `kubectl` usando l'interfaccia della riga di comando di Azure. È possibile che gli utenti Linux debbano aggiungere al comando il prefisso `sudo`, perché distribuisce l'interfaccia della riga di comando di Kubernetes in `/usr/local/bin`.

   ```azurecli
   az aks install-cli
   ```

1. Scaricare le informazioni sulla configurazione del cluster, in modo da consentire la gestione del cluster dall'interfaccia Web di Kubernetes e `kubectl`. 

   ```azurecli
   az aks get-credentials --resource-group=wingtiptoys-kubernetes --name=wingtiptoys-akscluster
   ```

## <a name="deploy-the-image-to-your-kubernetes-cluster"></a>Distribuire l'immagine nel cluster Kubernetes

Questa esercitazione consente di distribuire l'app usando `kubectl` e quindi di esplorare la distribuzione tramite l'interfaccia Web di Kubernetes.

### <a name="deploy-with-kubectl"></a>Eseguire la distribuzione con kubectl

1. Aprire un prompt dei comandi.

1. Eseguire il contenitore nel cluster Kubernetes usando il comando `kubectl run`. Specificare un nome di servizio per l'app in Kubernetes e il nome completo dell'immagine. Ad esempio:

   ```bash
   kubectl run gs-spring-boot-docker --image=wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest
   ```

   In questo comando:

   * Il nome del contenitore `gs-spring-boot-docker` viene specificato immediatamente dopo il comando `run`.

   * Il parametro `--image` specifica il nome combinato del server di accesso e dell'immagine come `wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest`.

1. Esporre esternamente il cluster Kubernetes usando il comando `kubectl expose`. Specificare il nome del servizio, la porta TCP pubblica usata per accedere all'app e la porta di destinazione interna su cui è in ascolto l'app. Ad esempio:

   ```bash
   kubectl expose pod gs-spring-boot-docker --type=LoadBalancer --port=80 --target-port=8080
   ```

   In questo comando:

   * Il nome del contenitore `gs-spring-boot-docker` viene specificato immediatamente dopo il comando `expose pod`.

   * Il parametro `--type` specifica che il cluster usa il bilanciamento del carico.

   * Il parametro `--port` specifica la porta TCP pubblica, ovvero 80. Si accede all'app tramite questa porta.

   * Il parametro `--target-port` specifica la porta TCP interna, ovvero 8080. Il servizio di bilanciamento del carico inoltra le richieste all'app su questa porta.

1. Dopo la distribuzione dell'app nel cluster, eseguire query sull'indirizzo IP esterno e aprirlo nel Web browser:

   ```bash
   kubectl get services -o=jsonpath='{.items[*].status.loadBalancer.ingress[0].ip}'
   ```

   ![Esplorare l'app di esempio in Azure][SB02]

### <a name="deploy-with-the-kubernetes-web-interface"></a>Eseguire la distribuzione con l'interfaccia Web di Kubernetes

1. Aprire un prompt dei comandi.

1. Aprire il sito Web di configurazione per il cluster Kubernetes nel browser predefinito:

   ```azurecli
   az aks browse --resource-group=wingtiptoys-kubernetes --name=wingtiptoys-akscluster
   ```

> [!IMPORTANT]
> Se il cluster del servizio Azure Container utilizza RBAC, è necessario creare un *ClusterRoleBinding* prima di poter accedere correttamente al dashboard. Per impostazione predefinita, il dashboard Kubernetes viene distribuito con accesso in lettura minimo e visualizza errori di accesso di Controllo degli accessi in base al ruolo. Il dashboard Kubernetes attualmente non supporta credenziali specificate dall'utente per determinare il livello di accesso, ma usa i ruoli concessi all'account del servizio. Un amministratore del cluster può scegliere di concedere accesso aggiuntivo all'account del servizio *kubernetes-dashboard*. Questo tuttavia può costituire un vettore per l'escalation dei privilegi. È anche possibile integrare l'autenticazione di Azure Active Directory per fornire un livello di accesso più granulare.
>
> Per creare un binding, usare il comando [kubectl create clusterrolebinding]. L'esempio seguente illustra come creare un binding di esempio, tuttavia questo esempio non applica componenti di autenticazione aggiuntivi e potrebbe comportare un uso non sicuro. Il dashboard di Kubernetes è aperto a tutti gli utenti con accesso all'URL. Non esporre pubblicamente il dashboard di Kubernetes.
>
> ```bash
> kubectl create clusterrolebinding kubernetes-dashboard --clusterrole=cluster-admin --serviceaccount=kube-system:kubernetes-dashboard
> ```
>
> Per altre informazioni sull'uso dei diversi metodi di autenticazione, vedere [dashboard-authentication] nel wiki del dashboard di Kubernetes.

1. All'apertura del sito Web di configurazione di Kubernetes nel browser, selezionare il collegamento per **distribuire un'app inclusa in contenitori**:

   ![Sito Web di configurazione di Kubernetes che mostra il messaggio in cui informa che non ci sono elementi da visualizzare][KB01]

1. Quando viene visualizzata la pagina **Resource Creation** (Creazione risorse), specificare le opzioni seguenti:

   a. Selezionare **CREATE AN APP** (CREA APP).

   b. Immettere il nome dell'applicazione Spring Boot per **App name** (Nome app), ad esempio *gs-spring-boot-docker*.

   c. Immettere il server di accesso e l'immagine del contenitore dai passaggi precedenti in **Container image** (Immagine del contenitore), ad esempio *wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest*.

   d. Scegliere **External** (Esterno) per **Service** (Servizio).

   e. Specificare le porte esterne ed interne nelle caselle di testo **Port** (Porta) e **Target port** (Porta di destinazione).

   ![Pagina di creazione di un'app nel sito Web di configurazione di Kubernetes][KB02]

1. Selezionare **Deploy** (Distribuisci) per distribuire il contenitore.

   ![Distribuzione di Kubernetes][KB05]

1. Al termine della distribuzione dell'applicazione, l'applicazione Spring Boot verrà elencata in **Services** (Servizi).

   ![Sito Web di Kubernetes, elenco dei servizi][KB06]

1. Se si seleziona il collegamento **External endpoints** (Endpoint esterni), è possibile visualizzare l'applicazione Spring Boot in esecuzione in Azure.

   ![Sito Web di Kubernetes, endpoint esterni evidenziati][KB07]

   ![Esplorare l'app di esempio in Azure][SB02]

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni su Spring e Azure, passare al centro di documentazione di Spring in Azure.

> [!div class="nextstepaction"]
> [Spring in Azure](./index.yml)

### <a name="additional-resources"></a>Risorse aggiuntive

Per altre informazioni sull'uso di Spring Boot in Azure, vedere l'articolo seguente:

* [Distribuire un'applicazione Spring Boot nel servizio app di Azure](deploy-spring-boot-java-app-from-container-registry-using-maven-plugin.md)

Per altre informazioni sull'uso di Azure con Java, vedere [Azure per sviluppatori Java] e la documentazione relativa all'[uso di Azure DevOps e Java].

Per altre informazioni sulla distribuzione di un'applicazione Java in Kubernetes con Visual Studio Code, vedere le [esercitazioni su Java in Visual Studio Code].

Per altre informazioni sul progetto di esempio di Spring Boot in Docker, vedere [Spring Boot on Docker Getting Started] (Introduzione a Spring Boot in Docker).

I collegamenti seguenti forniscono informazioni aggiuntive sulla creazione di applicazioni Spring Boot:

* Per altre informazioni sulla creazione di una semplice applicazione Spring Boot, vedere Spring Initializr all'indirizzo https://start.spring.io/.

I collegamenti seguenti forniscono informazioni aggiuntive sull'uso di Kubernetes con Azure:

* [Introduzione ai cluster Kubernetes nel servizio Azure Kubernetes](/azure/aks/intro-kubernetes)

Per altre informazioni sull'uso dell'interfaccia della riga di comando di Kubernetes, vedere la guida dell'utente di **kubectl** all'indirizzo <https://kubernetes.io/docs/user-guide/kubectl/>.

Il sito Web Kubernetes include alcuni articoli relativi all'uso delle immagini nei registri privati:

* [Configuring Service Accounts for Pods] (Configurazione degli account del servizio per i pod)
* [Namespaces] (Spazi dei nomi)
* [Pulling an Image from a Private Registry] (Effettuare il pull di un'immagine da un registro privato)

Per altri esempi sull'uso delle immagini personalizzate di Docker con Azure, vedere [Uso di un'immagine Docker personalizzata per App Web di Azure in Linux].

Per altre informazioni sull'esecuzione iterativa e il debug di contenitori direttamente nel servizio Azure Kubernetes con Azure Dev Spaces, vedere [Guida introduttiva ad Azure Dev Spaces con Java]

<!-- URL List -->
[kubectl create clusterrolebinding]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#-em-clusterrolebinding-em-
[dashboard-authentication]: https://github.com/kubernetes/dashboard/wiki/Access-control
[Interfaccia della riga di comando di Azure]: /cli/azure/overview
[Servizio Azure Kubernetes]: https://azure.microsoft.com/services/kubernetes-service/
[Azure per sviluppatori Java]: ../index.yml
[Azure portal]: https://portal.azure.com/
[Create a private Docker container registry using the Azure portal]: /azure/container-registry/container-registry-get-started-portal
[Uso di un'immagine Docker personalizzata per App Web di Azure in Linux]: /azure/app-service/tutorial-custom-container
[Docker]: https://www.docker.com/
[Account Azure gratuito]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Uso di Azure DevOps e Java]: /azure/devops/java/
[Kubernetes]: https://kubernetes.io/
[Kubernetes Command-Line Interface (kubectl)]: https://kubernetes.io/docs/user-guide/kubectl-overview/
[Maven]: http://maven.apache.org/
[vantaggi per i sottoscrittori di MSDN]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Boot on Docker Getting Started]: https://github.com/spring-guides/gs-spring-boot-docker (Introduzione a Spring Boot in Docker)
[Spring Framework]: https://spring.io/
[Configuring Service Accounts for Pods]: https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/ (Configurazione degli account del servizio per i pod)
[Namespaces]: https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/ (Spazi dei nomi)
[Pulling an Image from a Private Registry]: https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/ (Effettuare il pull di un'immagine da un registro privato)

[Java Development Kit (JDK)]: ../fundamentals/java-jdk-long-term-support.md
<!-- http://www.oracle.com/technetwork/java/javase/downloads/ -->

<!-- Newly added -->
[Authenticate with Azure Container Registry from Azure Kubernetes Service]: /azure/container-registry/container-registry-auth-aks/
[Esercitazioni su Java in Visual Studio Code]: https://code.visualstudio.com/docs/java/java-kubernetes/
[Guida introduttiva ad Azure Dev Spaces con Java]: /azure/dev-spaces/get-started-java
<!-- IMG List -->

[SB01]: media/deploy-spring-boot-java-app-on-kubernetes/SB01.png
[SB02]: media/deploy-spring-boot-java-app-on-kubernetes/SB02.png

[AR01]: media/deploy-spring-boot-java-app-on-kubernetes/AR01.png
[AR02]: media/deploy-spring-boot-java-app-on-kubernetes/AR02.png
[AR03]: media/deploy-spring-boot-java-app-on-kubernetes/AR03.png
[AR04]: media/deploy-spring-boot-java-app-on-kubernetes/AR04.png

[KB01]: media/deploy-spring-boot-java-app-on-kubernetes/KB01.png
[KB02]: media/deploy-spring-boot-java-app-on-kubernetes/KB02.png
[KB03]: media/deploy-spring-boot-java-app-on-kubernetes/KB03.png
[KB04]: media/deploy-spring-boot-java-app-on-kubernetes/KB04.png
[KB05]: media/deploy-spring-boot-java-app-on-kubernetes/KB05.png
[KB06]: media/deploy-spring-boot-java-app-on-kubernetes/KB06.png
[KB07]: media/deploy-spring-boot-java-app-on-kubernetes/KB07.png