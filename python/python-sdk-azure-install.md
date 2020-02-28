---
title: Installare Azure SDK per Python
description: Come installare Azure SDK per Python con pip o GitHub. Azure SDK può essere installato in forma di librerie singole o come pacchetto completo.
ms.date: 10/31/2019
ms.topic: conceptual
ms.custom: seo-python-october2019
ms.openlocfilehash: c244280703400a3dabd150c10e66550dbc064abc
ms.sourcegitcommit: c34647aee3b9a72fa0ee6aeac2dfa1e36d67c7ef
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/20/2020
ms.locfileid: "77504560"
---
# <a name="install-the-azure-sdk-for-python"></a>Installare Azure SDK per Python

Azure SDK per Python offre un'API tramite la quale è possibile interagire con Azure dal codice Python. Installare singole librerie dall'SDK in base a specifiche esigenze.

Azure SDK per Python viene testato e supportato con CPython versioni 2.7 e 3.5.3+ e con PyPy 5.4+. Gli sviluppatori usano anche l'SDK con altri interpreti, ad esempio IronPython e Jython, ma è possibile che si verifichino problemi isolati e incompatibilità. Se è necessario un interprete Python, installare la versione più recente da [python.org/downloads](https://www.python.org/downloads).

## <a name="install-sdk-libraries-using-pip"></a>Installare le librerie SDK tramite pip

Azure SDK per Python è costituito da una serie di librerie singole che effettuano il provisioning o funzionano con servizi specifici di Azure. È possibile installare ognuna usando `pip install <library>`. Per istruzioni e documentazione specifiche per ogni libreria, vedere la [pagina relativa alle versioni dell'SDK](https://azure.github.io/azure-sdk/releases/latest/python.html).

Se ad esempio si usa Archiviazione di Azure, è possibile installare la libreria `azure-storage-file`, `azure-storage-blob` o `azure-storage-queue` library. Se si usano tabelle di Azure Cosmos DB, installare `azure-cosmosdb-table`. Funzioni di Azure è supportato tramite la libreria `azure-functions` e così via. Le librerie che iniziano con `azure-mgmt-` forniscono l'API per effettuare il provisioning delle risorse di Azure.

### <a name="install-specific-library-versions"></a>Installare versioni specifiche della libreria

Se è necessario installare una versione specifica di una libreria, indicarla nella riga di comando:

```bash
pip install azure-storage-blob==12.0.0
```

> [!NOTE]
> Nei sistemi Linux, l'SDK non supporta l'uso di `sudo pip install` per installare una libreria per tutti gli utenti. Ogni utente deve usare `pip install` separatamente. 

### <a name="install-preview-packages"></a>Installare i pacchetti di anteprima

Microsoft rilascia regolarmente le librerie SDK di anteprima che supportano le funzionalità future. Per installare l'anteprima più recente di una libreria, includere il flag `--pre` nella riga di comando. 

```bash
# Install all preview versions of the Azure SDK for Python
pip install --pre azure

# Install the preview version for azure-storage-blob only.
pip install --pre azure-storage-blob
```

## <a name="verify-sdk-installation-details-with-pip"></a>Verificare i dettagli dell'installazione dell'SDK con pip

Usare il comando `pip show <library>` per verificare che sia stata installata una libreria. Se la libreria è installata, il comando consente di visualizzare la versione e altre informazioni di riepilogo. Se la libreria non è installata, il comando non visualizza alcun elemento.

```bash
# Check installation of the Azure SDK for Python
pip show azure

# Check installation of a specific library
pip show azure-storage-blob
```

È anche possibile usare `pip freeze` o `pip list` per visualizzare tutte le librerie installate nell'ambiente Python corrente.

## <a name="uninstall-azure-sdk-for-python-libraries"></a>Disinstallare le librerie di Azure SDK per Python

Per disinstallare una singola libreria, usare `pip uninstall <library>`.

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"]
> [Informazioni su come usare l'SDK](python-sdk-azure-get-started.yml)
