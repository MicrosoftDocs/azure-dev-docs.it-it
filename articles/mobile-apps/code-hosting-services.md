---
title: Ospitare il codice sorgente delle applicazioni per dispositivi mobili nel cloud con GitHub e Azure DevOps
description: Informazioni sui servizi Microsoft disponibili per ospitare il codice delle applicazioni per dispositivi mobili nel cloud.
author: codemillmatt
ms.assetid: 12a8a079-9b3c-4faf-2222-ccff02097224
ms.service: mobile-services
ms.topic: article
ms.date: 06/08/2020
ms.author: masoucou
ms.openlocfilehash: 157a9e7a3f9f366035512d97a843d31dcfdc3bdb
ms.sourcegitcommit: f02ff55cef47581303a217cf1d310879cd722237
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/09/2020
ms.locfileid: "84632343"
---
# <a name="cloud-hosted-source-code-management-services"></a>Servizi di gestione del codice sorgente ospitato nel cloud

Se si hanno team di sviluppo con più membri che lavorano sulla stessa codebase, è necessario trovare un posto appropriato in cui ospitare il codice. Con l'archiviazione dei dati nel cloud e con un repository centralizzato, tutti possono caricare, modificare e gestire i file di codice. Possono anche interagire con altri sviluppatori sui progetti. Il codice può essere immediatamente accessibile ovunque ci si trovi, purché si abbia una connessione Internet.

L'hosting nel cloud è molto più semplice rispetto alle opzioni locali. Richiede una minore configurazione hardware e consente alle organizzazioni di completare il processo di implementazione in modo più agile.

## <a name="benefits-of-hosting-source-code-in-the-cloud"></a>Vantaggi dell'hosting di codice sorgente nel cloud

- **Archiviazione centralizzata nel cloud:** visualizzare e gestire i dati ovunque.
- **Collaborazione più efficace e codice più pulito:** tenere traccia del codice all'interno dei team e gestire i progetti per assicurare la distribuzione continua di software eccellente.
- **Maggiore facilità di partecipazione:** contribuire facilmente ai progetti.
- **Ciclo di rilascio più veloce:** collaborare più rapidamente con i team e contribuire facilmente ai propri progetti.
- **Ridurre i costi:** evitare di preoccuparsi della gestione di hardware, server, VPN e così via.

Usare i servizi seguenti per gestire e archiviare i dati delle applicazioni nel cloud.

## <a name="github"></a>GitHub

[GitHub](https://github.com/) è un servizio di repository open source che ospita progetti di codice sorgente in un'ampia gamma di linguaggi di programmazione diversi. GitHub tiene traccia delle varie modifiche apportate a ogni iterazione.

### <a name="github-key-features"></a>Principali funzionalità di GitHub

- Usare l'hosting per mantenere tutto il codice in un'unica posizione. I repository privati, pubblici e open source sono tutti dotati di strumenti per l'hosting, il controllo delle versioni e il rilascio di codice.
- Esaminare il codice e usare gli strumenti predefiniti per fare in modo che la revisione del codice diventi una parte essenziale del processo del team:
  - Proteggere i rami, proporre modifiche e richiedere revisioni.
  - Individuare le differenze, aggiungere commenti nel contesto e ottenere feedback ben definito.
- Coordinare in anticipo, rimanere allineati e aumentare le produttività con gli strumenti di gestione progetti di GitHub:
  - Avere una visione generale del progetto.
  - Usare le lavagne delle attività situate accanto al codice all'interno di GitHub.
  - Trascinare le schede per assegnare problemi o richieste pull ai membri del team.
  - Impostare attività cardine per organizzare e tenere traccia dello stato di avanzamento.
  - Scrivere note per acquisire idee che potrebbero risultare utili ma non appartengono a uno specifico problema o richiesta pull.
- Individuare e scegliere con facilità gli strumenti appropriati per migliorare la comunicazione e l'automazione del lavoro acquistando applicazioni da [GitHub Marketplace](https://github.com/marketplace).
- Gestire ed espandere i team usando: 
  - Ruoli utente per organizzare i team e le autorizzazioni di accesso.
  - Strumenti per thread di discussione per fare in modo che le conversazioni rimangano in tema e il team resti concentrato.
  - Linee guida della community per configurare rapidamente i nuovi membri del team con un account.
- Esplorare e valutare i progetti più popolari per seguirli.
- Sviluppare una rete di contatti utili nel settore.

### <a name="github-references"></a>Informazioni di riferimento per GitHub

- [GitHub](https://github.com/)
- [Guide di GitHub](https://guides.github.com/)
- [Forum della community di GitHub](https://github.community/)
- [GitHub Marketplace](https://github.com/marketplace)

## <a name="azure-devops"></a>Azure DevOps

[Azure DevOps](https://azure.microsoft.com/services/devops/) supporta [Azure Repos](https://azure.microsoft.com/services/devops/repos/) e il [controllo della versione di Team Foundation](https://docs.microsoft.com/azure/devops/repos/tfvc/index?view=azure-devops) come opzioni di controllo del codice sorgente. Include un numero illimitato di repository privati gratuiti, con revisioni collaborative del codice, gestione avanzata dei file, ricerca di codice e criteri di ramo per garantire codice di alta qualità. Azure Repos è ideale per piccoli progetti e grandi organizzazioni che necessitano del supporto nativo e dei criteri avanzati di Azure Active Directory.

### <a name="azure-devops-features"></a>Funzionalità di Azure DevOps

- Usare un repository illimitato di codice sorgente Git ospitato sul cloud per i repository pubblici e privati:
  - Ottenere supporto per qualsiasi client Git.
  - Usare webhook e l'integrazione delle API.
- Avviare la compilazione successiva da una richiesta pull nel repository:
  - Collaborare per creare codice migliore usando thread di discussioni e l'integrazione continua per ogni modifica.
  - Configurare l'integrazione continua/distribuzione continua (CI/CD) per attivare automaticamente le compilazioni, i test e le distribuzioni con ogni richiesta pull completata. È possibile usare Azure Pipelines o i propri strumenti.
  - Proteggere la qualità del codice con i criteri di ramo.
- Usare il controllo della versione di Team Foundation, che include la revisione del codice, per mantenerlo centralizzato.
- Connettersi al codice usando Xcode, Eclipse, IntelliJ, Android Studio, Visual Studio, Visual Studio Code e altro ancora.
- Usare potenti funzionalità di ricerca semantica del codice.
- Trarre vantaggio da funzionalità specifiche per aziende. Azure DevOps offre l'integrazione nativa con Azure Active Directory, che semplifica il processo di gestione dell'accesso ai repository di codice.
- Assicurare la qualità del codice con i criteri di ramo, ad esempio un numero minimo di revisioni del codice, i requisiti per compilazioni di successo e l'applicazione di strategie di merge di Git.
- Connettere il proprio ambiente di sviluppo preferito per accedere ai repository e gestire il lavoro.

### <a name="azure-devops-references"></a>Informazioni di riferimento per Azure DevOps

- [Introduzione ad Azure Repos](https://azure.microsoft.com/services/devops/repos/) 
- [Documentazione di Azure Repos](/azure/devops/repos/?view=azure-devops)
