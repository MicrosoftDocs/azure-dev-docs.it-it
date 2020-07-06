---
title: Scegliere la piattaforma di sviluppo front-end per la creazione di applicazioni client con Visual Studio e i servizi di Azure
description: Informazioni sui linguaggi nativi e multipiattaforma supportati per la creazione di applicazioni client.
author: codemillmatt
ms.service: mobile-services
ms.assetid: 355f0959-aa7f-472c-a6c7-9eecea3a34b9
ms.topic: conceptual
ms.date: 06/08/2020
ms.author: masoucou
ms.openlocfilehash: 8972c2b24334c964e00f5a3ccc27fa0c99e6f597
ms.sourcegitcommit: f02ff55cef47581303a217cf1d310879cd722237
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/09/2020
ms.locfileid: "84632483"
---
# <a name="choose-a-mobile-development-framework"></a>Scegliere il framework di sviluppo per dispositivi mobili

Gli sviluppatori possono usare tecnologie lato client per creare autonomamente applicazioni per dispositivi mobili con framework e modelli specifici per un approccio multipiattaforma. In base ai loro fattori decisionali, gli sviluppatori possono creare:

- Applicazioni native per piattaforma singola usando linguaggi come Objective C e Java
- Applicazioni multipiattaforma usando Xamarin, .NET e C#
- Applicazioni ibride usando Cordova e relative varianti

## <a name="native-platforms"></a>Piattaforme native

La creazione di un'applicazione nativa richiede linguaggi di programmazione, SDK, ambienti di sviluppo e altri strumenti specifici per la piattaforma offerti da fornitori di sistemi operativi.

### <a name="ios"></a>iOS

Creato e sviluppato da Apple, iOS viene usato per creare app in dispositivi Apple, in particolare iPhone e iPad.

- **Linguaggi di programmazione**: Objective-C, Swift
- **IDE**: Xcode
- **SDK**: iOS SDK

### <a name="android"></a>Android

Progettato da Google, Android è il sistema operativo più diffuso nel mondo e viene usato per creare applicazioni eseguibili in un'ampia gamma di smartphone e tablet.

- **Linguaggio di programmazione**: Java, Kotlin 
- **IDE**: Android Studio e strumenti di sviluppo Android 
- **SDK**: Android SDK

### <a name="windows"></a>Windows

- **Linguaggio di programmazione**: C#
- **IDE**: Visual Studio, Visual Studio Code
- **SDK**: Windows SDK

#### <a name="native-platform-pros"></a>Vantaggi delle piattaforme native

- Esperienza utente soddisfacente
- Applicazioni reattive con prestazioni elevate e possibilità di interfacciarsi con librerie native
- Applicazioni altamente sicure

#### <a name="native-platform-cons"></a>Svantaggi delle piattaforme native

- Applicazioni eseguibili in un'unica piattaforma
- Uso più intensivo di risorse di sviluppo e costi elevati per la creazione di applicazioni
- Basso riutilizzo del codice

## <a name="cross-platforms-and-hybrid-applications"></a>Applicazioni ibride e multipiattaforma

Le applicazioni multipiattaforma offrono la possibilità di scrivere una volta le applicazioni native per dispositive mobili, di condividere il codice e di eseguirle in iOS, Android e Windows.

### <a name="xamarin"></a>Xamarin

Di proprietà di Microsoft, [Xamarin](https://visualstudio.microsoft.com/xamarin/) si usa per creare applicazioni affidabili per dispositivi mobili multipiattaforma in C#. Xamarin include una libreria di classi e un runtime compatibili con molte piattaforme, come iOS, Android e Windows. Compila inoltre applicazioni native (non interpretate) che offrono prestazioni elevate. Xamarin combina tutte le funzionalità delle piattaforme native con l'aggiunta di diverse funzionalità potenti.

- **Linguaggio di programmazione**: C#
- **IDE**: Visual Studio in Windows o Mac

### <a name="react-native"></a>React Native

Rilasciato da Facebook nel 2015, [React Native](https://facebook.github.io/react-native/) è un framework JavaScript [open source](https://github.com/facebook/react-native) per la scrittura di applicazioni per dispositivi mobili reali e con rendering nativo per iOS e Android. Si basa su React, la libreria JavaScript di Facebook per la creazione di interfacce utente. Invece che al browser, è destinato a piattaforme mobili. React Native usa componenti nativi invece di componenti Web come blocchi predefiniti.

- **Linguaggio di programmazione**: JavaScript
- **IDE**: Visual Studio Code

### <a name="unity"></a>Unity

 Unity è un motore ottimizzato per la creazione di giochi. È possibile usarlo per creare applicazioni 2D o 3D di alta qualità con C# per piattaforme come Windows, iOS, Android e Xbox.

### <a name="cordova"></a>Cordova

Cordova consente di creare applicazioni ibride usando Strumenti di Visual Studio per Apache Cordova o Visual Studio Code con estensioni per Cordova. Con l'approccio ibrido, è possibile condividere i componenti con i siti Web e riutilizzare le applicazioni basate su server Web con approcci di applicazioni Web ospitate basati su Cordova.

#### <a name="cross-platform-pros"></a>Vantaggi della multipiattaforma

- Maggiore usabilità del codice tramite la creazione di un'unica codebase per più piattaforme
- Possibilità di rivolgersi a un pubblico più ampio in molte piattaforme
- Riduzione significativa dei tempi di sviluppo
- Facilità di avvio e aggiornamento

#### <a name="cross-platform-cons"></a>Svantaggi della multipiattaforma

- Prestazioni inferiori
- Mancanza di flessibilità
- Ogni piattaforma prevede un set univoco di caratteristiche e funzionalità per rendere più creativa l'applicazione nativa
- Tempi più lunghi di progettazione dell'interfaccia utente
- Disponibilità limitata di strumenti
