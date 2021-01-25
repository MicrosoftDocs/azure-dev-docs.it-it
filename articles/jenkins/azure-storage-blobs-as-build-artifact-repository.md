---
title: 'Esercitazione: Usare Archiviazione di Azure per gli artefatti di compilazione'
description: Informazioni su come usare il servizio BLOB di Azure come repository di artefatti di compilazione creati da una soluzione di integrazione continua Jenkins.
keywords: jenkins, azure, devops, archiviazione, cicd, artefatti di compilazione
ms.topic: article
ms.date: 01/12/2021
ms.custom: devx-track-jenkins, devx-track-azurecli
ms.openlocfilehash: 0f7f9bd8ca7997064745a5be431e17f4e3d3fc13
ms.sourcegitcommit: 6fbf9e489b194586887a2c11152044be5b3a2b99
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/25/2021
ms.locfileid: "98759523"
---
# <a name="tutorial-use-azure-storage-for-build-artifacts"></a>Esercitazione: Usare Archiviazione di Azure per gli artefatti di compilazione

[!INCLUDE [jenkins-integration-with-azure.md](includes/jenkins-integration-with-azure.md)]

L'articolo illustra come usare l'archiviazione BLOB come archivio di elementi di compilazione creati dalla soluzione di integrazione continua Jenkins o come origine di file scaricabili da usare in un processo di compilazione. Questa soluzione può rivelarsi utile nel caso in cui si codifichi in un ambiente di sviluppo agile (usando Java o altri linguaggi), le compilazioni vengano eseguite in base all'integrazione continuata e sia necessario un archivio per gli artefatti di compilazione, ad esempio per poterli condividere con altri membri dell'organizzazione o clienti oppure per gestire un archivio. Un altro scenario è quando il processo di compilazione stesso richiede altri file, ad esempio dipendenze da scaricare come parte dell'input di compilazione.

## <a name="prerequisites"></a>Prerequisiti

- **Sottoscrizione di Azure**: Se non si ha una sottoscrizione di Azure, [creare un account Azure gratuito](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) prima di iniziare.
- **Server Jenkins**: se non è installato un server Jenkins, [creare un server Jenkins in Azure](./configure-on-linux-vm.md).
- **Interfaccia della riga di comando di Azure**: Installare l'interfaccia della riga di comando di Azure (versione 2.0.67 o successiva) nel server Jenkins.
- **Account di archiviazione di Azure**: Se non è già disponibile, [creare un account di archiviazione](/azure/storage/common/storage-account-create).

## <a name="add-azure-credential-needed-to-execute-azure-cli"></a>Aggiungere le credenziali di Azure necessarie per eseguire l'interfaccia della riga di comando di Azure

1. Passare al portale di Jenkins.

1. Nel menu selezionare **Manage Jenkins** (Gestisci Jenkins).

1. Selezionare **Manage Credentials** (Gestisci credenziali).

1. Selezionare il dominio **global**.

1. Selezionare **Aggiungi credenziali**.

1. Compilare i campi obbligatori nel modo seguente:

    - **Tipologia**: selezionare **Username with password** (Nome utente con password).
    - **Nome utente**: specificare il valore `appId` dell'entità servizio.
    - **Password**: specificare il valore `password` dell'entità servizio.
    - **ID**: specificare un identificatore di credenziali, ad esempio `azuresp`.
    - **Descrizione**: se si vuole, includere una descrizione significativa dell'ambiente.

1. Scegliere **OK** per creare le credenziali.

## <a name="create-a-pipeline-job-to-upload-build-artifacts"></a>Creare un processo della pipeline per caricare gli artefatti della compilazione

La procedura seguente illustra in dettaglio la creazione di un processo della pipeline. Il processo crea diversi file e li carica nell'account di archiviazione usando l'interfaccia della riga di comando di Azure.

1. Nel dashboard di Jenkins selezionare **New Item** (Nuovo elemento).

1. Assegnare al processo il nome **myjob**, selezionare **Pipeline** e quindi scegliere **OK**.

1. Nella sezione **Pipeline** della configurazione del processo selezionare **Pipeline script** (Script pipeline) e incollare il codice seguente in **Script**. Modificare i segnaposto in modo che corrispondano ai valori dell'ambiente.

    ```groovy
    pipeline {
      agent any
      environment {
        AZURE_SUBSCRIPTION_ID='99999999-9999-9999-9999-999999999999'
        AZURE_TENANT_ID='99999999-9999-9999-9999-999999999999'
        AZURE_STORAGE_ACCOUNT='myStorageAccount'
      }
      stages {
        stage('Build') {
          steps {
            sh 'rm -rf *'
            sh 'mkdir text'
            sh 'echo Hello Azure Storage from Jenkins > ./text/hello.txt'
            sh 'date > ./text/date.txt'
          }
    
          post {
            success {
              withCredentials([usernamePassword(credentialsId: 'azuresp', 
                              passwordVariable: 'AZURE_CLIENT_SECRET', 
                              usernameVariable: 'AZURE_CLIENT_ID')]) {
                sh '''
                  echo $container_name
                  # Login to Azure with ServicePrincipal
                  az login --service-principal -u $AZURE_CLIENT_ID -p $AZURE_CLIENT_SECRET -t $AZURE_TENANT_ID
                  # Set default subscription
                  az account set --subscription $AZURE_SUBSCRIPTION_ID
                  # Execute upload to Azure
                  az storage container create --account-name $AZURE_STORAGE_ACCOUNT --name $JOB_NAME --auth-mode login
                  az storage blob upload-batch --destination ${JOB_NAME} --source ./text --auth-mode login
                  # Logout from Azure
                  az logout
                '''
              }
            }
          }
        }
      }
    }
    ```
    
1. Selezionare **Build Now** (Compila) per eseguire **myjob**.

1. Esaminare l'output di console per ottenere informazioni sullo stato. Quando l'azione di post-compilazione carica gli artefatti della compilazione, nella console vengono scritti i messaggi di stato per l'archiviazione di Azure.

1. Se si verifica un errore simile al seguente, significa che è necessario concedere l'accesso a livello di contenitore: `ValidationError: You do not have the required permissions needed to perform this operation.` Se viene visualizzato questo messaggio di errore, vedere gli articoli seguenti per risolvere l'errore:

    - [Scegliere come autorizzare l'accesso ai dati dei BLOB con l'interfaccia della riga di comando di Azure - Archiviazione di Azure](/azure/storage/blobs/authorize-data-operations-cli)
    - [Usare il portale di Azure per assegnare un ruolo di Azure per l'accesso ai dati - Archiviazione di Azure](/azure/storage/common/storage-auth-aad-rbac-portal)

1. Dopo avere completato il processo, esaminare gli artefatti della compilazione aprendo il BLOB pubblico:

    1. Accedere al [portale di Azure](https://portal.azure.com).
    1. Selezionare **Archiviazione**.
    1. Fare clic sul nome dell'account di archiviazione usato per Jenkins.
    1. Selezionare **Contenitori**.
    1. Selezionare il contenitore denominato **myjob** nell'elenco dei BLOB.
    1. Dovrebbero essere visualizzati due file: **hello.txt** e **date.txt**.
    1. Copiare l'URL di uno di questi elementi e incollarlo nel browser. 
    1. Viene visualizzato il file di testo caricato come elemento di compilazione.
    
    **Note**:

    - In Archiviazione di Azure i nomi di contenitori e i nomi di BLOB sono riportati in lettere minuscole (e si applica la distinzione maiuscole/minuscole).

## <a name="create-a-pipeline-job-to-download-from-azure-blob-storage"></a>Creare un processo della pipeline da scaricare dall'archiviazione BLOB di Azure

I passaggi seguenti illustrano come configurare un processo della pipeline per scaricare gli elementi dall'archivio BLOB di Azure.

1. Nella sezione **Pipeline** della configurazione del processo selezionare **Pipeline script** (Script pipeline) e incollare il codice seguente in **Script**. Modificare i segnaposto in modo che corrispondano ai valori dell'ambiente.

    ```groovy
    pipeline {
      agent any
      environment {
        AZURE_SUBSCRIPTION_ID='99999999-9999-9999-9999-999999999999'
        AZURE_TENANT_ID='99999999-9999-9999-9999-999999999999'
        AZURE_STORAGE_ACCOUNT='myStorageAccount'
      }
      stages {
        stage('Build') {
          steps {
            withCredentials([usernamePassword(credentialsId: 'azuresp', 
                            passwordVariable: 'AZURE_CLIENT_SECRET', 
                            usernameVariable: 'AZURE_CLIENT_ID')]) {
              sh '''
                # Login to Azure with ServicePrincipal
                az login --service-principal -u $AZURE_CLIENT_ID -p $AZURE_CLIENT_SECRET -t $AZURE_TENANT_ID
                # Set default subscription
                az account set --subscription $AZURE_SUBSCRIPTION_ID
                # Execute upload to Azure
                az storage blob download --account-name $AZURE_STORAGE_ACCOUNT --container-name myjob --name hello.txt --file ${WORKSPACE}/hello.txt --auth-mode login
                # Logout from Azure
                az logout
              '''   
            }
          }
        }
      }
    }
    ```
    
1. Dopo l'esecuzione di una compilazione, controllare l'output della console della cronologia delle compilazioni. In alternativa, si può anche controllare il percorso di download per verificare se i BLOB previsti sono stati scaricati correttamente.  

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"]
> [Jenkins in Azure](/azure/Jenkins/)
