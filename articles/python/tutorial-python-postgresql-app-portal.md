---
title: "Esercitazione: Distribuire un'app Django con PostgreSQL tramite il portale di Azure"
description: Effettuare il provisioning di un'app Web e di un database PostgreSQL in Azure e distribuire il codice dell'app da GitHub.
ms.devlang: python
ms.topic: tutorial
ms.date: 07/23/2020
ms.custom: devx-track-python
ms.openlocfilehash: 4ad5086e0bc0500a7b7a117142a153e56ee4b481
ms.sourcegitcommit: 5ab6e90e20a87f9a8baea652befc74158a9b6613
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/09/2020
ms.locfileid: "89614286"
---
# <a name="tutorial-deploy-a-django-web-app-with-postgresql-using-the-azure-portal"></a>Esercitazione: Distribuire un'app Web Django con PostgreSQL tramite il portale di Azure

Usando il portale di Azure, è possibile distribuire un'app Web [Django](https://www.djangoproject.com/) Python basata sui dati nel [servizio app di Azure](/azure/app-service/overview#app-service-on-linux) e connetterla a un database di [Database di Azure per PostgreSQL](/azure/postgresql/). È possibile iniziare con un piano tariffario gratuito che può essere aggiornato in un secondo momento.

Il codice dell'app Web in questo caso deriva da un repository GitHub e l'app Web viene configurata per la distribuzione continua da GitHub. Dopo la configurazione, è possibile svilupparla ulteriormente nel computer locale ed eseguire il commit delle modifiche nel repository. L'app Web in Azure distribuisce quindi automaticamente queste modifiche.

In questa esercitazione si usa il portale di Azure per completare le attività seguenti:

> [!div class="checklist"]
> - Effettuare il provisioning di un'app Web in Azure distribuita da un repository GitHub
> - Effettuare il provisioning di un server e di un database PostgreSQL in Azure e connetterli all'app Web.
> - Aggiornare il codice ed eseguire il commit delle modifiche per la ridistribuzione automatica da GitHub.
> - Visualizzare i log di diagnostica
> - Gestire l'app Web nel portale di Azure

È anche possibile usare la **[versione di questa esercitazione basata sull'interfaccia della riga di comando di Azure](/azure/app-service/tutorial-python-postgresql-app)** .

## <a name="fork-the-sample-repository"></a>Creare una copia tramite fork del repository di esempi

In un browser passare a [https://github.com/Azure-Samples/djangoapp](https://github.com/Azure-Samples/djangoapp) e creare una copia tramite fork del repository nel proprio account GitHub.

È possibile creare una copia tramite fork di questo repository in modo da poter apportare modifiche e ridistribuire il codice in un passaggio successivo.

**(Facoltativo) Informazioni sull'esempio:** l'esempio djangoapp contiene l'app di sondaggi Django basata sui dati che si ottiene seguendo l'esercitazione [Scrivere la prima app Django](https://docs.djangoproject.com/en/2.1/intro/tutorial01/) nella documentazione di Django. L'esempio viene anche modificato usando l'[elenco di controllo della distribuzione Django](https://docs.djangoproject.com/en/2.1/howto/deployment/checklist/) per l'esecuzione in un ambiente di produzione come servizio app di Azure. Queste modifiche sono destinate a qualsiasi ambiente di produzione e non sono specifiche di Azure.

- Le impostazioni di produzione si trovano nel file *azuresite/production.py*. I dettagli di sviluppo si trovano in *azuresite/settings.py*.

- L'app usa le impostazioni di produzione quando la variabile di ambiente `DJANGO_ENV` è impostata su "production". Questa variabile di ambiente viene creata più avanti nell'esercitazione insieme ad altre usate per la configurazione del database PostgreSQL.

[Problemi? Segnalarli](https://aka.ms/DjangoPortalTutorialHelp).

## <a name="provision-the-web-app-in-azure"></a>Effettuare il provisioning dell'app Web in Azure

1. Aprire il [portale di Azure](https://portal.azure.com)

1. Selezionare **Crea una risorsa**.

1. Nella pagina **Nuovo** selezionare **App Web** nella colonna **Più comuni**. Se l'opzione **App Web** non viene visualizzata immediatamente, selezionare **Web** in **Azure Marketplace**, quindi selezionare **App Web** in **In primo piano**. 

1. Nella pagina **Crea app Web** immettere le informazioni seguenti:

    | Campo | Valore |
    | --- | --- |
    | Subscription | Selezionare la sottoscrizione da usare, se diversa da quella predefinita. |
    | Resource group | Selezionare **Crea nuovo** e immettere "DjangoPostgres-Tutorial-rg". |
    | Nome app | Un nome per l'app Web univoco in tutto Azure. L'URL dell'app è `https://\<app-name>.azurewebsites.net`. I caratteri consentiti sono `A`-`Z`, `0`-`9` e `-`. Un criterio valido consiste nell'usare una combinazione del nome della società e di un identificatore dell'app. |
    | Pubblica | Selezionare **Codice**. |
    | Stack di runtime | Selezionare **Python 3.8** nell'elenco a discesa. |
    | Region | Selezionare una località nelle vicinanze. |
    | Piano Linux | Il portale popola questo campo con un nome di piano del servizio app in base al gruppo di risorse. Se si vuole cambiare il nome, selezionare **Crea nuovo**. |
    | SKU e dimensioni | Per ottenere prestazioni ottimali, usare il piano predefinito, anche se implica addebiti nella sottoscrizione. Per evitare addebiti, selezionare **Modifica dimensioni**, quindi **Sviluppo/test**, **B1** (gratuito per 30 giorni) e infine **Applica**. È possibile aggiornare il piano in seguito per migliorare le prestazioni. |

1. Selezionare **Rivedi e crea** e quindi **Crea**. Il provisioning dell'app Web in Azure richiede alcuni minuti.

1. Al termine del provisioning, selezionare **Vai alla risorsa** per aprire la pagina di panoramica per l'app Web. Lasciare aperta la finestra o la scheda del browser per i passaggi successivi.

[Problemi? Segnalarli](https://aka.ms/DjangoPortalTutorialHelp).

## <a name="provision-the-postgresql-database-server-in-azure"></a>Effettuare il provisioning del server di database PostgreSQL in Azure

1. Aprire una nuova finestra del browser con il [portale di Azure](https://portal.azure.com). Si usa una nuova scheda per il provisioning del database perché è necessario trasferire alcune informazioni dalla pagina del database alla pagina dell'app Web ancora aperta dalla sezione precedente.

1. Selezionare **Crea una risorsa**.

1. Nella pagina **Nuovo** selezionare **Database** in **Azure Marketplace**, quindi selezionare **Database di Azure per PostgreSQL** in **In primo piano**.

1. Nella pagina successiva selezionare **Crea** in **Server singolo**.

1. Nella pagina **Server singolo** immettere le informazioni seguenti:

    | Campo | Valore |
    | --- | --- |
    | Subscription | Selezionare la sottoscrizione da usare, se diversa da quella predefinita. |
    | Resource group | Selezionare il gruppo "DjangoPostgres-Tutorial-rg" creato nella sezione precedente. |
    | Nome server | Un nome per il server di database univoco in tutto Azure. L'URL dell'app è `https://\<server-name>.postgres.database.azure.com`. I caratteri consentiti sono `A`-`Z`, `0`-`9` e `-`. Un criterio valido consiste nell'usare una combinazione del nome della società e di un identificatore del server. |
    | Origine dati | **Nessuno** |
    | Location | Selezionare una località nelle vicinanze. |
    | Versione | Mantenere il valore predefinito, ovvero l'ultima versione. |
    | Calcolo e archiviazione | Selezionare **Configura server**, quindi selezionare **Basic** e **Generazione 5**. Impostare **vCore** su 1 e **Archiviazione** su 5 GB, quindi selezionare **OK**. Queste scelte consentono di effettuare il provisioning del server meno costoso disponibile per PostgreSQL in Azure. |
    | Nome utente amministratore, Password, Conferma password | Immettere le credenziali per un account amministratore nel server di database. Prendere nota di queste credenziali, perché saranno necessarie più avanti nell'esercitazione. |

1. Selezionare **Rivedi e crea** e quindi **Crea**. Il provisioning dell'app Web in Azure richiede alcuni minuti.

1. Al termine del provisioning, selezionare **Vai alla risorsa** per aprire la pagina di panoramica per il server di database.

[Problemi? Segnalarli](https://aka.ms/DjangoPortalTutorialHelp).

## <a name="create-the-pollsdb-database-on-the-postgresql-server"></a>Creare il database pollsdb nel server PostgreSQL

In questa sezione ci si connette al server di database in Azure Cloud Shell e si usa un comando PostgreSQL per creare un database "pollsdb" al suo interno. Questo database è previsto dal codice dell'app di esempio.

1. Nella pagina di panoramica del server PostgreSQL selezionare **Sicurezza connessione** in **Impostazioni** sul lato sinistro.

    ![Pagina Sicurezza connessione portale per le regole del firewall](media/tutorial-python-postgresql-app-portal/server-firewall-rules.png)

1. Selezionare il pulsante **Aggiungi 0.0.0.0 - 255.255.255.255**, quindi selezionare **Continua** nel messaggio popup visualizzato e poi **Salva** nella parte superiore della pagina. Queste azioni aggiungono una regola che consente di connettersi al server di database da Cloud Shell e da SSH (come si farà in una sezione successiva per eseguire le migrazioni dei modelli di dati Django).

1. Aprire Azure Cloud Shell dal portale di Azure selezionando l'icona corrispondente nella parte superiore della finestra:

    ![Pulsante Cloud Shell sulla barra degli strumenti del portale di Azure](media/tutorial-python-postgresql-app-portal/portal-launch-icon.png)

1. In Cloud Shell eseguire il comando seguente:

    ```bash
    psql --host=<server-name>.postgres.database.azure.com --port=5432 --username=<user-name>@<server-name> --dbname=postgres
    ```

    Sostituire `<server-name>` e `<user-name>` con i nomi usati nella sezione precedente durante la configurazione del server. Si noti che il valore del nome utente completo è `<user-name>@<server-name>`.

    È possibile copiare il comando precedente e incollarlo in Cloud Shell facendo clic con il pulsante destro del mouse e quindi scegliendo **Incolla**.

    Quando richiesto, immettere la password di amministratore.

1. Quando la shell si connette correttamente, verrà visualizzato il messaggio `postgres=>`. Questo messaggio indica che è stata eseguita la connessione al database amministrativo predefinito denominato "postgres". Il database "postgres" non è destinato all'utilizzo dell'app.

1. Al prompt eseguire il comando `CREATE DATABASE pollsdb;`. Assicurarsi di includere il punto e virgola finale, che completa il comando.

1. Se il database viene creato correttamente, il comando dovrà visualizzare `CREATE DATABASE`. Per verificare se il database è stato creato, eseguire `\c pollsdb`. Con questo comando il messaggio cambia in `pollsdb=>`, che indica l'esito positivo.

1. Uscire da psql eseguendo il comando `exit`.

[Problemi? Segnalarli](https://aka.ms/DjangoPortalTutorialHelp).

## <a name="connect-the-database"></a>Connettere il database

In questa sezione vengono create le impostazioni per l'app Web necessarie per la connessione al database `pollsdb`. Queste impostazioni vengono visualizzate come variabili di ambiente nel codice dell'app. Per altre informazioni, vedere [Accedere alle variabili di ambiente](/azure/app-service/configure-language-python#access-environment-variables).

1. Tornare nella scheda o nella finestra del browser per l'app Web creata in una sezione precedente.

1. Selezionare **Configurazione** in **Impostazioni** sul lato sinistro, quindi selezionare **Impostazioni applicazione** nella parte superiore della pagina.

    ![Configurazione delle impostazioni del portale per le app Web](media/tutorial-python-postgresql-app-portal/web-app-settings.png)

1. Usare il pulsante **Nuova impostazione applicazione** per creare le impostazioni per ognuno dei valori seguenti:

    | Nome impostazione | valore |
    | --- | --- |
    | DJANGO_ENV | `production` (Questo valore indica all'app di usare una configurazione di produzione, come descritto in precedenza nella [panoramica dell'esempio](#fork-the-sample-repository).) |
    | DBHOST | L'URL del server di database della sezione precedente, nel formato `<server-name>.postgres.database.azure.com`. È possibile copiare l'intero URL dalla pagina di panoramica del server di database. |
    | DBNAME | `pollsdb` |
    | DBUSER | Il nome utente completo dell'amministratore, usato nella sezione precedente. Il nome utente completo è di nuovo `<user-name>@<server-name>`. |
    | DBPASS | La password di amministratore creata in precedenza. |

1. Selezionare **Salva** quindi **Continuare** per applicare le impostazioni.

    > [!IMPORTANT]
    > È essenziale selezionare **Salva** dopo aver apportato modifiche alle impostazioni. Tutte le impostazioni create con il pulsante **Nuova impostazione applicazione** vengono applicate solo quando si seleziona **Salva**.

[Problemi? Segnalarli](https://aka.ms/DjangoPortalTutorialHelp).

## <a name="deploy-app-code-to-the-web-app-from-a-repository"></a>Distribuire il codice dell'app nell'app Web da un repository

Una volta specificate le impostazioni del database e della connessione, è ora possibile configurare l'app Web per distribuire il codice direttamente dal repository GitHub.

1. Nella finestra o nella scheda del browser per l'app Web selezionare **Centro distribuzione** in **Distribuzione** sul lato sinistro.

1. Nel passaggio **Controllo del codice sorgente** selezionare **GitHub**, quindi seguire le istruzioni per l'accesso oppure selezionare **Continua** per usare l'account di accesso di GitHub corrente.

1. Nel passaggio **Provider di compilazione** selezionare **Servizio di compilazione del servizio app**, quindi selezionare **Continua**.

1. Nel passaggio **Configura** selezionare i valori seguenti:

    | Campo | Valore |
    | --- | --- |
    | Organization | L'account GitHub in cui è stata creata una copia tramite fork del repository di esempio. |
    | Archivio | djangoapp |
    | Ramo | master |

1. Selezionare **Continua** per selezionare il repository, quindi selezionare **Fine**. Entro pochi secondi, Azure distribuisce il codice e avvia l'app.

    Il servizio app rileva un progetto Django cercando un file *wsgi.py* in ogni sottocartella. Quando trova tale file, il servizio app carica l'app Web Django. Per altre informazioni, vedere [Configurare l'immagine Python predefinita](/azure/app-service/configure-language-python).

[Problemi? Segnalarli](https://aka.ms/DjangoPortalTutorialHelp).

## <a name="run-django-database-migrations"></a>Eseguire le migrazioni del database Django

Con il codice distribuito e il database implementato, l'app è quasi pronta per l'uso. Rimane solo da stabilire lo schema necessario nel database stesso. A questo scopo, eseguire la migrazione dei modelli di dati dell'app Django al database.

1. Nella finestra o nella scheda del browser per l'app Web selezionare **SSH** in **Strumenti di sviluppo** sul lato sinistro e quindi fare clic su **Go** per aprire una console SSH nel server dell'app Web. La prima connessione potrebbe richiedere un minuto, perché è necessario avviare il contenitore dell'app Web.

1. Nella console passare alla cartella dell'app Web:

    ```bash
    cd site/wwwroot
    ```

1. Attivare l'ambiente virtuale del contenitore:

    ```bash
    source /antenv/bin/activate
    ```

1. Installare i pacchetti Python:

    ```bash
    pip install -r requirements.txt
    ```

1. Eseguire le migrazioni del database:

    ```bash
    python manage.py migrate
    ```

1. Creare un account di accesso dell'amministratore per l'app:

    ```bash
    python manage.py createsuperuser
   ```

    Il comando `createsuperuser` richiede le credenziali di utente con privilegi avanzati (o amministratore) di Django, che vengono usate nell'app Web. Ai fini di questa esercitazione, usare il nome utente predefinito `root`, premere **INVIO** per l'indirizzo di posta elettronica per lasciarlo vuoto e immettere `Pollsdb1` per la password.

[Problemi? Segnalarli](https://aka.ms/DjangoPortalTutorialHelp).

### <a name="create-a-poll-question-in-the-app"></a>Creare una domanda del sondaggio nell'app

A questo punto è possibile eseguire un rapido test dell'app per verificare che funzioni con il database PostgreSQL.

1. Nella finestra o nella scheda del browser per l'app Web tornare alla pagina **Panoramica**, quindi selezionare l'**URL** per l'app Web nel formato `http:\//\<app-name>.azurewebsites.net`.

1. L'app dovrebbe visualizzare un messaggio che indica che non sono disponibili sondaggi perché nel database non sono ancora presenti sondaggi specifici.

1. Passare a `http:\//\<app-name>.azurewebsites.net/admin`, la pagina di amministrazione di Django, e accedere usando le credenziali di utente con privilegi avanzati della sezione precedente (`root` e `Pollsdb1`).

1. In **Polls** (Sondaggi) selezionare **Add** (Aggiungi) accanto a **Questions** (Domande) e creare una domanda del sondaggio con alcune scelte.

1. Passare di nuovo a `http:\//\<app-name>.azurewebsites.net/` per verificare se le domande ora vengono presentate all'utente. Rispondere alle domande come desiderato per generare alcuni dati nel database.

**Congratulazioni** Un'app Web Python Django è ora in esecuzione nel servizio app di Azure per Linux, con un database PostgreSQL attivo.

[Problemi? Segnalarli](https://aka.ms/DjangoPortalTutorialHelp).

## <a name="update-the-app-and-redeploy"></a>Aggiornare e ridistribuire l'app

Come descritto in precedenza in questa esercitazione, Azure ridistribuisce il codice dell'app ogni volta che si esegue il commit delle modifiche nel repository GitHub.

Se si cambiano i modelli di dati dell'app Django, tuttavia, è necessario eseguire la migrazione di tali modifiche al database:

1. Connettersi di nuovo all'app Web tramite SSH, come descritto in [Eseguire le migrazioni del database Django](#run-django-database-migrations).

1. Passare alla cartella dell'app con `cd site/wwwroot`.

1. Attivare l'ambiente virtuale con `source /antenv/bin/activate`.

1. Eseguire di nuovo le migrazioni con `python manage.py migrate`.

[Problemi? Segnalarli](https://aka.ms/DjangoPortalTutorialHelp).

## <a name="view-diagnostic-logs"></a>Visualizzare i log di diagnostica

È possibile accedere ai log della console generati dall'interno del contenitore che ospita l'app in Azure.

Nella pagina dell'app Web nel portale di Azure selezionare **Flusso di registrazione** in **Monitoraggio** sul lato sinistro. I log vengono visualizzati come output della console.

È anche possibile esaminare i file di log nel browser all'indirizzo `https://<app-name>.scm.azurewebsites.net/api/logs/docker`.

[Problemi? Segnalarli](https://aka.ms/DjangoPortalTutorialHelp).

## <a name="clean-up-resources"></a>Pulire le risorse

Per continuare a eseguire attività di sviluppo, è possibile lasciare l'app e il database in esecuzione. In caso contrario, per evitare di incorrere in addebiti ricorrenti, eliminare il gruppo di risorse creato per questa esercitazione, rimuovendo in questo modo tutte le risorse al suo interno:

1. Nel portale di Azure immettere "DjangoPostgres-Tutorial-rg" nella barra di ricerca nella parte superiore della finestra, quindi selezionare lo stesso nome in **Gruppi di risorse**.

1. Nella pagina del gruppo di risorse selezionare **Elimina gruppo di risorse**.

1. Quando richiesto, immettere il nome del gruppo di risorse e selezionare **Elimina**.

[Problemi? Segnalarli](https://aka.ms/DjangoPortalTutorialHelp).

## <a name="next-steps"></a>Passaggi successivi

Vedere le informazioni sul modo in cui il servizio app esegue un'app Python:

> [!div class="nextstepaction"]
> [Configurare un'app Python](/azure/app-service/configure-language-python)
