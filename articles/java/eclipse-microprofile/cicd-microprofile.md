---
title: CI/CD per app MicroProfile con Azure Pipelines
description: Informazioni su come configurare un ciclo di rilascio CI/CD per distribuire un'app MicroProfile in un'istanza di app Web per contenitori di Azure con Azure Pipelines.
services: devops
documentationcenter: MicroProfile
author: ruyakubu
manager: brunoborges
ms.author: ruyakubu
ms.date: 07/31/2019
ms.tgt_pltfrm: multiple
ms.topic: tutorial
ms.workload: web
ms.openlocfilehash: cdd704626b51105f93c19378511f4a267cb56649
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/05/2020
ms.locfileid: "81670297"
---
# <a name="cicd-for-microprofile-apps-using-azure-pipelines"></a>CI/CD per app MicroProfile con Azure Pipelines

Questa esercitazione illustra come configurare facilmente un ciclo di rilascio di integrazione continua e distribuzione continua (CI/CD) Azure Pipelines per distribuire un'applicazione Java EE [MicroProfile](http://microprofile.io) in un'app Web per contenitori di Azure. L'app MicroProfile in questa esercitazione usa un'immagine di base [Payara Micro](https://www.payara.fish/payara_micro) per creare un file WAR. 

```Dockerfile
FROM payara/micro:5.182
COPY target/*.war $DEPLOY_DIR/ROOT.war
EXPOSE 8080
```
Avviare il processo di inserimento in un contenitore di Azure Pipelines creando un'immagine Docker ed eseguendo il push dell'immagine del contenitore in un Registro Azure Container. Completare il processo creando una pipeline di versione Azure Pipelines e distribuendo l'immagine del contenitore in un'app Web.

## <a name="prerequisites"></a>Prerequisites

1. Nel [portale di Azure](https://portal.azure.com) creare un [Registro Azure Container](https://azure.microsoft.com/services/container-registry).
   
1. Nel portale di Azure creare un'[app Web per contenitori di Azure](https://azure.microsoft.com/services/app-service/containers/). Selezionare **Linux** come **Sistema operativo** e per **Configura contenitore** selezionare **Avvio rapido** come **Origine immagine**.  
   
1. Copiare e salvare l'URL di clonazione dal repository GitHub di esempio all'indirizzo [https://github.com/Azure-Samples/microprofile-hello-azure](https://github.com/Azure-Samples/microprofile-hello-azure).
   
1. Eseguire la registrazione o l'accesso all'organizzazione [Azure DevOps](https://dev.azure.com) e creare un nuovo [progetto](/vsts/organizations/projects/create-project). 
   
1. Importare il repository GitHub di esempio in Azure Repos:
   
   1. Dalla pagina del progetto di Azure DevOps selezionare **Repos** nel riquadro di spostamento sinistro.
   1. In **o importa un repository** selezionare **Importa**. 
   1. In **URL clonazione** immettere l'URL di clonazione Git salvato e selezionare **Importa**.
  
## <a name="create-a-build-pipeline"></a>Creare una pipeline di compilazione

La pipeline di compilazione di integrazione continua in Azure Pipelines esegue automaticamente tutte le attività di compilazione ogni volta che è presente un commit nell'app di origine Java EE. In questo esempio Azure Pipelines usa Maven per compilare il progetto Java MicroProfile.

1. Nella pagina del progetto di Azure DevOps selezionare **Pipeline** > **Compilazioni** nel riquadro di spostamento sinistro. 
   
1. Selezionare **Nuova pipeline**.
   
1. Selezionare **Usa l'editor classico per creare una pipeline senza YAML**. 
   
1. Verificare che il nome del progetto e il repository GitHub importato siano presenti nei campi e scegliere **Continua**.
   
1. Selezionare **Maven** dall'elenco di modelli e quindi fare clic su **Applica**.
   
1. Nel riquadro destro verificare che venga visualizzato **Ubuntu 1604 Ospitato** nell'elenco a discesa **Pool di agenti**.
   
   > [!NOTE]
   > Questa impostazione indica ad Azure Pipelines quale server di compilazione usare.  È anche possibile usare un server di compilazione personalizzato privato.
   
1. Per configurare la pipeline per l'integrazione continua, selezionare la scheda **Trigger** nel riquadro sinistro e quindi selezionare la casella di controllo accanto ad **Abilita l'integrazione continua**.  
   
1. Nella parte superiore della pagina selezionare l'elenco a discesa accanto a **Salva e accoda** e fare clic su **Salva**. 

   ![Abilitare l'integrazione continua](media/cicd-microprofile/continuous-integration.png)

## <a name="create-a-docker-build-image"></a>Creare un'immagine di compilazione Docker

Azure Pipelines usa un Dockerfile con un'immagine di base Payara Micro per creare un'immagine Docker.  

1. Selezionare la scheda **Attività** e quindi fare clic sul segno più **+** accanto a **Processo agente 1** per aggiungere un'attività.
   
   ![Aggiungere una nuova attività](media/cicd-microprofile/add-task.png)
   
1. Nel riquadro destro selezionare**Docker** dall'elenco dei modelli e quindi fare clic su **Aggiungi**. 
   
1. Nel riquadro sinistro selezionare **buildAndPush** e nel riquadro destro immettere una descrizione nel campo **Nome visualizzato**.
   
1. In **Container Repository** selezionare**Nuovo** accanto al campo **Registro Container**. 
   
1. Completare la finestra di dialogo **Aggiungi una connessione al servizio Registro Docker** nel modo seguente:
   
   |Campo|valore|
   |---|---|
   |**Tipo di registro**|Selezionare **Registro Azure Container**.|
   |**Connection Name** (Nome connessione)|Immettere un nome per la connessione.|
   |**Sottoscrizione di Azure**|Selezionare la sottoscrizione di Azure nell'elenco a discesa e, se necessario, fare clic su **Autorizza**.|
   |**Registro contenitori di Azure**|Selezionare il nome del Registro Azure Container nell'elenco a discesa.| 
   
1. Selezionare **OK**.
   
   ![Aggiungere una connessione al servizio Registro Docker](media/cicd-microprofile/dockerconnection.png)
   
   > [!NOTE]
   > Se si usa Docker Hub o un altro registro, selezionare **Hub Docker** o **Altri** anziché **Registro Azure Container** accanto a **Tipo di registro**. Specificare quindi le credenziali e le informazioni di connessione per il registro contenitori.
   
1. In **Comandi** selezionare **Compila**nell'elenco a discesa**Comando**.
   
1. Selezionare i puntini di sospensione **...** accanto al campo**Dockerfile**, individuare e selezionare il **Dockerfile** dal repository GitHub e quindi fare clic su **OK**. 
   
   ![Salvare il Dockerfile](media/cicd-microprofile/selectdockerfile.png)
   
1. In **Tag** immettere*più recente* in una nuova riga. 
   
1. Nella parte superiore della pagina selezionare l'elenco a discesa accanto a **Salva e accoda** e fare clic su **Salva**. 

## <a name="push-the-docker-image-to-acr"></a>Eseguire il push dell'immagine Docker nel Registro Azure Container

Azure Pipelines esegue il push dell'immagine Docker nel Registro Azure Container e la usa per eseguire l'app per le API MicroProfile come app Web Java aggiunta a contenitori.

1. Poiché si sta usando Docker in Azure Pipelines, creare un altro modello Docker ripetendo la procedura descritta in [Creare un'immagine di compilazione Docker](#create-a-docker-build-image). Questa volta selezionare **Esegui push** nell'elenco a discesa **Comando**.
   
1. Selezionare l'elenco a discesa accanto a **Salva e accoda** e fare clic su **Salva e accoda**. 
   
1. Nella finestra popup **Esegui la pipeline** verificare che sia selezionato **Ubuntu 1604 Ospitato** in **Pool di agenti** e fare clic su **Salva ed esegui**. 
   
1. Al termine della compilazione, è possibile selezionare il collegamento ipertestuale nella pagina **Compila** per verificare l'esito della compilazione e visualizzare altri dettagli.
   
   ![Selezionare il collegamento ipertestuale della compilazione](media/cicd-microprofile/checkbuild.png)

## <a name="create-a-release-pipeline"></a>Creare una pipeline di versione

Una pipeline di versione continua Azure Pipelines attiva automaticamente la distribuzione in un ambiente di destinazione come Azure nel momento in cui una compilazione ha esito positivo. È possibile creare pipeline di versione per ambienti di sviluppo, di test, di gestione temporanea o di produzione.

1. Nella pagina del progetto di Azure DevOps selezionare **Pipeline** > **Versioni** nel riquadro di spostamento sinistro. 
   
1. Selezionare **Nuova pipeline**.
   
1. Selezionare **Distribuisci un'app Java in Servizio app di Azure** nell'elenco di modelli e quindi fare clic su **Applica**. 
   
   ![Selezionare il modello Distribuisci un'app Java in Servizio app di Azure](media/cicd-microprofile/selectreleasetemplate.png)
   
1. Nella finestra popup cambiare **Fase 1** in un nome di fase, ad esempio *Sviluppo*, *Test*, *Gestione temporanea* o *Produzione* e quindi chiudere la finestra. 
   
1. In **Artefatti** nel riquadro sinistro fare clic su **Aggiungi** per collegare gli artefatti dalla pipeline di compilazione alla pipeline di rilascio. 
   
1. Nel riquadro destro selezionare la pipeline di compilazione nell'elenco a discesa in **Origine (pipeline di compilazione)** e quindi fare clic su **Aggiungi**.
   
   ![Aggiungere un artefatto di compilazione](media/cicd-microprofile/addbuildartifact.png)
   
1. Selezionare il collegamento ipertestuale nella fase **Produzione** per eseguire l'operazione **Visualizza le attività nella fase**.
   
   ![Selezionare il nome della fase](media/cicd-microprofile/viewstagetasks.png)
   
1. Nel riquadro destro completare il modulo come segue:
   
   |Campo|valore|
   |---|---|
   |**Sottoscrizione di Azure**|Selezionare la sottoscrizione di Azure nell'elenco a discesa.|
   |**Tipo di app**|Selezionare **App Web per contenitori (Linux)** nell'elenco a discesa.|
   |**Nome del servizio app**|Selezionare l'istanza di Registro Azure Container nell'elenco a discesa.|
   |**Registro o spazio dei nomi**|Immettere il nome del Registro Azure Container nel campo, ad esempio *mymicroprofileregistry.azure.io*.
   |**Repository**|Immettere il repository che contiene l'immagine Docker.| 
   
   ![Configurare le attività della fase](media/cicd-microprofile/configurestage.png)
   
1. Nel riquadro sinistro selezionare **Deploy War to Azure App Service** (Distribuisci War in Servizio app di Azure) e nel riquadro destro immettere il tag *più recente* nel campo **Tag**. 
   
1. Nel riquadro sinistro selezionare **Esegui su agente** e quindi nel riquadro destro selezionare **Ubuntu 1604 Ospitato** nell'elenco a discesa **Pool di agenti**. 

## <a name="set-up-environment-variables"></a>Configurare le variabili di ambiente

Aggiungere e definire le variabili di ambiente per la connessione al registro contenitori durante la distribuzione.

1. Selezionare la scheda **Variabili** e quindi fare clic su **Aggiungi** per aggiungere le variabili seguenti per l'URL, il nome utente e la password del registro contenitori. 
   
   |Nome|valore|
   |---|---|
   |*registry.url*|Immettere l'URL del registro contenitori, ad esempio *https:\//mymicroprofileregistry.azure.io*|
   |*registry.username*|Immettere il nome utente per il Registro di sistema.|
   |*registry.password*|Immettere la password per il Registro di sistema. Per motivi di sicurezza, selezionare l'icona a forma di lucchetto per mantenere nascosta la password.|
   
   ![Aggiungere le variabili](media/cicd-microprofile/addvariables.png)
   
1. Nella scheda **Attività** selezionare **Deploy War to Azure App Service** (Distribuisci War in Servizio app di Azure) nel riquadro sinistro. 
   
1. Nel riquadro destro espandere **Impostazioni applicazione e configurazione** e quindi fare clic sui puntini di sospensione **...** accanto al campo **Impostazioni app**.
   
1. Nella finestra popup **Impostazioni app** fare clic su **Aggiungi** per definire e assegnare le variabili delle impostazioni dell'app:
   
   |Nome|valore|
   |---|---|
   |*DOCKER_REGISTRY_SERVER_URL*|*$(registry.url)*|
   |*DOCKER_REGISTRY_SERVER_USERNAME*|*$(registry.username)*|
   |*DOCKER_REGISTRY_SERVER_PASSWORD*|*$(registry.password)*|
   
1. Selezionare **OK**.
   
   ![Aggiungere e impostare le variabili](media/cicd-microprofile/appsettings.png)
   
## <a name="set-up-continuous-deployment"></a>Configurare la distribuzione continua 

Per abilitare la distribuzione continua: 

1. In **Artefatti** nella scheda **Pipeline** selezionare l'icona a forma di fulmine nell'artefatto della compilazione. 
   
1. Nel riquadro destro impostare **Trigger di distribuzione continua** su **Abilitato**.
   
1. Fare clic su **Salva** nell'angolo in alto a destra e quindi di nuovo su **Salva**. 
   
   ![Abilitare il trigger di distribuzione continua](media/cicd-microprofile/setcontinuousdeployment.png)
   
## <a name="deploy-the-java-app"></a>Distribuire l'app Java

Dopo aver abilitato CI/CD, modificando il codice sorgente vengono create ed eseguite automaticamente build e versioni. È anche possibile creare ed eseguire le versioni manualmente, come indicato di seguito:

1. Nella parte superiore destra della pagina della pipeline di versione selezionare **Crea versione**.
   
1. Nella pagina **Crea una nuova versione** selezionare il nome della fase in **Fasi per la modifica di un trigger da automatizzato a manuale**. 
   
1. Selezionare **Create** (Crea). 
   
1. Selezionare il nome della versione, passare il mouse sulla fase o selezionarla e quindi scegliere **Distribuisci**. 
   
## <a name="test-the-java-web-app"></a>Testare l'app Web Java

Dopo aver completato la distribuzione, testare l'app Web. 

1. Creare l'URL dell'app Web dal portale di Azure.
   
   ![App del servizio app nel portale di Azure](media/cicd-microprofile/portalurl.png)
   
1. Immettere l'URL nel Web browser per eseguire l'app. Nella pagina Web verrà visualizzato **Hello Azure!**
   
   ![Pagina dell'app Web Java](media/cicd-microprofile/webapp.png)

