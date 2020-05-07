---
title: Installare Azure SDK per le librerie Python
description: Informazioni su come installare, disinstallare e verificare Azure SDK per le librerie Python tramite pip. Sono inclusi i dettagli per l'installazione di versioni specifiche e pacchetti di anteprima.
ms.date: 04/23/2020
ms.topic: conceptual
ms.openlocfilehash: 172091bcdff5ba55ccc7c2f13c60f3c7d645efc1
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/05/2020
ms.locfileid: "82138747"
---
# <a name="install-azure-sdk-for-python-libraries"></a>Installare Azure SDK per le librerie Python

Azure SDK per Python offre un'API tramite la quale è possibile interagire con Azure dal codice Python. I nomi di tutte le librerie correnti sono disponibili nella [pagina di indice di Azure SDK per Python](https://azure.github.io/azure-sdk/releases/latest/all/python.html).

Le librerie i cui nomi iniziano con `azure-mgmt` sono librerie di *gestione*, da usare per il provisioning e la gestione delle risorse di Azure come si farebbe tramite il [portale di Azure](https://portal.azure.com) o l'[interfaccia della riga di comando di Azure](/cli/azure/install-azure-cli). Ad esempio, per eseguire il provisioning e la gestione di risorse di Archiviazione di Azure, usare la libreria `azure-mgmt-storage`.

Tutte le altre librerie dell'SDK sono librerie *client*, che servono per usare dal codice dell'applicazione le risorse già sottoposte a provisioning. Ad esempio, per usare i BLOB del servizio di archiviazione di Azure dal codice dell'applicazione, usare la libreria `azure-storage-blob`.

## <a name="install-the-latest-version-of-a-library"></a>Installare l'ultima versione di una libreria

Con l'esecuzione di `pip install` viene installata l'ultima versione di una libreria nell'ambiente Python corrente:

```bash
pip install azure-storage-blob
```

Nei sistemi Linux, l'SDK non supporta l'uso di `sudo pip install` per installare una libreria per tutti gli utenti. Ogni utente deve usare `pip install` separatamente.

## <a name="install-specific-library-versions"></a>Installare versioni specifiche della libreria

Se è necessario installare una versione specifica di una libreria, indicarla nella riga di comando:

```bash
pip install azure-storage-blob==12.0.0
```

## <a name="install-preview-packages"></a>Installare i pacchetti di anteprima

Microsoft rilascia regolarmente le librerie dell'SDK di anteprima che supportano funzionalità imminenti, con l'avvertenza che la libreria è soggetta a modifiche e non deve essere usata nei progetti di produzione.

Per installare l'anteprima più recente di una libreria, includere il flag `--pre` nella riga di comando.

```bash
pip install --pre azure-storage-blob
```

## <a name="verify-a-library-installation"></a>Verificare l'installazione di una libreria

Usare `pip show <library>` per verificare se una libreria è installata. Se la libreria è installata, il comando visualizza la versione e altre informazioni riepilogative, altrimenti non visualizza nulla.

```bash
pip show azure-storage-blob
```

È anche possibile usare `pip freeze` o `pip list` per visualizzare tutte le librerie installate nell'ambiente Python corrente.

## <a name="uninstall-a-library"></a>Disinstallare una libreria

Per disinstallare una libreria, usare `pip uninstall <library>`.

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"]
> [Informazioni su come usare l'SDK](azure-sdk-get-started.yml)
