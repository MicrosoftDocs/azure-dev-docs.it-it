---
title: Come installare i pacchetti di librerie di Azure SDK per Python
description: Informazioni su come installare, disinstallare e verificare Azure SDK per le librerie Python tramite pip. Sono inclusi i dettagli per l'installazione di versioni specifiche e pacchetti di anteprima.
ms.date: 01/22/2021
ms.topic: conceptual
ms.custom: devx-track-python
ms.openlocfilehash: 109631849b94f79530c93a8984813c424968f199
ms.sourcegitcommit: 6fbf9e489b194586887a2c11152044be5b3a2b99
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/25/2021
ms.locfileid: "98759553"
---
# <a name="how-to-install-azure-library-packages-for-python"></a>Come installare i pacchetti di librerie di Azure per Python

Azure SDK per Python è costituito esclusivamente da molte librerie singole, elencate nell'[indice dei pacchetti](azure-sdk-library-package-index.md). Per installare i pacchetti di librerie specifici necessari per un progetto, usare `pip install`.

Con queste librerie è possibile effettuare il provisioning e gestire le risorse nei servizi di Azure tramite le librerie di gestione, i cui nomi includono`-mgmt`, quindi connettersi a tali risorse dal codice dell'app tramite le librerie client.

## <a name="install-the-latest-version-of-a-library"></a>Installare l'ultima versione di una libreria

```cmd
pip install azure-storage-blob
```

```cmd
pip install azure-mgmt-storage
```

`pip install` recupera l'ultima versione di una libreria nell'ambiente Python corrente.

Nei sistemi Linux è necessario installare separatamente una libreria per ogni utente. L'installazione delle librerie per tutti gli utenti con `sudo pip install` non è supportata.

## <a name="install-specific-library-versions"></a>Installare versioni specifiche della libreria

```cmd
pip install azure-storage-blob==12.0.0
```

```cmd
pip install azure-mgmt-storage==10.0.0
```

Specificare la versione sulla riga di comando con `pip install`.

## <a name="install-preview-packages"></a>Installare i pacchetti di anteprima

```cmd
pip install --pre azure-storage-blob
```

```cmd
pip install --pre azure-mgmt-storage
```

Per installare l'anteprima più recente di una libreria, includere il flag `--pre` nella riga di comando.

Microsoft rilascia regolarmente pacchetti di librerie in anteprima che supportano funzionalità imminenti, con l'avvertenza che la libreria è soggetta a modifiche e non deve essere usata nei progetti di produzione.

## <a name="verify-a-library-installation"></a>Verificare l'installazione di una libreria

```cmd
pip show azure-storage-blob
```

```cmd
pip show azure-mgmt-storage
```

Usare `pip show <library>` per verificare se una libreria è installata. Se la libreria è installata, il comando visualizza la versione e altre informazioni riepilogative, altrimenti non visualizza nulla.

È anche possibile usare `pip freeze` o `pip list` per visualizzare tutte le librerie installate nell'ambiente Python corrente.

## <a name="uninstall-a-library"></a>Disinstallare una libreria

```cmd
pip uninstall azure-storage-blob
```

Per disinstallare una libreria, usare `pip uninstall <library>`.
