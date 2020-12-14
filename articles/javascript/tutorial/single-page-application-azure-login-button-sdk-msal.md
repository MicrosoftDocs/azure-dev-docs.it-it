---
title: "Esercitazione: Aggiungere il pulsante di accesso Microsoft all'applicazione a pagina singola React"
description: L'autenticazione di Azure Active Directory presentata in questa esercitazione è un pulsante di accesso e disconnessione e fornisce l'accesso al nome di un utente (indirizzo di posta elettronica). Sviluppare l'applicazione con un SDK lato client di Azure, `@azure/msal-browser`, per gestire l'interazione dell'utente nell'applicazione a pagina singola.
ms.topic: tutorial
ms.date: 12/01/2020
ms.custom: devx-track-js, "azure-sdk-javascript-@azure/msal-browser-2.7.0"
ms.openlocfilehash: a5c07696c6c774408bf2772542234e59f5c0b58c
ms.sourcegitcommit: 09b4a2dbe13601fdf16fcc4082a5075b46ad3459
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/03/2020
ms.locfileid: "96563724"
---
# <a name="add-microsoft-login-button-to-a-single-page-application-for-authentication"></a>Aggiungere un pulsante di accesso Microsoft a un'applicazione a pagina singola per l'autenticazione

L'autenticazione di Azure presentata in questa esercitazione è un pulsante di accesso e disconnessione e fornisce l'accesso all'account di un utente. Sviluppare l'applicazione con un SDK lato client di Azure, `@azure/msal-browser`, per gestire l'interazione dell'utente nell'applicazione a pagina singola.

* [Codice sorgente](https://github.com/Azure-Samples/js-e2e-client-azure-login-button)

## <a name="application-architecture-and-functionality"></a>Architettura e funzionalità dell'applicazione

L'applicazione a pagina singola creata in questa esercitazione è un'app React (create-react-app) con le attività seguenti:

- Eseguire l'accesso usando un accesso supportato da Microsoft, ad esempio Office 365 oppure Outlook.com
- Disconnettersi dall'applicazione

Per fornire un'applicazione a pagina singola rapida e semplice, l'esempio usa **create-react-app** con TypeScript. Questo framework front-end offre diverse scelte rapide da tastiera nella distribuzione client tipica con SDK di Azure:

- Creazione di bundle, necessaria per SDK di Azure usati in un'applicazione client
- Variabili di ambiente nel file `.env`

## <a name="1-set-up-development-environment"></a>1. Configurare l'ambiente di sviluppo

Verificare che gli elementi seguenti siano installati nel computer locale.

- Un account utente di Azure con una sottoscrizione attiva. [È possibile crearne uno gratuitamente](https://azure.microsoft.com/free/).
- [Node.js e npm](https://nodejs.org/en/download): installati nel computer locale.
- [Visual Studio Code](https://code.visualstudio.com/) o un ambiente di sviluppo integrato equivalente con shell Bash o terminale: installato nel computer locale. 

## <a name="2-keep-value-for-environment-variable"></a>2. Mantenere il valore per la variabile di ambiente

Definire una posizione per la copia del valore dell'ID client dell'app. 

## <a name="3-create-app-registration-for-authentication"></a>3. Creare una registrazione app per l'autenticazione

1. **Accedere** al [portale di Azure](https://portal.azure.com/?quickstart=True#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredApps) per le registrazioni app della directory predefinita.
1. Selezionare **+ Nuova registrazione**.
1. **Immettere i dati di registrazione dell'app** usando la tabella seguente:

   | Campo                   | valore                                                                                                                                                                      |Descrizione|
   | ----------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |--|
   | Nome                    | `Simple Auth Tutorial`|Questo è il nome dell'app che verrà visualizzato agli utenti nel modulo di autorizzazione quando eseguono l'accesso all'app.                                                 |
   | Tipi di account supportati | **Gli account presenti in qualsiasi directory dell'organizzazione (qualsiasi directory Azure AD - Multi-tenant) e gli account Microsoft personali**|Include la maggior parte dei tipi di account. |
   | Tipo di URI di reindirizzamento           | **Applicazione a singola pagina (SPA)**                                                                                        | |
   | Valore dell'URI di reindirizzamento           | `http://localhost:3000` | URL a cui tornare dopo l'esito positivo o negativo dell'autenticazione.                                                                                        |

    :::image type="content" source="../media/tutorial-login-button/azure-active-directory-create-new-app-registration.png" alt-text="Nuova registrazione app di Azure.":::  

1. Selezionare **Registra**. Attendere il completamento del processo di registrazione.
1. **Copiare l'ID (client) applicazione** dalla sezione di panoramica della registrazione dell'app. Questo valore verrà aggiunto alla variabile di ambiente per l'app client in un secondo momento.

## <a name="4-create-react-single-page-application-for-typescript"></a>4. Creare l'applicazione a pagina singola React per TypeScript

1. In una shell Bash **creare create-react-app** usando TypeScript come linguaggio:

   ```bash
   npx create-react-app tutorial-demo-login-button --template typescript
   ```

1. Passare alla nuova directory e installare il pacchetto `@azure/msal-browser`:

   ```bash
   cd tutorial-demo-login-button && npm install @azure/msal-browser
   ```

1. Creare un `.env` a livello di file radice e aggiungere le righe seguenti:

    :::code language="env" source="~/../js-e2e-client-azure-login-button/.env"  :::

    Il file `.env` viene letto come parte del framework create-react-app.

1. Copiare l'ID (client) applicazione nel secondo valore.

## <a name="5-add-login-and-logoff-buttons"></a>5. Aggiungere pulsanti di accesso e disconnessione

1. Creare una sottocartella denominata `azure`, per i file specifici di Azure, nella cartella `./src`. 

1. Creare un nuovo file per la configurazione dell'autenticazione nella cartella `azure`, denominato `azure-authentication-config.ts`, e inserire tramite copia il codice seguente:

    :::code language="typescript" source="~/../js-e2e-client-azure-login-button/src/azure/azure-authentication-config.ts"  highlight="3-4,, 11-12":::

    Questo file legge l'ID applicazione dal file `.env`, imposta la sessione come archiviazione browser invece di cookie e fornisce l'accesso che rispetta le informazioni personali.

1. Creare un nuovo file per il middleware dell'autenticazione di Azure nella cartella `azure`, denominato `azure-authentication-context.ts`, e inserire tramite copia il codice seguente:

    :::code language="typescript" source="~/../js-e2e-client-azure-login-button/src/azure/azure-authentication-context.ts"  highlight="43, 58, 65":::

    Questo file fornisce l'autenticazione ad Azure per un utente con le funzioni `login` e `logout`.

1. Creare un nuovo file per il file del componente pulsante dell'interfaccia utente nella cartella `azure`, denominato `azure-authentication-component.tsx`, e inserire tramite copia il codice seguente:

   :::code language="typescript" source="~/../js-e2e-client-azure-login-button/src/azure/azure-authentication-component.tsx"  highlight="3, 11, 23, 29, 36":::

   Questo componente pulsante esegue l'accesso di un utente e restituisce l'account utente al componente chiamante/padre.

   Il testo e la funzionalità del pulsante vengono attivati e disattivati in base allo stato di connessione attuale dell'utente, acquisito con la funzione `onAuthenticated` come proprietà passata al componente.

   Quando un utente esegue l'accesso, il pulsante chiama il metodo della libreria di autenticazione di Azure, `authenticationModule.login` con `returnedAccountInfo` come funzione di callback. L'account utente restituito viene quindi passato al componente padre con la funzione `onAuthenticated`.


1. Aprire il file `./src/App.tsx` e sostituire tutto il codice con il codice seguente per incorporare il componente pulsante di accesso/disconnessione:

   :::code language="typescript" source="~/../js-e2e-client-azure-login-button/src/App.tsx"  highlight="10, 37-42":::

    Dopo l'accesso da parte dell'utente e il reindirizzamento a questa app da parte dell'autenticazione, viene visualizzato l'oggetto currentUser. 

## <a name="6-run-react-spa-app-with-login-button"></a>6. Eseguire l'applicazione a pagina singola React con il pulsante di accesso

1. Nel terminale di Visual Studio Code avviare l'app:

    ```bash
    npm run start
    ```

1. Selezionare il pulsante **Login** nel Web browser. 

    :::image type="content" source="../media/tutorial-login-button/create-react-app-before-authentication-login-button-display.png" alt-text="Selezionare il pulsante **Login**.":::  

1. Selezionare un account utente. Non è necessario che sia lo stesso account usato per accedere al portale di Azure, ma deve essere un account che offre l'autenticazione Microsoft.

    :::image type="content" source="../media/tutorial-login-button/authentication-popup-select-user-account.png" alt-text="Selezionare un account utente. Non è necessario che sia lo stesso account usato per accedere al portale di Azure, ma deve essere un account che offre l'autenticazione Microsoft.":::

1. Verificare il popup che mostra 1) nome utente, 2) nome dell'app, 3) autorizzazioni accettate dall'utente e quindi selezionare **Yes**.

    :::image type="content" source="../media/tutorial-login-button/authentication-popup-let-this-app-access-your-info.png" alt-text="Verificare il popup che mostra 1) nome utente, 2) nome dell'app, 3) autorizzazioni accettate dall'utente e quindi selezionare &quot;Yes&quot;.":::

1. Verificare le informazioni sull'account utente. 

    :::image type="content" source="../media/tutorial-login-button/create-react-app-after-authentication-login-button-succeeds.png" alt-text="Verificare le informazioni sull'account utente.":::  

1. Selezionare il pulsante **Logout** dall'app. L'app fornisce collegamenti utili che consentono alle app utente Microsoft di revocare autorizzazioni. 

## <a name="7-clean-up-resources"></a>7. Pulire le risorse

Al termine dell'esercitazione, eliminare l'applicazione dall'[elenco di registrazione app](https://portal.azure.com/?quickstart=True#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredApps) del portale di Azure.

Se si vuole mantenere l'app ma revocare l'autorizzazione concessa all'app dall'account utente specifico, usare uno dei collegamenti seguenti:

* [Revocare un'autorizzazione di AAD](https://myapps.microsoft.com/)
* [Revocare un'autorizzazione Consumer](https://account.live.com/consent/manage)

## <a name="next-steps"></a>Passaggi successivi

Questa app fornisce l'autenticazione utente per l'app e restituisce le informazioni sull'utente. La funzionalità di autenticazione può interrompersi qui per una versione semplice dell'app oppure è possibile aggiungere funzionalità per gestire la gestione degli utenti dell'app e l'autorizzazione degli utenti per le funzionalità dell'app. 

La gestione degli utenti può essere archiviata in un database di Azure Active Directory o in un database personalizzato, in base alle funzionalità e agli strumenti selezionati. 

L'autorizzazione degli utenti può essere fornita da Azure oppure è possibile sviluppare un'autorizzazione senza Azure o combinare le due opzioni per un'esperienza personalizzata di autorizzazione, ruoli e funzionalità dell'app. 

* Continuare a usare la [libreria MSAL](https://docs.microsoft.com/azure/active-directory/develop/msal-overview) per ottenere il profilo utente e fornire l'accesso non interattivo
* Aggiungere [Microsoft Graph](https://docs.microsoft.com/graph/overview) per accedere ad account utente in Microsoft 365 e includere funzionalità di posta elettronica e appuntamenti del calendario