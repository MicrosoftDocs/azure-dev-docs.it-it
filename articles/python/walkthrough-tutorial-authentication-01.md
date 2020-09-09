---
title: 'Procedura dettagliata: Autenticare le app Python con i servizi di Azure'
description: Una procedura dettagliata su come autenticare un'app Python con Azure Active Directory, Azure Key Vault e Archiviazione code di Azure usando la libreria azure-identity di Azure Python SDK.
ms.date: 08/24/2020
ms.topic: conceptual
ms.custom: devx-track-python
ms.openlocfilehash: 7f716f3276c0b93ec37ba0941f4029b2ee817fa0
ms.sourcegitcommit: 324da872a9dfd4c55b34739824fc6a6598f2ae12
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/02/2020
ms.locfileid: "89379542"
---
# <a name="walkthrough-integrated-authentication-for-python-apps-with-azure-services"></a>Procedura dettagliata: Autenticazione integrata per le app Python con i servizi di Azure

Azure Active Directory (Azure AD) e Azure Key Vault offrono un modo completo e pratico per l'autenticazione delle applicazioni con i servizi di Azure e con i servizi di terze parti in cui sono coinvolte le chiavi di accesso.

Dopo aver fornito alcune informazioni di base, questa procedura dettagliata illustra queste funzionalità di autenticazione nel contesto dell'esempio [github.com/Azure-Samples/python-integrated-authentication](https://github.com/Azure-Samples/python-integrated-authentication).

Per praticità, l'esempio è già distribuito in Azure in modo da poterlo vedere in funzione. Se si vuole distribuire l'esempio nella propria sottoscrizione di Azure, il repository include anche gli script di distribuzione dell'interfaccia della riga di comando di Azure.

## <a name="part-1-background"></a>Parte 1: Background

Sebbene molti servizi di Azure si basino esclusivamente sul controllo degli accessi in base al ruolo per l'autorizzazione, alcuni servizi controllano l'accesso alle rispettive risorse usando segreti o chiavi. Tali servizi includono Archiviazione di Azure, database, Servizi cognitivi, Key Vault e Hub eventi.

Quando si crea un'app cloud che usa risorse all'interno di questi servizi, si usa il portale di Azure, l'interfaccia della riga di comando di Azure o Azure PowerShell per creare e configurare le chiavi per l'app associate a criteri di accesso specifici. Tali chiavi impediscono l'accesso a tali risorse specifiche dell'app da qualsiasi altro codice non autorizzato.

All'interno di questa progettazione generale, le app cloud devono in genere gestire tali chiavi ed eseguire l'autenticazione con ogni servizio individualmente, un processo che può essere noioso e soggetto a errori. La gestione delle chiavi direttamente nel codice dell'app rischia anche l'esposizione di tali chiavi nel controllo del codice sorgente e le chiavi potrebbero essere archiviate in workstation per sviluppatori non protette.

Fortunatamente, Azure offre due servizi specifici per semplificare il processo e garantire una maggiore sicurezza:

- Azure Key Vault offre un'archiviazione sicura basata sul cloud per le chiavi di accesso, insieme a chiavi di crittografia e certificati, che non sono trattati in questo articolo. Con Key Vault, l'app accede a tali chiavi solo in fase di esecuzione, in modo che non vengano mai visualizzate direttamente nel codice sorgente.

- Con le identità gestite di Azure Active Directory (Azure AD), l'app deve eseguire l'autenticazione una sola volta con Active Directory. L'app viene quindi autenticata automaticamente con gli altri servizi di Azure, tra cui Key Vault. Di conseguenza, il codice non deve mai occuparsi di chiavi o altre credenziali per tali servizi di Azure. Meglio ancora, è possibile eseguire lo stesso codice in locale e nel cloud con requisiti di configurazione minimi.

Usando Azure AD e Key Vault insieme, l'app non deve mai autenticarsi con i singoli servizi di Azure e può accedere in modo semplice e sicuro alle chiavi necessarie per i servizi di terze parti.

> [!IMPORTANT]
> Questo articolo usa il termine generico comune "chiave" per fare riferimento a ciò che viene archiviato come "segreti" in Azure Key Vault, ad esempio una chiave di accesso per un'API REST. Questo utilizzo non deve essere confuso con la gestione delle chiavi di *crittografia* di Key Vault, che è una funzionalità separata dai *segreti* di Key Vault.

## <a name="example-cloud-app-scenario"></a>Scenario di app cloud di esempio

Per comprendere in modo più approfondito il processo di autenticazione di Azure, considerare lo scenario seguente:

- Un'app principale espone un endpoint API pubblico (non autenticato) che risponde alle richieste HTTP con dati JSON. L'endpoint di esempio, come illustrato in questo articolo, viene implementato come una semplice app Flask distribuita nel Servizio app di Azure.

- Per generare la risposta, l'API richiama un'API di terze parti che richiede una chiave di accesso. L'app recupera la chiave di accesso da Azure Key Vault in fase di esecuzione.

- Prima di restituire la risposta, l'API scrive un messaggio in una coda di Archiviazione di Azure per l'elaborazione successiva. L'elaborazione specifica di questi messaggi non è pertinente per lo scenario principale.

![Diagramma dello scenario dell'applicazione](media/walkthrough-tutorial-authentication/scenario-diagram.png)

> [!NOTE]
> Anche se un endpoint API pubblico è generalmente protetto dalla propria chiave di accesso, ai fini di questo articolo si presuppone che l'endpoint sia aperto e non autenticato. Questa ipotesi evita confusione tra le esigenze di autenticazione dell'app con quelle di un chiamante *esterno* di questo endpoint. Questo scenario non illustra tale chiamante esterno.

> [!div class="nextstepaction"]
> [Parte 2: Esigenze di autenticazione >>>](walkthrough-tutorial-authentication-02.md)
