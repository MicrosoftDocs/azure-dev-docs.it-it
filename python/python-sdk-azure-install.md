---
title: Installazione
description: Come installare Azure Python SDK
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 06/05/2017
ms.topic: conceptual
ms.devlang: python
ms.openlocfilehash: e72d150ce8902d556045f74c5df0c7ffabe08cf2
ms.sourcegitcommit: 2efdb9d8a8f8a2c1914bd545a8c22ae6fe0f463b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/15/2019
ms.locfileid: "68285752"
---
# <a name="installation"></a>Installazione

## <a name="which-python-and-which-version-to-use"></a>Tipo e versione di Python da utilizzare

Sono disponibili diversi interpreti Python, ad esempio:

* CPython: l'interprete Python standard e più comunemente usato
* PyPy - implementazione alternativa rapida e conforme a CPython
* IronPython: interprete Python eseguito in .NET/CLR
* Jython: interprete Python eseguito sulla macchina virtuale Java

**CPython** versione v2.7 o v3.4+ e PyPy 5.4.0 sono testati e supportati per Python Azure SDK.

## <a name="where-to-get-python"></a>Dove è possibile reperire Python

È possibile ottenere CPython in diversi modi:

* Direttamente da [Python](https://www.python.org/)
* Da un distributore attendibile, ad esempio [Anaconda](https://www.anaconda.com/), [Enthought](https://www.enthought.com/) o [ActiveState](https://www.activestate.com/)
* Compilandolo da codice sorgente.

Salvo esigenze specifiche, si consiglia di scegliere le prime due opzioni.

## <a name="installation-with-pip"></a>Installazione con pip

È possibile installare singolarmente la libreria di ogni servizio di Azure:

```bash
pip install azure-batch          # Install the latest Batch runtime library
pip install azure-mgmt-scheduler # Install the latest Storage management library
```

I pacchetti di anteprima possono essere installati mediante il flag `--pre` :

```bash
pip install --pre azure-mgmt-compute # will install only the latest Compute Management library
```

È inoltre possibile installare un set di librerie di Azure in una singola riga utilizzando il meta pacchetto `azure` .

```bash
pip install azure
```

Viene pubblicata una versione di anteprima del pacchetto, a cui è possibile accedere tramite il flag --pre:

```bash
pip install --pre azure
```

## <a name="install-from-github"></a>Eseguire l'installazione da GitHub

Se si vuole installare `azure` dall'origine:

```bash
git clone git://github.com/Azure/azure-sdk-for-python.git
cd azure-sdk-for-python
python setup.py install
```

## <a name="install-an-older-version-with-pip"></a>Installare una versione precedente con pip
È possibile installare una versione precedente di `azure` specificando 'azure==3.0.0' come dettagli della versione.
```bash
pip install azure==3.0.0 
```
## <a name="check-sdk-installation-details-with-pip"></a>Controllare i dettagli dell'installazione dell'SDK con pip
È possibile controllare il percorso di installazione e i dettagli della versione dell'SDK per `azure`, oltre ad altre informazioni.
```bash
pip show azure # Show installed version, location details etc.
pip freeze     # Output installed packages in requirements format.
pip list       # List installed packages, including editables.
```
## <a name="to-uninstall-with-pip"></a>Per eseguire la disinstallazione con pip
È possibile disinstallare un set di librerie di Azure in una singola riga usando il meta pacchetto `azure`.
```bash
pip uninstall azure 
```
> [!NOTE]
> Il comando `pip uninstall azure` consente di rimuovere il meta pacchetto `azure`, lasciando indietro i singoli pacchetti `azure-*`, oltre ad altri, quali `adal` e `msrest`. Una peculiarità di Python e pip è che se sono presenti pacchetti con dipendenze, disinstallando il pacchetto iniziale le dipendenze non verranno disinstallate. Per rimuovere `azure-` e i pacchetti di supporto, eseguire il comando `pip freeze | grep 'azure-' | xargs pip uninstall -y`, quindi eseguire separatamente le disinstallazioni di adal, msrest e msrestazure.

