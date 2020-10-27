---
title: file di inclusione tutorial-azure-web-app-mongodb-00.md
description: file di inclusione tutorial-azure-web-app-mongodb-00.md
ms.date: 10/13/2020
ms.topic: include
ms.custom: devx-track-javascript
ms.openlocfilehash: b9ab409c845b7690474eb47117df4401ecaf59cd
ms.sourcegitcommit: ced8331ba36b28e6e2eacd23a64b39ddc7ffe6ab
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/21/2020
ms.locfileid: "92344164"
---
In questa esercitazione si usa un'app React per caricare un file in un BLOB di archiviazione di Azure. 

L'attività di programmazione viene eseguita automaticamente. Questa esercitazione è incentrata sull'uso corretto degli ambienti di Azure locali e remoti all'interno Visual Studio Code con le estensioni di Azure.

## <a name="top-tasks"></a>Attività principali

Questa esercitazione include diverse **attività principali di Azure** per sviluppatori JavaScript:

* Eseguire un'app React in locale con Visual Studio Code
* Creare una risorsa di archiviazione e configurarla per i caricamenti di file
    * Configurare CORS
    * Creare un token di firma di accesso condiviso
* Configurare il codice per la libreria client di Azure SDK per l'uso del token di firma di accesso condiviso per l'autenticazione nel servizio

## <a name="sample-application"></a>Applicazione di esempio

L'app React di esempio, [disponibile in GitHub](https://github.com/Azure-Samples/js-e2e-browser-file-upload-storage-blob), è costituita dagli elementi seguenti:

* **App React** ospitata sulla porta 3000
* Script della libreria client di Azure SDK da caricare nei BLOB di archiviazione

:::image type="content" source="../../media/tutorial-browser-file-upload/browser-react-app-azure-storage-resource-image-uploaded-displayed.png" alt-text="Semplice app React connessa ai BLOB di archiviazione di Azure.":::
