---
title: 'Esercitazione: Usare Archiviazione di Azure per gli artefatti di compilazione'
description: Informazioni su come usare il servizio BLOB di Azure come repository di artefatti di compilazione creati da una soluzione di integrazione continua Jenkins.
keywords: jenkins, azure, devops, archiviazione, cicd, artefatti di compilazione
ms.topic: article
ms.date: 11/19/2020
ms.custom: devx-track-jenkins
ms.openlocfilehash: 22ce5c0ee00af64bd5bf8dc43f0df1ad1b5e2400
ms.sourcegitcommit: 4dac39849ba2e48034ecc91ef578d11aab796e58
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/20/2020
ms.locfileid: "96035399"
---
# <a name="tutorial-use-azure-storage-for-build-artifacts"></a>Esercitazione: Usare Archiviazione di Azure per gli artefatti di compilazione

[!INCLUDE [jenkins-integration-with-azure.md](includes/jenkins-integration-with-azure.md)]

L'articolo illustra come usare l'archiviazione BLOB come archivio di elementi di compilazione creati dalla soluzione di integrazione continua Jenkins o come origine di file scaricabili da usare in un processo di compilazione. Questa soluzione può rivelarsi utile nel caso in cui si codifichi in un ambiente di sviluppo agile (usando Java o altri linguaggi), le compilazioni vengano eseguite in base all'integrazione continuata e sia necessario un archivio per gli artefatti di compilazione, ad esempio per poterli condividere con altri membri dell'organizzazione o clienti oppure per gestire un archivio. Un altro scenario è quando il processo di compilazione stesso richiede altri file, ad esempio dipendenze da scaricare come parte dell'input di compilazione.

In questa esercitazione si userà il plug-in di Archiviazione di Azure per l'Integrazione continua Jenkins reso disponibile da Microsoft.

## <a name="jenkins-overview"></a>Panoramica di Jenkins

Jenkins abilita l'integrazione continuata di un progetto software consentendo agli sviluppatori di integrare le modifiche apportate al codice e produrre compilazioni automaticamente. Questo criterio riduce i problemi e aumenta la produttività del team. Alle compilazioni è applicato il controllo delle versioni ed è possibile caricare gli elementi di compilazione in archivi distinti. Questo articolo illustra come usare l'archivio BLOB di Azure come archivio per gli artefatti di compilazione. Si apprenderà anche come scaricare le dipendenze dall'archivio BLOB di Azure.

Per ulteriori informazioni su Jenkins, vedere [Meet Jenkins](https://wiki.jenkins-ci.org/display/JENKINS/Meet+Jenkins).

## <a name="benefits-of-using-the-blob-service"></a>Vantaggi dell'uso del servizio BLOB

Di seguito sono indicati i vantaggi dell'utilizzo del servizio BLOB per ospitare gli elementi di compilazione prodotti dallo sviluppo Agile:

* Disponibilità elevata degli elementi di compilazione o delle dipendenze scaricabili.
* Migliori prestazioni nel caricamento degli elementi di compilazione da parte della soluzione di Integrazione continuata Jenkins.
* Migliori prestazioni durante il download degli elementi di compilazione da parte di clienti e partner.
* Controllo dei criteri di accesso da parte dell'utente, con opzioni quali accesso anonimo, accesso condiviso con scadenza, accesso con firma e accesso privato.

## <a name="prerequisites"></a>Prerequisiti

- **Sottoscrizione di Azure**: Se non si ha una sottoscrizione di Azure, [creare un account Azure gratuito](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) prima di iniziare.
- **Server Jenkins**: se non è installato un server Jenkins, [creare un server Jenkins in Azure](./configure-on-linux-vm.md).
- **Account di archiviazione di Azure**: Se non è già disponibile, [creare un account di archiviazione](/azure/storage/common/storage-account-create).

## <a name="configure-your-environment"></a>Configurare l'ambiente

1. Se si è sprovvisti di una soluzione di Integrazione continuata Jenkins è possibile eseguire una soluzione di Integrazione continuata Jenkins applicando la tecnica seguente:
  
  1. In un computer in cui è abilitato Java scaricare il file jenkins.war dall'indirizzo <https://jenkins-ci.org>.
  2. Al prompt dei comandi aperto nella cartella che contiene jenkins.war eseguire:
     
      `java -jar jenkins.war`

  3. Passare a `http://localhost:8080/` per aprire il dashboard di Jenkins. Usare il dashboard di Jenkins per installare e configurare il plug-in di Archiviazione di Azure.
     
## <a name="how-to-use-the-blob-service-with-jenkins-ci"></a>Come usare il servizio BLOB con l'Integrazione continuata Jenkins

Per usare il servizio BLOB con Jenkins, è necessario installare il plug-in di Archiviazione di Azure, configurare il plug-in per usare l'account di archiviazione e creare un'operazione post-compilazione per il caricamento degli elementi di compilazione nell'account di archiviazione. Questi passaggi vengono descritti nelle sezioni seguenti.

## <a name="how-to-install-the-azure-storage-plugin"></a>Come installare il plug-in di Archiviazione di Azure

1. Nel dashboard di Jenkins selezionare **Manage Jenkins** (Gestisci Jenkins).
2. Nella pagina **Manage Jenkins** (Gestisci Jenkins) selezionare **Manage Plugins** (Gestisci plug-in).
3. Selezionare la scheda **Available**.
4. Nella sezione **Artifact Uploaders** selezionare **Microsoft Azure Storage plugin**.
5. Fare clic su **Install without restart** (Installa senza riavviare) oppure su **Download now and install after restart** (Scarica subito e installa dopo il riavvio).
6. Riavviare Jenkins.

## <a name="how-to-configure-the-azure-storage-plugin-to-use-your-storage-account"></a>Come configurare il plug-in di Archiviazione di Azure per usare l'account di archiviazione

1. Nel dashboard di Jenkins selezionare **Manage Jenkins** (Gestisci Jenkins).
2. Nella pagina **Manage Jenkins** (Gestisci Jenkins) selezionare **Configure System** (Configura sistema).
3. Nella sezione **Microsoft Azure Storage Account Configuration** :
   1. Immettere il nome dell'account di archiviazione, che è possibile ottenere dal [portale di Azure](https://portal.azure.com).
   2. Immettere la chiave dell'account di archiviazione, che è possibile ottenere dal [portale di Azure](https://portal.azure.com).
   3. Se si usa il servizio Cloud di Azure globale, immettere il valore predefinito in **Blob Service Endpoint URL**. Se si usa un cloud di Azure diverso, usare l'endpoint specificato nel [portale di Azure](https://portal.azure.com) per l'account di archiviazione. 
   4. Fare clic su **Validate storage credentials** (Convalida credenziali di archiviazione) per convalidare l'account di archiviazione. 
   5. [Facoltativo] Se si dispone di account di archiviazione aggiuntivi che si intende rendere disponibili all'Integrazione continua Jenkins, fare clic su **Add more Storage Accounts** (Aggiungi altri account di archiviazione).
   6. Per salvare le impostazioni, fare clic su **Save** (Salva).

## <a name="how-to-create-a-post-build-action-that-uploads-your-build-artifacts-to-your-storage-account"></a>Creazione di un'operazione post-compilazione per il caricamento degli elementi di compilazione nell'account di archiviazione

Ai fini di questa esercitazione, è necessario innanzitutto creare un processo che crei più file e quindi aggiungere l'operazione post-compilazione per caricare i file nell'account di archiviazione.

1. Nel dashboard di Jenkins selezionare **New Item** (Nuovo elemento).
2. Denominare il processo **MyJob**, fare clic su **Build a free-style software project** (Compila un progetto software libero) e quindi fare clic su **OK**.
3. Nella sezione **Build** della configurazione del processo fare clic su **Add build step** (Aggiungi passaggio di compilazione) e scegliere **Execute Windows batch command** (Esegui comando batch di Windows).
4. Nella sezione **Command** usare i comandi seguenti:

    ```   
    md text
    cd text
    echo Hello Azure Storage from Jenkins > hello.txt
    date /t > date.txt
    time /t >> date.txt
    ```

5. Nella sezione **Post-build Actions** (Azioni di post-compilazione) della configurazione del processo fare clic su **Add post-build action** (Aggiungi azione di post-compilazione) e scegliere **Upload artifacts to Azure Blob storage** (Carica artefatti su archiviazione BLOB di Azure).
6. In **Storage Account Name** scegliere l'account di archiviazione da utilizzare.
7. In **Container Name** specificare il nome del contenitore. Il contenitore verrà creato se non esiste già al momento del caricamento degli elementi di compilazione. È possibile usare variabili di ambiente, pertanto in questo esempio immettere `${JOB_NAME}` come nome del contenitore.
   
    > [!TIP]
    > Sotto la sezione **Command** in cui è stato immesso uno script per **Execute Windows batch command** è presente un collegamento alle variabili di ambiente riconosciute da Jenkins. Fare clic sul collegamento per ottenere dettagli sui nomi e le descrizioni delle variabili di ambiente. Le variabili di ambiente che contengono caratteri speciali, ad esempio la variabile di ambiente **BUILD_URL** non sono ammesse come nome di contenitore o come percorso virtuale comune.
    
8. Ai fini di questo esempio, fare clic su **Make new container public by default** (Rendi pubblico questo contenitore per impostazione predefinita). Se si intende usare un contenitore privato, è necessario creare una firma di accesso condiviso per consentire l'accesso, che esula dall'ambito di questo articolo. Per altre informazioni sulle firme di accesso condiviso, vedere [Uso delle firme di accesso condiviso](/azure/storage/common/storage-sas-overview).
9. [Facoltativo] Fare clic su **Clean container before uploading** (Pulisci contenitore prima del caricamento) se si vuole cancellare i contenuti dal contenitore prima di caricare gli artefatti di compilazione (lasciare questa casella deselezionata se non si vuole pulire i contenuti del contenitore).
10. In **List of Artifacts to upload** (Elenco di artefatti da caricare) immettere `text/*.txt`.
11. In **Common virtual path for uploaded artifacts** (Percorso virtuale comune per gli artefatti caricati), ai fini di questa esercitazione, immettere `${BUILD\_ID}/${BUILD\_NUMBER}`.
12. Per salvare le impostazioni, fare clic su **Save** (Salva).
13. Nel dashboard di Jenkins fare clic su **Build Now** (Compila ora) per eseguire **MyJob**. Esaminare l'output di console per ottenere informazioni sullo stato. I messaggi di stato per il servizio di archiviazione di Azure verranno inclusi nell'output della console quando l'operazione post-compilazione avvierà il caricamento degli elementi di compilazione.
14. Dopo avere completato il processo, è possibile esaminare gli elementi di compilazione aprendo il BLOB pubblico.
    1. Accedere al [portale di Azure](https://portal.azure.com).
    2. Selezionare **Archiviazione**.
    3. Fare clic sul nome dell'account di archiviazione usato per Jenkins.
    4. Selezionare **Contenitori**.
    5. Fare clic sul contenitore denominato **myjob**, ossia la versione in lettere minuscole del nome processo assegnato al momento della creazione del processo Jenkins. In Archiviazione di Azure i nomi di contenitori e i nomi di BLOB sono riportati in lettere minuscole (e si applica la distinzione maiuscole/minuscole). Nell'elenco di BLOB per il contenitore denominato **myjob** dovrebbero essere inclusi **hello.txt** e **date.txt**. Copiare l'URL di uno di questi elementi e aprirlo nel browser. Viene visualizzato il file di testo caricato come elemento di compilazione.

È possibile creare solo un'azione post-compilazione per il caricamento di elementi nell'archivio BLOB di Azure per ogni processo. La singola azione post-compilazione per il caricamento di elementi nell'archivio BLOB di Azure può specificare file (inclusi caratteri jolly) e percorsi di file diversi in **List of Artifacts to upload** (Elenco di elementi da caricare) usando un punto e virgola come separatore. Ad esempio, se la compilazione Jenkins crea file con estensione jar e txt nella cartella **build** dell'area di lavoro dell'utente e si intende caricarli entrambi nell'archiviazione BLOB di Azure, usare quanto segue per il valore **List of Artifacts to upload** (Elenco di artefatti da caricare): `build/\*.jar;build/\*.txt`. Per specificare il percorso da usare nel nome del BLOB, è anche possibile usare la sintassi con i due punti. Ad esempio, se si vuole caricare i file con estensione jar usando **binaries** nel percorso del BLOB e i file con estensione txt usando **notices** nel percorso del BLOB, immettere quanto segue per il valore **List of Artifacts to upload** (Elenco di artefatti da caricare): `build/\*.jar::binaries;build/\*.txt::notices`.

## <a name="how-to-create-a-build-step-that-downloads-from-azure-blob-storage"></a>Come creare un passaggio di compilazione per il download di elementi dall'archivio BLOB di Azure

La procedura seguente illustra come configurare un passaggio di compilazione per scaricare elementi dall'archiviazione BLOB di Azure, operazione utile se si vuole includere elementi nella build. Un esempio di uso di questo criterio sono i file con estensione jar che è possibile salvare in modo permanente in archiviazione BLOB di Azure.

1. Nella sezione **Build** (Compilazione) della configurazione del processo fare clic su **Add build step** (Aggiungi un passaggio di compilazione) e scegliere **Download from Azure Blob storage** (Scarica dallo spazio di archiviazione BLOB di Azure).
2. In **Storage Account Name** scegliere l'account di archiviazione da utilizzare.
3. In **Container name** specificare il nome del contenitore in cui si trovano i BLOB da scaricare. A questo scopo, è possibile usare le variabili di ambiente.
4. In **Blob name** specificare il nome del BLOB. A questo scopo, è possibile usare le variabili di ambiente. È anche possibile usare un asterisco come carattere jolly dopo avere specificato le lettere iniziali del nome del BLOB. Ad esempio, digitando **project\\** _ si specificano tutti i BLOB i cui nomi iniziano per _*project**.
5. [Facoltativo] In **Download path** specificare il percorso nel computer Jenkins in cui si vuole scaricare i file dall'archivio BLOB di Azure. A questo scopo, è anche possibile usare le variabili di ambiente. Se non si specifica un valore per **Download path**, i file dall'archivio BLOB di Azure verranno scaricati nell'area di lavoro del processo.

Per scaricare elementi aggiuntivi dall'archiviazione BLOB di Azure, è possibile creare altri passaggi di compilazione.

Dopo avere eseguito una build, è possibile verificare l'output della cronologia di compilazione della console o accedere al percorso di download per verificare che siano stati scaricati i BLOB previsti.  

## <a name="components-used-by-the-blob-service"></a>Componenti usati dal servizio BLOB

Questa sezione offre una panoramica delle componenti del servizio BLOB.

* **Account di archiviazione**: l'accesso ad Archiviazione di Azure viene eseguito esclusivamente tramite un account di archiviazione. Un account di archiviazione è il livello più alto dello spazio dei nomi per accedere ai BLOB. Un account può contenere un numero illimitato di contenitori, purché la dimensione totale di questi sia inferiore a 100 TB.
* **Contenitore**: Un contenitore fornisce un raggruppamento di un set di BLOB. Tutti i BLOB devono essere inclusi in un contenitore. Un account può contenere un numero illimitato di contenitori. In un contenitore è possibile archiviare un numero illimitato di BLOB.
* **BLOB**: file di qualsiasi tipo e dimensioni. Esistono due tipi di oggetti BLOB che è possibile archiviare in Archiviazione di Azure: BLOB in blocchi e BLOB di pagine. La maggior parte dei file sono BLOB in blocchi. Un singolo BLOB in blocchi può raggiungere fino a 200 GB di dimensione. In questa esercitazione vengono utilizzati BLOB in blocchi. I BLOB di pagine, di altro tipo, possono raggiungere dimensioni fino a 1 TB e risultano più efficienti quando all'interno di un file vi sono intervalli di byte soggetti a modifiche frequenti. Per altre informazioni sui BLOB, vedere [Informazioni sui BLOB in blocchi, sui BLOB di aggiunta e sui BLOB di pagine](/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs).
* **Formato dell'URL:** è possibile fare riferimento ai BLOB usando il formato di URL seguente:
  
    `http://storageaccount.blob.core.windows.net/container_name/blob_name`
  
**Note**:

- il formato riportato sopra si riferisce al servizio cloud Azure globale. Se si usa un cloud di Azure diverso, usare l'endpoint all'interno del [portale di Azure](https://portal.azure.com) per determinare l'endpoint dell'URL.
- Il segnaposto `storageaccount` rappresenta il nome del proprio account di archiviazione.
- Il segnaposto `container_name` rappresenta il nome del contenitore.
- Il segnaposto `blob_name` rappresenta il nome del BLOB.
- Nel nome contenitore è possibile avere percorsi multipli, separati da una barra. 
- Il nome del contenitore di esempio usato per questa esercitazione è **MyJob**.
- Il percorso virtuale comune viene definito come **${ID\_BUILD}/${NUMERO\_BUILD}** . Questo valore determina l'URL del BLOB con il formato seguente:
  
    `http://example.blob.core.windows.net/myjob/2014-04-14_23-57-00/1/hello.txt`

## <a name="troubleshooting-the-jenkins-plugin"></a>Risoluzione dei problemi del plug-in Jenkins

Se si rilevano bug con i plug-in Jenkins, segnalare un problema in [Jenkins JIRA](https://issues.jenkins-ci.org/) per il componente specifico.

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"]
> [Jenkins in Azure](/azure/Jenkins/)