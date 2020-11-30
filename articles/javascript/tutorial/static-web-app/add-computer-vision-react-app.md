---
title: Codice React con Visione artificiale
description: Questo esempio include tutto il codice necessario per aggiungere Visione artificiale all'app React. Questa sezione dell'esercitazione _esamina_ le procedure e il codice.
ms.topic: tutorial
ms.date: 11/13/2020
ms.custom: devx-track-js
ms.openlocfilehash: a246e2e367aef4027516468ae691aa31117cd6c7
ms.sourcegitcommit: 4dac39849ba2e48034ecc91ef578d11aab796e58
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/20/2020
ms.locfileid: "94993469"
---
# <a name="5-review-how-to-add-computer-vision-to-the-react-app"></a>5. Esaminare come aggiungere Visione artificiale all'app React

Questo esempio include tutto il codice necessario per aggiungere Visione artificiale all'app React. Questa sezione dell'esercitazione _esamina_ le procedure e il codice. Non è necessario eseguire questa procedura per questa esercitazione. 

## <a name="add-computer-vision-to-local-react-app"></a>Aggiungere Visione artificiale all'app React locale

Usare npm per aggiungere Visione artificiale al file package.json. 

```bash
npm install @azure/cognitiveservices-computervision 
```

## <a name="add-computer-vision-code-as-separate-module"></a>Aggiungere il codice di Visione artificiale come modulo separato

Il codice di Visione artificiale è contenuto in un file separato denominato `./src/VisualAI.js`. La funzione principale del modulo è evidenziata. 

:::code language="javascript" source="~/../js-e2e-client-cognitive-services/src/VisualAI.js" highlight="55-75" :::

## <a name="add-catalog-of-images-as-separate-module"></a>Aggiungere un catalogo di immagini come modulo separato

L'app seleziona un'immagine casuale da un catalogo se l'utente non immette un URL di immagine. La funzione di selezione casuale è evidenziata 

:::code language="javascript" source="~/../js-e2e-client-cognitive-services/src/DefaultImages.js" highlight="33-35" :::

## <a name="add-custom-computer-vision-module-to-react-app"></a>Aggiungere un modulo personalizzato di Visione artificiale all'app React

Aggiungere metodi al file `app.js` dell'app React. L'analisi delle immagini e la visualizzazione dei risultati sono evidenziate.

:::code language="javascript" source="~/../js-e2e-client-cognitive-services/src/App.js" highlight="20-25, 29-42" :::

## <a name="next-step"></a>Passaggio successivo

> [!div class="nextstepaction"]
> [Pulire le risorse](clean-up-resources.md) 