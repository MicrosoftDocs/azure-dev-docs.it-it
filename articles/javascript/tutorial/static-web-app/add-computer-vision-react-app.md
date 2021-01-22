---
title: Codice React con Visione artificiale
description: Questa sezione dell'esercitazione _esamina_ le procedure e il codice. Non è necessario eseguire questa procedura per questa esercitazione.
ms.topic: tutorial
ms.date: 12/17/2020
ms.custom: devx-track-js
ms.openlocfilehash: 95486bd7551b87e0db9e01d372888bbaefb02443
ms.sourcegitcommit: 593d177cfb5f56f236ea59389e43a984da30f104
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/18/2021
ms.locfileid: "98560977"
---
# <a name="6-review-how-to-add-computer-vision-to-the-react-app"></a>6. Esaminare come aggiungere Visione artificiale all'app React

Questo esempio include tutto il codice TypeScript necessario per aggiungere Visione artificiale all'app React. Questa sezione dell'esercitazione _esamina_ le procedure e il codice. Non è necessario eseguire questa procedura per questa esercitazione. 

* [Codice di esempio](https://github.com/Azure-Samples/js-e2e-client-cognitive-services)
* Servizi di Azure
    * [App Web statica](/azure/static-web-apps)
    * [Visione artificiale di Servizi cognitivi](/azure/cognitive-services/computer-vision/)

## <a name="add-computer-vision-to-local-react-app"></a>Aggiungere Visione artificiale all'app React locale

Usare npm per aggiungere Visione artificiale al file package.json. 

```bash
npm install @azure/cognitiveservices-computervision 
```

## <a name="add-computer-vision-code-as-separate-module"></a>Aggiungere il codice di Visione artificiale come modulo separato

Il codice di Visione artificiale è contenuto in un file separato denominato `./src/azure-cognitiveservices-computervision.js`. La funzione principale del modulo è evidenziata. 

:::code language="javascript" source="~/../js-e2e-client-cognitive-services/src/azure-cognitiveservices-computervision.js" highlight="55-75" :::

## <a name="add-catalog-of-images-as-separate-module"></a>Aggiungere un catalogo di immagini come modulo separato

L'app seleziona un'immagine casuale da un catalogo se l'utente non immette un URL di immagine. La funzione di selezione casuale è evidenziata 

:::code language="javascript" source="~/../js-e2e-client-cognitive-services/src/DefaultImages.js" highlight="33-35" :::

## <a name="add-custom-computer-vision-module-to-react-app"></a>Aggiungere un modulo personalizzato di Visione artificiale all'app React

Aggiungere metodi al file `app.js` dell'app React. L'analisi delle immagini e la visualizzazione dei risultati sono evidenziate.

:::code language="javascript" source="~/../js-e2e-client-cognitive-services/src/App.js" highlight="20-25, 29-42" :::

## <a name="next-step"></a>Passaggio successivo

> [!div class="nextstepaction"]
> [Pulire le risorse](clean-up-resources.md)