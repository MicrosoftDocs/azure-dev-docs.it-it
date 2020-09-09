---
title: 'Procedura dettagliata, parte 2: Autenticare le app Python con i servizi di Azure'
description: Informazioni sulle diverse esigenze e problematiche dell'autenticazione nello scenario di esempio e su come rispondere con l'autenticazione integrata di Azure.
ms.date: 08/24/2020
ms.topic: conceptual
ms.custom: devx-track-python
ms.openlocfilehash: c6719e3c86b590edff551d98e5a961fd08f857c3
ms.sourcegitcommit: 324da872a9dfd4c55b34739824fc6a6598f2ae12
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/02/2020
ms.locfileid: "89379541"
---
# <a name="part-2-authentication-needs-in-the-scenario"></a>Parte 2: Esigenze di autenticazione nello scenario

[Parte precedente: Introduzione e contesto](walkthrough-tutorial-authentication-01.md)

In questo scenario di esempio l'app principale presenta i requisiti di autenticazione seguenti:

- Eseguire l'autenticazione con Azure Key Vault per accedere alla chiave API di terze parti archiviata.
- Eseguire l'autenticazione con l'API di terze parti usando la chiave API.
- Eseguire l'autenticazione con Archiviazione code di Azure usando le credenziali necessarie per l'account di archiviazione.

Con questi tre requisiti distinti, l'applicazione deve gestire tre set di credenziali: due per le risorse di Azure (Key Vault e Archiviazione code) e una per una risorsa esterna (l'API di terze parti).

Come indicato in precedenza, è possibile gestire in modo sicuro tutte le credenziali in Key Vault, ad eccezione di quelle necessarie per Key Vault stesso. Una volta eseguita l'autenticazione con Key Vault, l'applicazione può recuperare qualsiasi altra chiave in fase di esecuzione per l'autenticazione con servizi come Archiviazione code.

Questo approccio, tuttavia, richiede comunque all'app di gestire separatamente le credenziali per Key Vault. In che modo, dunque, è possibile gestire le credenziali in modo sicuro e averle a disposizione sia per lo sviluppo locale che per la distribuzione di produzione nel cloud?

Una soluzione parziale consiste nell'archiviare la chiave in una variabile di ambiente lato server, ovvero tramite un'impostazione dell'applicazione con il servizio app di Azure e Funzioni di Azure, che mantiene almeno la chiave al di fuori del controllo del codice sorgente. Tuttavia, per eseguire il codice in una workstation di sviluppo, è necessario replicare la variabile di ambiente in locale, rischiando l'esposizione e/o l'inclusione accidentale delle credenziali nel controllo del codice sorgente. È possibile risolvere il problema in una certa misura implementando procedure speciali nella versione di sviluppo del codice, ma in questo caso il processo di sviluppo risulta più complesso.

Fortunatamente, l'autenticazione integrata con Azure Active Directory (AD) consente a un'app di evitare completamente la gestione delle credenziali di Azure.

## <a name="integrated-authentication-with-managed-identity"></a>Autenticazione integrata con identità gestita

Molti servizi di Azure, come Archiviazione e Key Vault, sono integrati con Azure Active Directory (Azure AD), per cui l'autenticazione dell'applicazione con Azure AD eseguita tramite un'[identità gestita](/azure/active-directory/managed-identities-azure-resources/overview) viene automaticamente estesa ad altre risorse connesse. L'autorizzazione per l'identità viene gestita tramite [controllo degli accessi in base al ruolo](how-to-assign-role-permissions.md) e talvolta tramite altri criteri di accesso.

Questa integrazione implica che non è mai necessario gestire credenziali correlate ad Azure nel codice dell'app e che tali credenziali non vengono mai visualizzate nelle workstation di sviluppo o nel controllo del codice sorgente. Inoltre, tutte le operazioni di gestione delle chiavi per le API e i servizi di terze parti vengono eseguite interamente in fase di esecuzione, preservandone così la sicurezza.

L'identità gestita funziona specificamente con le app distribuite in Azure. Per lo sviluppo locale, creare un'entità servizio separata che funge da identità dell'app durante l'esecuzione in locale. È possibile rendere questa entità servizio disponibile per le librerie di Azure usando le variabili di ambiente, come descritto in [Configurare l'ambiente di sviluppo locale - Configurare l'autenticazione](configure-local-development-environment.md#configure-authentication). A questa entità servizio si assegnano anche autorizzazioni di ruolo, oltre all'identità gestita usata nel cloud.

Dopo aver eseguito questi passaggi per l'entità servizio locale, lo stesso codice funziona sia localmente che nel cloud per autenticare l'app con le risorse di Azure. Queste informazioni sono riportate in [Come autenticare e autorizzare le app](azure-sdk-authenticate.md), ma la versione breve è la seguente:

1. Nel codice creare un oggetto `DefaultAzureCredential` che usi automaticamente l'identità gestita durante l'esecuzione in Azure e l'entità servizio separata durante l'esecuzione in locale.

1. Usare queste credenziali quando si crea l'oggetto client appropriato per qualsiasi risorsa a cui si vuole accedere (Key Vault, Archiviazione code e così via).

1. L'autenticazione viene quindi eseguita quando si chiama un metodo di operazione tramite l'oggetto client, che genera una chiamata API REST alla risorsa.

1. Se l'identità dell'app è valida, Azure controlla se tale identità è anche autorizzata per l'operazione specifica.

Nella parte restante di questa esercitazione vengono illustrati tutti i dettagli del processo nel contesto dello scenario di esempio e del codice di esempio associato.

Nello script di provisioning dell'esempio tutte le risorse vengono create in un gruppo di risorse denominato `auth-scenario-rg`. Questo gruppo viene creato usando il comando [`az group create`](/cli/azure/group?view=azure-cli-latest#az-group-create) dell'interfaccia della riga di comando di Azure.

> [!div class="nextstepaction"]
> [Parte 3: Implementazione dell'API di terze parti di esempio >>>](walkthrough-tutorial-authentication-03.md)
